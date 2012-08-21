---
comments: true
date: 2010-11-22 10:38:25
layout: post
slug: measuring-typing-accuracy
title: Measuring Typing Accuracy
wordpress_id: 27
categories:
- Technical
- Typing
---

It turns out that measuring typing accuracy (how accurately you hit the intended keys) is non-trivial. A quick survey of the online typing courses out there shows that there is considerable confusion over how to correctly measure typing accuracy. One site actually gives a _negative percentage_ if you backspace too many times!

This was kind of surprising to me. I figured it would take me perhaps 10 minutes to implement. It ended up taking a day and a half of my time, $10 spent on an ACM article, and some quality time with a recent CS grad's [PhD thesis](http://www.dynamicnetservices.com/~will/academic/Soukoreff%20PhD%20Dissertation.pdf). No joke.

Well, to be completely honest, in the end, the implementation _was_ in fact fairly trivial. What was hard was figuring out _what to measure_. It's more of a human problem than a technical one.


### Why is this hard?


Your intuition might be to simply measure the ratio of correct characters to total characters typed. At first glance this seems OK, but consider the following case:

    
    intended: The quick brown frog jumped over the lazy fox.
    actual  : The qu<span style="color:#993300;font-family:Monospace;">ck brown frog jumped over the lazy fox.</span>


If we match up the typed characters with what was expected, the accuracy of this typed statement is a very low 15 %. But looking at it from a human perspective, what were the actual errors here? The typist missed the "i" character in the word "quick", and as a result the rest of the line was offset by one.  It seems like there ought to be a way to more accurately capture the fact that the user made a single typo. There is.


### Edit Distance


[Edit Distance](http://en.wikipedia.org/wiki/Edit_distance) is the number of operations required to transform one string into another. The operations include _insertion_, _deletion_ and _substitution_. In this example, it requires only a single edit- 'insert an i'.

Soukoreff's [thesis](http://www.dynamicnetservices.com/~will/academic/Soukoreff%20PhD%20Dissertation.pdf) describes a _Minimum String Distance Error Rate_ as the Edit Distance divided by the maximum of the lengths of the presented vs typed text, times 100%. For our "off by one" example above, the error rate is a mere 2%, i.e. 98% accurate. That seems more reasonable!

So for a while I was using the [Levenshtein Distance algorithm](http://en.wikipedia.org/wiki/Levenshtein_distance) to compute the edit distance, and hence the accuracy. This certainly worked better than my initial naive algorithm, but it still had a problem- What if the typist made _lots_ of errors, but was fastidious about backspacing over the mistakes and correcting them? His error rate would be zero in this case, with an accuracy of 100%.

While correcting all your mistakes is admirable, it seems wrong to award a "100% accurate" rating to a typist sporting a bruised backspace key. Somehow, we've got to take the mistakes into account, even if they've been corrected.


### Text-Entry Taxonomy


Soukoreff's PhD thesis again provides a solution. Classify each of the keystrokes into one of the following categories:



	
  * **C** - Correctly typed

	
  * **IF** - Incorrect, but Fixed

	
  * **INF** - Incorrect, Not Fixed

	
  * **F** - Fixes, i.e. backspace or cursor movement used to correct mistakes


Using these classifications, he proposes a _Total Error Rate_ defined as:


> (INF + IF) / (C + INF + IF) * 100%


So, going back to our "off by one" example, if the typist noticed immediately that he missed the 'i' in 'quick', and backspaced to correct it,

    
    The qu<span style="color:#993300;font-family:Monospace;">c</span>⌫ick brown frog jumped over the lazy fox.


the breakdown would be C: 47, F: 1, IF: 1,. INF: 0, for a total error rate of (0 + 1) / (47 + 0 + 1) = 2%, or 98% accurate. Same as using the Edit Distance alone, in this example.

But what if the user didn't notice the mistake until 10 characters later? The input stream would look something like this:

    
    The qu<span style="color:#993300;font-family:Monospace;">ck brown f</span>⌫⌫⌫⌫⌫⌫⌫⌫⌫⌫ick brown frog jumped over the lazy fox.


What's the error rate now? C:47, F:10, IF: 10, INF: 0, or (0 + 10) / (47 + 0 + 10) * 100% = 17.5%, or roughly 82% accurate. Aha!

I plugged this formula into [Quick Brown Frog](http://www.quickbrownfrog.com) and watched the error rate go up and down while typing. _Finally_, it seemed to be doing the right thing- rewarding accurate, deliberate typing, and taking off points for outright mistakes as well expensive corrections.

Yet another example of why it's hard to estimate software schedules- sometimes the 'trivial' is anything but.
