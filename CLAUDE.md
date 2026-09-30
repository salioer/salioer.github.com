# CLAUDE.md

Guidance for AI assistants working in this repository.

## What this repository is

This is the **generated output** of an [Octopress](http://octopress.org) (Jekyll 2.x-era) blog,
"Salioer's Dev Blog" (subtitle: 记录学习和生活, "recording study and life"), published with
GitHub Pages as a user site at `http://salioer.github.io`.

Key facts:

- **Only compiled static files live here.** There are no Markdown posts, `_config.yml`, `Rakefile`,
  `Gemfile`, `source/` or `sass/` directories. The Octopress source tree (normally kept on a
  `source` branch or in a separate checkout) is **not** in this repository; the only remote branch
  besides feature branches is `master`.
- Every commit on `master` except the first was produced by Octopress's `rake deploy`, which
  commits with the message `Site updated at <UTC timestamp>`.
- The site currently has **no published posts**: `index.html`, `blog/archives/index.html`, and
  `atom.xml` render empty post lists.
- The last content update was June 2013. The site is effectively dormant.

## Directory layout

```
/
├── index.html                 # Home page (blog index, empty post list + sidebar)
├── blog/archives/index.html   # Archives page
├── atom.xml                   # Atom feed (no entries)
├── sitemap.xml                # Sitemap for /, /blog/archives/, /robots.txt
├── robots.txt                 # Allows all crawlers, points to sitemap
├── favicon.png
├── stylesheets/screen.css     # Compiled Octopress classic theme CSS (from Sass; do not hand-format)
├── javascripts/
│   ├── octopress.js           # Theme behaviour (mobile nav, code blocks, etc.)
│   ├── modernizr-2.0.js
│   ├── github.js              # GitHub repos sidebar widget (unused: no aside configured)
│   ├── pinboard.js            # Pinboard sidebar widget (unused)
│   ├── twitter.js             # Twitter sidebar widget (unused)
│   └── libs/                  # jquery.min.js, jXHR.js, swfobject-dynamic.js
├── images/                    # Theme images (backgrounds, icons, rss/search/email icons)
└── assets/jwplayer/           # JW Player (player.swf + "glow" skin) bundled with Octopress
```

## Development workflow

### Preferred: regenerate from Octopress source

Because these files are build artifacts, the correct way to change the site is:

1. Work in the Octopress source checkout (not present here), e.g. `rake new_post["title"]`,
   edit `source/_posts/*.markdown`, adjust `_config.yml` / `sass/`.
2. `rake generate` then `rake preview` to check locally (http://localhost:4000).
3. `rake deploy`, which copies `public/` into `_deploy/` (a clone of this repo's `master`) and
   commits "Site updated at ...".

If the user asks for content changes and the source is unavailable, say so and ask whether to
(a) hand-edit the generated HTML here, or (b) migrate the site to a maintained generator.

### Hand-editing generated files (when asked to)

- Keep edits consistent across all generated pages: the `<head>`, header, navigation, sidebar
  and footer markup is duplicated in `index.html` and `blog/archives/index.html`.
- If you add or remove a page or post, also update `atom.xml` and `sitemap.xml` by hand.
- Use root-relative URLs (`/stylesheets/screen.css`, `/blog/archives`), matching existing pages.
- Do not reformat minified/compiled assets (`screen.css`, `jquery.min.js`, etc.).
- Site metadata in use: title `Salioer's Dev Blog`, author `salioer`, canonical URL
  `http://salioer.github.io`, timezone `+08:00`.

### Local preview

No build step is needed to view the committed output. Serve the repo root statically, e.g.:

```sh
python3 -m http.server 8000
```

Then open http://localhost:8000. (Opening `index.html` via `file://` breaks root-relative links.)

### Testing / linting

There are no tests, linters, CI workflows, or package manifests. Verification is manual:
load the pages in a browser and check that links, CSS, and scripts resolve.

## Conventions and caveats

- **Deployment:** GitHub Pages serves the `master` branch root. Anything merged to `master` goes
  live. There is no `.nojekyll`, so Pages runs Jekyll over these files; avoid adding files or
  directories starting with `_` unless you also add `.nojekyll`.
- **Commit messages:** deploy commits use `Site updated at YYYY-MM-DD HH:MM:SS UTC`. Manual changes
  should use a normal descriptive message instead so they are distinguishable from deploys.
- **Legacy/outdated dependencies:** pages load jQuery 1.9.1 from `//ajax.googleapis.com`
  (protocol-relative), and Google Fonts (PT Serif / PT Sans) and Twitter widgets over plain `http://`. On an HTTPS-served site these
  mixed-content requests may be blocked. Fix them only if asked, and fix them in every page.
- The jQuery fallback in `<head>` references `./javascripts/lib/jquery.min.js`, but the file lives
  at `javascripts/libs/jquery.min.js` (pre-existing Octopress bug; the fallback path is broken).
- `assets/jwplayer/player.swf` is Flash and no longer works in modern browsers; it is unused by
  any current page.
- Content language is mixed Chinese/English. Pages declare `lang="en"` and UTF-8; keep files UTF-8.
