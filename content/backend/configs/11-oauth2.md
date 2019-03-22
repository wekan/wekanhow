---
title: Introduction
pageTitle: Learn GraphQL Fundamentals with Fullstack Tutorial
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

- [OAuth2 small bug](https://github.com/wekan/wekan/issues/1874) - currently OAuth2 works mostly

# OAuth2 providers

You can use some OAuth2 providers for logging into Wekan, for example:
- [Auth0](https://github.com/wekan/wekan/wiki/OAuth2#auth0) - works
- [Rocket.Chat](https://github.com/wekan/wekan/wiki/OAuth2#rocketchat-providing-oauth2-login-to-wekan) - not tested yet
- Google - not tested yet

You can ask your identity provider (LDAP, SAML etc) do they support adding OAuth2 application like Wekan.

## Rocket.Chat providing OAuth2 login to Wekan

Also, if you have Rocket.Chat using LDAP/SAML/Google/etc for logging into Rocket.Chat, then same users can login to Wekan when Rocket.Chat is providing OAuth2 login to Wekan.

If there is existing username/password account in Wekan, OAuth2 merges both logins.

Source: [OAuth2 Pull Request](https://github.com/wekan/wekan/pull/1578)

# Docker

https://github.com/wekan/wekan-mongodb/blob/master/docker-compose.yml#L146-L166

# Snap

### 1) Install Rocket.Chat

[Rocket.Chat Snap](https://rocket.chat/docs/installation/manual-installation/ubuntu/snaps/) has Node at port 3000 and mongodb at port 27017.
```
sudo snap install rocketchat-server
```

### 2) Install Wekan

[Wekan Snap](https://github.com/wekan/wekan-snap/wiki/Install) has Node at port 3001 and MongoDB at port 27019.
```
sudo snap install wekan
sudo snap set wekan root-url="https://wekan.example.com"
sudo snap set wekan port='3001'
sudo snap set core refresh.schedule=02:00-04:00
sudo snap set wekan with-api='true'
```
Email settings [ARE NOT REQUIRED](https://github.com/wekan/wekan/wiki/Troubleshooting-Mail), Wekan works without setting up Email.
```
sudo snap set wekan mail-url='smtps://user:pass@mailserver.example.com:453'
sudo snap set wekan mail-from='Wekan Boards <support@example.com>'
```
Edit Caddyfile:
```
sudo nano /var/snap/wekan/common/Caddyfile
```
Add Caddy config:
```
wekan.example.com {
        proxy / localhost:3001 {
          websocket
          transparent
        }
}

chat.example.com {
        proxy / localhost:3000 {
          websocket
          transparent
        }
}
```
Enable Wekan's Caddy:
```
sudo snap set wekan caddy-enabled='true'
```

### 3) Add Rocket.Chat settings

Login to Rocket.Chat at https://chat.example.com .

Accept chat URL to be https://chat.example.com .

Click: (3 dots) Options / Administration / OAuth Apps / NEW APPLICATION

Add settings:

```
Active: [X] True
Application Name: Wekan
Redirect URI: https://wekan.example.com/_oauth/oidc
Client ID: abcde12345         <=== Rocket.Chat generates random text to here
Client Secret: 54321abcde     <=== Rocket.Chat generates random text to here
Authorization URL: https://chat.example.com/oauth/authorize
Access Token URL: https://chat.example.com/oauth/token
```
Save Changes.

### 4) Add Wekan settings

```
sudo snap set wekan oauth2-client-id='abcde12345'
sudo snap set wekan oauth2-secret='54321abcde'
sudo snap set wekan oauth2-server-url='https://chat.example.com'
sudo snap set wekan oauth2-auth-endpoint='/oauth/authorize'
sudo snap set wekan oauth2-userinfo-endpoint='/oauth/userinfo'
sudo snap set wekan oauth2-token-endpoint='/oauth/token'
```

### 5) Login to Wekan

1) Go to https://wekan.example.com

2) Click `Sign in with Oidc`

3) Click `Authorize` . This is asked only first time when logging in to Wekan with Rocket.Chat.

<img src="https://wekan.github.io/oauth2-login.png" width="60%" alt="Wekan login to Rocket.Chat" />

### 6) Set your Full Name

Currently Full Name is not preserved, so you need to change it.

1) Click `Your username / Profile`

2) Add info and Save.

<img src="https://wekan.github.io/oauth2-profile-settings.png" width="60%" alt="Wekan login to Rocket.Chat" />

### 7) Add more login options to Rocket.Chat

1) At Rocket.Chat, Click: (3 dots) Options / Administration

2) There are many options at OAuth menu. Above and below of OAuth are also CAS, LDAP and SAML.

