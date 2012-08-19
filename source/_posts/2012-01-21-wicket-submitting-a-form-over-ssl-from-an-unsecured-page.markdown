---
comments: true
date: 2012-01-21 18:54:37
layout: post
slug: wicket-submitting-a-form-over-ssl-from-an-unsecured-page
title: 'Wicket: submitting a form over SSL from an unsecured page'
wordpress_id: 166
categories:
- web development
- wicket
---

Lots of folks are making great use of [Twitter Bootstrap](http://twitter.github.com/bootstrap/), which includes a handy login button right at the top:

![image](/images/2012/01/twitter-bootstrap-wicket1.png)

To protect your users' privacy, you should make sure that form is sent over SSL. If the hosting page is https that happens automatically, but most domains don't secure their entire site; only a subset of pages are typically secured with SSL. But since this header likely appears on _all your pages_, how can you secure the form?

The first step is to manually adjust the form's **action** attribute to ensure that the form submission happens over https, rather than http.

But this is where we run into a problem with Wicket- if the hosting page is http, and you have also installed an HttpsMapper in your WicketApplication like this:

    
    setRootRequestMapper(new HttpsMapper(getRootRequestMapper(), new HttpsConfig(HTTP_PORT, HTTPS_PORT)));


then Wicket will not allow your form to be sent over https; the mapper will notice the http/https mismatch, and instead of calling your form's onSubmit() method, it will simply serve up the hosting page again, discarding your form submission.

The solution is to manually post your form to a different, secure page that is marked for https via @RequireHttps. Then the HttpsMapper will allow the form submission to take place.

First, we need a LoginForm that will adjust the form's **action** attribute to point to our secure page:

    
    public class LoginForm extends StatelessForm
    {
        public LoginForm(String id)
        {
            super(id);
            add(new TextField("username").setRequired(true));
            add(new PasswordTextField("password").setRequired(true));
        }
    
       @Override
       protected void onComponentTag(ComponentTag tag)
       {
           super.onComponentTag(tag);
           String action = urlFor(LoginFormHandlerPage.class, null).toString();
           tag.put("action", action);
       }
    }


Now we'll need to create a page to handle the form submission:

    
    @RequireHttps
    public class LoginFormHandlerPage extends WebPage
    {
        public LoginFormHandlerPage(PageParameters parameters)
        {
            HttpServletRequest req = (HttpServletRequest) getRequest().getContainerRequest();
            String username = req.getParameter("username");
            String password = req.getParameter("password");
    
            if (loginSuccessful(username, password))
            {
                 if (! continueToOriginalDestination());
                 {
                     setResponsePage(AccountPage.class);
                 }
            }
            else
            {
                getSession().error("login failed"));
                // on failure send user to our regular login page
                setResponsePage(LoginPage.class);
            }
        }
    }


Note that if you're using Wicket 1.5.3 [there is a bug](https://issues.apache.org/jira/browse/WICKET-4338) that prevents the processing of form POST parameters (that's why we're reading the params manually from the HttpServletRequest). Fixed in Wicket 1.5.4.

The LoginFormHandlerPage will process the submitted form data over https, and if successful, log the user in, else send them to a page where then can re-enter their password.

You can get all the code (and quite a bit more useful login-related stuff) from [github](https://github.com/armhold/justaddwater).

Credit where it's due: the real gem here (submitting the form to a secure url) comes from this [blog posting](http://www.petrikainulainen.net/programming/tips-and-tricks/wicket-https-tutorial-part-three-creating-a-secure-form-submit-from-a-non-secure-page/) by Petri Kainulainen.
