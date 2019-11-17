---
layout: post
title: Q&A for Python interview
date: 2019-11-17 00:00:00 +0000
description: Important questions for Python interview.
img: # Add image post (optional)
tags: [Python]
---
Important topics in Q&A format is based on my experience. Feel free to update me in case of any observation or you want to add anything.

##### 1. How to debug pip installation error?
For debugging purpose, pip displays some installation message on console. It also offers ways to control console level log by -v,--verbose, -q and --quiet. Full log can be saved by providing --log option with file path, it will have complete installation log. There are cases where currupt download also creates issues at the time of installation, in that case try --no-cache-dir. If that also does not work then you can always try the installation from source file.

