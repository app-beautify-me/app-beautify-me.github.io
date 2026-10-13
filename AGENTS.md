# Repository Guidelines

## Project Structure & Module Organization

- `index.html` is the homepage using the theme's `home` layout.
- `_posts/` contains dated blog posts; `_pages/` contains About and 404 pages.
- `_data/navigation.yml` defines navigation; `_data/ui-text.yml` supplies English labels.
- `_config.yml` defines site settings, plugins, layouts, and publishing exclusions.
- `Gemfile`, `Gemfile.lock`, and `.ruby-version` define the Ruby dependencies.
- Layouts, styles, and JavaScript come from the pinned remote theme. Generated
  assets appear in `_site/assets/`; keep `_site/`, caches, and `vendor/` untracked.

## Build, Test, and Development Commands

Use Ruby 3.3.x; `.ruby-version` selects 3.3.12.

```sh
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve
JEKYLL_ENV=production bundle exec jekyll build --strict_front_matter
bundle exec jekyll doctor
```

The first two commands install checkout-local dependencies. `serve` previews
the site at `http://127.0.0.1:4000/`; restart after configuration changes.
`build` generates production output, and `doctor` checks configuration issues.

## Coding Style & Naming Conventions

Use two-space YAML indentation and Markdown with YAML front matter. Name posts
`YYYY-MM-DD-kebab-case-title.md`, using current or earlier dates. Give standalone
pages explicit `title` and `permalink` values. Use Liquid's `relative_url` filter
for internal links, for example `{{ '/about/' | relative_url }}`.

VS Code recommends Prettier and formats on save. No repository-wide formatter
or lint command is configured. Preserve theme defaults and minimize overrides.

## Testing Guidelines

No automated test suite, test naming convention, or coverage target is configured.
For site changes, run the production build and `doctor`. Check `/`, `/welcome/`,
`/about/`, the feed, and a nonexistent path. Verify desktop/mobile navigation,
pagination when affected, and exclusion of development files from `_site/`.

## Commit & Pull Request Guidelines

History uses short descriptive subjects, such as `Create minimal Beautify.me
blog with Minimal Mistakes`. Use concise imperative summaries. PRs should explain
the change, link relevant issues, report validation, and include screenshots for
visible changes. Merging into `main` triggers the existing GitHub Pages deployment.

## Security & Configuration Tips

Favor simple, evidence-backed changes. Never commit secrets or private data.
Keep analytics and comments disabled unless requested. Update pinned dependencies
deliberately, commit lockfile changes, and verify builds. Preserve the theme's MIT
license. Keep `AGENTS.md` excluded from publication; coordinate custom-domain
changes with Pages settings and DNS, following the README.
