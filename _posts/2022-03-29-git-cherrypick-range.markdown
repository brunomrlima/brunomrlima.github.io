---
layout: post
title:  "Git Cherrypick Range of Commits"
date:   2022-03-29 08:00:00 -0500
categories: git
---

You can use `git cherrypick` to pick a commit from one branch to another.
Sometimes it is usefull to bring a range of commits. So you can:

{% highlight bash %}
git cherry-pick <oldest-commit-sha>..<newest-commit-sha>
{% endhighlight %}

However, the way cherrypick works is a bit non-intuitive. 
*The code above does not include the oldest sha, but it includes the newest sha (and everything in between).*

In order to include the oldest sha, then you can make use of `^`:

{% highlight bash %}
git cherry-pick <oldest-commit-sha>^..<newest-commit-sha>
{% endhighlight %}

The code above includes the oldest sha, the newest sha and everything in between.