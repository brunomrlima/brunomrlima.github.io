---
layout: post
title:  "Linked List in Ruby"
date:   2022-03-08 08:00:00 -0500
categories: ruby
---

{% highlight ruby %}
class LinkedList
  attr_accessor :head

  def initialize
    @head = nil
  end

  def append(value)
    if @head
      tail.next = new_node(value)
    else
      @head = new_node(value)
    end
  end

  def tail
    return @head if @head.nil?
    node = @head
    while !node.next.nil?
      node = node.next
    end
    
    node
  end

  private

  def new_node(value)
    Node.new(value)
  end
end

class Node
  attr_accessor :value, :next

  def initialize(value, next = nil)
    @value = value
    @next = next
  end
end
{% endhighlight %}
