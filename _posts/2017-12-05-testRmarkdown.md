---
layout: post
title: "Test!"
categories: test
tags: test1
author: Wenhu
mathjax: true
---

## This is a test beginning!

{% capture includeGuts %}
{% include 2017-12-05-testRmarkdown.html %}
{% endcapture %}
{{ includeGuts | replace: '    ', ''}}

## This is end!
