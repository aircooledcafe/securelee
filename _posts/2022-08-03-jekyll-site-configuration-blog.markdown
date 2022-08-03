---
layout: post
title: How I configure Jekyll and set up my Github Blog
date: 2022-08-03 14:00
tags: jekyll bundle blog github pages website DNS Windows
---
# Setting up Jekyll locally

I just wanted a basic blog with very little overhead that I could host on GitHub pages, discovering that Github supports Jekyll I decide to use it. It's a piece of software written in Ruby on Rails and has many decent plugins, though I didn't use many.  

Firs step are to install Ruby on Rails, bundle and Jekyll. To that end I just followed the guide on Jekyll's own site for Windows [here][jekyllinstall].  

Next step is to create the template folder, install additional dependencies and run the server:  

{% highlight console %}
jekyll new blog  
cd blog
gem install webrick
bundle exec jekyll serve  
{% endhighlight %}

This is supposed to work, though I had issues, I needed to delete my gemfile.lock.  
Add `gem "webrick" line to the end of my gemfile, then run `bundle exec jekyll serve`.  

You can now access your website on https://localhost:4000/  

# Configuration for GitHub Pages





[jekyllinstall]: https://jekyllrb.com/docs/installation/windows/