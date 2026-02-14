## Hard Rules

1.  **Themes are READ-ONLY.** Both `themes/dream/` and `themes/hugo-narrow/` are Git submodules — never create, edit, or delete anything inside them. To customize, copy the file to the identical path under project root and edit the copy.
    `themes/dream/layouts/_default/single.html` → `layouts/_default/single.html`
2.  **Never commit `public/` or `resources/`.** Both are gitignored build artifacts.
3.  **Read before writing.** Always `cat hugo.dream.yaml` or `hugo.narrow.yaml` before changing config. Always read an existing file in the target content section before creating new content — match its structure exactly.
4.  **Prefer `hugo new`** for content creation — it applies the theme's archetypes and produces correct front matter.
5.  **Internal links must use ref**: `[text]({{< ref "posts/slug" >}})` — never hardcode paths.

## Core Architecture

-   **Stack**: Hugo Static Site Generator (Extended Version).
-   **Themes**: Two themes available via Git Submodules:
    -   `dream` — Masonry Grid layout with "Flip" cards. Content is displayed as cards that flip to reveal summaries before clicking through.
    -   `hugo-narrow` — A modern, clean, and minimal Hugo theme built with Tailwind CSS 4.0.
-   **Hosting**: GitHub Pages.
-   **Config**: Separate YAML configuration files for each theme.
-   **Hugo Extended** ≥ 0.146.0 required (Themes use SCSS/Sass/Tailwind).

## Config Structure

The site uses separate configuration files for each theme:

-   `hugo.dream.yaml` — Configuration for Dream theme (default for GitHub Pages deployment).
-   `hugo.narrow.yaml` — Configuration for Hugo Narrow theme.
-   **Legacy**: `hugo.toml` — Replaced by `hugo.dream.yaml` (can be deleted).

**Note**: Dream theme relies heavily on `params` for layout control (masonry columns, widgets, etc.).

## Front Matter Schema

Dream Theme uses specific keys for the masonry grid.

```yaml
title: ""
date: 2025-01-01T00:00:00+00:00
draft: false
description: ""        # SEO meta description
cover: "/img/pic.jpg"  # VITAL: The image shown on the front of the masonry card.
summary: ""            # Text shown on the back of the card (flip side).
tags: []
categories: []
toc: false             # Show table of contents (per-page override)
math: false            # Enable KaTeX rendering
slug: ""               # URL override
weight: 0              # Sort order
# Dream Specifics
header:                # Optional: Custom header settings for this page
  background: ""       # CSS background property (color or image)
```

**Key Notes for Dream:**
*   **`cover` vs `image`**: Dream uses **`cover`** for the grid image. Ensure this is set, or the card will look empty in the masonry layout.
*   **Page Bundles**: Highly recommended. Place `cover.jpg` inside the post folder (e.g., `content/posts/my-trip/cover.jpg`) and set `cover: "cover.jpg"`.
*   **Summary**: If `summary` is empty, Hugo may auto-truncate the body text for the card back, which can look messy. Explicitly define it.

## Commands

```sh
# Running with Dream theme
hugo server --config hugo.dream.yaml -D      # Dev server (includes drafts)
hugo server --config hugo.dream.yaml        # Dev server (published only)

# Running with Hugo Narrow theme
hugo server --config hugo.narrow.yaml -D    # Dev server (includes drafts)
hugo server --config hugo.narrow.yaml      # Dev server (published only)

# Production build (uses config specified in GitHub Actions)
hugo --gc --minify --config hugo.dream.yaml

# Create new content
hugo new posts/slug/index.md # New blog post (Page Bundle - Recommended)
```

## Content Patterns

Dream treats top-level folders in `content/` as sections in the masonry grid.

```
content/
├── posts/       # Main blog entries (Masonry cards)
├── projects/    # Portfolio entries (Masonry cards)
├── about/       # The "About" section (renders as a card or sidebar widget depending on config)
└── archives/    # Archive listing
```

**Visual Hierarchy:**
1.  **Home**: Displays a Masonry grid of cards from sections defined in `params.mainSections`.
2.  **Single Page**: The standard article view after clicking a card.

## Active Overrides (project-level)

-   `layouts/_partials/` — Custom partials.
-   `layouts/_default/baseof.html` — If overridden, check strictly against theme version.

## Multilingual

The site has Chinese (`zh-cn`) translations.
-   English (default): `content/posts/my-post/index.md`
-   Chinese: `content/posts/my-post/index.zh-cn.md`
-   Create only the English version unless explicitly asked for translations.

## Deployment

-   GitHub Actions workflow in `.github/workflows/` handles CI/CD.
-   Pushes to `main` trigger a build and deploy to GitHub Pages.
-   **Site URL**: `https://musingmaddy.github.io/`

## Customization Lookup Table

| I need to...                 | Look here / do this                                                                               |
| :--------------------------- | :------------------------------------------------------------------------------------------------ |
| **Fix Sidebar/Header**       | **Dream**: Check `hugo.dream.yaml` → `params` section for layout control.                         |
| **Override a template**      | Copy from `themes/dream/layouts/` or `themes/hugo-narrow/layouts/` → `layouts/`                   |
| **Add custom CSS**           | Create `assets/scss/custom.scss` (Dream uses SCSS, check params to enable custom file injection). |
| **Add a nav menu item**      | Edit `menus` section in `hugo.dream.yaml` or `hugo.narrow.yaml`                                   |
| **Configure social links**   | Edit `params.author.social` in the respective config file.                                        |
| **Change Card Grid**         | **Dream only**: Edit `params` in `hugo.dream.yaml` (masonry-related settings).                    |
| **Read theme documentation** | https://hugo-theme-dream.g1en.site/                                                               |

## Before Completing Any Task

1.  **Run `hugo server --config hugo.dream.yaml`** — confirm **zero errors**. Themes are strict about SCSS/Tailwind compilation; ensure `hugo extended` is working.
2.  **Visual Check**: Verify the **Masonry Grid** (Dream) or layout (Narrow) renders correctly. Ensure cards have `cover` images and flip animations work.
3.  **Ref Check**: Confirm no files inside `themes/dream/` or `themes/hugo-narrow/` were touched (`git diff --name-only`).
4.  **Clean Build**: If CSS looks broken (cards overlapping), delete `resources/` and restart server.