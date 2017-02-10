---
layout: post
title: "3rd Party API Authentication"
description: "Announcing 3rd Party API Authentication with Auth0"
date: 2017-02-09 8:30
category: Technical Guide, API, Authentication
author:
  name: "Prosper Otemuyiwa"
  url: "http://twitter.com/unicodeveloper?lang=en"
  mail: "prosper.otemuyiwa@auth0.com"
  avatar: "https://en.gravatar.com/avatar/1097492785caf9ffeebffeb624202d8f?s=200"
design:
  image: https://cdn.auth0.com/blog/migration/PHPlogo.png
  bg_color: "#312A4C"
tags:
- api
- authentication
- third-party
related:
- 2016-11-07-migrating-a-php5-app-to-7
- 2016-11-09-migrating-a-php5-app-to-7-part-2
---

Up until now, Auth0 was mainly used for application authentication where both the client and API were owned by the same organization. We are now introducing the concept of an API as a first class citizen and the ability to define scopes.

Third party developers can now have several APIs alongside several clients under an account in Auth0. Clients can then go ahead to request consent from users to access the API.

Auth0's API authorization features allow you to manage the authorization requirements for server-to-server and client-to-server applications. Using Auth0, you can easily support different flows in your own APIs without worrying about the OAuth 2.0/OpenID Connect specification, or the many other technical aspects of API authorization. You don't have to code up an Authorization Server again. Auth0 capitalizes on the OAuth 2.0 authorization framework.

With Auth0, you can do the following:

