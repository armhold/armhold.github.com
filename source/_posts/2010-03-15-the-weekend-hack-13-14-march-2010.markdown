---
comments: true
date: 2010-03-15 13:21:00
layout: post
slug: the-weekend-hack-13-14-march-2010
title: 'The Weekend Hack: 13-14 March, 2010'
wordpress_id: 44
---

Inspired by [this recent post](http://news.ycombinator.com/item?id=1133613) over at Hacker News, I was itching for a fun weekend project.  My criteria for the project were the following:






	
  * be small & well-defined- doable in a weekend

	
  * produce something useful for _someone_

	
  * give my rusty Google App Engine and GWT skills a workout







I use [Wicket](http://wicket.apache.org/) for my livelihood these days, but [GWT](http://code.google.com/webtoolkit/) is my true love. I miss working with it.







So I decided to create a searchable database of Weight Watchers "food points" values.  Last year, the company that I was working for held weekly WW meetings in our lunch room.  For about 6 months it was all but impossible to avoid hearing "how many points for X?" type questions several times daily.




There are a lot of existing sites for this kind of thing, but I found the interfaces somewhat lacking (are we really still producing sites with MIDI soundtracks in 2010?)




Thus was born [this weekend's project](http://slimpoints.appspot.com/). At least until WW decides to shut it down. Here's the breakdown of where the time went:








	
  * 2 hours brainstorming

	
  * 4 hours getting Intellij, GWT and Google App Engine to all play nicely (this was the most painful part, really)

	
  * 3 hours basic coding, page layout, getting the tabs to work

	
  * 45 minutes with an Emacs macro converting HTML tables with food point values into a nice, CSV-style GWT properties file

	
  * 2 hours playing around with font sizes and colors.  I'm still not 100% happy with this, but the project was time-limited by definition.

	
  * 1/2 hour enabling AdSense, getting things to look relatively nice







A grand total of 260 lines of code.  GWT's SuggestBox made autocomplete so much easier than Wicket Extensions' AutoCompleteTextField, which I had to use recently.










The project is "done" in the sense that I am happy with it, and would feel content if I never spent another hour of time on it; that was part of my criteria for something doable in a weekend.  But that doesn't mean I'll never revisit it.




One potential idea is to make an iPhone-friendlier version of it, with some big calculator-style buttons right on the page to facilitate data entry.  I might come back to this idea in the future if the site analytics tell me it's worthwhile.  (As a side note, compared to the time and effort required to get an [iPhone app of equivalent complexity](http://itunes.apple.com/us/app/another-tip-calculator-yatc/id316488754?mt=8) published, this was a piece of cake.)




So, what fun projects are you working on?













![](https://blogger.googleusercontent.com/tracker/3562558747791280858-8903738812890999672?l=garmhold.blogspot.com)
