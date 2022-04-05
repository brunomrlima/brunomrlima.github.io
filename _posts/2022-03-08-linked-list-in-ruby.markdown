---
layout: post
title:  "Linked List in Ruby"
date:   2022-03-08 08:00:00 -0500
categories: ruby
---

{:refdef: style="text-align: center;"}
![My helpful screenshot](/assets/images/linked_list.png)
{: refdef}

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

  def has_cycle?
    slow_runner = @head
    fast_runner = @head&.next

    while !fast_runner.nil? && !slow_runner.nil? && fast_runner != slow_runner
      slow_runner = slow_runner.next
      fast_runner = fast_runner.next&.next
    end

    return false if slow_runner.nil? || fast_runner.nil?
    return true if slow_runner == fast_runner
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
