# Beautify.me Blog

A minimal blog built with [Jekyll](https://jekyllrb.com/docs/) and the
[Minimal Mistakes remote theme](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/),
pinned to version **4.28.1** and hosted on [GitHub Pages](https://docs.github.com/en/pages/quickstart).

Initial site URL: **https://app-beautify-me.github.io/**. The intended custom
domain, `blog.beautify.me`, can be configured after the initial deployment is
verified.

## Prerequisites

- Ruby **3.3.x**, matching [GitHub Pages' Jekyll builder](https://github.com/actions/jekyll-build-pages/blob/main/Dockerfile).
  `.ruby-version` selects **3.3.12** for version managers such as
  [rbenv](https://github.com/rbenv/rbenv#installation).
- Bundler, included with Ruby 3.3.
- Git and a compiler toolchain for native gem dependencies.
- Internet access for RubyGems and the remote theme download.

The Gemfile selects Ruby 3.3 and the GitHub Pages 232 dependency set. This avoids
silently resolving an older Pages release to satisfy incompatible dependencies.

On Fedora, install the build tools and libraries, then select Ruby 3.3 using
your Ruby version manager. If needed, install
[rbenv](https://github.com/rbenv/rbenv#installation) and its
[ruby-build plugin](https://github.com/rbenv/ruby-build#installation), initialize
rbenv for Bash, and open a new terminal before continuing:

```sh
sudo dnf install gcc gcc-c++ make openssl-devel libyaml-devel zlib-devel readline-devel
sudo dnf install rbenv

rbenv install --skip-existing

# With rbenv already installed, run from this repository:

rbenv local 3.3.12
eval "$(rbenv init -)"
ruby -v
bundle --version
```

## Install dependencies

Install dependencies inside this checkout:

```sh
bundle config set --local path vendor/bundle
bundle install
```

Commit `Gemfile.lock` when dependency versions change. `.bundle/`,
`vendor/`, Jekyll caches, and `_site/` are ignored by Git.

### If `bundle install` fails with Ruby 4

GitHub Pages 232 depends on `commonmarker` 0.x, which requires Ruby below 4.0.
Changing the Gemfile to allow Ruby 4 cannot resolve that dependency conflict.
Keep the Gemfile on Ruby 3.3 and activate **3.3.12** before running Bundler.
The `.ruby-version` file only selects Ruby when a version manager is installed
and initialized in your shell; it does not change the system Ruby by itself.

With rbenv installed, initialize it and check both executables:

```sh
eval "$(rbenv init - bash)"
rbenv install --skip-existing
rbenv local 3.3.12
ruby -v
command -v ruby
command -v bundle
bundle install
```

`ruby -v` should report 3.3.12, and both executables should come from rbenv's
shims. If they still resolve to `/usr/bin/ruby` or a Bundler executable in
`~/bin`, check your shell's `PATH` order and initialize rbenv again.

## Preview locally

```sh
bundle exec jekyll serve
```

Open **http://127.0.0.1:4000/**. The server watches content changes; restart it
after editing `_config.yml`. Press `Ctrl+C` to stop it.

## Verify before publishing

```sh
JEKYLL_ENV=production bundle exec jekyll build --strict_front_matter
bundle exec jekyll doctor
```

Check the homepage, `/welcome/`, `/about/`, and `/404.html` in the local preview.
Confirm that Home/About navigation and the Feed link work, the mobile menu
opens, and a nonexistent path displays the custom 404 page. Inspect `_site/`
to ensure README, Gemfile, Gemfile.lock, and development files were excluded.

## Add a post or page

Create a post in `_posts/` using a filename of `YYYY-MM-DD-title.md`:

```markdown
---
title: "My next post"
excerpt: "A short summary for the homepage."
---

Write the post here.
```

Use the current date or an earlier date; future posts are not published by
default. Posts use the theme's `single` layout and appear on the homepage.

Add standalone pages in `_pages/` with a `title` and `permalink` in their front
matter. Update `_data/navigation.yml` to add a navigation link. Use Jekyll's
`relative_url` filter for internal links, for example:

```liquid
[About]({{ '/about/' | relative_url }})
```

The initial site includes a homepage, one welcome post, an About page, and a
custom 404 page. Pagination is configured for five posts per page. Analytics,
comments, search, social sharing, and author profiles are disabled.

## GitHub Pages deployment

This repository assumes GitHub Pages is configured to publish from **`main`**
at **`/ (root)`** using **Deploy from a branch**. Confirm the existing source
under **Settings → Pages → Build and deployment**.

After local verification, commit and push the source changes to `main`.
GitHub Pages builds Jekyll and publishes the result automatically. Check the
Pages build/deployment result in GitHub, then verify the live pages and HTTPS.
Publication can take up to ten minutes. Generated `_site/` files are not
committed.

### Custom domain later

To use `blog.beautify.me`, follow GitHub's
[custom-domain instructions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site):

1. Verify domain ownership, then add the custom domain in Pages settings.
2. Point its DNS CNAME record to `app-beautify-me.github.io`.
3. Change `url` in `_config.yml` to `https://blog.beautify.me`, keeping `baseurl: ""`.
4. Enable HTTPS once the certificate is available and verify the live site.

## Theme updates and attribution

Change the pinned `remote_theme` version in `_config.yml` deliberately and
rebuild before publishing. The English interface labels in `_data/ui-text.yml`
come from Minimal Mistakes 4.28.1; their MIT license is retained in
`LICENSE.minimal-mistakes`.
