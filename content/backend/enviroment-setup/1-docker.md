---
title: Docker
pageTitle: Docker
description: "Learn all about GraphQL and why it is an API technology that's superior to REST. It is not only for React & Javascript developers but can be used for any API."
question: Which of these statements is true?
answers: ["GraphQL is a database technology", "GraphQL can only be used together with SQL", "GraphQL was invented by Facebook", "GraphQL was developed by Netflix and Coursera"]
correctAnswer: 2
videoId: oCT4HOJsUZQ
duration: 5
---

## DockerBunker: Easy Docker management

[Managing Docker containers with DockerBunker](https://github.com/chaosbunker/dockerbunker)

## CaptainDuckDuck

[Managing Docker containers with CaptainDuckDuck](https://github.com/wekan/wekan/issues/1375#issuecomment-413626075)

## Backup and Upgrade

[Import/Export MongoDB data to/from Docker container](https://github.com/wekan/wekan/wiki/Export-Docker-Mongo-Data)

[Move Docker containers to other computer](https://github.com/wekan/wekan/wiki/Move-Docker-containers-to-other-computer), needs more details

[Backup and Upgrade](https://github.com/wekan/wekan-mongodb#backup-before-upgrading)

Quay: `image: quay.io/wekan/wekan:v1.07`
Docker Hub: maybe is broken.

Based on Debian 10, Docker image at Quay.io Docker reqistry


[Wekan for Docker bug reports and feature requests](https://github.com/wekan/wekan-mongodb/issues)

## Cleanup

[Cleanup and delete all Docker data to get Docker Compose working](https://github.com/wekan/wekan/issues/985)

[Cleanup scripts to remove old data](https://github.com/wekan/wekan-cleanup)

## Docker Compose

[Docker Compose: Wekan <=> MongoDB](https://github.com/wekan/wekan-mongodb). REQUIRED: READ AND ADD SETTINGS LIKE ROOT_URL ETC TO docker-compose.yml textfile. It also has info about using same MongoDB database for office and VPN users.

[Docker Compose: Wekan <=> MongoDB <=> ToroDB => PostgreSQL read-only mirroring](https://github.com/wekan/wekan-postgresql)

TODO: [Docker Compose: Wekan <=> MongoDB <=> ToroDB => MySQL read-only mirroring](https://github.com/torodb/stampede/issues/203)

## OpenShift

[OpenShift](https://github.com/wekan/wekan/wiki/OpenShift)

## SLES

[SLES SP1](https://github.com/wekan/wekan/wiki/Install-Wekan-Docker-on-SUSE-Linux-Enterprise-Server-12-SP1)

## Rancher

[Rancher Rancher Active Proxy](https://github.com/wekan/wekan/wiki/Rancher---Rancher-Active-Proxy---Wekan-MongoDB-Docker)

## Testing

[Install for testing](https://github.com/wekan/wekan/wiki/Install-Wekan-Docker-for-testing)

## Production

[Production setup for thousands of users with Docker at AWS](https://github.com/wekan/wekan/wiki/AWS)

[Other way to do production](https://github.com/wekan/wekan/wiki/Install-Wekan-Docker-in-production)

## External MongoDB auth

[External MongoDB authentication](https://github.com/wekan/wekan/issues/1375)

## Admin Panel

First registered Wekan user will get Admin Panel on new Docker and source based
installs. You can also [enable Admin Panel manually](https://github.com/wekan/wekan/blob/devel/CHANGELOG.md#v0111-rc2-2017-03-05-wekan-prerelease)

## Quay

[![Docker Repository on Quay](https://quay.io/repository/wekan/wekan/status "Docker Repository on Quay")](https://quay.io/repository/wekan/wekan)

[Many tags available](https://quay.io/repository/wekan/wekan?tab=tags)

Example for latest Wekan:
```
docker run -d --restart=always --name wekan-db mongo:3.2.20

docker run -d --restart=always --name wekan --link "wekan-db:db" -e "MONGO_URL=mongodb://db" -e "ROOT_URL=http://192.168.1.200:8080" -p 8080:80 quay.io/wekan/wekan
```
Specific release in above URL, not latest:
```
quay.io/wekan/wekan:v1.25
```
For latest development version, use without tag:
```
quay.io/wekan/wekan
```

There is much more complete well-documented `docker-compose.yml` at [wekan-mongodb](https://github.com/wekan/wekan-mongodb) with instructions on installing there also.

## Docker Hub - usually broken

Currently there are two dockerhub builds for wekan. One at [mquandalle dockerhub](https://hub.docker.com/r/mquandalle/wekan/builds/) and another at [wekanteam dockerhub](https://hub.docker.com/r/wekanteam/wekan/builds/). 

[wekanteam dockerhub](https://hub.docker.com/r/wekanteam/wekan/builds/) is usually broken.

## Development:

### `docker run` examples

- MongoDB:

```
docker run -d --restart=always --name wekan-db mongo:3.2.20
```

- No build step, pull from the [quay](https://quay.io/repository/wekan/wekan?tab=tags) and
specify docker variables

```
docker run -d --restart=always --name wekan --link "wekan-db:db" -e "MONGO_URL=mongodb://db" -e "ROOT_URL=http://localhost:8080" -p 8080:8080 quay.io/wekan/wekan
```


### `docker-compose` examples

- No build step and pull from [quay](https://quay.io/repository/wekan/wekan?tab=tags)

```
sudo docker-compose up -d --nobuild
```

- Build default
```
sudo docker-compose up -d --build
```

- Build with newer Node version:
```
echo 'NODE_VERSION=v8.11.1' >> .env && \
sudo docker-compose up -d --build
```

- Build custom image off a release candidate or beta for meteor
```
echo 'METEOR_EDGE=1.5-beta.17' >> .env && \
echo 'USE_EDGE=true' >> .env && \
sudo docker-compose up -d --build
```

## Docker env for Wekan dev

* [Docker environment for Wekan Development](https://github.com/wekan/wekan-dev)

## Alpine, needs testing

* [Docker Compose: Alpine Linux and Wekan <=> MongoDB](https://github.com/wekan/wekan-launchpad)

## Webserver Config

* [Caddy Webserver Config](https://github.com/wekan/wekan/wiki/Caddy-Webserver-Config)
* [Nginx Webserver Config](https://github.com/wekan/wekan/wiki/Nginx-Webserver-Config)
* [Apache Webserver Config](https://github.com/wekan/wekan/wiki/Apache)