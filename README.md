# [![Icons](https://skillicons.dev/icons?i=ruby)](https://skillicons.dev)  Blog using Jekyll 

Jekyll allows us to **generate static sites** and write content in markdown.
The content can be stored in Github itself, and hosted freely via Github Pages.
Jekyll uses *Liquid* templating language for creating custom layouts and *includes*.

### Contents

- [Requirements](#requirements)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Front Matter](#front-matter)
- [Posts](#posts)
- [Pages](#pages)
- [Permalinks](#permalinks)
- [Default Front Matter](#default-front-matter)
- [Themes](#themes)

### Requirements
- Ruby 
  ```bash
  sudo apt-get install ruby-full
  ```
- Ruby Gems (Package Manager for Ruby) 
  ```bash
  gem -v
  ````
- Jekyll 
  ```bash
  gem install jekyll bundler
  jekyll -v
  ```
### Getting Started

Create a new project:
```bash
jekyll new my_blog
```
Move into the project directory, and install missing gems:
```bash
cd my_blog
bundle install
```
Serve the website:
```bash
bundle exec jekyll serve --livereload
```
The site is now live at http://127.0.0.1:4000/. 
> After the first serve, just do `jekyll serve`

### Project Structure

- `_posts`: Blog posts
- `_site`: Final product generated by Jekyll
- `_config.yml`: Settings for the website

### Front Matter

The front matter is a `YAML/JSON` block that provides metadata for a specific page or post.
It is placed at the beginning of the file between two sets of three dashes (`---`). 
```yaml
---
layout: post
title: My First Blog Post
date: 2022-01-01
author: John Doe 
categories:
  - Jekyll
  - Markdown
---
```
The URL of the page also depends on the front matter.
Custom variables (like 'author' above) can be accessed from the layouts.

### Posts

To add a post, create a new file somewhere inside `_posts`.
Jekyll requires blog post files to be named according to the following format:

`YYYY-MM-DD-title.markdown`

- Common formats for posts include markdown and html.

Include the `layout: "post"` front matter, or a custom layout.

By default, the title and date is generated from the filename.

The blog posts can be organized into different folders in the `_posts` directory, without causing any problems to the site.

> Draft posts can be kept inside `_drafts`.
> To see the drafts, serve the projects through
  ```
  jekyll serve --draft
  ```
> Drafts does not have to follow the naming convention.

### Pages

Pages like the `about.markdown` can be included by simply creating a markdown file on the root of the project.

### Permalinks

To make the URL independent of the front matter, we need to use permanent links for the posts.
```yml
permalink: "/url-for-this-post/"
```
We can use variables in the link:
```yml
permalink: "/:title"
```

### Default Front Matter

We can define default front matter in the `_config.yml`:

```yml
defaults:
  - 
    scope:
      path: "" # Path to the files in which the defaults will be applied
      type: "post"
    values:
      layout: "post" # Default layout for the files in the above path
```

### Themes

The default theme is `minima`. More themes can be found at [rubygems.org](https://rubygems.org).

To change theme, just add a new line to the `Gemfile` and install
```Gemfile
gem "jekyll-theme-hacker"
```
```bash
bundle install
```
Finally, update the theme in `_config.yml` and restart the server via `bundle exec jekyll serve`.

Make sure the *layouts* in the front matter are available for the current theme.

### Layouts

Each theme defines some layouts. 
We can override / create custom layouts by creating a new folder `_layouts`.
Inside this, add new layouts as `mylayout.html`.

To grab the content of the posts, use the *Liquid* templating language:
```html
{{ content }}
```
We can assign hierarchical layouts, for example, a general layout followed by all the pages, and specific layouts for each category of posts.

This is done by creating the general `layout.html`, and then using it as the `layout` front matter in another layout. 

---
References: 
- [Giraffe Academy's Playlist](https://youtube.com/playlist?list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&si=d5X-u4ORJgHZ558h)
- [Chad Baldwin's Blog](https://chadbaldwin.net/2021/03/14/how-to-build-a-sql-blog.html)