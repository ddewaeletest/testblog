---
title: Removing branches in GIT
date: 2015-09-14T14:07:14+02:00
layout: post
categories: git
tags: [git, branches, scm]
---
When you're properly merging all your branches your repository can end up with a lot of branches that aren't being used anymore.
In that case you want to remove both the local branch and the remote branch.

This can be done using the following commands :

First look at the current set of branches
{% highlight bash %}
git branch -a
  gh_pages
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/gh_pages
  remotes/origin/master
{% endhighlight %}

Delete the remote branch
{% highlight bash %}
git push origin --delete gh_pages
To git@github_ddewaeletest:ddewaeletest/coolproject1.git
 - [deleted]         gh_pages
{% endhighlight %}

Verify that the remote branch is removed.
{% highlight bash %}
git branch -a
  gh_pages
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
{% endhighlight %}

Delete the branch
{% highlight bash %}
git branch -d gh_pages
error: The branch 'gh_pages' is not fully merged.
If you are sure you want to delete it, run 'git branch -D gh_pages'.
{% endhighlight %}

Force delete the branch
{% highlight bash %}
git branch -D gh_pages
Deleted branch gh_pages (was 9cdc93a).
{% endhighlight %}
