---
layout: post
title:  "Getting ruby-saml to work with Azure AD"
date:   2015-05-21 23:57:00
categories: ruby, rails
comments: true
description: >
  Dymistify the processes of authenticating Azure AD.
---

I'm going to make this short and sweet. So you've been tasked with using SAML to authenticate users with  Azure AD. Sure, there are other mechanisms available, but this client wants SAML. No problem. Knowning nothing about AD or SAML, but having some experience with OAuth, you think "This couldn't be that hard". Then you find out SAML 2.0 is a little more complicated than OAuth.

I'm not going to walk you though a example on how to build. There are examples available, specifically [ruby-saml-example](https://github.com/onelogin/ruby-saml-example). I'll use this to properly show you how to configure Azure AD.

## Signup for an Azure account

While I'm happy to write this quick tutorial, I'm not going to walk you through create an Azure account. Its self explanitory. You can do it.

## Create an Active Directory 

## Add an Application

## Configure ruby-saml Settings

```ruby
  idp_metadata_parser = OneLogin::RubySaml::IdpMetadataParser.new
  # This does all the hard work. it grabs the federation metadata from Azure
  settings = idp_metadata_parser.parse_remote("https://login.microsoftonline.com/FunnyImNotGivingYouThis/federationmetadata/2007-06/federationmetadata.xml")
  
  # This is the URL that Azure AD calls back after a successful login
  settings.assertion_consumer_service_url = "http://localhost:3000/saml/acs"
  
  # This URL will be calledback after a logout event
  settings.assertion_consumer_logout_service_url = "http://localhost:3000/saml/logout"
  
  # Microsoft calls this the Client ID. Copy it from your application's 
  # properties in Azure and drop it here.
  settings.issuer = "55555555-5555-5555-5555-555555555555"
  
  # You need to copy the  X509 certificate out of the <ds:Signature> node of the
  # federation metadata xml file. Then go to https://www.samltool.com/fingerprint.php
  # and paste it in to the x509 cert field. Use the Formatted Fingerprint in the line 
  # below
  settings.idp_cert_fingerprint = "It:Is:So:me:Th:in:gL:ik:eT:hi:sB:ut:Re:al"
  
  # You shouldn't have to change this line. Works like a champ.
  settings.name_identifier_format = "urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress"
```

