---
comments: true
date: 2012-02-19 14:26:22
layout: post
slug: twilio-and-adobe-flash
title: Twilio and Adobe Flash
wordpress_id: 194
categories:
- twilio
- web development
---

I started doing some [Twilio development](http://www.twilio.com/api/sms) recently and ran into a problem with Adobe Flash. [Twilio Client](http://www.twilio.com/api/client) (which lets you make phone calls right from your browser) relies on the Flash plugin. It pops up this nice little settings dialog the first time it runs to ask your permission:

![image](/images/2012/02/flash.png)

The problem is that on Chrome, **it won't let you actually click any of those buttons**- the dialog is non-responsive to mouse clicks. This was really frustrating, and a few minutes of Googling showed that this was an old problem supposedly fixed by a Flash update months ago.

Updating to the latest Flash didn't help (it's apparently bundled with Chrome, and doesn't use the version you can install manually on OSX).

Then I came across [this trick](http://reviews.cnet.com/8301-13727_7-20090579-263/adobe-flash-update-fixes-unresponsive-settings-in-os-x-lion/): use tab to navigate the dialog checkboxes, and spacebar to select/deselect. Works like a charm.
