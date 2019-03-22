---
title: install wekan docker in production
pageTitle:  install wekan docker in production
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
# question: Which of these statements is true?
# answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
# correctAnswer: 2
# videoId: oCT4HOJsUZQ
# duration: 5
---

# Different wiki page: [Production setup for thousands of users at AWS](https://github.com/wekan/wekan/wiki/AWS)


***


## Single server install, for small teams

## Also see: [Using same database for both LAN and VPN Wekan](https://github.com/wekan/wekan-mongodb/blob/master/docker-compose.yml#L86-L100)

**Purpose:** run Wekan on a production Linux server with Docker and Apache or Nginx as a front-end server (reverse proxy)

## 1. Install newest Docker and Docker Compose

[Docker website](https://docker.com)

## 2. Use Wekan-MongoDB with Docker Compose

https://github.com/wekan/wekan-mongodb

[External MongoDB authentication](https://github.com/wekan/wekan/issues/1375)

## 3. Email

[Troubleshooting Email](https://github.com/wekan/wekan/wiki/Troubleshooting-Mail)

## 4. Configure webserver as a front-end proxy

* [Caddy](https://github.com/wekan/wekan/wiki/Caddy-Webserver-Config)
* [Nginx](https://github.com/wekan/wekan/wiki/Nginx-Webserver-Config)
* [Apache](https://github.com/wekan/wekan/wiki/Apache)

## 5. Launch Wekan

As `wekan` user and from `/home/wekan`, run `docker-compose up -d`

## 6. Improvements to bring to this doc

* Verify everything works


## 7. Tested on...

This procedure has been tested on:

* [VPS-SSD 2016 from OVH](https://www.ovh.com/fr/vps/vps-ssd.xml) with Ubuntu 14.04