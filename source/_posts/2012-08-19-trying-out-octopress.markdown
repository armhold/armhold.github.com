---
layout: post
title: "Trying out Octopress"
date: 2012-08-19 12:24
comments: true
categories: 
---

I'm experimenting with Octopress as a potential Wordpress replacement. I've grown tired of trying to properly
format code with the Wordpress editor.

So here's some code:

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
