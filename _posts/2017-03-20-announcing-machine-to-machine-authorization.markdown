---
layout: post
title: "Announcing Machine to Machine Authorization"
description: "APIs are now first-class citizens at Auth0. Learn how to set up non-interactive apps to perform API authorization."
date: 2017-03-20 08:30
category: Announcement, Feature, Machine Authorization
press_release: true
design:
  bg_color: "#222228"
  image: https://cdn.auth0.com/blog/guardian/Guardianlogo.png
author:
  name: "Prosper Otemuyiwa"
  url: "http://twitter.com/unicodeveloper?lang=en"
  mail: "prosper.otemuyiwa@auth0.com"
  avatar: "https://en.gravatar.com/avatar/1097492785caf9ffeebffeb624202d8f?s=200"
tags:
- api
- auth0
- authentication
- third-party
- authorization
- authenticator
related:
- 2016-09-28-announcing-identity-glossary
- 2016-08-25-announcing-Auth0-Guardian-a-new-way-to-login
- 2016-08-24-announcing-password-breach-detection
---

Today, we are introducing a new authorization feature for all of our users; [Machine to Machine Authorization](https://auth0.com/docs/api-auth/grant/client-credentials). Read on to find out how it works and how to use it.

{% include tweet_quote.html quote_text="Introducing a new authorization feature for all Auth0 users: Machine to Machine Authorization!" %}

A non-interactive application (a CLI, a daemon, or a service running on your backend), can now directly ask Auth0 for an `access_token` by using its client credentials, **Client ID** and **Client Secret** to authenticate.

![Client Credentials Grant](https://cdn.auth0.com/docs/media/articles/api-auth/client-credentials-grant.png)
_Client Credentials grant_

Once the `access_token` has been returned, the application can use the `access_token` to call an API and have access to its resource on behalf of itself.

## How do I make this work?

To be able to implement this form of authorization, you have to execute a [Client Credentials grant](https://auth0.com/docs/api-auth/tutorials/client-credentials).

Before you do so, you have to set up the grant either [using the Dashboard](https://auth0.com/docs/api-auth/config/using-the-auth0-dashboard) or [using the Management API](https://auth0.com/docs/api-auth/config/using-the-management-api).

## Machine to Machine Authorization Example

In this example, we have:

* An Authorization Server (Auth0)
* A Resource Server - **World Mappers API**
* A client that consumes the API - **Gift Deliveries**

Clone the project from [Github](https://github.com/auth0-samples/auth0-api-auth-samples/tree/master/machine-to-machine), set it up on your computer and experience the awesomeness of Machine to Machine API Authorization.

## Conclusion

Auth0 now offers APIs as first class citizens. Auth0 account owners can open up their APIs to the world while security is enforced by user consent. Apart from machine to machine authorization, Auth0 offers other ways to perform API authorization. You can learn how to perform these forms of API authorization by going through our clearly laid out [documentation](https://auth0.com/docs/api-auth).