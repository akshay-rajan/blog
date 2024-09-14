# [![Icons](https://skillicons.dev/icons?i=ruby)](https://skillicons.dev)  Blog using Jekyll 

Jekyll allows us to **generate static sites** and write content in markdown.
The content can be stored in Github itself, and hosted freely via Github Pages.
Jekyll uses *Liquid* templating language for creating custom layouts and *includes*.

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
bundle exec jekyll serve
```
The site is now live at http://127.0.0.1:4000/. 
> After the first serve, just do `jekyll serve`



---
References: 
- https://chadbaldwin.net/2021/03/14/how-to-build-a-sql-blog.html
- https://youtube.com/playlist?list=PLLAZ4kZ9dFpOPV5C5Ay0pHaa0RJFhcmcB&si=d5X-u4ORJgHZ558h