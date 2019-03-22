---
title: LDAP
pageTitle: LDAP
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

## Snap

LDAP is available on Snap Stable channel. Settings can be seen with command `wekan.help` and from repo https://github.com/wekan/wekan-ldap . More settings at https://github.com/wekan/wekan-snap/wiki/Supported-settings-keys

## Docker

LDAP login works now by using this docker-compose.yml file:
https://raw.githubusercontent.com/wekan/wekan/edge/docker-compose.yml
adding ROOT_URL, LDAP settings etc to that file.

Using this docker-compose:
https://docs.docker.com/compose/install/

With this command:
``` 
docker-compose up -d --no-build
```

## Bugs and Feature Requests

[LDAP Bugs and Feature Requests](https://github.com/wekan/wekan-ldap/issues)
