---
comments: true
date: 2011-08-04 19:58:34
layout: post
slug: adding-git-shas-to-wicket-pages-automatically
title: Adding Git SHAs to Wicket Pages Automatically
wordpress_id: 128
---

If you have a non-trivial project, it's handy to be able to tell what code was used to build a particular release once it's been deployed. Especially if you've recently discovered the joys of branching and merging with Git.

Here's a handy way to **add a Git SHA** to all your app's pages via **Wicket and Maven**.


# Maven


First, we'll use the exec-maven-plugin to create a git.properties file for us. Add this to the <plugins> section in your pom.xml:

    
    <plugin>
       <groupId>org.codehaus.mojo</groupId>
       <artifactId>exec-maven-plugin</artifactId>
       <version>1.1</version>
       <executions>
           <execution>
              <phase>compile</phase>
              <goals>
                 <goal>exec</goal>
              </goals>
           </execution>
       </executions>
       <configuration>
           <executable>git</executable>
           <arguments>
                <argument>log</argument>
                <argument>--pretty=format:gitsha=%H %ci</argument>
                <argument>-n1</argument>
           </arguments>
           <outputFile>target/classes/git.properties</outputFile>
       </configuration>
    </plugin>


This will create a git.properties file containing the Git SHA, along with the commit timestamp whenever your code is compiled. You can learn how to further customize this [here](http://www.kernel.org/pub/software/scm/git/docs/git-log.html).


# Wicket Application Subclass


Now we'll need to read in the git.properties file when our application starts up.

    
    public class Application extends WebApplication
    {
        private String gitSHA;
    
        public AppgravityApplication()
        {
            java.util.Properties props = new java.util.Properties();
            try
            {
                props.load(Thread.currentThread().getContextClassLoader().getResourceAsStream("git.properties"));
                gitSHA = props.getProperty("gitsha");
                log.info("gitsha: " + gitSHA);
            }
            catch (IOException e)
            {
                log.severe(e.getMessage());
                gitSHA = "unknown";
            }
        }
    
        public String getGitSHA()
        {
            return gitSHA;
        }




# Wicket WebPage Subclass


Now we'll create a WebPage subclass that renders the Git SHA into a <meta> tag when the page is rendered.

    
    public abstract class MyPage extends WebPage
    {
        @Override
        protected void onBeforeRender()
        {
            Label metaGitSHA = new Label("metaGitSHA", "");
            metaGitSHA.add(new AttributeModifier("content", Model.of(((Application) getApplication()).getGitSHA())));
            addOrReplace(metaGitSHA);
            super.onBeforeRender();
        }
    }


You'll want to extend MyPage for each of your pages. You'll need to add the placeholder meta tag to each of your HTML pages like this:

    
    <head>
        <meta wicket:id="metaGitSHA" id="metaGitSHA" name="metaGitSHA" content=""/>
    </head>


And you're done!
