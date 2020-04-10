---
layout: article-layout.njk
title: Create a Docker Rust playground
date: 2020-04-10
tags: post
---
##### Published on {{ date | date : '%Y-%m-%d' }}
#### {{ title }}
###### `Rust`, `Docker`

As I start to learn Rust, in addition to the Rust playground, I want to create something more local. Docker seems like a good choice, but I don't want [1.8 GB image](https://hub.docker.com/_/rust).  

I found this [Docker file](https://github.com/justincormack/docker-alpine-rust/blob/master/Dockerfile) (thanks [Justin Cormack](https://github.com/justincormack)) which uses Alpine and what it creates is exactly what I need.


First, I create a file called `Dockerfile` with the following content:
```
FROM alpine:edge
RUN echo http:"//dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
  apk update && apk upgrade && apk add \
  gcc musl-dev rust cargo
```

Once I have a Docker file, the next step is to build the image
```
$ docker build --tag alpinerust .
```

To verify the image has been built:
```
$ docker images
```

Now I can run it:
```
$ docker run --rm -it alpine /bin/sh
```

Now I get a prompt which indicates I'm inside a Linux Alpine machine. To verify that I can run Rust, type
```
# cargo
```

All is good. When done, simply type `exit` at the Linux prompt. To run again repeat

```
$ docker run --rm -it alpine /bin/sh
# cargo
# exit
```
