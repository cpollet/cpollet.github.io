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

### Debian 8, with Docker Cloud Agent

```
$ apt-get update
$ apt-get install -y curl htop
$ curl -Ls https://get.cloud.docker.com/ | sh -s {yourKey}
```

{youKey} comes from Docker Cloud

See as well [Docker Cloud doc](https://docs.docker.com/docker-cloud/tutorials/byoh/).

### Move docker directory, Ubuntu/Debian

```
$ vi /etc/default/docker
DOCKER_OPTS="-g /somewhere/else/docker/"
```

1. Stop docker: `service docker stop`. Verify no docker process is running `ps faux`
1. Double check docker really isn't running. Take a look at the current docker directory: `ls /var/lib/docker/`
1. Make a backup: `tar -zcC /var/lib docker > /mnt/pd0/var_lib_docker-backup-$(date +%s).tar.gz`
1. Move the `/var/lib/docker` directory to your new partition: `mv /var/lib/docker /mnt/pd0/docker`
1. Make a symlink: `ln -s /mnt/pd0/docker /var/lib/docker`
1. Take a peek at the directory structure to make sure it looks like it did before the mv: `ls /var/lib/docker/` (note the trailing slash to resolve the symlink)
1. Start docker back up: `service docker start`
1. Restart your containers

source: https://github.com/docker/docker/issues/3127
