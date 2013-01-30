---
layout: post
title: "Spork not reloading classes"
description: "Have been caught out by this for a while with a Rails app that suddenly stopped seeing code changes to classes as I was redeveloping.. Not being able to test with spork running really slowed down my development cycle. Thankfully someone found a fi..."
date: Wed Mar 21 02:37:00 -0700 2012
comments: true
author: Adam
categories: [dev,rails,ruby]
---

Have been caught out by this for a while with a Rails app that suddenly stopped seeing code changes to classes as I was redeveloping.. Not being able to test with spork running really slowed down my development cycle. Thankfully someone found a fix.

Replace this line in test.rb

    config.cache_classes = true

with this

    config.cache_classes = !(ENV['DRB'] == 'true')

and development is back to full speed.

More info here&nbsp;<a href="http://Spork not reloading classes">http://www.avenue80.com/tiny-tip-spork-not-reloading-classes/</a>
