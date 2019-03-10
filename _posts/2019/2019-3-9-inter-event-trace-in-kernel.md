---
layout: post
title: "Inter-event trace in kernel"
description: "Utilizing histogram triggers to do inter-event trace in kernel."
thumb_image: "documentation/sample-image.jpg"
tags: [howto, kernel, linux]
---
### The problem: Acquiring wakeup latency for a thread

First, let's define wakeup latency as the duration after a thread wakes up (sched_wakeup) to the time it actually being run on the CPU (sched_switch). And we're trying to get that info with low overhead and without writing additional code.

### The solution: Histogram triggers
Linux has a number of 'trace events' i.e. trace points in kernel where data can be written to a buffer. More information on kernel trace events [here](https://github.com/torvalds/linux/blob/master/Documentation/trace/events.rst). To list all events categorized by subsystem:
{% highlight bash %}
$ ls /sys/kernel/debug/tracing/events
{% endhighlight %}

Trace event(s) can be turned on like so: (e.g. enabling all events on sched subsystem)
{% highlight bash %}
# echo 1 > /sys/kernel/debug/tracing/events/sched/enable
{% endhighlight %}

But events also can trigger actions e.g. dump a stacktrace, store values to variables, etc.. **A histogram triggers is a special event trigger that aggregates and encapsulates event hits**. More info on histogram triggers [here](https://github.com/torvalds/linux/blob/master/Documentation/trace/histogram.rst). Below is an example of aggregating timestamps into a variable ts0 when sched_wakeup of ktimersoftd/1 thread is being called:
{% highlight bash %}
# echo 'hist:keys=$saved_pid:saved_pid=pid:ts0=common_timestamp.usecs if comm=="ktimersoftd/1"' >> \
       /sys/kernel/debug/tracing/events/sched/sched_wakeup/hist
# cat /sys/kernel/debug/tracing/events/sched/sched_wakeup/hist
{% endhighlight %}

### My use case: Finding wakeup latency for TAPRIO qdisc timer
TAPRIO qdisc is a software implementation of Qbv which uses hrtimer to schedule the transmission window of TSN packets. In my use case, I have to find the wakeup latency of this timer to further understand the different factors that will affect the latency performance.

A high-level explanation on my method is:
- On sched_wakeup, save start timestamp
- On sched_switch, save end timestamp when event match (onmatch) and retrieve start timestamp from hash table i.e. hist trigger where key=pid
- Use a user-defined synthetic event to aggregate latency information

#### The BKM:
1. Turn on histogram triggers in kernel config
{% highlight bash %}
CONFIG_HIST_TRIGGERS=y
{% endhighlight %}

2. Enable all tracing events for sched subsystem
{% highlight bash %}
# echo 1 >/sys/kernel/debug/tracing/events/sched/enable
{% endhighlight %}

3. Define a 'wakeup latency' synthetic event
{% highlight bash %}
# echo 'wakeup_latency u64 lat; pid_t pid; int prio' >> \
       /sys/kernel/debug/tracing/synthetic_events
{% endhighlight %}

4. Save timestamp as 'ts0' when the thread wakes up. "ktimersofd/1" is the thread name for taprio qdisc timer.
{% highlight bash %}
# echo 'hist:keys=$saved_pid:saved_pid=pid:ts0=common_timestamp.usecs if comm=="ktimersoftd/1"' >> \
       /sys/kernel/debug/tracing/events/sched/sched_wakeup/trigger
{% endhighlight %}

5. Calculate the latency when thread is scheduled onto the CPU and use that along with another variable and an event field to generate wakeup_latency synthetic event
{% highlight bash %}
# echo 'hist:keys=next_pid:wakeup_lat=common_timestamp.usecs-$ts0:\
		onmatch(sched.sched_wakeup).wakeup_latency($wakeup_lat,$saved_pid,next_prio) if next_comm=="ktimersoftd/1"' >> \
		/sys/kernel/debug/tracing/events/sched/sched_switch/trigger
{% endhighlight %}

6. Aggregate synthetic data
{% highlight bash %}
# echo 'hist:keys=pid,prio,lat:sort=pid,lat' >> \
       /sys/kernel/debug/tracing/events/synthetic/wakeup_latency/triggers
{% endhighlight %}

7. Show the output i.e. histogram data
{% highlight bash %}
# cat /sys/kernel/debug/tracing/events/synthetic/wakeup_latency/histogram
{% endhighlight %}

8. You can process the raw data and convert them nicely to a visual histogram chart or whatever. TODO: probably write a script to do this.