<img src="https://wekan.github.io/oauth-rocketchat-options.png" width="100%" alt="Wekan login to Rocket.Chat" />

# Auth0

[Auth0](https://auth0.com) can provide PasswordlessEmail/Google/Facebook/LinkedIn etc login options to Wekan.

### 1) Auth0 / Applications / Add / Regular Web Application / Auth0 Settings

```
Client ID:                                 <== Copy to below snap settings
Secret:                                    <== Copy to below snap settings
Account url: youraccount.eu.auth0.com      <== Copy to below snap settings
Application Logo:                          <== Add your logo
Application Type: Single Page Application
Token Endpoint Authentication Method: Post
Allowed Callback URLs: https://wekan.example.com/_oauth/oidc  <== Change your Wekan address
Allowed Web Origins: https://wekan.example.com                <== Change your Wekan address
Use Auth0 instead of the IdP to do Single Sign On: [X]
```
If you  need more info, they are at bottom of the page Advanced Settings / Endpoint / OAuth

2) Auth0 Dashboard => Rules => Add Rule

Rule Name: Encrich Wekan login
```
  function (user, context, callback) {
    user.user_metadata = user.user_metadata || {};
    var ns = "https://boards.example.com/";
    context.idToken[ns + "id"] = user.user_id;
    context.idToken[ns + "email"] = user.email;
    context.idToken[ns + "name"] = user.name || user.user_metadata.name;
    callback(null, user, context);
  }
```

### 3) Snap settings, change to it from above client-id, secret, server-url and web-origin (=namespace for rules function above).

Note: namespace works for multiple apps. For example, you can use same namespace url for many different wekan board apps that have different client-id etc, and different board url, and still use same namespace url like https://boards.example.com .
```
sudo snap set wekan oauth2-client-id='abcde12345'
sudo snap set wekan oauth2-secret='54321abcde'
sudo snap set wekan oauth2-server-url='https://youraccount.eu.auth0.com'
sudo snap set wekan oauth2-auth-endpoint='/authorize'
sudo snap set wekan oauth2-userinfo-endpoint='/userinfo'
sudo snap set wekan oauth2-token-endpoint='/oauth/token'
sudo snap set wekan oauth2-id-map='https://boards.example.com/id'
sudo snap set wekan oauth2-username-map='https://boards.example.com/email'
sudo snap set wekan oauth2-fullname-map='https://boards.example.com/name'
sudo snap set wekan oauth2-email-map='https://boards.example.com/email'
```
For login to work, you need to:
- Create first Admin user
- Add other users with REST API or Password registration
- Login with OIDC button
- Have Auth0 configured for passwordless email login (on some other login)

### 4) Auth0 ID provider to Custom OAuth RocketChat

These do work currently so that Auth0 passwordless login to RocketChat does work,
but there is some additional code also that is not added as PR to RocketChat yet.
Code mainly has generating custom authorization cookie from user email with addition to
RocketChat API, and using it and login_token + rc_token to check on RocketChat login page
using router repeating trigger so that if those cookies exist then automatically login
user in using RocketChat Custom OAuth2.

```
Enable: [X] True
URL: https://example.eu.auth0.com/
Token Path: oauth/token
Token Sent Via: Payload
Identity Token Sent Via: Same as "Token Sent Via"
Identity Path: userinfo
Authorize Path: authorize
Scope: openid profile email
ID: 12345abcde
Secret: abcde54321
Login Style: Redirect
Button Text: JOIN CHAT
Button Text Color: #FFFFFF
Button Color: #000000
Username field: (empty)
Merge users: [X] True
```