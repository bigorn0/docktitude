# Docktitude - A mastering Docker tool to enhance your docker experience

![](https://raw.githubusercontent.com/docktitude/docktitude/master/docs/docktitude-logo.png "https://docktitude.com")

Display Tree | Update | Snapshot your Docker images hierarchy and much more...

## Requirements

### Node.js (v4.4+)
- Download: [Node.js](https://nodejs.org)

### Docker
- Obviously you need it :)
- Download: [Docker](https://www.docker.com)


## Installation

- Node.js package
[![NPM Version](http://img.shields.io/npm/v/docktitude.svg?style=flat)](https://www.npmjs.org/package/docktitude)
```
$> npm install -g docktitude
```

- Docker image
```
$> docker run -it --rm --name docktitude \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $(which docker):/bin/docker \
  -v your-docker-contexts-dir:/docker-contexts \
  docktitude/docktitude
```


## Getting started

1. **cd docker-contexts** *(where you store your Docker contexts)*
2. **docktitude config** *(check the computed tags for the build according to the contexts naming convention)*
3. **docktitude upgrade** *(cascade build: from the root to the leafs)*


## Usage examples

```bash
$> docktitude tree
.
+-- alpine*
|   +-- transmission
+-- debian:latest*
|   +-- debian:local
|       +-- clamav
|       +-- debian:jdk8
|       |   +-- apache/activemq
|       |   +-- debian:jdk8-ui
|       |       +-- debian:jdk8-scm
|       |       |   +-- idea
|       |       +-- libreoffice
|       +-- vscode
+-- nginx*
|   +-- demo-site
+-- ubuntu:14.04*
    +-- gitlab/gitlab-runner:local
```


```bash
$> docktitude info
+---------------+-------------------+
| BASE IMAGE    | DISTRIBUTION (12) |
+---------------+-------------------+
| alpine        | 8.333 %  (1)      |
| debian:latest | 75.00 %  (9)      |
| nginx         | 8.333 %  (1)      |
| ubuntu:14.04  | 8.333 %  (1)      |
+---------------+-------------------+
```


```bash
$> docktitude script libreoffice
--------------------------------
+++ libreoffice [ SHELL SCRIPT ]
--------------------------------
#!/bin/sh -

docker run -it --rm --name libreoffice \
  -e DISPLAY=unix$DISPLAY \
  -e GDK_SCALE \
  -e GDK_DPI_SCALE \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v /etc/localtime:/etc/localtime:ro \
  libreoffice
--------------------------------
```


## Documentation

```java
$> docktitude -h
usage: docktitude [-h | --help | help] <command> [<args>]

Commands:
   build <context>    Build context Docker image. Use -f to force build
   clean              Remove exited Docker containers and useless images
                      Use -v to remove the associated volumes
   config             List auto-configured Docker images building tags
   export             Export all contexts except binaries to a tar archive
   info               Show information relating to the Dockerfile files
   op <name>          Change maintainer information in the Dockerfile files
   play <context>     Run shell script for defined docktitude script tags
   print <context>    Show context Dockerfile
   script <context>   Show shell script for defined docktitude script tags
   snapshot           Display Docker images and save the selected one (.tar)
   status             Show local Docker images update status
   tree               List Docker images in a tree-like format
   update             Update external Docker images
   upgrade            Build cascade local Docker images
   version            Show version information
```


## Licensing

Docktitude is licensed under Apache License, Version 2.0.
