---
layout: post
title: "The trick to getting CruiseControl.net build label into PowerShell script "
description: "is to use an environment variable. The PowerShell Task is a subclassed Executable Task which loads a series of environment variables before the task is executed. You then just need to access those in your PowerShell script via the $Env variable. I..."
date: Wed Jan 27 07:22:24 -0800 2010
comments: true
author: Adam
categories: [dev]
---

is to use an environment variable. <p /> The PowerShell Task is a subclassed Executable Task which loads a series of environment variables before the task is executed. You then just need to access those in your PowerShell script via the $Env variable. <p /> In my case <br />$label = $Env:CCNetLabel <p /> Scroll down to the Notes section of this page <a href="http://confluence.public.thoughtworks.org/display/CCNET/Executable%2BTask">http://confluence.public.thoughtworks.org/display/CCNET/Executable%2BTask</a> for a full list of the environment variables that are set.