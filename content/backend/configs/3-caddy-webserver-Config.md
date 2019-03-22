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

[List of Let's Encrypt implementations](https://community.letsencrypt.org/t/list-of-client-implementations/2103)

## Caddy webserver config

Create directory for caddy, website and logs:
```bash
mkdir -p ~/caddy/example.com ~/caddy/logs
```

Add this config to ~/caddy/Caddyfile

There's also some extra examples.

```bash
example.com {
        root /home/username/caddy/example.com
        # Static website, markdown or html
        ext .md .html

        proxy /wekan 127.0.0.1:3000 {
                websocket
        }

	log /home/username/caddy/logs/wekan-access.log {
	    rotate {
		size 100 # Rotate after 100 MB
		age  7   # Keep log files for 7 days
		keep 52  # Keep at most 52 log files
	    }
	}
	errors {
		log /home/username/caddy/logs/wekan-error.log {
			size 100 # Rotate after 100 MB
			age  7   # Keep log files for 7 days
			keep 52  # Keep at most 52 log files
		}
	}
}

example.com/files {
	root /home/username/files
	# View files in directory, has sorting in browser
	browse
}
```

Install Caddy. Change username to what user you run caddy, like in /home/username , and Let's Encrypt email to your email adderess:

```bash
# Install caddy with some plugins
curl https://getcaddy.com | bash -s personal http.ipfilter,http.mailout,http.ratelimit,http.realip
```

# Give permissions to caddy to bind 80 and 443

```
sudo setcap cap_net_bind_service=+ep /usr/local/bin/caddy
```

And this service file for Caddy to /etc/systemd/system/caddy@.service

```bash
; see `man systemd.unit` for configuration details
; the man section also explains *specifiers* `%x`

[Unit]
Description=Caddy HTTP/2 web server %I
Documentation=https://caddyserver.com/docs
After=network-online.target
Wants=network-online.target
Wants=systemd-networkd-wait-online.service

[Service]
; run user and group for caddy
User=username
Group=username
ExecStart=/home/username/caddy/caddy -conf=/home/username/caddy/Caddyfile -agree -email="admin@example.com"
Restart=on-failure
StartLimitInterval=86400
StartLimitBurst=5
RestartSec=10
ExecReload=/bin/kill -USR1 $MAINPID
; limit the number of file descriptors, see `man systemd.exec` for more limit settings
LimitNOFILE=1048576
LimitNPROC=64
; create a private temp folder that is not shared with other processes
PrivateTmp=true
PrivateDevices=true
ProtectSystem=full
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

Start caddy and enable service:
```
sudo systemctl daemon-reload
sudo systemctl start caddy@username
sudo systemctl enable caddy@username
```