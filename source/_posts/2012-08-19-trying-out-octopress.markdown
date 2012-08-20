---
layout: post
title: "Blog Moved to Octopress"
date: 2012-08-19 12:24
comments: true
categories: 
---

I'm experimenting with [Octopress](http://octopress.org) as a potential Wordpress replacement.


I've grown tired of trying to properly format code with the Wordpress editor, so I've moved my blog from
Wordpress (and before that Blogger) to Jekyll + Octopress, hosted on [Github](http://github.com).

Let's see how code formats under Octopress:

``` ruby print_tree
  def print_tree(node, indent)

    puts indent + "#{node} -> "

    if node.kind_of? Container
      node.children.each do |child|
        print_tree(child, indent + Control::INDENT)
      end
    end

  end
```

Beautiful!
