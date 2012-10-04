---
layout: post
title: "Announcing Ocarina- Optical Character Recognition for Ruby"
date: 2012-10-04 19:45
comments: true
categories: 
---

I just published a bare-bones implementation of an
[Optical Character Recognizer implemented in Ruby](https://github.com/armhold/ocarina).

It's pretty basic, but it does successfully recognize its training set as well as the same characters
with added "noise". It uses a straightforward implementation of a
[feed-forward neural network](http://en.wikipedia.org/wiki/Feedforward_neural_network).

It uses RMagick/ImageMagick to handle image processing, but apart from that it's built from scratch!
