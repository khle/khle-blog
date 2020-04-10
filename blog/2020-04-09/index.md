---
layout: article-layout.njk
title: Building an Eleventy site with PaperCSS
date: 2020-04-09
tags: post
---
##### Published on {{ date | date : '%Y-%m-%d' }}
#### {{ title }}
###### `Eleventy`, `PaperCSS`

There are many awesome static site generators and CSS frameworks. But none impresses me more than [Eleventy](https://www.11ty.dev) and [PaperCSS](https://www.getpapercss.com). They are both simple and elegant, and together seem like a perfect combination.

In just a few hours, even I could get this site up and running. This is my primary goal as I don't want to spend too much time on making a "blog website".

To build one like this, just follow the following steps.

From your Terminal or Console, install Eleventy:
```
$ npm install -g @11ty/eleventy
```

For my site, I want to have 3 areas: `blog`, `about this site` and `about me`. So I'll make 3 folders:
```
$ mkdir blog
$ mkdir about-site
$ mkdir about-me
```

For this site, I decide to have 2 layouts. So let's create a folder and the layout files.
```
$ mkdir _includes
$ touch _includes/layout.njk
$ touch _includes/article-layout.njk
```

The main page which lists all the articles and the 2 "about" pages should follow 1 layout. The article pages should follow the other. These 2 layouts are similar as I could have get by with just one. You might not need both. Anyway, here's a link to the full content of
[layout.njk](https://gist.github.com/khle/c0e2bda3a3b4b0bbc9e6e0469dd3cfcb).

Now comes the fun part which is we get to write with `markdown` syntax. Let's create the `about me` page.
```
$ touch about-me/index.md
```

and copy the following content
```
---
layout: layout.njk
pageTitle: About me
---

<article class="article">
  <h1 class="article-title">About me</h1>
  <p class="text-lead">Hi, put your intro here</p>
  <p>Lorem...</p>
</article>
```

Next let's create the main page which is the page that lists all articles:
```
touch index.md
```

and copy the following content to it.

<script src="https://gist.github.com/khle/0036de9bdd94c18ab947f4737c35653b.js"></script>

The first line above allows my articles to be listed in reverse chronological order.

Finally, to organize my blog articles in the file system, I keep them in sub-folders under the `blog` folder that was created earlier.
```
$ cd blog
$ mkdir 2020-04-09
$ cd ..
$ touch blog/2020-04-09/index.md
```

and start writing
```
---
layout: article-layout.njk
title: Building an Eleventy site with PaperCSS
date: 2020-04-09
tags: post
---
##### Published on {{ date | date : '%Y-%m-%d' }}
#### {{ title }}
###### `Eleventy`, `PaperCSS`
Lorem...
```
---
Some tips:
1. Table
You can use [markdown extended syntax](https://www.markdownguide.org/extended-syntax/) to embed tables in your content. There is this [handy tool](https://www.tablesgenerator.com/markdown_tables) to help out.

2. If you need to have buttons to perform some actions we can use the following snippets:
```
<div class="row">
    <button class="article-meta"><a href="#">Do something</a></button>
    <button class="article-meta">Do something else</button>
  </div>
```
 which results in 

<div class="row">
    <button class="article-meta"><a href="#">Do something</a></button>
    <button class="article-meta">Do something else</button>
</div>







