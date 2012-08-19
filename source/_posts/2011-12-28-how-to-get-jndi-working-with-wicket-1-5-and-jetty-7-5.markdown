---
comments: true
date: 2011-12-28 03:29:03
layout: post
slug: how-to-get-jndi-working-with-wicket-1-5-and-jetty-7-5
title: How to get JNDI working with Wicket 1.5 and Jetty 7.5
wordpress_id: 151
---

The Wicket 1.5 archetype sets up a project to use Jetty 7.5. Quite a lot has changed in Jetty since version 6, and this broke my JNDI config. Here's how I put things right again.

First of all, the imports have all been moved in 7.x.  Here's where they landed:

    
    import org.eclipse.jetty.plus.webapp.EnvConfiguration;
    import org.eclipse.jetty.webapp.WebInfConfiguration;
    import org.eclipse.jetty.webapp.Configuration;
    import org.eclipse.jetty.webapp.WebXmlConfiguration;




Next, you'll need a jetty-env.xml.



    
    <?xml version="1.0"?>
    <!DOCTYPE Configure PUBLIC "-//Mort Bay Consulting//DTD Configure//EN" "http://www.eclipse.org/jetty/configure.dtd">
    
    <Configure id="wac" class="org.eclipse.jetty.webapp.WebAppContext">
        <New class="org.eclipse.jetty.plus.jndi.EnvEntry">
        <Arg>jdbc/mydatasource</Arg>
        <Arg>
            <New class="com.mysql.jdbc.jdbc2.optional.MysqlConnectionPoolDataSource">
                <Set name="Url">jdbc:mysql://localhost/mydatabase?characterEncoding=utf8</Set>
                <Set name="User">username</Set>
                <Set name="Password">password</Set>
            </New>
        </Arg>
        </New>
    </Configure>


Normally this goes into src/main/webapp/WEB-INF, but you probably don't want to deploy that with your app in your production war file. So instead I put mine in src/test/jetty/jetty-env.xml. You'll need to modify your Start.java to tell Jetty to find the relocated config file.

    
    EnvConfiguration envConfiguration = new EnvConfiguration();
    URL url = new File("src/test/jetty/jetty-env.xml").toURI().toURL();
    envConfiguration.setJettyEnvXml(url);
    bb.setConfigurations(new Configuration[]{
        new WebInfConfiguration(),
        envConfiguration,
        new WebXmlConfiguration()
    });


I found that I also had to set a couple of environment properties:

    
    System.setProperty("java.naming.factory.url.pkgs",
                       "org.eclipse.jetty.jndi");
    System.setProperty("java.naming.factory.initial",
                       "org.eclipse.jetty.jndi.InitialContextFactory");


With this, I can finally access my JNDI datasource happily from Wicket/Jetty.

Update: I've created a [gist with the full source code.](https://gist.github.com/1539302)
