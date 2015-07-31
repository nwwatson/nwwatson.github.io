---
layout: post
title:  "PushIonic - A Gem For Sending Push Notifications With Ionic.io"
date:   2015-07-30 21:00:00
categories: ruby, ionic, gem
comments: true
description: >
  Introducing a PushIonic, a quick gem I put together
  to test Ionic.io's Push Notification Service
---

We are in the the middle of adding push notificaitons to our mobile application
and started to look at available push notification services. We took a look
at all the usual suspects such as Amazon SMS, Parse and Pusher. One service 
that intrigued us with Ionic Push Notifications, because our application is
written using the Ionic Framework.

In order to test things out, we created a simple gem that would allow us to
send push notifications using the service. [IonicPush](https://github.com/nwwatson/ionic_push) is a Ruby gem that provides 
a simple to use interface with Ionic.io's Push Notification service. Work on this 
is on-going, however the current version available works completely.

## Installation

Add the following to your Gemfile and bundle install

```
gem 'ionic_push', github: 'nwwatson/ionic_push', branch: 'master'
```

Run the Rails generator to install the initializer in to your Rails project.

```
bundle exec rails g ionic_push:install
```

Configure the ionic_push.rb file within config/initializers. You can view the environment 
variables that are there, or change them for someting else.

{% gist 0c12bfe317fc8c96e8b0 %}

## Sending a Push Notification

Sending a push notification is simple. All you need to have is an array of device tokens and a message.


{% gist bcf7483dc464d1ad5835 %}

## Wrapping It Up

Our experience so far has been really positive. Ionic Push Notifications were simple to integrate
[by following their online documentation](http://docs.ionic.io/v1.0/docs/push-from-scratch).