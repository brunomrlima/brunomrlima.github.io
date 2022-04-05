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

Tests in RSpec for it:

{% highlight ruby %}
require("./linked_list")

RSpec.describe LinkedList do
  let(:node) { Node.new(3, nil) }
  let(:list) { LinkedList.new }

  context "Node" do
    it "returns a node value" do 
        expect(node.value).to eq(3)
    end
  end

  describe "LinkedList" do
    it "initialize head of linked list as nil" do
        expect(list.head).to eq(nil)
    end

    context "#append" do
      it "appends a node to an empty list" do
        list.append(4)

        expect(list.head.value).to eq(4)
        expect(list.head.next).to eq(nil)
      end

      it "appends a node to the end of the list" do
        list.append(4).append(5).append(6)

        expect(list.head.value).to eq(4)
        expect(list.head.next.value).to eq(5)
        expect(list.head.next.next.value).to eq(6)
        expect(list.head.next.next.next).to eq(nil)
      end
    end

    context "#tail" do
      it "returns tail node" do
        list.append(4).append(5)

        expect(list.tail.value).to eq(5)
        expect(list.tail.next).to eq(nil)
      end
    end

    context "#has_cycle?" do
      it "returns true because it has cycle" do
        list.append(4).append(5).append(6)
        list.tail.next = list.head

        expect(list.head.next.next.next).to eq(list.head)
        expect(list.has_cycle?).to eq(true)
      end

      it "returns false because it has no cycle" do
        list.append(4).append(5).append(6)

        expect(list.head.next.next.next).to eq(nil)
        expect(list.has_cycle?).to eq(false)
      end
    end
  end
end
