---
title: REST API Role
pageTitle: REST API Role
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

# Disclaimer

This page tries to be as up to date as possible. If you see something wrong here, feel free to update the page and help other people like you, that greatly depends on our APIs. If you don't feel comfortable doing this kind of changes, please contact us by creating an [issue](https://github.com/wekan/wekan/issues/new).

## Add New Board Member with Role
This example adds with normal role. See examples below for other roles.
```
curl -H "Authorization: Bearer a6DM_gOPRwBdynfXaGBaiiEwTiAuigR_Fj_81QmNpnf" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/BOARD-ID-HERE/members/USER-ID-HERE/add \
     -d '{"action": "add","isAdmin": "false", "isNoComments":"false", "isCommentOnly": "false"}'
```

## Remove Member from Board
```
curl -H "Authorization: Bearer a6DM_gOPRwBdynfXaGBaiiEwTiAuigR_Fj_81QmNpnf" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/BOARD-ID-HERE/members/USER-ID-HERE/remove \
     -d '{"action": "remove"}'
```

# Change Role of Existing Board Member

## Admin
```
curl -H "Authorization: Bearer a6DM_gOPRwBdynfXaGBaiiEwTiAuigR_Fj_81QmNpnf" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/BOARD-ID-HERE/members/USER-ID-HERE \
     -d '{"isAdmin": "true", "isNoComments":"false", "isCommentOnly": "false"}'
```
## Normal
```
curl -H "Authorization: Bearer a6DM_gOPRwBdynfXaGBaiiEwTiAuigR_Fj_81QmNpnf" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/BOARD-ID-HERE/members/USER-ID-HERE \
     -d '{"isAdmin": "false", "isNoComments":"false", "isCommentOnly": "false"}'
```
## No Comments
```
curl -H "Authorization: Bearer a6DM_gOPRwBdynfXaGBaiiEwTiAuigR_Fj_81QmNpnf" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/BOARD-ID-HERE/members/USER-ID-HERE \
     -d '{"isAdmin": "false", "isNoComments":"true", "isCommentOnly": "false"}'
```
## Comment Only
```
curl -H "Authorization: Bearer a6DM_gOPRwBdynfXaGBaiiEwTiAuigR_Fj_81QmNpnf" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/BOARD-ID-HERE/members/USER-ID-HERE \
     -d '{"isAdmin": "false", "isNoComments":"false", "isCommentOnly": "true"}'
```
