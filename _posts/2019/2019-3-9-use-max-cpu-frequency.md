---
layout: post
title: "Use maximum CPU frequency"
description: "Running CPU at top speed."
thumb_image: "documentation/sample-image.jpg"
tags: [howto, linux, performance]
---

Example for 4 cores CPU:
{% highlight bash %}
# echo performance > /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
# echo performance > /sys/devices/system/cpu/cpu1/cpufreq/scaling_governor
# echo performance > /sys/devices/system/cpu/cpu2/cpufreq/scaling_governor
# echo performance > /sys/devices/system/cpu/cpu3/cpufreq/scaling_governor
{% endhighlight %}
