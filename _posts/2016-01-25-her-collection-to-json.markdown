---
layout: post
title:  "Converting a Her collection to JSON"
date:   2016-01-25 10:34:00 +000
categories: howto
---
When converting a collection from [Her](http://www.her-rb.org) to JSON in our rails application, attempting just a simple `collection.to_json` would result in `SystemStackError: stack level too deep`. In order to get around this, I needed to get the fetched data from the collection, and convert that to JSON. Doing this directly, however, would result in a lot of metadata included as well, which isn't desirable.

I came up with the following as a workaround, I'm sure there's possibly better ways of doing this, but this is what I've managed:

{% highlight ruby %}
collection.fetch.map { |item| item.attributes }.to_json
{% endhighlight %}

Hopefully this helps someone, or at least future me!