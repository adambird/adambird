---
layout: post
title: "Pushing info to my Pebble watch"
date: 2013-05-12 15:06
comments: true
categories: 
- dev
---

<a href="http://www.flickr.com/photos/adambird/8730919595/" title="Weather forecast to my Pebble by adamebird, on Flickr"><img src="http://farm8.staticflickr.com/7283/8730919595_b631670ee7.jpg" width="375" height="500" alt="Weather forecast to my Pebble"></a>

My [Pebble Watch](http://getpebble.com) finally arrived this week so I thought I should have a go at pushing information to it. As a cycle commuter I look at the weather every day and am primarily interested in wind direction and rain forecast so I though it would be neat to have this appear on my watch every morning.
<!-- more -->
[IFTTT](http://ifttt.com) was the obvious place to start, especially as weather notifications are standard recipes. SMS or Twitter would be good channels to start but neither would work for me.

SMS just didn't work, I'm in the UK on Orange. And Twitter wouldn't allow sending a DM to myself (though as you'll see from my solution I could have just done this). 

I had heard reports of a Pebble push API but haven't been able to find any documentation so I resorted to handcrafting my own solution.

## Pebblebot

I created a simple Ruby command line script to handle the weather initially [adambird/pebblebot](https://github.com/adambird/pebblebot). 

I've resorted to twitter DMs from another 'bot' account I've created for the purpose. Not ideal as I end up with DMs on several devices which I don't want. Once the Pebble Push API is available then this should be an easy swap out.

I host this on a Heroku at zero cost by having an app with no dynos and just a free [Heroku Scheduler](https://addons.heroku.com/scheduler) that runs the script every morning.

At the moment this will need to be manually changed to daylight saving so I might need to thing of something more powerful for scheduling.

Have various ideas for improving and extending this, including making something that anyone could use. If you're interested let me know what would be useful to you [@adambird](http://twitter.com/adambird).

## Update

I've now added support for [Pushover](http://pushover.net) and extended the command line options. See the [adambird/pebblebot](https://github.com/adambird/pebblebot) for full details
