---
title: "Setting Up Hugo on GitHub Pages"
date: 2018-06-04T22:39:12+05:30
draft: false
slug: ""
tags: ["Hugo"]
categories: []
type: "post"
---
Recently I started learning [GoLang](https://golang.org/) and came across [Hugo](https://gohugo.io/) which is an open source and the fastest static site generator. Before migrating to Hugo this blog was on self-hosted [Ghost](https://ghost.org/) on [DigitalOcean](https://m.do.co/c/06ee52cfc556). Ghost is a minimal and straightforward blogging platform which is more into WordPress, Medium or Tumblr league because it comes with a really beautiful admin panel.

For shell lovers [Jekyll](https://jekyllrb.com/), [Hugo](https://gohugo.io/), [Hexo](https://hexo.io/) and many other static site generators are no-nonsense tools for quickly spinning up "portfolio-cum-personal-blog" type websites. For now, I am quite convinced with Hugo and will continue using it until I find a better alternative.

### Why GitHub Pages?
Because, it's free. [GitHub Pages](https://pages.github.com/)  allows you to host your websites and project's websites with few other freebies:

- Custom domain with SSL
- Travis CI

### Setup Repositories
Create two repositories, `username.github.io` for website and `awesome-hugo-project` for Hugo source. Repository name for Hugo source can be anything. Whatever floats your boat!

### Install Hugo
```brew install hugo```

You may follow official [installation guide](https://gohugo.io/getting-started/installing/) for other operating systems.

### Create Hugo project
- Create a new Hugo site
```
hugo new site awesome-hugo-project
```
- Setup git for `awesome-hugo-project`, install any [Hugo theme](https://themes.gohugo.io/) of your choice , create first post and push your project to GitHub.

```
### Configure git
git init
git config user.name "Darth Vader"
git config user.email "darthvader@darkside.com"
git remote add origin https://github.com/{user}/awesome-hugo-project.git

### Install theme
git submodule add https://github.com/<repo>/<theme>.git themes/theme
echo 'theme = "theme"' >> config.toml

# Create first post
hugo new posts/hello-world.md

# Push to GitHub
git add .
git commit -m "Initial commit"
git push -u origin master
```
- Generate static files in website's repo.

```
# Navigate to Hugo project
cd ~/Projects/awesome-hugo-project

# Generate static files
hugo -d ~/Projects/username.github.io
```

- Setup git for `username.github.io` and publish your static site.

```
# Configure git
git init
git config user.name "Darth Vader"
git config user.email "darthvader@darkside.com"
git remote add origin https://github.com/{user}/username.github.io.git

# Push to GitHub
git add .
git commit -m "Initial commit"
git push -u origin master
```

Your site is now available at [http://username.github.io](http://username.github.io).

### Configure custom domain
- Delete previous A records for '@' or 'example.com' in DNS configuration and point them to `185.199.108.153`, `185.199.109.153`, `185.199.110.153` and `185.199.111.153`.
- On project's setting page of username.github.io, add your custom domain e.g. [example.com](https://2km.co) and click "Save".

In few minutes or 48 hours your site will be available on your custom domain.

### References
- [Hugo quick start guide](https://gohugo.io/getting-started/quick-start/)
- [Hugo configurations](https://gohugo.io/getting-started/configuration/)
- [Deployment on gh-pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/#github-project-pages)
- [Using custom domains with GitHub](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)
