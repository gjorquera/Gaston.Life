---
title: "Hugo: From Zero to One"
categories:
- meta
description: >
  A (non magical) quickstart guide to build a personal website from scratch
  using Hugo.
date: 2019-12-14
---

## What is Hugo?

Hugo is a static HTML generator to help you build and maintain complex static
HTML sites from source code. With Hugo you have to define the structure of your
site only once and then it allows you to focus purely on the content.

After you have the structure defined, Hugo will be able to include partials,
build layouts, automatically generate sitemaps, and build index pages based
completely on the content you have authored.

### Features

* Incredibly fast. It takes less than a second to build sites with dozens or
  hundreds of pages.
* Sane directory layout. Everything has a place and there's a place for
  everything, and it makes sense.
* Easy to install. All it needs is one CLI command and that's it.

### Why Another Guide?

I started migrating my site from Jekyll to Hugo and found out that [Hugo's
quickstart](https://gohugo.io/getting-started/quick-start/) is too magical for
me. I wanted to know how each moving part worked so that I could use them to my
benefit.

This guide attempts to explain how the different moving parts work together.

### What About Jekyll?

I used [Jekyll](jekyllrb.com/) for a few years. It opened the doors for me to
the idea of building static HTML sites to host cheaply or for free.

But I moved to Hugo for the same reasons why I think Hugo is a great tool:
speed, organization, and convenience:

* Speed: Jekyll started taking several seconds to build my site which forced me
  to make a change, wait for the backend to rebuild the site, and then refresh
  the page. With Hugo the refresh even happens automatically.
* Organization: Jekyll was initially built to handle blogs with posts based on
  dates and my site is more like articles organized by categories. Hugo works
  out of the box for this organization style.
* Convenience: Having to install Ruby and rbenv and Rubygems only to build my
  static site was getting very annoying. With Hugo I only have to do `brew
  install|upgrade hugo`.

Jekyll does provide more flexibility and the option to extend the backend engine
but I've found out that Hugo's defaults are good enough for me while Jekyll's
defaults weren't.

## Walking Skeleton

The first step will be to go from nothing to a "Hello, World!" deployed and
live without magic.

### Install Hugo

Follow [Hugo's quick install](https://gohugo.io/getting-started/installing)
instructions for your platform.

I use macOS so I ran:

```plain
$ brew install hugo
```

```plain
$ hugo version
Hugo Static Site Generator v0.61.0/extended darwin/amd64 BuildDate: unknown
```

### New Empty Site

Use Hugo to create the minimum necessary files for a new empty site. The name
doesn't have to follow any convention, it can be anything. This guide will use
`my-personal-site`:

```plain
$ hugo new site my-personal-site
Congratulations! Your new Hugo site is created in /Users/gaston/Code/my-personal-site.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```

If you want to understand how Hugo works, then don't download a theme and don't
use `hugo new`.

#### Initialize Git

```plain
$ cd my-personal-site
$ git init
$ git add .
$ git commit -m 'Initial commit'
```

### Scripts

#### Installation

Several years ago GitHub proposed using a set of [standardized
scripts](https://github.com/github/scripts-to-rule-them-all) to interact with a
codebase. I've found this useful so I'll do the same here.

I wrote [script helpers](https://github.com/gjorquera/script-helpers) to make
writing these scripts easier and that's why every script will start with:

```bash
#!/usr/bin/env bash
cd "$(dirname $0)/.."
[ -f ".h" ] || curl -s -o ".h" -L https://git.io/v14Zc; . ".h"
```

To avoid adding script helper files into your codebase, run:

```plain
$ echo ".cache.*" >> .gitignore
$ echo ".h" >> .gitignore
```

#### Bootstrap

Write the following `script/bootstrap` file:

```bash
#!/usr/bin/env bash
cd "$(dirname $0)/.."
[ -f ".h" ] || curl -s -o ".h" -L https://git.io/v14Zc; . ".h"

set -e

title "Checking dependencies..."

ensure "which hugo"
```

#### Server

Now write the following `script/server` file:

```bash
#!/usr/bin/env bash
cd "$(dirname $0)/.."
[ -f ".h" ] || curl -s -o ".h" -L https://git.io/v14Zc; . ".h"

set -e

./script/bootstrap

title "Running local webserver..."

hugo serve -D
```

#### Local Development

You can now launch a local webserver. It will show multiple warnings but you can
ignore them for now:

```plain
$ ./script/server

Checking dependencies...

which hugo... ✓ done

Running local webserver...

Building sites … WARN 2019/12/14 12:23:38 found no layout file for "HTML" for "taxonomyTerm": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN 2019/12/14 12:23:38 found no layout file for "HTML" for "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
WARN 2019/12/14 12:23:38 found no layout file for "HTML" for "taxonomyTerm": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.

                   | EN
+------------------+----+
  Pages            |  3
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  0
  Processed images |  0
  Aliases          |  0
  Sitemaps         |  1
  Cleaned          |  0

Built in 8 ms
Watching for changes in /Users/gaston/Code/my-personal-site/{archetypes,content,data,layouts,static}
Watching for config changes in /Users/gaston/Code/my-personal-site/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Now open http://localhost:1313 to see your brand you empty site.

#### Commit

```plain
$ git add .
$ git commit -m 'Add management scripts'
```

### Homepage

Let's build the home page now.

Hugo assigns a type to every page in your site. There are a few already defined
content types and new content types can be defined based on the directory
structure.

The homepage is a special page (with its layout) different from all other single
pages. This is because homepages usually have a different style.

#### Content

Write the following `content/_index.md` file:

```md
---
title: Hello, World!
description: |
  My personal site.
---

Welcome to my personal site.

## Contact

You can contact me via e-mail but I don't have one yet.
```

More information [about content
types](https://gohugo.io/content-management/types/) and [homepage
template](https://gohugo.io/templates/homepage/).

#### Baseof

After saving the previous file, Hugo will rebuild the site but still nothing
will show. This is because we have content but no layouts to render the content.

The `baseof` layout is the base of all layouts and where the main HTML5 markup
should live.

Write the following `layouts/_default/baseof.html` file:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>{{ .Title }}</title>
  </head>
  <body>
    {{ block "main" . }}{{ end }}
  </body>
</html>
```

#### Layout

The `baseof` layout is not enough. This is the layout for all pages, we now need
the layout for the homepage.

Write the following `layouts/index.html` file:

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>

{{ .Content }}
{{ end }}
```

You can now refresh http://localhost:1313 and you should see an ugly hello
world.

It is safe to keep the local webserver running and the tab open because Hugo
will rebuild and refresh everything anytime it detects a change.

#### Commit

```plain
$ git add .
$ git commit -m 'Add hello world homepage and layout'
```

### Deployment

The last step of the walking skeleton is deploying your brand new static site to
live.

You can host personal sites in GitHub (called [GitHub
Pages](https://pages.github.com/)) but to do that the whole codebase has to be
the static HTML site.

To achieve this with Hugo, we will follow [Hugo's
guidance](https://gohugo.io/hosting-and-deployment/hosting-on-github/) and build
the static HTML site into a GitHub user pages codebase.

#### Production Codebase

Create a new `<username>.github.io` repository in your GitHub account with a
default README file.

Clone the repository as a submodule into `public`:

```plain
$ git submodule add -b master git@github.com:<username>/<username>.github.io.git public"
```

Write the following `script/deploy` file:

```bash
#!/usr/bin/env bash
cd "$(dirname $0)/.."
[ -f ".h" ] || curl -s -o ".h" -L https://git.io/v14Zc; . ".h"

set -e

./script/bootstrap

title "Deploying updates to GitHub..."

rm -rf public/*
hugo
cd public
git add .
DATE="$(date)"
git commit -m "Site build $DATE"
git push origin master
cd ..
git add public
git commit -m "Site build $DATE"
```

Run `./script/deploy` and watch your site get built and pushed into your GitHub
Pages repository.

You can now go to `https://<username>.github.io` and see your brand new site.

## Single Pages

## Page Collections

### Taxonomies

### List Layout

## Last Details

### Sitemap

### Related Content

### Favicon

### Social Tags

### MathJax
