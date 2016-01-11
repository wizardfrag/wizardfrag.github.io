---
layout: post
title:  "Upgrading to postgresql 9.5 with homebrew"
date:   2016-01-11 10:45:00 +0000
categories: howto
---
Upgrading to postgresql 9.5 with homebrew is fairly simple. It just takes a few steps. This may seem like a lot, but depending on the size of your database, it should only take a few minutes to do all these steps.

### Stop the postgresql server (if using launchctl):
{% highlight bash %}
$ launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
{% endhighlight %}

### Update postgresql
{% highlight bash %}
$ brew update && brew upgrade postgresql
{% endhighlight %}

### Move the old postgresql directory
{% highlight bash %}
$ mv /usr/local/var/postgres /usr/local/var/postgres-9.4
{% endhighlight %}

### Create a new postgresql directory
{% highlight bash %}
$ initdb /usr/local/var/postgres -E utf8
{% endhighlight %}

### Migrate the data to the 9.5 format
{% highlight bash %}
$ pg_upgrade \
	-d /usr/local/var/postgres-9.4 \
	-D /usr/local/var/postgres \
	-b /usr/local/Cellar/postgresql/9.4.5_2/bin \
	-B /usr/local/Cellar/postgresql/9.5.0/bin
{% endhighlight %}

*Note*: You may need to change the versions above to whatever the current versions are.

### Start postgresql
{% highlight bash %}
$ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
{% endhighlight %}

### Done

That's really all there is to it! You can `brew cleanup postgresql` once you've verified that your server works using `psql` and looking around your tables.