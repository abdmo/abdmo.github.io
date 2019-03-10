---
layout: post
title: "The 'watch' command"
description: "Execute a program periodically. Useful to monitor program output without manually running the command."
thumb_image: "documentation/sample-image.jpg"
tags: [howto, linux]
---

{% highlight bash %}
$ watch -d -n 5 "ethtool -S eth0 | grep rx_queue"
{% endhighlight %}
