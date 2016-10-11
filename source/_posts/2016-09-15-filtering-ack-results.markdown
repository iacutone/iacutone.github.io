---
layout: post
title: "Filtering Ack Results"
date: 2016-09-15 10:23:52 -0400
comments: true
categories: ack grep
---

While setting up my VIM environment, I read a blogpost which states `ack` can ignore directories. For cleaner `ack` output you can setup the file below:

```bash .ackrc
--ignore-dir=log
--ignore-dir=public/assets
--ignore-dir=vendor/assets
--ignore-dir=tmp/cache
```
