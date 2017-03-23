---
layout: post
title: "Announcing Third Party Client API Authorization"
description: "APIs are now first-class citizens at Auth0. Learn how third party client API authorization works."
date: 2017-03-20 08:30
category: Announcement, Feature, API Authorization
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

Today, we are introducing a new feature for all of our users: [Third Party Client API Authorization](https://auth0.com/docs/apis). Read on to find out how it works and how to use it.

{% include tweet_quote.html quote_text="Introducing a new feature for all Auth0 users: Third Party Client API Authorization!" %}

Up until now, Auth0 has been known and widely used for application authentication where both the client and API were owned by the same organization. Now, APIs have been elevated to become first class citizens at Auth0. 

Auth0 customers (tenant admins) can now create several APIs and Clients under an account. Optionally, a tenant admin might give a third party developer a set of `client ID` and `client secret` credentials, representing a third party client. With those credentials, a third party client could ask for consent to access an Auth0 customer's API on behalf of a customer's user.

The third party client has the sole responsibility of executing any of the available [OAuth flows](https://auth0.com/docs/api-auth/which-oauth-flow-to-use) in order to get valid access tokens for any APIs.

Follow the flow below to identify which method best matches your use case.

![OAuth 2.0 Grant flows](https://cdn.auth0.com/docs/media/articles/api-auth/oauth2-grants-flow.png)

## Where do I configure the APIs?

Open up your [Auth0 Dashboard](https://manage.auth0.com/#/account/advanced). If you can't find an **APIs** section on the sidebar to the left, navigate to your [Account Advanced settings](https://manage.auth0.com/#/account/advanced), scroll down to the *Settings* section and toggle the **Enable APIs Section** switch.

![Enable APIs section](https://cdn.auth0.com/blog/blog/enable_apis_section.png)
_Enable APIs section_

Next, click the `Create API` button:

![Create API](https://cdn.auth0.com/docs/media/articles/api/overview/create-api.png)

You need to provide the following information for your API:

* **Name:** a friendly name for the API. Does not affect any functionality.
* **Identifier:** a unique identifier for the API. We recommend using a URL, but note that this doesn't have to be a publicly available URL as Auth0 will not call your API at all. This value cannot be modified afterwards.
* **Signing Algorithm:** the algorithm to sign the tokens with. The available values are `HS256` and `RS256`. When selecting `RS256` the token will be signed with the tenant's private key.

Once you are finished filling in this information, you can go ahead and click the `Create` button. You will then be navigated to the *Quick Start* of your API. Here you can find details on the implementation changes you have to do to your API, which consists of choosing a JWT library from a predefined list and configuring this library to validate the access tokens in your API.

![Quickstart View](https://cdn.auth0.com/docs/media/articles/api/overview/quickstarts-view.png)
_Quickstart view_

The other available views for your API are:

**Settings:** lists the settings for your API. Some are editable. Here you can change the token expiration time and enable offline access (this way Auth0 will allow clients to ask for Refresh Tokens for this API). For details, refer to the [API Settings paragraph](https://auth0.com/docs/apis#api-settings).

**Scopes:** here you can define the scopes for this API by setting a name and a description.

**Non-Interactive Clients:** lists your Non Interactive Clients. You can authorize which Non Interactive Clients can request access tokens for your API. You can optionally select a subset of the defined scopes to further limit the access that an authorized client has. Only Non Interactive Clients require explicit permission. That is because when you authorize a non-interactive Client to access an API, Auth0 is creating a Client Grant for that Client. For more details on this case refer to: [Setting up a Client Credentials Grant using the Management Dashboard.](https://auth0.com/docs/api-auth/config/using-the-auth0-dashboard)

**Test:** from this view you can execute a sample Client Credentials flow with any of your Authorized Non-Interactive Clients to check that everything is working as expected.

## Authorizing a Client

You need to authorize a client to be able to perform a `client_credentials` exchange to obtain an `access_token` for access to an API resource. Before the authorization can happen, the client needs to have a `client_id` and `client_secret` which can be generated via [dynamic client registration](https://auth0.com/docs/api-auth/dynamic-client-registration).

The available flows for implementing authorization for your use case can be found below:

* [Authorizing Server-side apps to access an API](https://auth0.com/docs/api-auth/grant/authorization-code)
* [Authorizing SPAs to access an API](https://auth0.com/docs/api-auth/grant/implicit)
* [Authorizing Mobile apps to access an API](https://auth0.com/docs/api-auth/grant/authorization-code-pkce)
* [Authorizing CLIs to access an API](https://auth0.com/docs/api-auth/grant/client-credentials)
* [Authoring Trusted clients to access an API](https://auth0.com/docs/api-auth/grant/password)

You can read through any of the above flows to know how to implement authorization for your use case.

Now that you have a `client_id` and `client_secret`, you can configure your application to authenticate users with Auth0.

For example:

{% highlight html %}

<a href="https://tenant.auth0.com/authorize?scope=appointments%20contacts&audience=http://chknorris.com&response_type=id_token%20token&client_id=WQT9iLJRSLL5u2tAxYiCTELmRwmkGHpR&redirect_uri=http://localhost:3000/callback">
  Sign In
</a>

{% endhighlight %}

* **scope:** scopes defined for the API.
* **audience:** API identifier.
* **client_id:** `client_id` of the application trying to make an authorization request.
* **redirect_uri:** The redirect URI defined during the dynamic client registration of the application. 

This call will redirect the user to Auth0. The user will be authenticated like so:

![Authentication](https://cdn.auth0.com/blog/third-party/auth.png)
_Authentication happens first_

The first time the user goes through this flow, a consent page will be shown where the client permissions are listed (for example: post messages, list contacts, and so forth) like so:

![Authorization](https://cdn.auth0.com/blog/third-party/userconsent.png)
_User consent_

Upon successful consent and authentication, Auth0 redirects the user back to your application (specifically to the `redirect_uri`).

## API Access

If you need API access, your app needs to extract the `access token` from the hash fragment of the URL after successful authentication. The token can then be used to make calls to the API by sending `Bearer [token]` in the `Authorization header` of the HTTP request.

![Access token](https://cdn.auth0.com/blog/third-party/access_token.png)
_Grab Access token from the hash fragment of the URL_

In a Single Page Application (SPA), this would be done using Javascript. In a mobile application, this is typically handled by interacting with a Web View.

## Developer Portal

At Auth0, we live to make developers' lives easy and reduce the time required to ship! So, we have built a developer portal that makes generation of clients with `client_id` and `client_secret` a breeze. You don't have to manually call the *Dynamic Client Registration* API, it has been automated for you!

An Auth0 customer can fork this [application](https://github.com/auth0/developer-centre) and provide the necessary configuration as required by the software(developer-portal). This will allow third party developers to easily create clients with a set of `client_id` and `client_secret` credentials. They can also showcase their API documentation.

![Settings.json](https://cdn.auth0.com/blog/third-party/settingsjson.png) 
_Developer Portal Config_

![Developer Centre](https://cdn.auth0.com/blog/third-party/developercentre.gif)
_Developer Centre_

**Note:** The application has a [frontend](https://github.com/auth0/developer-centre) and a [backend](https://github.com/auth0/developer-centre-api). Follow the instructions in the README to set it up.


## Conclusion

Auth0 now offers APIs as first class citizens and numerous ways to consume them. With Dynamic Client Registration, Auth0 account owners can open up their APIs to the world while security is enforced by user consent.

Developers, you can start taking advantage of this opportunity by getting started with the [docs](https://auth0.com/docs/api-auth)