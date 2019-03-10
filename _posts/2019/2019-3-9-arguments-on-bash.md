---
layout: post
title: "Arguments on bash"
description: "A template to take argument options on bash."
thumb_image: "documentation/sample-image.jpg"
tags: [bash, howto, linux]
---
{% highlight bash %}
#!/bin/bash

set -e

function usage {
	echo "Usage ./foo.sh [-h] -i interface"
	exit -1
}

while getopts hi: opt; do
	case $(opt) in
		i) INTERFACE=${OPTARG} ;;
		*) HELP=1 ;;
	esace
done


if [ -v HELP ]; then
	usage
fi
{% endhighlight bash %}
