---
title: Introducing Jekyll Blogging Engine
date: 2015-09-15T21:08:45+02:00
layout: post
categories: jekyll blogging
---
In this post we'll be discussing Jekyll, a popular blog engine that is very powerfull and easy to use.

## Installing

You need to install Jekyll through a ```gem```

	gem install jekyll

## Creating a blog

Creating a blog is as simple 

	jekyll new myblog
	
This will create the folder structure and files needed to setup the blog.
Consider this the source code for your blog. By building it, you can generate the necessary static content so that it can be hosted as your blog.

## Basic blog features

- adding posts
- code highlights
- themes
- tags
- post excerpts
- drafts
- publishing
- search
- comments
- rss feeds

### Adding posts

In order to [add posts](https://jekyllrb.com/docs/posts/), we need to create a text file in the ```_posts``` folder. 

{% highlight bash %}
ls -ltr _posts/

-rw-r--r--  1 ddewaele  staff  1221 Sep 14 07:18 2015-09-13-welcome-to-jekyll.markdown
-rw-r--r--  1 ddewaele  staff  2442 Sep 14 07:18 2015-09-12-helloworld.md
-rw-r--r--  1 ddewaele  staff  1257 Sep 14 14:15 2015-09-14-removing-branches-in-git.markdown
-rw-r--r--  1 ddewaele  staff  1773 Sep 14 20:02 2015-09-14-setting-up-a-nodejs-server.markdown
{% endhighlight %}

Each post should have a proper [YAML Front Matter](https://jekyllrb.com/docs/frontmatter/).



### Code highlights

Code highlights using pygments are delivered out of the box.
Simply use the proper ```highlight``` and ```endhighlight``` placeholders

### Themes

Complete this.

### Tags

Complete this.

### Post Excerpts

[Post excerpts](https://jekyllrb.com/docs/posts/#post-excerpts) allow you to define a sneak preview of the content in your posts. By default Jekyll doesn't show them so you'll need to add them to your ```index.html``` like this :

{% highlight html %}
{% raw %}
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

        <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
          <span class="post-meta">{{ post.excerpt }}</span>
        </h2>
      </li>
    {% endfor %}
  </ul>
   {% endraw %}
{% endhighlight %}

by default an excerpt is the first paragraph of the post, but you can [customize this behavior](https://jekyllrb.com/docs/posts/#post-excerpts).

### Deploying

In this section we'll go over deploying your blog to Github pages.

As can we seen in the screenshot below, a jekyll blog can be hosted by Github very easily :

![Git gh-pages]({{ site.url }}/assets/git-gh-pages.png)


There are 2 kinds of GitHub pages

- Project pages
- User pages

In order to publish to a project page we need to do the following steps :

Create a new gh-pages branch
Put your jekyll code-base on the gh-pages branch
push it to github



	gem install github-pages



We can easily customize the look and feel of our site by configuring templates.

The [Solar theme for Jekyll](http://mattvh.github.io/solar-theme-jekyll/) is a nice-looking theme.


Some functionalities we want to cover


## References

- https://github.com/octopress/genesis-theme
- https://github.com/imathis/octopress/wiki/3rd-party-plugins
- https://www.justinrummel.com/migrating-from-octopress-2-to-octopress-3/
- https://github.com/vladigleba/readify
- https://github.com/github/pages-gem
- https://help.github.com/articles/using-jekyll-with-pages/
- https://help.github.com/articles/creating-project-pages-manually/
- https://24ways.org/2013/get-started-with-github-pages/


