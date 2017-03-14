---
layout: post
title:  "Ignoring Changes in Tracked Files with Git"
date:   2015-05-12 08:18:00
categories: learning git
comments: true
description: >
  Ever need the ability to change a tracked file in git
  but not have it committed each time you check in? Here's how.
---

We use Rubinius in production, but in development Rubinuis is slow. Those few seconds of waiting for Rubinius to compile all your Ruby files add up and I'm impatient.

<!--more-->

To combat this problem, I like to be able to change the .ruby-version file (we use rbenv) to use the latest version of MRI. Naturally, we don't want to check this in. Luckily, we can do this with git:

```
git update-index --assume-unchanged <file>
```

When we want to track it again

```
git update-index --no-assume-unchanged <file>
```
