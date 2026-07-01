# Local Development Guide

This site is built with [Jekyll](https://jekyllrb.com/) and hosted on GitHub Pages. This guide explains how to preview changes locally before pushing.

---

## Prerequisites

- **Homebrew** — macOS package manager ([brew.sh](https://brew.sh))
- **Ruby** (Homebrew version, not the macOS system one)

### Install Homebrew Ruby (one-time)

```bash
brew install ruby
```

Then add it to your shell path so it takes priority over the macOS system Ruby:

```bash
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Verify you have the right one:

```bash
ruby --version   # should show 3.x or 4.x, not 2.6.x
```

### Install Jekyll (one-time, per repo)

From the repo root, install the gems listed in `Gemfile`:

```bash
bundle install
```

This uses `Gemfile` to pin the exact versions of Jekyll and its plugins. The `Gemfile.lock` file records the resolved versions — commit both to git.

---

## Running the local server

```bash
bundle exec jekyll serve --livereload
```

Then open **http://localhost:4000** in your browser.

- `--livereload` — the browser automatically refreshes when you save a file
- Changes to most files (pages, layouts, CSS, posts) are picked up instantly
- Changes to `_config.yml` require restarting the server

To stop the server: `Ctrl+C`

### If you get "command not found: bundle"

Your shell PATH may not include the Homebrew Ruby bin. Run:

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
```

Then retry. To make this permanent, add that line to `~/.zshrc`.

---

## Site structure

```
sidathchandrasena.github.io/
├── _config.yml           # Site-wide settings (title, plugins, nav)
├── _layouts/
│   ├── home.html         # Layout for post-listing pages (e.g. Contrails Australia)
│   └── post.html         # Layout for individual blog posts
├── _includes/
│   ├── head.html         # <head> tag — fonts, CSS, JS loaded here
│   └── navlinks.html     # Prev/next navigation inside posts
├── _posts/               # Blog posts (YYYY-MM-DD-title.md)
├── css/
│   └── override.css      # All custom styles on top of the Minima theme
├── assets/
│   └── images/           # Images used in posts and pages
├── index.md              # Portfolio homepage (two project cards)
├── contrails-australia.md# Contrails project page (hero + post list)
├── about.md              # About page
├── Gemfile               # Ruby gem dependencies
└── Gemfile.lock          # Locked gem versions (commit this)
```

---

## Portfolio structure

The site is set up as a **personal portfolio** with individual project pages.

### Homepage (`index.md`)
Uses `layout: default`. Contains hand-written HTML cards for each project. To add a new project, copy one of the `<a class="project-card">` blocks and update the title, description, and `href`.

### Adding a new project page
1. Create a new `.md` file at the repo root (e.g. `my-project.md`)
2. Use `layout: home` if it should list posts, or `layout: default` for a freeform page
3. Add a matching card in `index.md`

### Controlling the nav bar
The nav is controlled by `header_pages` in `_config.yml`:

```yaml
header_pages:
  - about.md
```

Only files listed here appear in the top navigation. Omitting a page from this list hides it from the nav without removing it from the site.

---

## Deploying

Push to the `main` branch and GitHub Pages builds and deploys automatically (usually within a minute).

```bash
git add .
git commit -m "your message"
git push
```

There is no separate build step — GitHub runs Jekyll on their servers.

---

## Theme

The site uses [Minima](https://github.com/jekyll/minima) (v2.5), Jekyll's default theme. All visual customisations live in `css/override.css` — this file is loaded on every page via `_includes/head.html`. Avoid editing theme files directly; put overrides in `override.css` instead.
