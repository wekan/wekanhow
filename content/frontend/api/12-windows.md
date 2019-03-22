---
title: Windows
pageTitle: Windows
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

a) [Docker](https://github.com/wekan/wekan/wiki/Docker)

b) Windows Subsystem for Linux on Windows 10
- [Install Windows Subsystem for Linux](https://wiki.debian.org/InstallingDebianOn/Microsoft/Windows/SubsystemForLinux)
- Install Debian from Windows Store
- Use [VirtualBox scripts](https://github.com/wekan/wekan-maintainer/tree/master/virtualbox) of rebuild-wekan.sh etc to install and build Wekan
- Run Wekan locally with meteor at http://localhost:3000 : `cd wekan && meteor`
- Or: try to modify start-wekan.sh etc to run wekan at http://ip-address or http://example.com
- You could try to proxy from IIS SSL website to Wekan localhost port, for example when ROOT_URL=https://example.com and PORT=3001 , and you make IIS config that supports websockets proxy to Wekan http port 3001.

c) [Install from source directly on Windows](https://github.com/wekan/wekan/wiki/Install-Wekan-from-source-on-Windows) to get Wekan running natively on Windows. [git clone on Windows has been fixed](https://github.com/wekan/wekan/issues/977). Related: [running standalone](https://github.com/wekan/wekan/issues/883) and [nexe](https://github.com/wekan/wekan/issues/710).

## Related

[Linux stuff in Windows 10 part 1](https://cepa.io/2018/02/10/linuxizing-your-windows-pc-part1/) and [part 2](https://cepa.io/2018/02/20/linuxizing-your-windows-pc-part2/).
