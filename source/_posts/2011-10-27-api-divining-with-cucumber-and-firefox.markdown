---
layout: post
title: "API Divining With Cucumber and Firefox"
description: "I've been discovering the wonders of RSpec and Cucumber recently (I know, I know, I'm horribly late to that particular party) and revelling in how the BDD > TDD cycle helps keep one focused on the feature being delivered. Rest APIs are a common to..."
date: 2011-10-27 00:00
comments: true
author: Adam
categories: [dev]
---

I've been discovering the wonders of <a href="https://www.relishapp.com/rspec">RSpec</a> and <a href="http://cukes.info/">Cucumber</a> recently (I know, I know, I'm horribly late to that particular party) and revelling in how the BDD &gt; TDD cycle helps keep one focused on the feature being delivered.

<a href="http://en.wikipedia.org/wiki/Representational_state_transfer">Rest APIs</a> are a common topic of conversation among the <a href="http://www.esendex.com">Esendex</a> dev team and it was a conversation with @jbjon and @samwessel this week that inspired an experiment. 
<!-- more -->
We were discussing <a href="http://en.wikipedia.org/wiki/HATEOAS">HATEOAS</a> whereby a REST client needs no prior knowledge about how to interact with any particular application or server beyond a generic understanding of hypermedia. Your API shouldn't be fixed but should be discoverable. Clients shouldn't be locked into a tight coupling with your API but should be able to adapt has it changes.

What also came up was the notion that the mark of a well tested system was that you could through away all of your code and be confident that you could rebuild it completely based on the tests.

So I set myself an exercise. Could I build an API by putting myself in the shoes of a developer discovering the API for the first time. My tools Cucumber and Firefox.

<img alt="Screen_shot_2011-10-27_at_23" height="195" src="http://getfile2.posterous.com/getfile/files.posterous.com/temp-2011-10-27/aGwuihqfGqeaygvwAvmFstJbDglEChlCxvCjhDjrfBDtuJcgbxvhdIGnBeGa/Screen_Shot_2011-10-27_at_23.17.42.png.scaled500.png" width="351" />

