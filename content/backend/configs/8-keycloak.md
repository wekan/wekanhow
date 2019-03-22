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

## OIDC Integration

[Outstanding Bug](https://github.com/wekan/wekan/issues/1874#issuecomment-460802250): Create the first user (admin) with the regular process.  Then the remaining users can use the Register with OIDC process.

Environment Variables that need to be set in your Wekan environment:

* OAUTH2_ENABLED = TRUE
* OAUTH2_CLIENT_ID = `<Keycloak create Client ID>`
* OAUTH2_SERVER_URL = `<Keycloak server name>/auth`
* OAUTH2_AUTH_ENDPOINT = `/realms/<keycloak realm>/protocol/openid-connect/auth`
* OAUTH2_USERINFO_ENDPOINT = `/realms/<keycloak realm>/protocol/openid-connect/userinfo`
* OAUTH2_TOKEN_ENDPOINT = `/realms/<keycloak realm>/protocol/openid-connect/token`
* OAUTH2_SECRET = `<keycloak client secret>`
* OAUTH2_ID_MAP = `preferred_username`
* OAUTH2_USERNAME_MAP = `preferred_username`
* OAUTH2_FULLNAME_MAP = `given_name`
* OAUTH2_EMAIL_MAP = `email`
> When creating a Client in keycloak, ensure the access type is confidential under the settings tab.  After clicking save, you will have a Credentials tab.  You can retrieve the secret from that location.