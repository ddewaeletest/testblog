---
title: Fun with tags
date: 2015-09-16T08:25:44+02:00
layout: post
tags: [jekyll,blogging,tags]
---
Some fun with tags.

The following script displays all the tags :

{% highlight html %}
	{% raw %}
{% capture tags %}
  {% for tag in site.tags %}
    {{ tag[0] }}
  {% endfor %}
{% endcapture %}
{% assign sortedtags = tags | split:' ' | sort %}

{% for tag in sortedtags %}
  <h3 id="{{ tag }}">{{ tag }}</h3>
  <ul>
  {% for post in site.tags[tag] %}
    <li><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></li>
  {% endfor %}
  </ul>
{% endfor %}
	{% endraw %}
{% endhighlight %}

Output : 

{% capture tags %}
  {% for tag in site.tags %}
    {{ tag[0] }}
  {% endfor %}
{% endcapture %}
{% assign sortedtags = tags | split:' ' | sort %}

<ul>
{% for tag in sortedtags %}
  <li><a href="#{{ tag }}">{{ tag }}</a></li>
{% endfor %}
</ul>

<hr/>

{% for tag in sortedtags %}
  <h3 id="{{ tag }}">{{ tag }}</h3>
  <ul>
  {% for post in site.tags[tag] %}
    <li><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></li>
  {% endfor %}
  </ul>
{% endfor %}

end
