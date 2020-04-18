---
layout: article-layout.njk
title: Running a private blog using Eleventy and Docker
date: 2020-04-17
tags: post
---
##### Published on {{ date | date : '%Y-%m-%d' }}
#### {{ title }}
###### `Eleventy`, `Docker`

Create a `Dockerfile` with one single line
```
FROM nginx:alpine
```

Then build the image using the command
```
$ docker build -t my-private-blog .
```

Now run with the following command
```
$ docker run -d -p 8000:80 -v c:/Workspace/Non-Obex/my-private-blog/_site:/usr/share/nginx/html my-private-blog
```

Let's explain each of the option:

* `8000` is the port of the host machine. So later we will point the browser to http://localhost:8000
* `80` is the port that `nginx` runs on
* `c:/.../_site` is the folder where `Eleventy` generates the static `HTML` content.
* `/usr/share/nginx/html` is the directory where `nginx` looks for the index file.

If we need to run `sh` shell:
```
$ docker post
```
the look for the name in the output. It's usually 2 random words concat by an underscore. For example it might be something like `hungry_shamir`.

Then run

```
$ docker exec -it hungry_shamir /bin/sh
```


