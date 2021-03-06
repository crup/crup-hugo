---
title: "Deploy Hugo on Github Pages With Travis CI"
date: 2018-06-07T15:54:58+05:30
draft: false
slug: ""
tags: ["Hugo", "Travis CI", "Continuous Integration"]
categories: []
type: "post"
---
Getting started with Hugo is very interesting especially when you are new to static site generators. Just run `hugo serve` and your markdown files are converted into a website. But, when it comes deployment it's really painful if you're doing it manually and it's more painful when you have to do the whole process all over again just to fix a stupid typo.

In my previous post - [Setting Up Hugo on GitHub Pages]({{< ref "setting-up-hugo-on-github-pages.md" >}}), I talked about
hosting Hugo sites on GitHub pages and one of the reason for doing so was [Travis CI](https://travis-ci.org).

### What is Travis CI
Travis CI is a continuous integration service to build and test projects. Open source projects can be build and tested for free. It means I can build update my website as soon as I do a `git push` on my source repo.

### Initialize Travis
Initializing Travis is pretty straightforward. All you have to do is create a `.travis.yml` file in your project root.

Also, I went through almost every other tutorial on the internet and settled up with this config which is more like a compilation of best bits from all. So please don't complain about plagiarism.

Here's my current .travis.yml for this website. You can always check the latest file in [repository](https://github.com/crup/crup-hugo/blob/master/.travis.yml).

```
language: go

go: master

branches:
  only:
    - master

before_install:
  - git config --global user.name "${GITHUB_NAME}"
  - git config --global user.email "${GITHUB_EMAIL}"

install:
- wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_${HUGO_VERSION}_Linux-64bit.deb
- sudo dpkg -i hugo*.deb

before_script:
  - git clone "https://github.com/${SOURCE_REPO}"  ${SOURCE_FOLDER}
  - git clone "https://github.com/${WEBSITE_REPO}" ${WEBSITE_FOLDER}

script:
  - cd ${SOURCE_FOLDER}
  - hugo -d ${WEBSITE_FOLDER} --cleanDestinationDir

after_success:
  - cd ${WEBSITE_FOLDER}
  - cp ${SOURCE_FOLDER}/README.md README.md && cp ${SOURCE_FOLDER}/CNAME CNAME
  - git add .
  - git commit -m "Generated by Travis CI [Build#${TRAVIS_BUILD_NUMBER}]"
  - git push --quiet "https://${GITHUB_TOKEN}@github.com/${WEBSITE_REPO}" ${TARGET_BRANCH}

env:
  global:
    - TARGET_BRANCH=master
    - SOURCE_FOLDER=~/crup-source
    - WEBSITE_FOLDER=~/crup-hugo
```
As soon as you're done adding `.travis.yml` in your project root, connect your GitHub account with [Travis CI](https://travis-ci.org) and turn on build for your project repo.

### Travis' environment variables
You need to configure some environment variables to rum your build. Some of them need to to be configured in Travis' settings and some on `.travis.yml` itself.

- GITHUB_NAME: `Jon Snow`
- GITHUB_EMAIL: `aegon@targaryen.co.north`
- SOURCE_REPO: `crup/crup-hugo.git`
- SOURCE_FOLDER: `~/crup-source`
- WEBSITE_REPO: `crup/crup.github.io.git`
- WEBSITE_FOLDER: `~/crup-hugo`
- GITHUB_TOKEN: `somesecrecttextonlygillyandsamwellcandecode`
- TARGET_BRANCH: `master`
- HUGO_VERSION: `0.41`

Create a personal access token for `GITHUB_TOKEN` by going [here](https://github.com/settings/tokens) and check `repo:status` and `public_repo`.

> NEVER configure `GITHUB_TOKEN` in `.travis.yml`.

### Final thoughts
- Though Travis already creates a copy of source repo, yet I decided to create another copy in home directory `~/crup-source` to work on a copy created by me. :stuck_out_tongue_closed_eyes:
- Yes, just writing `hugo` generates site for you but `--cleanDestinationDir` parameter is really important if you don't want to `rm -rf` public content every time and clean all obsolete urls.
- Why `cp ${SOURCE_FOLDER}/README.md README.md`? Why not. Maybe, I'll add something interesting in future.

Hope that helps in updating Hugo sites on the fly - GitHub's web editor. :sweat_smile:
