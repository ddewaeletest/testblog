---
title: Setting up a private jekyll blog on your own server
date: 2015-09-17T00:03:41+02:00
layout: post
categories: [jekyll, blogging]
tags: [blogging,git]
---
In this post I'll go over the steps needed to setup a private Jekyll blog on your server. This post assumes you have your own server connected to the internet that is able to run Jekyll. We'll host the Jekyll code in a private repo (Github / Bitbucket / your own repo) but we'll also setup a second remote on your server so that it can build the Jekyll site for you.

There are a couple of steps involved here.

- Push your Jekyll blog on a private repo
- Setup a second remote pointing to your server
- Provide a commit hook so your server can build the jekyll site
- Setup Apache to server the generated jekyll site.

## Setting up your Jekyll blog

Start by creating a new Jekyll blog and commit it to a local git repo.

{% highlight bash %}
jekyll new privateblog
cd privateblog/
git init
git commit -m "Initial commit" -a
{% endhighlight %}

Setup 2 remotes: 

- one pointing to a private git repo where your sources will be hosted
- one pointing to your server where jekyll will be executed

{% highlight bash %}
git remote add origin git@bitbucket.org:davydewaele/privateblog.git
git remote add deploy deployer@deploy.server:~/myrepo.git
{% endhighlight %}

## Setup the git repo on your server

We need to create a user account on our server that will be responsible for 

- hosting a bare git repo
- executing a post commit hook when commits are pushed to the repo
- execute a jekyll build in the post commit hook to build up the site

We'll start by creating the ```deployer``` user on the server and generate a keypair so we can login to the server from our laptop.

{% highlight bash %}
useradd deployer
passwd deployer
su - deployer
ssh-keygen -t rsa

cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
{% endhighlight %}


On our laptop we can create an ```ssh config``` so that we can connect to the machine in an easy way.

{% highlight bash %}
Host deploy.server
  HostName 192.168.0.1
  User deployer
  Port 2347
  IdentityFile /path/to/deployer/id_rsa
{% endhighlight %}

Make sure the private key has the proper permissions (```chmod 400```)

## Ruby installation


## Instaling Jekyll on the server.

On my server I had an old version of Ruby installed, so when I execute the Jekyll install I got the following error :

{% highlight bash %}
gem install jekyll

Building native extensions.  This could take a while...
Building native extensions.  This could take a while...
ERROR:  Error installing jekyll:
	redcarpet requires Ruby version >= 1.9.2.
{% endhighlight %}


Installing a new version of Ruby involves installing rvm

{% highlight bash %}
curl -L get.rvm.io | bash -s stable
{% endhighlight %}

In case this commands fails with a GPG signature verification failure, you'll need to install the proper signatures.

### Installing the rvm signatures

{% highlight bash %}
gpg2 --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

gpg: keyring `/root/.gnupg/secring.gpg' created
gpg: requesting key D39DC0E3 from hkp server keys.gnupg.net
gpg: /root/.gnupg/trustdb.gpg: trustdb created
gpg: key D39DC0E3: public key "Michal Papis (RVM signing) <mpapis@gmail.com>" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
{% endhighlight %}


### Installing rvm

With the signatures in place, rvm should be installed without any problems.

{% highlight bash %}
curl -L get.rvm.io | bash -s stable
{% endhighlight %}

After having installed rvm, we can source the profile and proceed with installing ruby 1.9.3 by executing the following commands:

{% highlight bash %}
source /etc/profile.d/rvm.sh
rvm install 1.9.3
{% endhighlight %}

You can verify that all went well by checking the Ruby version:

{% highlight bash %}
[root@fleetprobe ~]# ruby -v
ruby 1.9.3p551 (2014-11-13 revision 48407) [x86_64-linux]
{% endhighlight %}

## Pushing your changes

{% highlight bash %}
git push deploy master

Counting objects: 23, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 8.53 KiB | 0 bytes/s, done.
Total 23 (delta 1), reused 0 (delta 0)
remote: Initialized empty Git repository in /home/deployer/tmp/myrepo/.git/
remote: Configuration file: /home/deployer/tmp/myrepo/_config.yml
remote:             Source: /home/deployer/tmp/myrepo
remote:        Destination: /home/deployer/www
remote:       Generating... 
remote:                     done.
remote:  Auto-regeneration: disabled. Use --watch to enable.
To deployer@deploy.server:~/myrepo.git
 * [new branch]      master -> master
{% endhighlight %}


## Setup Apache

{% highlight xml %}
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot /var/www/blog
    <Directory "/var/www/blog">
        AllowOverride All
    </Directory>
    ServerName blog.ecommit-consulting.be
    ErrorLog logs/blog-error_log
    CustomLog logs/blog-maccess_log common
</VirtualHost>
{% endhighlight %}

