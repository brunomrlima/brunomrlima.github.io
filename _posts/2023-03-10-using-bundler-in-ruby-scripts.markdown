---
layout: post
title:  "Using Bundler in Ruby scripts"
date:   2023-03-10 08:00:00 -0500
categories: git
---

Bundler offers a module that we can include in our script and that offers all 
the features of a Gemfile but to define our gems inside the script.

Instead of

{% highlight ruby %}
require 'rainbow'

puts Rainbow("My very important message in red").red
{% endhighlight %}

You can do

{% highlight ruby %}
require 'bundler/inline'

gemfile do
    source 'https://rubygems.org'

    gem 'rainbow', '~> 3.0.0'
end

puts Rainbow("My very important message in red").red
{% endhighlight %}
