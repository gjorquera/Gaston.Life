---
title: "Hugo: From Zero to One"
category: other
tags:
- hugo
- static-site
description: >
  A (non magical) quickstart guide to build a personal website from scratch
  using Hugo.
aliases:
- /articles/hugo
- /articles/hugo-from-zero-to-one
- /publications/hugo
- /publications/hugo-from-zero-to-one
date: 2019-12-14
---

## What is Hugo?

[Hugo](https://gohugo.io/) is a CLI tool to build and maintain static HTML sites
from source code. With Hugo you define the structure and style of the site once
so that you can focus on the content. After you have an initial structure,
style, and content, Hugo will be able to automatically include partials and
layouts into pages, generate sitemaps, and build lists based completely on the
content you write.

Hugo builds the following static HTML files:

```plain
├── articles
│   ├── first
│   │   └── index.html
│   ├── index.html
│   └── index.xml
├── index.html
├── index.xml
├── now
│   └── index.html
└── sitemap.xml
```

From the following source code:

```plain
├── config.toml
├── content
│   ├── _index.md
│   ├── articles
│   │   ├── _index.md
│   │   └── first.md
│   └── now.md
├── layouts
│   ├── _default
│   │   ├── baseof.html
│   │   ├── list.html
│   │   └── single.html
│   └── index.html
└── script
    ├── bootstrap
    └── server
```

### What About Jekyll?

I used [Jekyll](https://jekyllrb.com/) for a few years but I moved to Hugo for the
following reasons:

* Speed: Jekyll started taking several seconds to build my site which slowed my
  development workflow. With Hugo the refresh happens almost instantly.
* Organization: Jekyll was initially built for date based blog posts and my site
  is closer to a category based knowledge base. Hugo works out of the box for
  this organization style.
* Convenience: Having to install rbenv, Ruby, and Rubygems only to run Jekyll
  was getting very annoying. With Hugo this is enough: `brew install hugo`.

Jekyll does provide more flexibility and the option to extend the backend engine
but I've found out that Hugo's defaults are enough.

### Why Another Guide?

When migrating my site from Jekyll to Hugo I found out that [Hugo's
quickstart](https://gohugo.io/getting-started/quick-start/) is too short and
magical and the complete documentation is too generic.

This guide explains how to build a real world sample personal site from scratch.

## Walking Skeleton

The [GOOS]({{< ref "books/growing-object-oriented-software-guided-by-tests"
>}}) book introduced the concept of a walking skeleton. The absolute minimum
code that can be successfully deployed. In our case, this will be a production
ready, deployed, and ugly "Hello, World!" site.

### Bare bones

#### Install Hugo

Follow [Hugo's quick install](https://gohugo.io/getting-started/installing)
instructions for your platform. I use macOS so I ran:

```plain
$ brew install hugo
$ hugo version
Hugo Static Site Generator v0.61.0/extended darwin/amd64 BuildDate: unknown
```

#### New Site

Create the new, empty site. The site name doesn't have to follow any convention,
it can be anything. In this guide I will use `my-personal-site`:

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

Don't download a theme and don't use `hugo new`. It's too magical.

#### Initialize Git

```plain
$ cd my-personal-site
$ git init
$ git add .
$ git commit -m 'Initial commit'
```

### Scripts

#### Installation

GitHub proposed using a set of [standardized
scripts](https://github.com/github/scripts-to-rule-them-all) to interact with a
codebase. I've found this useful so I'll do the same here. Additionally, I wrote
[helpers](https://github.com/gjorquera/script-helpers) to make writing these
scripts easier, which requires you to ignore the following automatically
generated files:

```plain
$ echo ".cache.*" >> .gitignore
$ echo ".h" >> .gitignore
```

#### Bootstrap

Write this `script/bootstrap` file:

```bash
#!/usr/bin/env bash
cd "$(dirname $0)/.."
[ -f ".h" ] || curl -s -o ".h" -L https://git.io/v14Zc; . ".h"

set -e

title "Checking dependencies..."

ensure "which hugo"
```

#### Server

Write this `script/server` file:

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

Open [http://localhost:1313](http://localhost:1313) to see your new empty site.

#### Commit

```plain
$ git add .
$ git commit -m 'Add scripts to rule them all'
```

### Homepage

Hugo assigns a [content type](https://gohugo.io/content-management/types/) to
every page in your site. There are a few pre-defined content types and new
content types can be defined based on the directory structure. The
[homepage](https://gohugo.io/templates/homepage/) is a special content type with
its own layout different from all other single pages.

#### Content

Write this `content/_index.md` file:

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

#### Baseof

After saving the previous file, Hugo will rebuild the site but still nothing
will show. This is because we have content but no layouts to render the content.

The `baseof` layout is the base of all layouts, where the main HTML5 markup will
live.

Write this `layouts/_default/baseof.html` file:

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

The `baseof` is the layout all pages, we still need the specific layout for the
homepage.

Write this `layouts/index.html` file:

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>
{{ .Content }}
{{ end }}
```

You can now refresh [http://localhost:1313](http://localhost:1313) and you
should see the ugly "Hello, World!".

It is now safe to keep the local webserver running and the tab open because Hugo
will rebuild and refresh everything anytime it detects a change.

#### Commit

```plain
$ git add .
$ git commit -m 'Add hello world homepage and layout'
```

### Deployment Strategy

The last step of the walking skeleton is deploying to live. You can host
personal sites in GitHub (called [GitHub Pages](https://pages.github.com/)) but
to do that the whole codebase has to be the static HTML site.

To achieve this with Hugo, we will follow [Hugo's
guidance](https://gohugo.io/hosting-and-deployment/hosting-on-github/) and build
the static HTML site into a new GitHub user pages repository.

#### Config File

Make sure the configuration file points to your GitHub user pages repository and
has the write title. Update the `config.toml` file:

```toml {hl_lines=[1,3]}
baseURL = "https://<username>.github.io/"
languageCode = "en-us"
title = "My Personal Site"
```

#### Production Codebase

Create a new `<username>.github.io` repository in your GitHub account with a
default README file.

Clone the repository as a submodule into `public`:

```plain
$ git submodule add -b master git@github.com:<username>/<username>.github.io.git public
```

Write this `script/deploy` file:

```bash
#!/usr/bin/env bash
cd "$(dirname $0)/.."
[ -f ".h" ] || curl -s -o ".h" -L https://git.io/v14Zc; . ".h"

set -e

./script/bootstrap

DATE="$(date)"

title "Building site..."

rm -rf public/*
hugo
cd public
git add .
git commit -m "Site build $DATE"

title "Deploying site to GitHub..."

git push origin master
cd ..
git add public
git commit -m "Site build $DATE"
```

#### Commit and Deploy

```plain
$ git add .
$ git commit -m 'Add deploy script'
$ ./script/deploy
```

Watch your site get built and pushed into your GitHub Pages repository.

You can now go to `https://<username>.github.io` and see your brand new site.

## Main Content

### Single Pages

Single pages are pages without a collection or a page within a collection. For
example, if you want to show one article, or one blog post, or a contact page,
all of those are single pages.

#### NowNowNow

[NowNowNow](https://nownownow.com/) is an initiative by Derek Sivers for sites
with a `/now` page. Let's add ours.

Write this `content/now.md` file:

```md
---
title: What I'm Doing Right Now
description: |
  A nownownow page.
---

_(This is a [now](http://nownownow.com/about) page.)_

## Hugo

Learning how to build a personal site with Hugo.
```

#### Layout

Even though we have a homepage layout, we still don't have a layout for single
pages.

Write this `layouts/_default/single.html`:

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>

{{ .Content }}

{{ end }}
```

You can now visit [http://localhost:1313/now](http://localhost:1313/now).

#### Menu

Now we need a way to click our way through the site, to go from the homepage to
the now page.

Update the `layouts/_default/baseof.html` file:

```html {hl_lines=["8-13"]}
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>{{ .Title }}</title>
  </head>
  <body>
    <nav>
      <ul>
        <li><a class="navbar-item" href="{{ "/" | absURL }}">Home</a></li>
        <li><a class="navbar-item" href="{{ "now/" | absURL }}">Now</a></li>
      </ul>
    </nav>

    {{ block "main" . }}{{ end }}
  </body>
</html>
```

#### Commit and Deploy

```plain
$ git add .
$ git commit -m 'Add a now single page and layout'
$ ./script/deploy
```

### Page Collections

You can define different types of [content
types](https://gohugo.io/content-management/types/) by following a directory
structure convention inside of `content/`:

```plain
content/
├── type1/           < type1 content type
│   ├── _index.md    < list page
│   ├── content11.md < single page of type type1
│   └── content12.md < single page of type type1
├── type2/           < type2 content type
│   ├── _index.md    < list page
│   ├── content21.md < single page of type type1
└── _index.md        < homepage
```

#### Articles

Our site will have articles, which are grouped by categories and whose published
date are not important.

Write this `content/articles/_index.md` file:

```md
---
title: Articles
description: >
  All the articles since the beginning of time.
---
```

And this `content/articles/first.md` file:

```md
---
title: First Article
description: >
  The first article I've ever written.
category:
- meta
---

Introductory paragraph.

## Interesting Title

Some more interesting stuff.
```

#### List Layout

You can now access to
[http://localhost:1313/articles/first](http://localhost:1313/articles/first) but
[http://localhost:1313/articles](http://localhost:1313/articles) is blank
because we don't have a list layout yet.

Write this `layouts/_default/list.html` file:

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>
<p>{{ .Description }}</p>

{{ .Content }}

<ul>
  {{ range .Pages }}
  <li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
  {{ end }}
</ul>

{{ end }}
```

#### Menu

We need to add the new articles section to the menu.

Update the `layouts/_default/baseof.html` file:

```html {hl_lines=[11]}
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>{{ .Title }}</title>
  </head>
  <body>
    <nav>
      <ul>
        <li><a class="navbar-item" href="{{ "/" | absURL }}">Home</a></li>
        <li><a class="navbar-item" href="{{ "articles/" | absURL }}">Articles</a></li>
        <li><a class="navbar-item" href="{{ "now/" | absURL }}">Now</a></li>
      </ul>
    </nav>

    {{ block "main" . }}{{ end }}
  </body>
</html>
```

#### Commit and Deploy

```plain
$ git add .
$ git commit -m 'Add an article and a list layout'
$ ./script/deploy
```

### Not Found Page

GitHub Pages supports customizing the "not found" page.

Write this `content/404.md` file:

```md
---
title: Not Found
description: |
  Page not found.
url: /404.html
---

The page you're looking for does not exist.
```

#### Commit and Deploy

```plain
$ git add .
$ git commit -m 'Add a 404 page'
$ ./script/deploy
```

## Next Steps

We now have a fully functional site where you can:

* Add single pages.
* Add new articles.

But Hugo has many additional features. I encourage you to dive deep into the
[official documentation](https://gohugo.io/documentation/).
