---
title: install from source without root
pageTitle:  install from source without root
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
# question: Which of these statements is true?
# answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
# correctAnswer: 2
# videoId: oCT4HOJsUZQ
# duration: 5
---

# Quick install for development / debugging

* Install [nvm](https://github.com/creationix/nvm)
* `source NVMPATH/nvm.sh` for example `source ~/.nvm/nvm.sh`
* `nvm install v8.11.3`
* `nvm use v8.11.3`
* `nvm install-latest-npm`
* `cd ~`
* Clone repo to home: `git clone https://github.com/wekan/wekan.git`
* Install meteor (you can skip sudo by entering invalid password): `curl https://install.meteor.com/ | sh`
* `cd wekan/`
* `~/.meteor/meteor npm install --save babel-runtime xss meteor-node-stubs`
* `~/.meteor/meteor npm install --save bcrypt`
* `~/.meteor/meteor`

When you get this output, wekan is ready:
```
=> Started your app.

=> App running at: http://localhost:3000/
```

Register new user for administrator
