---
comments: true
date: 2009-10-28 10:26:00
layout: post
slug: snow-leopard-migration-assistant-fail
title: Snow Leopard Migration Assistant Fail
wordpress_id: 15
---

[![](http://armhold.files.wordpress.com/2009/10/macpro.jpg?w=300)](http://armhold.files.wordpress.com/2009/10/macpro.jpg)

I've been using a Mac almost exclusively since about 2006. Unsurprisingly, I have quite a lot of data on my system, and on its Time Machine backups. Until recently I have been quite impressed with Apple's Migration Assistant- the tool that (used to!) seamlessly migrate your apps and data from an old machine to a new one. It's one of those things that us smug Mac users brag about to our PC-using friends. Recently however, I've been disappointed by it.


First, my wife got a new MBPro from her job, which replaced her 3 year old MBPro. I told her the migration would be a piece of cake. I expected to do the usual trick where you boot the old machine into Firewire Target Mode, and the new machine sees the old one as a Firewire volume. No such luck- Apple changed the Firewire port on the newer MBPro, so she had to make a trip down to the Apple Store to get a new cable. Turns out they sold her the wrong one, so at this point I opted for doing the migration using our WiFi. Despite entering all the appropriate network configuration, the two computers couldn't talk to each other.




So I ran a Cat5 cable between the two machines. I was able to connect this time, but I kept getting thrown into an endless loop where you are prompted to enter a PIN to give the new computer access to the old one- it would connect for an instant, and then loop back to the beginning all over again.




Wife, sensing my frustration level, decided to bring it into her office and have the IT guys deal with it. They had the appropriate Firewire cable and did the transfer without a hitch.




So fast forward to me getting a new job, and the requisite new toy: a totally decked-out quad-core MacPro with all the bells and whistles. Time to migrate from my old MBPro to this new tower. I'm about 4 hours into the transfer using Migration Assistant (we have the right cable now!) and it's still telling me that there are 10+ hours left in the transfer. This doesn't look good. So I pop open the Console tool, and lo and behold, I see that MigrateTool has in fact crashed, **despite the lack of any error messages on the screen.** I quit Migration Assistant, and start over. Same deal.


[![](http://armhold.files.wordpress.com/2009/10/migrationassistant.png?w=300)](http://armhold.files.wordpress.com/2009/10/migrationassistant.png)




So I try erasing the Mac Pro's HD and starting with a fresh OS install from DVD. Migration Assistant **still** silently crashes.






So I try doing a migration from my Time Machine volume as the source volume in Migration Assistant. Same deal. So I try doing a "manual" installation using Time Machine. A few hours in, I finally get a somewhat helpful error message indicating that it cannot continue because **the source volume is case-sensitive, the target volume is case-insensitive**, and there is a pair of conflicting files on the source drive "3.3.1.ga" and "3.3.1.GA" (Hibernate, my favorite!)




This is not totally surprising, and is what I suspected might be the problem- back in 2006 I had made the poor decision to install my MBPro as case-sensitive, since that's what most Unixes use. That's finally come back to bite me. Anyway, I remove the duplicate file from the source machine, and carry on. Several hours later, Migration Assistant crashes again. And since it doesn't print any helpful error message, I can't locate what is likely another duplicate file. Ugh!




Obviously one solution might be to re-install the machine as case-sensitive, but I don't want to go through this kind of hell again during some future migration; I'd rather pay the piper now and be done with it. I have a call into Apple Tech Support (you get 90 days free support on any new machine, even without Apple Care.. did you know that?) I will update as things progress. Any suggestions for solving this are greatly welcome.



