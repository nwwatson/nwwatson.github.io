---
layout: post
title:  "Play Framework - The Intro"
date:   2014-10-30 10:54:52
categories: java play
---

For years I've been a Ruby developer. What I love about Ruby its succinct syntax and powerful frameworks. Ruby on Rails for example allows you to quickly create web based applications and Ruby Gems such as Devise and Paperclip make handling things like authentication and uploads trivial.

Recently, I found myself back in the Java world. I have nothing against writing Java per se, however I find the web frameworks to be overly complex and the amount of XML configuration that is required to build application has me wishing for the days of convention over configuration found in Rails. With this, my search began for a JVM based web framework that allows me to utilize all the goodness of the Java echosystem, but with the simplicity and power of the Ruby/Rails.

Naturally I hit Google up to start doing research on possible Java web frameworks that weren't named Struts or Spring. After some searching I found an aritlce by ZeroTurnAround called (The Curious Coder’s Java Web Frameworks Comparison: Spring MVC, Grails, Vaadin, GWT, Wicket, Play, Struts and JSF)[http://zeroturnaround.com/rebellabs/the-curious-coders-java-web-frameworks-comparison-spring-mvc-grails-vaadin-gwt-wicket-play-struts-and-jsf/] and from there I learned about the Play Framework.

What interests me in the Play Framework is its similarity to Ruby on Rails, a framework which I have a lot of knowledge in. Whats awesome about Play is that it sheds J2EE's Java Servlet APIs in favor of a modern framework approach. The removal of the complexity of teh Servlet API provides the developer with an approach much like Rails or Express. Gone are complex XML configuration files in favors of a declarative router and standarized application structure.

In addition, the Play Framework operates outside of standard Java web containers, which use one thread per request and instead allows for the construction of non-blocking asyncronous applications. Bundled with Netty, Play doesn't require the configuration of a Web container and simplifies the configuration and deployment of Java based Web applications.

Over the next few posts I will be constructing a simple Play application, creating a mobile front end using the Ionic Framework, and document how I constructed this application.
