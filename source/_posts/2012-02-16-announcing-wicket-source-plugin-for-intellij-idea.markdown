---
comments: true
date: 2012-02-16 00:06:07
layout: post
slug: announcing-wicket-source-plugin-for-intellij-idea
title: 'Announcing: Wicket-Source plugin for Intellij IDEA'
wordpress_id: 185
categories:
- intellij-idea
- web development
- wicket
tags:
- intellij-idea
---

The folks at [42lines](https://www.42lines.net/) have released an awesome [Firefox plugin](https://www.42lines.net/2012/01/31/announcing-wicket-source/) called "Wicket-Source". It allows you to easily navigate from your browser to the corresponding Wicket source code.

Since their plugin is Eclipse-based, I wrote up a compatible plugin for Intellij IDEA. You can install it from [the repository](http://plugins.intellij.net/plugin/?idea&id=6846), or build it yourself from the [source on Github](https://github.com/armhold/wicket-source-intellij).

There are two parts to this plugin: the Firefox extension (provided by 42lines) and the IDE plugin; you need both. To install the Firefox plugin, follow the [directions from 42lines](https://github.com/42Lines/wicket-source/wiki). Then to install the Intellij plugin do the following:



	
  1. Open the Preferences dialog (Intellij IDEA menu -> Preferences)

	
  2. Under "IDE Settings" select "Plugins"

	
  3. Click the "Browse Repositories" button.

	
  4. In the search box type "wicket", which should narrow the results significantly.

	
  5. Right-click "Wicket Source", and select "Download and Install".




You'll be asked to re-start Intellij, and then you should be in business. The plugin uses port  9123 and no password by default (same as the Firefox plugin defaults). To change this, open the IDE Settings dialog and click "Wicket Source" to enter a password.










Enjoy!


![image](/images/2012/02/wicket-source.png)
