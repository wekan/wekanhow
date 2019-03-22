---
title: Mac
pageTitle: Mac
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

## Docker

- [Docker](https://github.com/wekan/wekan/wiki/Docker)
- [Docker Dev Environment](https://github.com/wekan/wekan-dev)

## Source

1. Install XCode
2. Install [Node 8.15.0](https://nodejs.org/en/)
3. Use [rebuild-wekan.sh script](https://github.com/wekan/wekan/blob/edge/releases/virtualbox/rebuild-wekan.sh) to first install Wekan dependencies and then build Wekan.
4. Change to Wekan directory: `cd wekan`
5. Run meteor: `meteor` - this runs node in http://localhost:3000 and mongo at http://localhost:3001 .
6. In another bash shell, run: `meteor mongo` - this connects to MongoDB CLI
7. Alternatively, to use custom ports, use for example `meteor --port 4000` that runs node in port 4000 and mongo in next port 4001.