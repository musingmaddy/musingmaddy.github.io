

## Hard Rules

1.  **Theme is READ-ONLY.** `themes/hugo-theme-dream/` is a Git submodule — never create, edit, or delete anything inside it. To customize, copy the file to the identical path under project root and edit the copy.
    `themes/hugo-theme-dream/layouts/_default/single.html` → `layouts/_default/single.html`
2.  **Never commit `public/` or `resources/`.** Both are gitignored build artifacts.
3.  **Read before writing.** Always `cat hugo.yaml` (and files in `config/_default/`) before changing config. Always read an existing file in the target content section before creating new content — match its structure exactly.
4.  **Prefer `hugo new`** for content creation — it applies the theme's archetypes and produces correct front matter.
5.  **Internal links must use ref**: `[text]({{< ref "posts/slug" >}})` — never hardcode paths.

## Core Architecture

-   **Stack**: Hugo Static Site Generator (Extended Version).
-   **Theme**: `hugo-theme-dream` (Git Submodule).
-   **Design Philosophy**: Masonry Grid layout with "Flip" cards. Content is displayed as cards that flip to reveal summaries before clicking through.
-   **Hosting**: GitHub Pages.
-   **Config**: Split configuration in `config/_default/`.
-   **Hugo Extended** ≥ 0.146.0 required (Theme uses SCSS/Sass).

## Config Structure

Config is split across `config/_default/`. Note that Dream relies heavily on `params` for layout control.

-   `hugo.yaml` — Core site settings (baseURL, theme, languageCode).
-   `params.yaml` — **Crucial for Dream.** Controls `headerWidgets`, `footerWidgets`, masonry layout columns, and background colors.
-   `menus.yaml` — Navigation menus (Dream places these in the sidebar/header).
-   `languages.yaml` — i18n / language settings.

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
hugo server -D               # Dev server (includes drafts)
hugo server                  # Dev server (published only)
hugo --gc --minify           # Production build
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

| I need to... | Look here / do this |
| :--- | :--- |
| **Fix Sidebar/Header** | `params.yaml` → Look for `headerWidgets` (This controls what appears in the left column). |
| **Override a template** | Copy from `themes/hugo-theme-dream/layouts/` → `layouts/` |
| **Add custom CSS** | Create `assets/scss/custom.scss` (Dream uses SCSS, check params to enable custom file injection). |
| **Add a nav menu item** | `menus.yaml` → `menus.main` |
| **Configure social links** | `params.yaml` → `social` map. |
| **Change Card Grid** | `params.yaml` → `masonry_columns` (controls grid density). |
| **Read theme documentation** | https://hugo-theme-dream.g1en.site/ |

## Before Completing Any Task

1.  **Run `hugo server`** — confirm **zero errors**. Dream is strict about SCSS compilation; ensure `hugo extended` is working.
2.  **Visual Check**: Verify the **Masonry Grid** renders correctly. Ensure cards have `cover` images and flip animations work.
3.  **Ref Check**: Confirm no files inside `themes/hugo-theme-dream/` were touched (`git diff --name-only`).
4.  **Clean Build**: If CSS looks broken (cards overlapping), delete `resources/` and restart server.