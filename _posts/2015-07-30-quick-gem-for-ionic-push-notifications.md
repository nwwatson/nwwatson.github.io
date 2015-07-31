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

We in the the middle of adding push notificaitons to our mobile application
and started to look at available push notification services. We took a look
at all the usual suspects such as Amazon SMS, Parse and Pusher. One service 
that intrigued us with Ionic Push Notifications, because our application is
written using the Ionic Framework.

In order to test things out, we created a simple gem that would allow us to
send push notifications using the service. IonicPush is a Ruby gem that provides 
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

```
IonicPush.setup do |config|
  # ==> Configuration for the Ionic.io Application ID
  # The Application ID can be found on the dashboard of
  # https://apps.ionic.io/apps
  config.ionic_application_id = ENV["IONIC_APPLICATION_ID"]

  # ==> Configuration for the Private API Key
  # The Private API Key for your application can be found
  # within the Settings of your application on
  # https://apps.ionic.io/apps
  config.ionic_api_key = ENV["IONIC_API_KEY"]

  # ==> Configuration for the location of the API
  # Refer to the Ionic documentation for the correct location
  # Current documentation can be found here:
  # http://docs.ionic.io/docs/push-sending-push and
  # defaults to https://push.ionic.io
  # config.ionic_api_url = ENV["IONIC_API_URL"]
end
```

## Sending a Push Notification

```
# Create an array of device tokens you want to send to
device_tokens = [
  "APA91bEBoyoZ3EDXJbdjvzn2jdikRu7tdpz_65zkqfMDFTSNZfNgg-ohiNYQQ1TCTdjwqWZ",
  "WSt1XklxZeVj1WtxVBZXMIpIEt3YVaDs3f2Hp5QVmkwB0QgJ48d6KHVrHZCJxTER52yK3b0"
]

# Create a PushService instance passing it a message and device tokens
service = IonicPush::PushService.new("Is that you, John Wayne? Is this me?", device_tokens)

# Call the notify! method to send the push notification
service.notify!
```