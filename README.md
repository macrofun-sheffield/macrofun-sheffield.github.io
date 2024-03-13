


# How to?

## Add a post

 1. Write your post in simple markdown
 2. Add the following header:
```
---
author: theauthor
layout: page
title:  "My title"
teaser: "Brief summary"
categories:
    - session
tags:
    - tag1
    - tag2
---
```

 * `author`: you can leave this blank or add your author tag (e.g. `tguillerme` for me). See `Add an author` section below to add yourself.
 * `layout`: leave as `page`
 * `title`: keep it short
 * `teaser`: a brief summary that will appear below the title in the posts list ()
 * `categories`: leave as `session`
 * `tag`: can be any search key words. For example use `coding`, `tutorial`, `discussion` if it's any of these categories and then whatever you think is relevant (e.g. `R`, etc...)

 3. Save it as `YYYY-MM-DD-my_post.md` in `_posts/`
 4. Done.

## Add an author

You can add yourself as an author by editing the `_data/authors.yml` file.
Simply add yourself in the list as:

```
myname:
  name: "My Name"
  siterole: "author"
  uri: ""
  email: ""
```

You can fill the relevant bits like `uri` that redirects to your website or `email`.

## Add a picture

Simply store the picture in `images/` and call it from your post as `![Image description]({{ site.baseurl }}/images/image_file_name)`.

## Force refresh pages after a git push

If the pages are not deployed correctly after adding a post, you can look at the potential error message in the github action: https://github.com/macrofun-sheffield/macrofun-sheffield.github.io/actions/workflows/jekyll.yml or/and force to rerun it by clicking Run workflow > Run workflow on the `Deploy Jekyll site to Pages` workflow.