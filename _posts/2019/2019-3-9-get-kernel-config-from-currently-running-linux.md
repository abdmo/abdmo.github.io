---
layout: post
title: "Get kernel config from currently running linux"
description: "Ditto."
thumb_image: "documentation/sample-image.jpg"
tags: [howto, kernel, linux]
---

{% highlight bash %}
$ cat /proc/config.gz | gunzip > kernel.config
$ vi kernel.config
{% endhighlight %}
or
{% highlight bash %}
$ zcat /proc/config.gz > kernel.config
{% endhighlight %}
