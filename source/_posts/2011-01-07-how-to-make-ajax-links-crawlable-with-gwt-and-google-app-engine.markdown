---
comments: true
date: 2011-01-07 18:18:11
layout: post
slug: how-to-make-ajax-links-crawlable-with-gwt-and-google-app-engine
title: How to make Ajax links crawlable with GWT and Google App Engine
wordpress_id: 180
categories:
- Google App Engine
- GWT
- Technical
---

If you care about [SEO](http://en.wikipedia.org/wiki/Search_engine_optimization) you know that using GWT has a downside: much of your app's content is generated dynamically via Javascript, and is therefore invisible to search engines. You might have dozens of pages of awesome content, but all the Googlebot sees is the static HTML page that hosts your app. This can be a real problem.

Fortunately, Google has proposed a [solution](http://code.google.com/web/ajaxcrawling/docs/getting-started.html) for crawling ajax content:



	
  * change your hrefs to support "bang notation":  www.example.com/ajax.html**#!key=value**

	
  * **when Googlebot makes requests of the form: **www.example.com/ajax.html**?_escaped_fragment_=key=value ****, you return a static HTML version of the Ajax content**


So then the problem becomes one of generating static content from your Ajax links. You could do that by hand if your site is small and changes infrequently. More likely you'll want a way to automate this.

Google recommends using a "headless browser" approach, i.e. using something like [HtmlUnit](http://htmlunit.sourceforge.net/). That's a fine solution, but if you're running on App Engine _it's almost guaranteed not to work_ because of the request timeout.  So if you want to run on App Engine, you're probably going to have to spider your own pages and pre-generate your HTML content.

My solution to this problem is to break the spidering up into small chunks, and farm them out as tasks on App Engine's [Task Queues](http://code.google.com/appengine/docs/java/taskqueue/overview.html). Whenever I update my app's content, I submit a job that spiders the landing page looking for Ajax links. For each link that's found, I submit a task that recursively spiders the link (taking care not to get into loops). Each task saves the HTML content into the data store, which is then returned as cached static content to Googlebot.

Suddenly my "simple" solution is sounding quite complicated, but it gets the job done reliably. Here's some code to make it clearer.

I use a CachedAjaxLink data object to persist the static content:

    
    public class CachedAjaxLink implements Serializable
    {
        @Id
        private String href;
        private String cachedContent;
        private Date dateCached;
    }


Then I use an AjaxCacher which crawls a given link, stores the results as CachedAjaxLinks, and queues Task Queue tasks for each link that it finds:

    
    public class AjaxCacher
    {
        protected static final Logger log = Logger.getLogger(AjaxCacher.class.getName());
        protected static final DAO dao = new DAO();
    
        public static final long PUMP_TIME = 5000;
        protected WebClient webClient;
        protected String crawlServletUrl;
    
        public AjaxCacher(String crawlServletUrl)
        {
            this.crawlServletUrl = crawlServletUrl;
            webClient = Holder.get();
        }
    
        public void crawl(URL urlToCrawl, Date crawlRequestTimestamp)
        {
            // URLs we've already queued
            Set queuedURLs = new HashSet();
            queuedURLs.add(urlToCrawl);
    
            try
            {
                HtmlPage page = webClient.getPage(urlToCrawl);
    
                // appengine hack because it's single threaded
                webClient.getJavaScriptEngine().pumpEventLoop(PUMP_TIME);
    
                String pageContent = page.asXml();
    
                CachedAjaxLink cachedAjaxLink = new CachedAjaxLink();
                cachedAjaxLink.setHref(urlToCrawl.getRef());
                cachedAjaxLink.setCachedContent(pageContent);
                cachedAjaxLink.setDateCached(new Date());  // time actually cached
                dao.updateCachedAjaxLink(cachedAjaxLink);
    
                List anchors = page.getAnchors();
                for (HtmlAnchor anchor : anchors)
                {
                    // only care about ajax links
                    if (! anchor.getHrefAttribute().startsWith("#")) continue;
    
                    URL newUrl = new URL(urlToCrawl, anchor.getHrefAttribute());
    
                    // don't queue multiple requests for the same URL
                    if (queuedURLs.contains(newUrl)) continue;
    
                    queuedURLs.add(newUrl);
    
                    // prevent loops
                    CachedAjaxLink link = dao.getCachedAjaxLink(newUrl.getRef());
                    if (link == null || link.getDateCached().getTime() < crawlRequestTimestamp.getTime())
                    {
                        queueCrawlRequest(newUrl.toString(), crawlRequestTimestamp);
                    }
                }
    
            } catch (IOException e)
            {
                log.log(Level.SEVERE, e.getMessage(), e);
            }
            finally
            {
                webClient.closeAllWindows();
            }
        }
    
        /**
         * submits a crawl request to the queue; TaskQueueServlet will then handle the request asynchronously
         */
        public void queueCrawlRequest(String urlToCrawl, Date timeStamp)
        {
            Queue queue = QueueFactory.getDefaultQueue();
            TaskOptions options = TaskOptions.Builder.url(crawlServletUrl);
            options.param("encodedUrl", ServerUtils.encodeURL(urlToCrawl));
            options.param("timeStamp", ServerUtils.fromDate(timeStamp));
            options.method(TaskOptions.Method.GET);
            queue.add(options);
        }
    
        /**
         * try to cache a copy of the WebClient in ThreadLocal for faster startups on Google App Engine
         */
        public static class Holder
        {
            private static ThreadLocal holder = new ThreadLocal()
            {
                protected synchronized WebClient initialValue()
                {
                    WebClient result = new WebClient(BrowserVersion.FIREFOX_3);
                    result.setWebConnection(new UrlFetchWebConnection(result));
                    return result;
                }
            };
    
            public static WebClient get()
            {
                return holder.get();
            }
        }
    }


Finally, I use a TaskQueueServlet to handle the queued tasks:

    
    public class TaskQueueServlet extends HttpServlet
    {
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException
        {
            doPost(req, res);
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException
        {
            String encodedUrl = req.getParameter("encodedUrl");
            if (encodedUrl == null)
            {
                throw new IllegalArgumentException("missing param: encodedUrl");
            }
    
            String timeStamp = req.getParameter("timeStamp");
            if (timeStamp == null)
            {
                throw new IllegalArgumentException("missing param: timeStamp");
            }
    
            String decodedUrl = ServerUtils.decodeURL(encodedUrl);
            URL urlToCrawl = new URL(decodedUrl);
    
            getServletContext().getInitParameter("taskQueuePath");
            AjaxCacher cacher = new AjaxCacher(getServletContext().getInitParameter("taskQueuePath"));
            cacher.crawl(urlToCrawl, ServerUtils.toDate(timeStamp));
        }
    }


Thanks Google, for making it so easy. ;-)

You can grab all of this code from my [gwtquickstarter](http://code.quickbrownfrog.com/) library. It's the library that powers the [best typing tutor](http://www.quickbrownfrog.com) on the web.
