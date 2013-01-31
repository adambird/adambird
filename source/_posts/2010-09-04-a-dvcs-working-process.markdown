---
layout: post
title: "A DVCS working process"
description: "I've started working with Git using GitHub as a remote repository. This introduces into my life branching as a cheap operation and makes for a potentially different way of working. After a discussion with the guys at work on Friday afternoon, here..."
date: 2010-09-04 00:00
comments: true
author: Adam
categories: [dev]
---

I've started working with Git using <a href="http://github.com">GitHub</a> as a remote repository. This introduces into my life branching as a cheap operation and makes for a potentially different way of working.

After a discussion with the guys at work on Friday afternoon, here's what I've come up with:

Start a new feature =&gt; start a new branch.

` git branch master [feature branch name] `

The master branch is preserved for the current release code. My <a href="http://www.hudson-ci.org">Hudson</a> instance is only monitoring that branch for changes and ignoring all other branches.

I then push the branch to my remote repository so I get the benefits of a back-up as well being able to share the branch if I need to collaborate with someone else on it in the future.

`git push [remote repository address] [feature branch name]`

The code rythym then becomes add files, commit to the branch and then push to the remote repo branch.

Once the feature is finished then it's a case of squashing the commits on my local master branch and then pushing them to the remote copy.

`git checkout master`

`git merge --squash [feature branch name]`

`git push`

Then it's just a case of deleting the feature branch when I'm happy it's complete

Would appreciate any thoughts on this, does it sound sensible?
