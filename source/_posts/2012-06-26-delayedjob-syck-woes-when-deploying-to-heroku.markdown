---
layout: post
title: "Delayed_job Syck woes when deploying to Heroku Cedar"
description: "This caused me a bit of heartache and I found little to help via the usual channels. I'd upgraded a Rails 2 app that used Delayed_Job to Rails 3 and was deploying the Heroku Cedar stack. Jobs were being queued but would fail immediately when tryin..."
date: Tue Jun 26 04:11:22 -0700 2012
comments: true
author: Adam
categories: [dev,rails]
---

This caused me a bit of heartache and I found little to help via the usual channels.

I'd upgraded a Rails 2 app that used Delayed_Job to Rails 3 and was deploying the Heroku Cedar stack. Jobs were being queued but would fail immediately when trying to deserialise with

    uninitialized constant Syck::Syck

when trying to parse and initialise the handler. Irritatingly the worker logs were showing the jobs as successful.

After messing around with trying to explicitly set the YAML parser in `config/boot.rb` I found the solution.

`Syck` isn't available on the Heroku Cedar stack so you have to include the `Psych` gem.

    gem 'pysch'

I then ran this on the console to resubmit the jobs and all was good

    Delayed::Job.all.each do |job| job.invoke_job ; job.destroy end