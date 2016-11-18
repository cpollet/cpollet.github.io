---
layout: post
title: Docker hosts on gandi.net
---

### Ubuntu 14.04 LTS, raw Docker
Just follow the instructions at [Docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/):

```
$ apt-get update
$ apt-get install apt-transport-https ca-certificates
$ apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
$ echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list
$ apt-get update
$ apt-get purge lxc-docker
$ apt-cache policy docker-engine
$ sudo apt-get install docker-engine
$ sudo service docker start
$ sudo docker run hello-world
```

### Ubuntu 16.04 LTS, raw Docker
Just follow the instructions at [Docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/).

```
$ vi /etc/default/docker
DOCKER_OPTS="-g /somewhere/else/docker/"
```


### Debian 8, with Docker Cloud Agent

```
$ apt-get update
$ apt-get install -y curl htop
$ curl -Ls https://get.cloud.docker.com/ | sh -s {yourKey}
```

{youKey} comes from Docker Cloud

See as well [Docker Cloud doc](https://docs.docker.com/docker-cloud/tutorials/byoh/).
