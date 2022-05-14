---
title: "Cloudflare Pages + Hugo"
date: 2022-05-08T16:16:09-05:00
Summary: This is my new favorite combo -- also how this website is built upon.
showtoc: true
tags:
- Tutorial
- Cloudflare
- Hugo
---

This is my new favorite combo -- also how this website is built upon. I am using [Hugo][hugo] and [PaperMod][papermod] (a Hugo theme) on [Cloudflare Pages][cf-pages].

**Hugo** is a static site generator written in Go, so it runs much faster than SSGs written in scripting languages like Jekyll and Hexo. **Cloudflare Pages** is a new alternative to [GitHub Pages][gh-pages], where we can take advantage of Cloudflare's global CDN and integrations with other Cloudflare services. It is easy, works great and I wanna share it with you.

## Set up Hugo and PaperMod

First, create an **empty** repo in your GitHub or GitLab account.

Below is a script with step-by-step explanations I compiled from the Hugo [Quick Start][quick-start] and PaperMod [installation][install-papermod] guide. Beware of the placeholders in the angle brackets (i.e., `<site-name>`).

```sh
# Install Hugo with Homebrew
brew install hugo

# Start a new Hugo site
hugo new site <site-name> -f yml
cd <site-name>

# Init a Git repo and install the theme as a submodule
git init
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)

# Push code to the GitHub / GitLab you just created
git commit -m "Init"
git remote add origin https://github.com/<username>/<repo>.git
git branch -M main
git push -u origin main
```

## Set up Cloudflare Pages

Go to your [Cloudflare dashboard][cf-dash]. Click `Pages` on the sidebar. Create a project. Link to your GitHub / GitLab account and select the repo you just pushed your code to.

Next step. In the "Build settings", choose `Hugo` for "Framework preset". Since the default Hugo version used by Cloudflare is very low (0.54.0), it may fail to build your site during deployment. You can add an environment variable `HUGO_VERSION` to use a specific version (source: [Cloudflare documentation][cf-doc]).

For the value of the environment variable, just use the version number on your machine. Check it with command `hugo version`, you will get something like this:

```sh
$ hugo version
hugo v0.98.0+extended darwin/amd64 BuildDate=unknown
```

In this case, just set `HUGO_VERSION` to `0.98.0`.

That's all you need for the setup. Wait for Cloudflare to build and deploy your site. Cloudflare will give you a default subdomain name of `pages.dev`, but you can always **bring your own domain name** by adding that domain to your "custom domains" of the Cloudflare Pages project and change your DNS settings accordingly.

## Sprinkle your site with some custom code

Wanna modify the Hugo theme to fit your needs and aesthetics? That's easy and intuitive in Hugo. You don't have to mess with the original theme files -- just create a new file of the same relative path under `/layouts` and Hugo will replace that for ya.

For example, I added a comment system [giscus][giscus] (that's also easy to configure and serverless, btw). Instead of modifying the theme file `/themes/PaperMod/layouts/partials/comments.html`, I created a new file `/layouts/partials/comments.html` and made changes to that.

## What's more

You can preview, retry, or even rollback to your previous deployments!

{{< figure
    src="https://assets.kuang.dev/images/blog/cf-pages.jpg"
    width="500"
    alt="Deployment features of Cloudflare Pages" >}}

**EOF**

[hugo]: https://gohugo.io/
[papermod]: https://github.com/adityatelange/hugo-PaperMod
[cf-pages]: https://pages.cloudflare.com/
[gh-pages]: https://pages.github.com/
[quick-start]: https://gohugo.io/getting-started/quick-start/
[install-papermod]: https://github.com/adityatelange/hugo-PaperMod/wiki/Installation
[cf-dash]: https://dash.cloudflare.com
[cf-doc]: https://developers.cloudflare.com/pages/platform/build-configuration
[giscus]: https://giscus.app/
