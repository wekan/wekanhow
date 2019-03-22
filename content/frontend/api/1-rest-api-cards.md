---
title: REST API Cards
pageTitle: REST API Cards
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

# Disclaimer

This page tries to be as up to date as possible. If you see something wrong here, feel free to update the page and help other people like you, that greatly depends on our APIs. If you don't feel comfortable doing this kind of changes, please contact us by creating an [issue](https://github.com/wekan/wekan/issues/new).

# Retrieve cards by swimlane id

Please somebody add example by looking this:

[Issue](https://github.com/wekan/wekan/issues/1934) and [code](https://github.com/wekan/wekan/pull/1944/commits/be42b8d4cbdfa547ca019ab2dc9a590a115cc0e2). Also add to [Swimlanes page](https://github.com/wekan/wekan/wiki/REST-API-Swimlanes)

# Add Card to List-Board-Swimlane

| API URL / Code Link | Requires Admin Auth | HTTP Method |
| :--- | :--- | :--- |
| [/api/boards/:boardId/lists/:listId/cards](https://github.com/wekan/wekan/blob/c115046a7c86b30ab5deb8762d3ef7a5ea3f4f90/models/cards.js#L487) | `yes` | `POST` |

```shell
curl -H "Authorization: Bearer t7iYB86mXoLfP_XsMegxF41oKT7iiA9lDYiKVtXcctl" \
     -H "Content-type:application/json" \
     -X POST \
     http://localhost:3000/api/boards/YRgy7Ku6uLFv2pYwZ/lists/PgTuf6sFJsaxto5dC/cards \
     -d '{ "title": "Card title text", "description": "Card description text", "authorId": "The appropriate existing userId", "swimlaneId": "The destination swimlaneId" }'
```
## Result example
The new card's ID is returned in the format:
```json
{
    "_id": "W9m9YxQKT6zZrKzRW"
}
```

# Update a card
You can change (any of) the card's title, list, and description.

| API URL / Code Link | Requires Admin Auth | HTTP Method |
| :--- | :--- | :--- |
| [/api/boards/:boardId/lists/:fromListId/cards/:cardId](https://github.com/wekan/wekan/blob/c115046a7c86b30ab5deb8762d3ef7a5ea3f4f90/models/cards.js#L520) | `yes` | `PUT` |

```shell
curl -H "Authorization: Bearer t7iYB86mXoLfP_XsMegxF41oKT7iiA9lDYiKVtXcctl" \
     -H "Content-type:application/json" \
     -X PUT \
     http://localhost:3000/api/boards/YRgy7Ku6uLFv2pYwZ/lists/PgTuf6sFJsaxto5dC/cards/ssrNX9CvXvPxuC5DE \
     -d '{ "title": "New title text", "listId": "New destination listId", "description": "New description text" }'
```
## Result example
The card's ID is returned in the format:
```json
{
    "_id": "W9m9YxQKT6zZrKzRW"
}
```
# Delete a card

| API URL / Code Link | Requires Admin Auth | HTTP Method |
| :--- | :--- | :--- |
| [/api/boards/:boardId/lists/:listId/cards/:cardId](https://github.com/wekan/wekan/blob/c115046a7c86b30ab5deb8762d3ef7a5ea3f4f90/models/cards.js#L554) | `yes` | `DELETE` |

```shell
curl -H "Authorization: Bearer t7iYB86mXoLfP_XsMegxF41oKT7iiA9lDYiKVtXcctl" \
     -H "Content-type:application/json" \
     -X DELETE \
     http://localhost:3000/api/boards/YRgy7Ku6uLFv2pYwZ/lists/PgTuf6sFJsaxto5dC/cards/ssrNX9CvXvPxuC5DE \
     -d '{ "authorId": "the appropriate existing userId"}'
```
## Result example
The card's ID is returned in the format:
```json
{
    "_id": "W9m9YxQKT6zZrKzRW"
}
```

# In Wekan code

If you believe that code is the best documentation, be our guest: [models/cards.js](https://github.com/wekan/wekan/blob/devel/models/cards.js "Card API code")