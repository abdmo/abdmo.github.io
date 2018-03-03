---
layout: post
title: "Linus Torvalds is bae"
description: "Debugging in kernel is a pain in the ass."
thumb_image: "documentation/sample-image.jpg"
tags: [blog, linux]
---

Currently I'm in the middle of a project where I deal with kernel festivities a lot. Specifically, I'm working with [yocto](https://www.yoctoproject.org/about) - recipes, layers, metas, drivers, modules, and others. I don't have much to complain except debugging in kernel is such a pain in the ass.

Don't get me wrong I love what I'm doing right now (i.e. embedded system). I think it's interesting and most importantly, hot too considering nowadays we seem to be stanning on AI, edge computing and whatnot and hardware optimization has become more crucial than ever. But the whole process of debugging really requires at least level 79 on your precision and patience game.

Consider this, you're working on a yocto image for a board. You've developed your code. Now you have to build it. Depending on how big of a system your image is, first-time build can take up to 8 hours (mine does) or more I don't know. Then you have to flash it to a flash drive. Flashing on 3.0s is my go-to (and probably the only feasible) route because otherwise (2.0s) I need to wait probably a good 40 minutes before it finishes. After that's done, you're going to have to boot the image onto your board to test it. Booting doesn't take long (thank god for that) but the face you make when you realize you can't boot the image? Priceless. Now you have to figure out what's wrong in your code and repeat the whole process again. Well, building the code probably won't take 8 hours now, will approximately take 30 minutes on average but, you got my point.

However, the most important things I've learned were one; to always striving to get my code right the first time. Two; to be extra cautious on making changes as I'm writing code that speaks directly to the hardware and bad code will risk making the hardware FUBAR. Good news is, up until this point I have not break a single hardware... yet. Bad news is, I can feel it coming.