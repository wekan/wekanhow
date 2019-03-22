---
title: Sandstorm
pageTitle: Sandstorm
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

## Sandstorm Website

[Sandstorm Website](https://sandstorm.io)

## Demo

[![Try on Sandstorm][sandstorm_button]][sandstorm_appdemo]

## Keep backups

- Keep backups. Download your Wekan grains.
- Importing boards have [data loss issue: Board not Found](https://github.com/wekan/wekan/issues/1430), it was also broken before this version. In that case, you can try to  [Export from Wekan Sandstorm grain .zip file to rescue data](https://github.com/wekan/wekan/wiki/Export-from-Wekan-Sandstorm-grain-.zip-file)

## Wekan App

Wekan at [experimental](https://apps.sandstorm.io/app/m86q05rdvj14yvn78ghaxynqz7u2svw6rnttptxx49g1785cdv1h?experimental=true) or [official](https://apps.sandstorm.io/app/m86q05rdvj14yvn78ghaxynqz7u2svw6rnttptxx49g1785cdv1h) Sandstorm App Market

Newest Wekap app .spk file download at https://releases.wekan.team

## Bug reports and Feature Requests

[Wekan for Sandstorm bug reports and feature requests](https://github.com/wekan/wekan/issues)

[Sandstorm bug reports and feature requests](https://github.com/sandstorm-io/sandstorm/issues)

## Building Wekan for Sandstorm

[Building Wekan for Sandstorm](https://github.com/wekan/wekan-maintainer/wiki/Building-Wekan-for-Sandstorm)

## Wekan Sandstorm cards to CSV using Python

[Wekan Sandstorm cards to CSV using Python](https://github.com/wekan/wekan/wiki/Wekan-Sandstorm-cards-to-CSV-using-Python)

## Importing to Trello workaround

It is not possible to import attachments directly from Trello when using Sandstorm version of Wekan. This is because Wekan is in secure sandbox at Sandstorm, and does not yet have Sandstorm-compatible way to import attachments from outside of Sandstorm. You need to:
1. Install Standalone version of Wekan, for example Docker/Snap/VirtualBox, for example to your own computer
2. Import board from Trello
3. Export board as Wekan board. Exported JSON file includes attachments as base64 encoded files.
4. Import board as Wekan board to Sandstorm.

[sandstorm_button]: https://img.shields.io/badge/try-Wekan%20on%20Sandstorm-783189.svg
[sandstorm_appdemo]: https://demo.sandstorm.io/appdemo/m86q05rdvj14yvn78ghaxynqz7u2svw6rnttptxx49g1785cdv1h