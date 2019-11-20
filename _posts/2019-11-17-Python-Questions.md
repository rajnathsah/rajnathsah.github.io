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

##### 2. Difference between list and dictionary?
###### List
* List in Python is a heterogeneous container for items.
* Elements present in List maintain their order.
* The elements present in list can be of any type (int, float, string, tuple etc.).
* Elements are accessed through their index values.
* If you have a collection of data that does not need random access, use List.
* Where you have to deal with values which can be changed, use List.
```python
mylist=[1,2,3,'4','sample']
```
###### Dictionary
* Dictionary is an unordered collection of key-value pairs.
* Dictionaries are used to handle large amount of data.
* Every element is having a key-value pair.
* Elements are accessed by using it’s key value.
* When you are dealing with unique keys and you are mapping values to the keys, use Dictionary.
```python
mydict = {1:'Python'}
```
