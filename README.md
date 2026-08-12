# Presentations

My collection of Jekyll + [reveal.js](https://revealjs.com/) presentations.

**Live site: [sfarrens.github.io/presentations](https://sfarrens.github.io/presentations/)**

Every push to `main` is built and deployed automatically by [.github/workflows/deploy.yml](.github/workflows/deploy.yml).

- [Repo layout](#repo-layout)
- [Adding a new presentation](#adding-a-new-presentation)
- [Previewing locally](#previewing-locally)
- [Writing slides](#writing-slides)
  - [Layout: title](#layout-title)
  - [Layout: slide](#layout-slide)
  - [Layout: graphic](#layout-graphic)
  - [Plain markdown slides](#plain-markdown-slides)
- [Using a custom theme](#using-a-custom-theme)

## Repo layout

```
.
├── _theme/                          shared reveal.js theme used by most presentations
├── _base_config.yml                 shared Jekyll config, inherited by every presentation
├── <presentation-name>/
│   ├── _config.yml                  title, description, type, author
│   └── _slides/                     one markdown file per slide
├── scripts/
│   ├── new_presentation.sh          scaffold a new presentation
│   └── serve.sh                     preview a presentation locally
└── .github/workflows/deploy.yml     builds & deploys every presentation to GitHub Pages
```

Each top-level directory containing a `_config.yml` is treated as its own presentation and gets its own page at `sfarrens.github.io/presentations/<presentation-name>/`. The root landing page (`index.html`) is regenerated at deploy time from each presentation's `_config.yml`.

## Adding a new presentation

```
./scripts/new_presentation.sh <directory-name> [Talk|Demo|Workshop]
```

This scaffolds `<directory-name>/_config.yml` plus a title slide and a content slide in `<directory-name>/_slides/`. Then:

1. Edit `<directory-name>/_config.yml` (title, description, type)
2. Add/edit slides in `<directory-name>/_slides/`
3. Drop any images in `<directory-name>/graphics/`
4. Preview with `./scripts/serve.sh <directory-name>`
5. Commit and push — the deploy workflow picks it up automatically

## Previewing locally

```
./scripts/serve.sh <directory-name>
```

Requires Ruby + Bundler (`bundle install` once). `reveal.js` is cloned automatically on first run rather than committed to the repo.

## Writing slides

Slides live in `<presentation-name>/_slides/` as one markdown file per slide (`slide_01.md`, `slide_02.md`, …), rendered in filename order. Each slide picks a layout via front matter.

### Layout: title

```markdown
---
layout: title
title: "My Talk"
subtitle: "A subtitle"
event_date: "May 2026"
location: "Paris, France"
---
```

### Layout: slide

Regular content slide. `+` at the start of a line makes that item a fragment (revealed one at a time on click). Optionally set `graphic` to an image path/URL and `align: left` or `align: right` to place it beside the content; omit `align` to use the image as a full-bleed background.

```markdown
---
layout: slide
title: "Slide Title"
align: left
graphic: "graphics/example.png"
---

Regular content.

+ Revealed first
+ Revealed second
```

### Layout: graphic

One or more images, optionally captioned:

```markdown
---
layout: graphic
title: "Results"
graphic:
  - "graphics/plot_1.png"
  - "graphics/plot_2.png"
caption: "Figure 1 vs Figure 2"
---
```

### Plain markdown slides

Slides without a recognised `layout` fall back to reveal.js's own markdown mode, which supports:

- `---` on its own line to split one file into multiple horizontal slides
- `--` on its own line for vertical slides
- `<fragment/>` (or a leading `+`) for fragments
- `<background>color</background>` / `<backgroundimage>url</backgroundimage>` for slide backgrounds
- `Note:` followed by text for speaker notes
- `<mermaid>...</mermaid>` for diagrams (requires `mermaid_diagrams: true` in `_config.yml`)

## Using a custom theme

Most presentations use the shared `_theme/`. To opt a presentation out and use its own look, give it its own `<presentation-name>/_layouts/` directory (see `scientific_software_development/` for an example) — `scripts/serve.sh` and the deploy workflow both detect this automatically and build it standalone instead of assembling it against `_theme/`.
