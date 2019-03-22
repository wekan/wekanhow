---
title: REST API
pageTitle: REST API
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

REST API is not complete yet, please add missing functionality with pull requests to devel branch.

If you are in a hurry, you can use these to have more functionality:
* https://restheart.org
* http://vulcanjs.org

For workflows see [If-this-then-that issue](https://github.com/wekan/wekan/issues/1160) than mentions Huginn, Flogo etc.

# Wekan REST API

The REST API allows you to control and extend Wekan with ease.

If you are an end-user and not a dev or a tester, [create an issue](https://github.com/wekan/wekan/issues/new) to request new APIs.

> All API calls in the documentation are made using `curl`.  However, you are free to use Java / Python / PHP / Golang / Ruby / Swift / Objective-C / Rust / Scala / C# or any other programming languages.

## Production Security Concerns
When calling a production Wekan server, ensure it is running via HTTPS and has a valid SSL Certificate. The login method requires you to post your username and password in plaintext, which is why we highly suggest only calling the REST login api over HTTPS. Also, few things to note:

* Only call via HTTPS
* Implement a timed authorization token expiration strategy
* Ensure the calling user only has permissions for what they are calling and no more

# Summary

### Authentication
| HTTP Method | Url | Short Description |
| :--- | :--- | :--- |
| `POST` | `/users/login` | [Authenticate with the REST API.](#login) |

### Users
| HTTP Method | Url | Short Description |
| :--- | :--- | :--- |
| `POST` | `/users/register` | [Register a new user.](https://github.com/wekan/wekan/wiki/REST-API-User#user-register) |
| `POST` | `/api/users` | [Create a new user.](https://github.com/wekan/wekan/wiki/REST-API-User#user-create) |
| `PUT` | `/api/users/:id` | [Disable an existing user.](https://github.com/wekan/wekan/wiki/REST-API-User#disable-a-user-the-user-is-not-allowed-to-login-and-his-login-tokens-are-purged) |
| `PUT` | `/api/users/:id` | [Enable an existing user.](https://github.com/wekan/wekan/wiki/REST-API-User#enable-a-user) |
| `PUT` | `/api/users/:id` | [Admin takes the ownership.](https://github.com/wekan/wekan/wiki/REST-API-User#the-admin-takes-the-ownership-of-all-boards-of-the-user-archived-and-not-archived-where-the-user-is-admin-on) |
| `DELETE` | `/api/users/:id` | [Delete an existing user.](https://github.com/wekan/wekan/wiki/REST-API-User#user-delete) ([Warning](https://github.com/wekan/wekan/issues/1289))|
| `GET` | `/api/users/:id` | [Gets a user's information.](https://github.com/wekan/wekan/wiki/REST-API-User#user-information) |
| `GET` | `/api/users` | [All of the users.](https://github.com/wekan/wekan/wiki/REST-API-User#user-list) |
| `GET` | `/api/user` | [Gets a logged-in user.](https://github.com/wekan/wekan/wiki/REST-API-User#user-logged-in) |
### Cards
| HTTP Method | Url | Short Description |
| :--- | :--- | :--- |
| `POST` | `/api/boards/:boardId/lists/:listId/cards` | [Add a card to a list, board, and swimlane.](https://github.com/wekan/wekan/wiki/REST-API-Cards#add-card-to-list-board-swimlane) |
| `PUT` | `/api/boards/:boardId/lists/:fromListId/cards/:cardId` | [Update a card.](https://github.com/wekan/wekan/wiki/REST-API-Cards#update-a-card) |
| `DELETE` | `/api/boards/:boardId/lists/:listId/cards/:cardId` | [Delete a card.](https://github.com/wekan/wekan/wiki/REST-API-Cards#update-a-card) |


---

# Login
| URL | Requires Auth | HTTP Method |
| :--- | :--- | :--- |
| `/users/login` | `no` | `POST` |

## Payload

### Authentication with username
| Argument | Example | Required | Description |
| :--- | :--- | :--- | :--- |
| `username` | `myusername` | Required | Your username |
| `password` | `my$up3erP@ssw0rd` | Required | Your password |

### Authentication with email
| Argument | Example | Required | Description |
| :--- | :--- | :--- | :--- |
| `email` | `my@email.com` | Required | Your email |
| `password` | `my$up3erP@ssw0rd` | Required | Your password |

* Notes:
 * **You will need to provide the `token` for any of the authenticated methods.**

## Example Call - As Form Data
```bash
curl http://localhost:3000/users/login \
     -d "username=myusername&password=mypassword"
```

```bash
curl http://localhost:3000/users/login \
     -d "email=my@email.com&password=mypassword"
```


## Example Call - As JSON
```bash
curl -H "Content-type:application/json" \
      http://localhost:3000/users/login \
      -d '{ "username": "myusername", "password": "mypassword" }'
```

```bash
curl -H "Content-type:application/json" \
      http://localhost:3000/users/login \
      -d '{ "email": "my@email.com", "password": "mypassword" }'
```


## Result
```json
{
  "id": "user id",
  "token": "string",
  "tokenExpires": "ISO encoded date string"
}
```

## Result example
```json
{
  "id": "XQMZgynx9M79qTtQc",
  "token": "ExMp2s9ML1JNp_l11sIfINPT3wykZ1SsVwg-cnxKdc8",
  "tokenExpires": "2017-12-15T00:47:26.303Z"
}
```
