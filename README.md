# Desktop Reports

**Plain-language release reports for Linux desktop environments and distros. Written by a bot, edited by a human.**

This repository holds the **built static site** that GitHub Pages serves at
https://justadreamerfl.github.io/desktop-reports/.

The **source** — the Astro project, Markdown posts, build scripts, and monitor
cron — lives in a separate checkout at
`~/.hermes/projects/linux-release-watch/site/`. This deploy repo is refreshed
from `site/dist/` whenever the deploy script runs (see "How to edit" below).

---

## How to edit

You edit the **Astro source project**, then rebuild and re-deploy. This repo
only contains generated HTML/JSON/XML — do not hand-edit files here, they will
be overwritten.

1. **Author or edit a post** in the Astro project:
   `~/.../linux-release-watch/site/src/content/blog/<slug>.md`

   Match the structure of an existing post. Each post uses this frontmatter:
   ```
   ---
   title: "Title of the post"
   description: "One-sentence summary."
   pubDate: 2026-09-11
   tags: ["kde", "desktop-environment", "release-notes"]
   distro: "KDE Plasma"      # optional
   version: "6.8.0"          # optional
   ---
   ```
   Sections, in order:
   - `## Things worth your attention` (with `###` subsections per area)
   - `## Smaller changes`
   - `## Bug fixes`
   - `## Known regressions` *(only if any)*
   - `## Sources` *(single flowing paragraph of inline links, not a list)*

2. **Run the Humanizer pass** before building — em-dashes, promo words, and
   mechanical bolding are blocked. (See the `humanizer` skill.)

3. **Build and deploy** in one step:
   ```
   ~/.hermes/projects/linux-release-watch/deploy.sh
   ```
   This runs `npm run build` in the Astro project, then `rsync --delete`s
   `site/dist/` into this deploy repo and pushes `main` to GitHub.

4. **Verify** within ~1 minute — GitHub Pages rebuilds automatically:
   ```
   curl -s -o /dev/null -w '%{http_code}\n' \
     https://justadreamerfl.github.io/desktop-reports/blog/<your-slug>/
   ```

Notes:
- `index.html`, `posts.json`, `rss.xml`, `sitemap-*.xml`, and `blog/*/index.html`
  are build **outputs**. Edit the Markdown source instead.
- The `feature-freeze` tag marks pre-release posts (freeze/beta/rc); final
  release posts are separate with a different URL — never overwrite a freeze
  post with its final release.
