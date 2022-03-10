---
layout: post
title:  "Array combination in Ruby"
date:   2022-03-10 08:00:00 -0500
categories: ruby
---

The challenge is to return all the combinations of an array that has adjacents
indexes.

Example:
{% highlight ruby %}
Input: nums = [1,2,3,4,5]
Output: [[1], [2], [3], [4], [5], [1, 2], [2, 3], [3, 4], [4, 5], [1, 2, 3], [2, 3, 4], [3, 4, 5], [1, 2, 3, 4], [2, 3, 4, 5], [1, 2, 3, 4, 5]]
{% endhighlight %}

Solution:
{% highlight ruby %}
def combination_with_index(array)
    final = []
    (1..array.size).each do |comb|
        final += array.combination(comb).to_a.select do |arr|
            result = true
            (0..arr.size-2).each do |i|
                result = (array.index(arr[i]) + 1 == array.index(arr[i+1]))
                break if result == false
            end

            result
        end
    end

    final
end
{% endhighlight %}