* [Decide which OAuth 2.0 flow to use](https://auth0.com/docs/api-auth/which-oauth-flow-to-use)
* [Configure your server-side web applications to use OAuth 2.0 to access an API](https://auth0.com/docs/api-auth/grant/authorization-code)
* [Configure your native mobile applications to use OAuth 2.0 to access an API](https://auth0.com/docs/api-auth/grant/authorization-code-pkce)
* [Configure your client-side JavaScript web applications to use OAuth 2.0 to access an API](https://auth0.com/docs/api-auth/grant/implicit)
* [Configure a server-to-server interaction to use OAuth 2.0](https://auth0.com/docs/api-auth/grant/client-credentials)
* [Dynamic Client Registration](https://auth0.com/docs/api-auth/dynamic-client-registration)

and [many more!](https://auth0.com/docs/api-auth).

Let’s quickly take a look at how you can use Auth0 to authorize Client-side web apps in order to access an API.

## Use Auth0 to authorize Client-side Web Apps in order to access an API

Auth0 can serve as your authorization server. Client-side web apps use the [**Implicit Grant**](https://tools.ietf.org/html/rfc6749#section-4.2) in order to access an API.

![Implicit Grant](https://cdn.auth0.com/docs/media/articles/api-auth/implicit-grant.png)
_Authorized by Auth0 in order to make calls to an API_

This is the process flow:

* The Client initiates the flow and redirects the user to the Authorization Server.
* The user authenticates.
* The Authorization Server redirects the user to the Client with an `access_token` (and optionally an `id_token`) in the hash fragment.
* The Client can now extract the tokens from the hash fragment. In a Single Page Application this would be done using Javascript and in a Mobile Application this is typically handled by interacting with a Web View.
* The Client can use the `access_token` to call the Resource Server on behalf of the user.

The first time the user goes through this flow a consent page will be shown where the permissions are listed that will be given to the Client.

Open up your [Auth0 dashboard](https://manage.auth0.com), then head over to [advanced account settings](https://manage.auth0.com/#/account/advanced). 

Activate the `Enable APIs Section` flag like so:

![Enable API section in Auth0 dashboard](https://cdn.auth0.com/blog/third-party/enable_api_flg.gif)

Go ahead and create an API
![Create an API](https://cdn.auth0.com/blog/third-party/create-api.png)

Add scopes: Scopes allow you to define the data that will be accessed through the applications to your API.

![Add scopes](https://cdn.auth0.com/blog/thirdparty/add_scopes.png)

After adding scopes, you can go ahead to grab the `access_token` for a client application programmatically like so:

```bash

curl --request POST \
  --url https://unicoder.auth0.com/oauth/token \
  --header 'content-type: application/json' \
  --data '{"client_id":"ddaFD139JGvU0lWKfVHdf09qWmbVa34e","client_secret":"KLWlTkqEkvRoUeIi697x3wk8XqniaumeFte2Zcl882gECUVAfTFSNOXhmy_oAbsw","audience":"http://localhost:3333/api/jokes/celebrity","grant_type":"client_credentials"}'

```

`audience` refers to your API, `client_id` and `client_secret` refers to that of the client application authorized on your dashboard.

This is an `access_token` for the request made above:

```bash

{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik5FWXdRelJHT1RjMU1ESTNNVGxEUVRZNFJqbERRa0pGUWtOQk5qVkROa1V6TWtKQ05qQkJSQSJ9.eyJpc3MiOiJodHRwczovL3VuaWNvZGVyLmF1dGgwLmNvbS8iLCJzdWIiOiJkZGFGRDEzOUpHdlUwbFdLZlZIZGYwOXFXbWJWYTM0ZUBjbGllbnRzIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDozMzMzL2FwaS9qb2tlcy9jZWxlYnJpdHkiLCJleHAiOjE0ODY4NDI1MzYsImlhdCI6MTQ4Njc1NjEzNiwic2NvcGUiOiIifQ.xvdRDuBaVeBHY8uZP5Ny2JbyQjvxx-3JP8I_mfG68vAxwKlVq8iuR3k6APVM82OZNkZT33-Z0udWov6gqIuX9TyLKzlfokuDVKCKhj-A6OiW2UjHG3eq8Zc9RtBOYA_olNfGsKsVCZh4gq8aGvHOCwsrOsT_W50BY7XHlISW6SQOZx_E3NfamHT7BFtJ2NWhGmAFNb87ei9Ag4XdTvIQlHLtgLf0uhgYm1dESXSDqNA3p8MyGQ-yXzxld9A3BEAZM8gFeOlpXzfJgHdfF_uNxtHaSsJY0xTque2sUwfu431YoBmUExN31uSg7Ysm2bM-ICaavo_dN7pTlP5_u4BS8Q",
  "token_type": "Bearer"
}

```

You can now use this `bearer token` with an `Authorization Header` in your request to obtain authorized access to your API like so:

```bash

curl --request GET \
  --url http://path_to_your_api/ \
  --header 'authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik5FWXdRelJHT1RjMU1ESTNNVGxEUVRZNFJqbERRa0pGUWtOQk5qVkROa1V6TWtKQ05qQkJSQSJ9.eyJpc3MiOiJodHRwczovL3VuaWNvZGVyLmF1dGgwLmNvbS8iLCJzdWIiOiJkZGFGRDEzOUpHdlUwbFdLZlZIZGYwOXFXbWJWYTM0ZUBjbGllbnRzIiwiYXVkIjoiaHR0cDovL2xvY2FsaG9zdDozMzMzL2FwaS9qb2tlcy9jZWxlYnJpdHkiLCJleHAiOjE0ODY4NDI1MzYsImlhdCI6MTQ4Njc1NjEzNiwic2NvcGUiOiIifQ.xvdRDuBaVeBHY8uZP5Ny2JbyQjvxx-3JP8I_mfG68vAxwKlVq8iuR3k6APVM82OZNkZT33-Z0udWov6gqIuX9TyLKzlfokuDVKCKhj-A6OiW2UjHG3eq8Zc9RtBOYA_olNfGsKsVCZh4gq8aGvHOCwsrOsT_W50BY7XHlISW6SQOZx_E3NfamHT7BFtJ2NWhGmAFNb87ei9Ag4XdTvIQlHLtgLf0uhgYm1dESXSDqNA3p8MyGQ-yXzxld9A3BEAZM8gFeOlpXzfJgHdfF_uNxtHaSsJY0xTque2sUwfu431YoBmUExN31uSg7Ysm2bM-ICaavo_dN7pTlP5_u4BS8Q'

```

Examples of making such requests via `NodeJS`, `Java`, `JQuery` and more are shown on the Auth0 dashboard.

### Executing the Implicit Grant Flow

The Client application can initiate the flow by sending the user to to the autorization URL like so:

{% highlight html %}

<a href="https://unicoder.auth0.com/authorize?scope=read:foodjokes&audience=http://localhost:3333/api/jokes/celebrity&response_type=id_token%20token&client_id=WQT9iLJRSLL5u2tAxYiCTELmRwmkGHpR&redirect_uri=http://localhost:3000/login">
  Sign In
</a>

{% endhighlight %}

The user authorizes the client like so:

![Authorize](https://cdn.auth0.com/blog/third-party/authorize.png)

You can now extract the `access_token` from the hash fragment of the URL after Auth0 has redirected back to the client like so:

```js

function getParameterByName(name) {
  var match = RegExp('[#&]' + name + '=([^&]*)').exec(window.location.hash);
  return match && decodeURIComponent(match[1].replace(/\+/g, ' '));
}

function getAccessToken() {
  return getParameterByName('access_token');
}

function getIdToken() {
  return getParameterByName('id_token');
}

$(function () {
  var access_token = getAccessToken();

  // Optional: an id_token will be returned by Auth0
  // if your response_type argument contained id_token
  var id_token = getIdToken();

  // Use the access token to make API calls
  // ...
});

```

Once you have the `access_token` you can use it to make calls to the API, by passing it as a Bearer Token in the Authorization header of the HTTP request like so:

```js

// Use the access token to make API calls
$('#get-appointments').click(function(e) {
  e.preventDefault();

  $.ajax({
    cache: false,
    url: "http://localhost:3333/api/jokes/celebrity",
    headers: { "Authorization": "Bearer " + access_token }
  });   
});

```

More examples can be found in the [awesome Auth0 documentation](https://auth0.com/docs/api-auth)


## Conclusion

Auth0 now offer APIs as first class citizens and a lot of ways to consume them. With Dynamic Client Registration, Auth0 account owners can open up their APIs to the world while security os enforced by user consent.

Developers, you can start taking advantage of this opportunity by getting started with the [docs](https://auth0.com/docs/api-auth).