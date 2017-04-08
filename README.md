[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: https://vaemendis.net/ubooquity/
[hub]: https://hub.docker.com/r/lsioarmhf/ubooquity/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/ubooquity
[![](https://images.microbadger.com/badges/version/lsioarmhf/ubooquity.svg)](https://microbadger.com/images/lsioarmhf/ubooquity "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/ubooquity.svg)](http://microbadger.com/images/lsioarmhf/ubooquity "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/ubooquity.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/ubooquity.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io-armhf/lsioarmhf-ubooquity)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io-armhf/job/lsioarmhf-ubooquity/)

Ubooquity is a free, lightweight and easy-to-use home server for your comics and ebooks. Use it to access your files from anywhere, with a tablet, an e-reader, a phone or a computer.

[![ubooquity](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/ubooquity-banner.png)][appurl]

## Usage

```
docker create \
  --name=ubooquity \
  -v <path to data>:/config \
  -v <path to books>:/books \
  -v <path to comics>:/comics \
  -v <path to raw files>:/files \
  -e MAXMEM=<maxmem> \
  -e PGID=<gid> -e PUID=<uid>  \
  -p 2202:2202 \
  lsioarmhf/ubooquity
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`



* `-p 2202` - the port(s)
* `-v /config` - Config files and database for ubooquity
* `-v /books` - Location of books.
* `-v /comics` - Location of comics.
* `-v /files` - Location of raw files.
* `-e MAXMEM` - to set the maximum memory - see below for explanation
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it ubooquity /bin/bash`.

### MAXMEM

The quantity of memory allocated to Ubooquity depends on the hardware your are running it on. If this quantity is too small, you might sometime saturate it with when performing memory intensive operations. That’s when you get `java.lang.OutOfMemoryError:` Java heap space errors.

You can explicitly set the amount of memory Ubooquity is allowed to use (be careful to set a value lower than the actual physical memory of your hardware). 

If no value is set it will default to 512MB.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application
`IMPORTANT... THIS IS THE ARMHF VERSION`

This container will automatically scan your files at startup.

**IMPORTANT**
Access the admin page at `http://<your-ip>:2202/ubooquity/admin/` and set a password. 

Then you can access the webui at `http://<your-ip>:2202/ubooquity/`


## Info

* Shell access whilst the container is running: `docker exec -it ubooquity /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f ubooquity`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' ubooquity`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/ubooquity`

## Versions

+ **08.04.17:** Switch to java from 3.5 repo, fixes login crashes.
+ **04.02.17:** Rebase to alpine 3.5.
+ **06.12.16:** Initial Release.
