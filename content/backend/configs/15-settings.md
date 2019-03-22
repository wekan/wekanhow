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

## Standalone Wekan Settings: Snap, Docker, Source etc (not Sandstorm)

## Webserver config
* [Nginx](https://github.com/wekan/wekan/wiki/Nginx-Webserver-Config)
* [Apache](https://github.com/wekan/wekan/wiki/Apache)
* [Caddy](https://github.com/wekan/wekan/wiki/Caddy-Webserver-Config)

Examples:

1) nginx SSL or without SSL, available at internet:
- root-url='https://example.com'   or https://example.com/something or https://something.example.com , or with http
- port='3001'
- [Nginx example, proxying to local port 3001](https://github.com/wekan/wekan/wiki/Nginx-Webserver-Config)
=> Wekan at https://example.com

2) only wekan, no SSL, internal lan, caddy not enabled:
- root-url='http://192.168.1.150'
- port='80'
=> Wekan locally http://192.168.1.150

3) only wekan, no SSL, internal lan, caddy not enabled, wekan at different port:
- root-url='http://192.168.1.150:5000'
- port='5000'
=> Wekan locally http://192.168.1.150:5000

4) wekan's caddy SSL, available at Internet:
- root-url='https://example.com'   or https://example.com/something or https://something.example.com
- port='3001'
- [Caddyfile example](https://github.com/wekan/wekan-snap/wiki/Install#7-replace-first-top-line-of-text-with-subdomainexamplecomsuburl-without-any-beginning-of-httphttps)
=> Wekan at https://example.com

Wekan runs http server on local port, so it is without SSL. To get SSL, some webserver like Caddy and Nginx that have SSL, can proxy to local Wekan http port where node.js runs.

## Admin Panel

First registered Wekan user will get [Admin Panel](https://github.com/wekan/wekan/wiki/Features) on new
Docker and source based installs. You can also on MongoDB 
[enable Admin Panel](https://github.com/wekan/wekan/blob/devel/CHANGELOG.md#v0111-rc2-2017-03-05-wekan-prerelease) and [change you as board admin](https://github.com/wekan/wekan/issues/1060#issuecomment-310545976).

## LAN + VPN

[Using same database for both LAN and VPN Wekan](https://github.com/wekan/wekan-mongodb/blob/master/docker-compose.yml#L86-L100)

## Proxy

[Using Proxy](https://github.com/wekan/wekan/issues/1480)

## Email

[Troubleshooting Mail](https://github.com/wekan/wekan/wiki/Troubleshooting-Mail). For Exchange, you can use [DavMail](http://davmail.sourceforge.net), Wekan SMTP => DavMail => Exchange.

## RAM usage

[RAM usage](https://github.com/wekan/wekan/issues/1088#issuecomment-311843230)