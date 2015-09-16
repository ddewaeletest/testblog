---
title: Organizing your blog
date: 2015-09-16T07:54:05+02:00
layout: post
tags: [blogging,git]
---
Organizing your content can be a difficult tasks. In this post we'll go over 2 important aspects that will help you organize your content, ```categories``` and ```tags```.

## Categories

In the Yaml Front Matter you can specify categories :

{% highlight yaml %}
---
title: Introducing Jekyll Blogging Engine
date: 2015-09-15T21:08:45+02:00
layout: post
categories: [jekyll, blogging]
---
{% endhighlight %}

As you can see here, 2 categories have been defined

- jekyll
- blogging

Jekyll will create a permalink to this post using the following URL :

http://[your blog address]/jekyll/blogging/2015/09/15/introducing-jekyll-blogging-engine.html

As you can see, the categories are included in the URL, just before the date.

Also note that you should see this as a category hierarchy ```jekykll/blogging```. There are no indiviual ```jekyll``` and ```blogging``` categories created here.

## Tags

The Yaml Front Matter also allows you to specify tags in the same way you specify categories :

{% highlight yaml %}
---
title: Introducing Jekyll Blogging Engine
date: 2015-09-15T21:08:45+02:00
layout: post
categories: [jekyll, blogging]
tags: [github, github-pages, jekyll, blogging]
---
{% endhighlight %}


## References

- [Q/A about tags and github-pages](http://stackoverflow.com/questions/1408824/an-easy-way-to-support-tags-in-a-jekyll-blog)
- [Sample repo using categories / tags](https://github.com/minddust/minddust.github.io)
- [HOW TO USE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/tags-and-categories-on-github-pages/)


