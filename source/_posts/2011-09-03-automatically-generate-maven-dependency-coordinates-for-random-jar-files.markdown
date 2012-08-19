---
comments: true
date: 2011-09-03 16:07:38
layout: post
slug: automatically-generate-maven-dependency-coordinates-for-random-jar-files
title: Automatically generate Maven dependency coordinates for random Jar files
wordpress_id: 142
---

Have you just inherited an Ant project that you're trying to convert to Maven? Maybe it came with a "lib" directory full or random jar files. And worse, some thoughtless developer neglected to include version strings in the filenames?

Fear not! The Sonatype [checksum search](https://repository.sonatype.org/service/local/lucene/search?sha1=";) REST service can give you the Maven coordinates based on the jar's SHA1 hash.

Still too much work? Not to worry, I just wrote a quick program to make it even easier for you. [Provenance](https://github.com/armhold/Provenance) will take a directory full of jar files and write out the XML dependency information for every jar it finds. You can then copy/paste this right into the `<dependencies>` section of your pom.xml.

Enjoy.
