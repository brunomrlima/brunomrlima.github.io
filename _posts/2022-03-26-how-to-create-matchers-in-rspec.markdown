---
layout: post
title:  "How to Create Matchers in RSpec"
date:   2022-03-26 08:00:00 -0500
categories: rspec
---
Reference: [RSpec Custom Matchers][RspecLink]{:target="_blank"}

[RspecLink]: https://relishapp.com/rspec/rspec-expectations/v/2-3/docs/custom-matchers/define-matcher

You can create custom matchers for your testing.

Let's say that you want to test if a hash (`hash_a`) contains another hash (`hash_b`).

At the end of your test file, add the following matcher:

{% highlight ruby %}
it "contains hash" do
    hash_a = {bruno: "monteiro"}
    hash_b = {bruno: "monteiro"}
    expect(hash_a).to contains_hash(hash_b)
end

RSpec::Matchers.define :contains_hash do |hash_b|
    match do |hash_a|
        # >= is a ruby method to check if a hash contains another
        hash_a >= hash_b
    end
end
{% endhighlight %}

But `hash_a` and `hash_b` are not the greatest names for variables within your matcher.
In this case, `hash_a` is going to be the `expected_hash` and `hash_b` is going to be
the `actual_hash`

{% highlight ruby %}
...

RSpec::Matchers.define :contains_hash do |expected_hash|
    match do |actual_hash|
        # >= is a ruby method to check if a hash contains another
        actual_hash >= expected_hash 
    end
end

{% endhighlight %}

### Using diffable

Now you have a working matcher, but is the output of your matcher good? Let's make it fail:

{% highlight ruby %}
it "contains hash" do
    hash_a = {bruno: "monteiro"}
    hash_b = {bruno: "rocha"}
    expect(hash_a).to contains_hash(hash_b)
end
{% endhighlight %}

{:refdef: style="text-align: center;"}
![with_diffable](/assets/images/2022-03-26/without_diffable.png)
{: refdef}

It is easy to see why it's failing because our hash is small, but if the hash was larger,
it would be difficult to see what is wrong with your test. So, you can use a built-in
function in Rspec: `diffable`.

{% highlight ruby %}
...

RSpec::Matchers.define :contains_hash do |expected_hash|
    match do |actual_hash|
        # >= is a ruby method to check if a hash contains another
        actual_hash >= expected_hash
    end

    diffable
end
{% endhighlight %}

{:refdef: style="text-align: center;"}
![with_diffable](/assets/images/2022-03-26/with_diffable.png)
{: refdef}

The tests are looking pretty good now. 

### Using chain
Now let's say that you have a hash of hashes and you want to know if that hash contains 
a certain hash. You can use the built-in RSpec function `chain`:

{% highlight ruby %}
it "contains hash" do
    hash_a = {first_name: {bruno: "monteiro"}, second_name: {rocha: "lima"}}
    hash_b = {bruno: "rocha"}
    expect(hash_a).to contains_hash(hash_b).within(:first_name)
end

RSpec::Matchers.define :contains_hash do |expected_hash|
    match do |actual_hash|
        # >= is a ruby method to check if a hash contains another
        actual_hash >= expected_hash
    end

    diffable
    
    chain :within do |hash_value|
        @hash_value = hash_value
    end
end
{% endhighlight %}