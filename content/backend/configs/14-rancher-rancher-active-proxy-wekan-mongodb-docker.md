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

You can read up how to set up Rancher on a host together with Rancher Active Proxy and a Wekan/MongoDB Docker Stack.

This way you have Wekan running on a rancher host with automatic letsencrypt retrieval/renewal and proxying to a domain of your choice.

Here's how to set up Rancher + Rancher Active Proxy:
https://github.com/adi90x/rancher-active-proxy/issues/21

Alter the wekan service in the docker-compose like this:
```
  wekan:
    image: wekanteam/wekan:meteor-1.4
    container_name: whatever-you-like
    restart: always
    ports:
      - 80
    labels:
      - io.rancher.container.pull_image=always
      - rap.port=80
      - rap.host=your.domain.com
      - rap.le_host=your.domain.com
      - rap.le_email=your@mail.com
    environment:
      - MONGO_URL=mongodb://wekandb:27017/wekan
      - ROOT_URL=https://your.domain.com
    depends_on:
      - wekandb
```