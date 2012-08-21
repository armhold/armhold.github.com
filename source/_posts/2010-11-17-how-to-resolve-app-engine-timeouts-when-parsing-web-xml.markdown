---
comments: true
date: 2010-11-17 17:09:48
layout: post
slug: how-to-resolve-app-engine-timeouts-when-parsing-web-xml
title: how to resolve App Engine timeouts when parsing web.xml
wordpress_id: 127
categories:
- Google App Engine
- Technical
---

For a few weeks now I've been plagued with timeouts while updating my App Engine apps. At first I thought it was a problem in the Intellij EAP beta I was running, but today I spent some time digging into it.

It seems to be unrelated to the EAP, as I get the same behavior with the App Engine command-line tools- timeout failure parsing web.xml roughly 75% of the time. It looks like this:


> Nov 17, 2010 4:32:54 PM com.google.apphosting.utils.config.AbstractConfigXmlReader readConfigXml
SEVERE: Received exception processing /Users/armhold/work/git-mega-repo/mystore/out/artifacts/Typing_Web_exploded/WEB-INF/web.xml
com.google.apphosting.utils.config.AppEngineConfigException: Received IOException parsing the input stream for /Users/armhold/work/git-mega-repo/mystore/out/artifacts/Typing_Web_exploded/WEB-INF/web.xml
at com.google.apphosting.utils.config.AbstractConfigXmlReader.getTopLevelNode(AbstractConfigXmlReader.java:210)
at com.google.apphosting.utils.config.AbstractConfigXmlReader.parse(AbstractConfigXmlReader.java:228)
at com.google.apphosting.utils.config.WebXmlReader.processXml(WebXmlReader.java:142)
at com.google.apphosting.utils.config.WebXmlReader.processXml(WebXmlReader.java:22)
at com.google.apphosting.utils.config.AbstractConfigXmlReader.readConfigXml(AbstractConfigXmlReader.java:111)
at com.google.apphosting.utils.config.WebXmlReader.readWebXml(WebXmlReader.java:73)
at com.google.appengine.tools.admin.Application.(Application.java:105)
at com.google.appengine.tools.admin.Application.readApplication(Application.java:151)
at com.google.appengine.tools.admin.AppCfg.(AppCfg.java:115)
at com.google.appengine.tools.admin.AppCfg.(AppCfg.java:61)
at com.google.appengine.tools.admin.AppCfg.main(AppCfg.java:57)
Caused by: java.net.ConnectException: Operation timed out
After some [digging around](http://code.google.com/p/googleappengine/issues/detail?id=1235#c15), it seems to be related to a timeout deep in the bowels of Java's XML libs while trying to validate the DTD for web.xml.


The fix is simple: elide the DTD declaration from the top of your web.xml:


> <!DOCTYPE web-app    PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"    "http://java.sun.com/dtd/web-app_2_3.dtd">
