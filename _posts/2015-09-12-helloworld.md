---
layout: post
title:  "Setting up Jekyll"
date:   2015-09-12 20:10:47
categories: jekyll update
---
I've decided to setup my blog again using Jekyll.
I simply followed the instructions from the [Jekyll website](http://jekyllrb.com/docs/home/).
The only thing I ended up doing was installing pygments

##Installing pyments
sudo pip install pygments
{% highlight bash %}
Collecting pygments
/Library/Python/2.7/site-packages/pip-7.1.2-py2.7.egg/pip/_vendor/requests/packages/urllib3/util/ssl_.py:90: InsecurePlatformWarning: A true SSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. For more information, see https://urllib3.readthedocs.org/en/latest/security.html#insecureplatformwarning.
  InsecurePlatformWarning
  Downloading Pygments-2.0.2-py2-none-any.whl (672kB)
    100% |████████████████████████████████| 675kB 24kB/s 
Installing collected packages: pygments
Successfully installed pygments-2.0.2
{% endhighlight %}

I then generated a default.css file for the pygments syntax highlighting

{% highlight bash %}
pygmentize -S default -f html > css/pygments/default.css
{% endhighlight %}

and added it to my template ```_includes/head.html```

The content looks like this 

{% highlight html%}
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <title>{% if page.title %}{{ page.title }}{% else %}{{ site.title }}{% endif %}</title>
  <meta name="description" content="{% if page.excerpt %}{{ page.excerpt | strip_html | strip_newlines | truncate: 160 }}{% else %}{{ site.description }}{% endif %}">

  <link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">
  <link rel="stylesheet" href="{{ "/css/pygments/defaults.css" | prepend: site.baseurl }}">
  <link rel="canonical" href="{{ page.url | replace:'index.html','' | prepend: site.baseurl | prepend: site.url }}">
  <link rel="alternate" type="application/rss+xml" title="{{ site.title }}" href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" />
</head>
{% endhighlight %}

conent....

 
{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}
