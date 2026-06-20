# Setting Up Docusaurus

## Introduction

For a library with a public API, the docs are part of the product. [Docusaurus](https://docusaurus.io) is a strong fit for OSS project documentation: it is purpose-built for technical content, generates a fully static site that deploys to GitHub Pages at no cost, and provides versioning, internationalization, auto-generated sidebars, and "Edit this page" source links without additional configuration.

### Why This Configuration

The default scaffold includes a blog, tutorial pages, example content, and a custom homepage. This guide strips all of that and configures Docusaurus as a docs-only site:

- The blog is disabled. Release notes belong in GitHub Releases.
- Tutorial and example pages are removed. They exist to demonstrate Docusaurus, not to document your project.
- `routeBasePath: '/'` serves docs at the root URL rather than under `/docs/`. There is no other content on this site to disambiguate.
- Deployment targets GitHub Pages: free for public repositories and already inside your existing review workflow.
- The sidebar auto-generates from the folder structure. Adding a page means adding a file; there is no manifest to maintain.

### What This Document Covers

Three sections that build on each other. You can stop after any one and have a working, deployable site.

**Section 1, Installation and Configuration** gets a branded, docs-only site built, organized, and deployed to GitHub Pages.

**Section 2, Versioning** covers maintaining separate docs for each released version of your package. It can be added at any point without changes to the work done in Section 1.

**Section 3, Internationalization (i18n)** covers adding translated versions of your docs, including how versioning and i18n interact when both are in use.

---

## Table of Contents

- [1. Installation and Configuration](#1-installation-and-configuration)
  - [Installation](#installation)
  - [Clean Up Default Content](#clean-up-default-content)
  - [Add Your Logo and Favicon](#add-your-logo-and-favicon)
  - [Update the CSS](#update-the-css)
  - [Update `docusaurus.config.js`](#update-docusaurusconfigjs)
    - [Site Metadata](#site-metadata)
    - [URLs and GitHub Pages Deployment](#urls-and-github-pages-deployment)
    - [Docs Configuration](#docs-configuration)
    - [Social Card](#social-card)
    - [Navbar](#navbar)
    - [Footer](#footer)
    - [Title Template](#title-template)
  - [Update `sidebars.js`](#update-sidebarsjs)
  - [Add Your First Docs Page](#add-your-first-docs-page)
  - [Organizing Docs into Folders and Subsections](#organizing-docs-into-folders-and-subsections)
    - [Creating a Subsection](#creating-a-subsection)
    - [Controlling Category Order](#controlling-category-order)
    - [Nesting Folders](#nesting-folders)
    - [Adding a Category Landing Page](#adding-a-category-landing-page)
    - [A Complete Example Structure](#a-complete-example-structure)
  - [Deploying to GitHub Pages](#deploying-to-github-pages)
    - [Step 1: Enable GitHub Pages in the Repository Settings](#step-1-enable-github-pages-in-the-repository-settings)
    - [Step 2: Copy the Caller Workflow](#step-2-copy-the-caller-workflow)
    - [How the Workflow Operates](#how-the-workflow-operates)
    - [Verifying the Deployment](#verifying-the-deployment)
- [2. Versioning](#2-versioning)
  - [How Versioning Works](#how-versioning-works)
  - [Enable Versioning in `docusaurus.config.js`](#enable-versioning-in-docusaurusconfigjs)
  - [Add the Version Dropdown to the Navbar](#add-the-version-dropdown-to-the-navbar)
  - [Tagging a New Version](#tagging-a-new-version)
  - [Tagging Subsequent Versions](#tagging-subsequent-versions)
  - [Controlling Which Versions Appear in the Dropdown](#controlling-which-versions-appear-in-the-dropdown)
  - [Editing a Frozen Version](#editing-a-frozen-version)
  - [Recommended Commit Strategy](#recommended-commit-strategy-versioning)
  - [Full `docusaurus.config.js` Versioning Example](#full-docusaurusconfigjs-versioning-example)
- [3. Internationalization (i18n)](#3-internationalization-i18n)
  - [How i18n Works](#how-i18n-works)
  - [Declare Locales in `docusaurus.config.js`](#declare-locales-in-docusaurusconfigjs)
  - [Add the Locale Dropdown to the Navbar](#add-the-locale-dropdown-to-the-navbar)
  - [Scaffold the Translation Files](#scaffold-the-translation-files)
  - [Add Explicit Heading IDs to Your Markdown](#add-explicit-heading-ids-to-your-markdown)
  - [Copy and Translate the Docs](#copy-and-translate-the-docs)
  - [Translate the JSON Files](#translate-the-json-files)
  - [Preview a Locale Locally](#preview-a-locale-locally)
  - [Recommended Commit Strategy](#recommended-commit-strategy-i18n)
  - [Full `docusaurus.config.js` i18n Example](#full-docusaurusconfigjs-i18n-example)
  - [Using Versioning and i18n Together](#using-versioning-and-i18n-together)
    - [Directory Structure](#directory-structure)
    - [Setup Order](#setup-order)
    - [Scaffolding Translations for Versioned Docs](#scaffolding-translations-for-versioned-docs)
    - [Version Banner Translation](#version-banner-translation)
    - [Navbar Dropdowns When Both Features Are Active](#navbar-dropdowns-when-both-features-are-active)
    - [Full `docusaurus.config.js` Combined Example](#full-docusaurusconfigjs-combined-example)

---

## 1. Installation and Configuration

### Installation

From the repository root, run:

```bash
npx create-docusaurus@latest docs classic
```

This creates a `docs/` folder containing the default Docusaurus scaffold. All subsequent changes are made inside that folder.

The scaffold includes a `README.md` with instructions for running the site locally. Read it before making any other changes. The commands it covers (`npm run start`, `npm run build`, and `npm run serve`) are the primary tools you will use throughout this process to verify your work.

> [!TIP]
> The `docs` argument is the name of the top-level folder that will be created. You can use any name, but this guide uses `docs/` throughout. If you choose a different name, substitute it wherever this guide references the `docs/` folder and update the `docs-path` input in the deployment workflow accordingly.

### Clean Up Default Content

Remove the following files and folders that are not needed:

- `blog/`
- `docs/tutorial-basics/`
- `docs/tutorial-extras/`
- `docs/intro.mdx`
- `src/components/HomepageFeatures/`
- `src/pages/index.js`
- `src/pages/index.module.css`
- `src/pages/markdown-page.mdx`
- `static/img/undraw_docusaurus_mountain.svg`
- `static/img/undraw_docusaurus_react.svg`
- `static/img/undraw_docusaurus_tree.svg`
- `static/img/logo.svg`

### Add Your Logo and Favicon

Replace the default images in `static/img/` with your own:

- Replace `favicon.ico` with your custom favicon.
- Add your logo image (e.g., `logo.png`). If you have a package icon, use that.

### Update the CSS

Open `src/css/custom.css` and replace the `--ifm-color-primary` variables in both `:root` and `[data-theme='dark']` with values derived from your brand color. The easiest approach is to paste the file's contents into a generative AI tool with a prompt like:

```
Update the `--ifm-color-primary` CSS variables using `#XXXXXX` as the primary color. Generate an appropriate light mode palette for `:root` and a lighter complementary palette for `[data-theme='dark']`.
```

Where `#XXXXXX` represents your desired primary color.

### Update `docusaurus.config.js`

The snippets below show only the values that need to change. Leave the rest of the file as-is.

#### Site Metadata

These three values appear in the browser tab, the `<title>` element, and social previews. `title` is your project's display name. `tagline` is a short descriptor used in metadata. `favicon` points to the icon added in the previous step; the path is relative to `static/`.

```js
title: 'Your Project Name',
tagline: 'A short description of your project.',
favicon: 'img/favicon.ico',
```

#### URLs and GitHub Pages Deployment

These four values tell Docusaurus where the site lives and how to deploy it. `url` is the root of your GitHub Pages domain (`https://[username].github.io` for personal accounts). `baseUrl` is the path prefix where the site is mounted, always `/[repo-name]/` for a project page. `organizationName` and `projectName` are used by the deploy command to push the built site to the correct repository.

```js
url: 'https://[organizationName].github.io',
baseUrl: '/[projectName]/',
organizationName: 'your-github-username',
projectName: 'your-repo-name',
```

#### Docs Configuration

`routeBasePath: '/'` serves docs from the root URL rather than under `/docs/`. `sidebarPath` points to the sidebar file updated later in this guide. `editUrl` enables the "Edit this page" link at the bottom of every doc, constructing a direct link to that file in your GitHub repository. Setting `blog: false` removes the blog plugin entirely.

```js
docs: {
  routeBasePath: '/',
  sidebarPath: './sidebars.js',
  editUrl: 'https://github.com/[organizationName]/[projectName]/tree/main/docs/',
},
blog: false,
```

#### Social Card

If you have a social card image, add it to `static/img/` and update this line:

```js
image: 'img/your-social-card.jpg',
```

If you do not have a social card, remove the `image` line entirely.

#### Navbar

`title` is the text shown next to the logo. The `logo` block points to your logo image: `alt` is the accessible description and `src` is the path relative to `static/`. The `docSidebar` item links to your documentation sidebar; `sidebarId` must match the ID defined in `sidebars.js`. The `href` item on the right links to your GitHub repository.

```js
navbar: {
  title: 'Your Project Name',
  logo: {
    alt: 'Your Project Name Logo',
    src: 'img/logo.png', // use your actual logo filename
  },
  items: [
    {
      type: 'docSidebar',
      sidebarId: 'docsSidebar',
      position: 'left',
      label: 'Documentation',
    },
    {
      href: 'https://github.com/[organizationName]/[projectName]',
      label: 'GitHub',
      position: 'right',
    },
  ],
},
```

#### Footer

The footer is organized into named link columns. Each object in `links` becomes a column with a `title` and a list of `items`. Use `to` for internal paths and `href` for external URLs. The `copyright` expression `new Date().getFullYear()` keeps the year current automatically.

The three columns below cover the most common OSS needs: a return to the docs, community resources, and project governance files. Adjust links to match what exists in your repository.

```js
footer: {
  style: 'dark',
  links: [
    {
      title: 'Documentation',
      items: [
        {
          label: 'Getting Started',
          to: '/',
        },
      ],
    },
    {
      title: 'Community',
      items: [
        {
          label: 'Discussions',
          href: 'https://github.com/[organizationName]/[projectName]/discussions',
        },
        {
          label: 'Stack Overflow',
          href: 'https://stackoverflow.com/questions/tagged/[projectName]',
        },
      ],
    },
    {
      title: 'Project',
      items: [
        {
          label: 'Contributing Guide',
          href: 'https://github.com/[organizationName]/[projectName]/blob/main/.github/contributing.md',
        },
        {
          label: 'Code of Conduct',
          href: 'https://github.com/[organizationName]/[projectName]/blob/main/.github/code_of_conduct.md',
        },
        {
          label: 'GitHub',
          href: 'https://github.com/[organizationName]/[projectName]',
        },
      ],
    },
  ],
  copyright: `Copyright © ${new Date().getFullYear()} Your Name`,
},
```

#### Title Template

`titleDelimiter` separates the site name from the page name in the browser tab. `titleTemplate` defines the full pattern; `%s` is replaced with the current page's title. Together they produce titles like "Your Project Name | Getting Started".

```js
titleDelimiter: '|',
titleTemplate: 'Your Project Name | %s',
```

### Update `sidebars.js`

The default scaffold names the sidebar `tutorialSidebar`, but the navbar config references `docsSidebar`; the two must match. The `autogenerated` type builds the sidebar automatically from the folder and file structure under `docs/docs/`, so no manual page listing is required.

```js
const sidebars = {
  docsSidebar: [{type: 'autogenerated', dirName: '.'}],
};
```

### Add Your First Docs Page

With the default content removed, the site has no pages and will fail to build. Create `index.md` inside `docs/docs/` as the landing page. Because `routeBasePath` is `/`, this file is served at the root URL.

`sidebar_position: 1` places this page first in the generated sidebar. Replace the placeholder content with your actual getting started material.

```md
---
sidebar_position: 1
---

# Getting Started

Your content here.
```

### Organizing Docs into Folders and Subsections

Docusaurus maps the folder structure inside `docs/docs/` directly onto the sidebar: a folder becomes a collapsible category and the files inside it become the items under that category. No manual sidebar configuration is needed beyond the `autogenerated` type already in `sidebars.js`.

#### Creating a Subsection

Create a folder inside `docs/docs/` and place your Markdown files inside it. The folder name becomes the sidebar category label by default, with hyphens converted to spaces and the first letter capitalized. A folder named `advanced-usage` becomes "Advanced Usage".

```
docs/docs/
  index.md                  <-- root landing page
  advanced-usage/
    configuration.md
    authentication.md
    error-handling.md
```

Use `sidebar_position` frontmatter to control page order within the section:

```md
---
sidebar_position: 1
---

# Configuration

Your content here.
```

#### Controlling Category Order

By default, Docusaurus sorts categories alphabetically. To control a category's position, add a `_category_.json` file inside the folder. `position` works the same way as `sidebar_position` in individual files.

```
docs/docs/
  index.md
  advanced-usage/
    _category_.json
    configuration.md
    authentication.md
    error-handling.md
```

`docs/docs/advanced-usage/_category_.json`:

```json
{
  "label": "Advanced Usage",
  "position": 2,
  "collapsible": true,
  "collapsed": false
}
```

`label` overrides the name derived from the folder name, which is useful when the desired label does not map cleanly to a valid folder name (e.g., "C# API Reference" from `csharp-api-reference` would not capitalize correctly without an override). `collapsible` controls whether the user can collapse the category; `collapsed` controls whether it starts collapsed on page load.

#### Nesting Folders

Folders can be nested to any depth. Each level becomes another tier in the sidebar hierarchy. Add `_category_.json` at each level where you need to control the label or position.

```
docs/docs/
  index.md
  getting-started/
    _category_.json         <-- label: "Getting Started", position: 1
    installation.md
    quickstart.md
  advanced-usage/
    _category_.json         <-- label: "Advanced Usage", position: 2
    configuration.md
    authentication.md
    internals/
      _category_.json       <-- label: "Internals", position: 1
      architecture.md
      extension-points.md
```

#### Adding a Category Landing Page

By default, clicking a category label expands or collapses the section without navigating anywhere. To make the category itself a navigable page, create an `index.md` inside the folder and add a `link` entry to `_category_.json`:

`docs/docs/advanced-usage/_category_.json`:

```json
{
  "label": "Advanced Usage",
  "position": 2,
  "collapsible": true,
  "collapsed": false,
  "link": {
    "type": "doc",
    "id": "advanced-usage/index"
  }
}
```

`id` is the path to the doc relative to `docs/docs/`, without the `.md` extension. With this in place, clicking "Advanced Usage" navigates to `advanced-usage/index.md` rather than just toggling the section.

#### A Complete Example Structure

```
docs/docs/
  index.md                          <-- Getting Started (root, sidebar_position: 1)
  getting-started/
    _category_.json                 <-- label: "Installation", position: 2
    prerequisites.md
    installation.md
    quickstart.md
  guides/
    _category_.json                 <-- label: "Guides", position: 3
    index.md                        <-- Guides overview (linked from _category_.json)
    configuration.md
    authentication.md
    error-handling.md
  api-reference/
    _category_.json                 <-- label: "API Reference", position: 4
    index.md                        <-- API overview
    endpoints.md
    models.md
    exceptions.md
  contributing/
    _category_.json                 <-- label: "Contributing", position: 5
    index.md
    development-setup.md
    pull-request-guide.md
```

The root `index.md` is the site landing page. Sections that benefit from an overview have their own `index.md` wired up via `_category_.json`. Pages within each section are ordered by `sidebar_position`.

### Deploying to GitHub Pages

Deployment is handled by a reusable GitHub Actions workflow in the [scottoffen/workflows](https://github.com/scottoffen/workflows) repository. Two one-time steps are required before it can run.

#### Step 1: Enable GitHub Pages in the Repository Settings

Go to **Settings > Pages** in your repository. Under "Build and deployment", set **Source** to **GitHub Actions**. Without this, the deployment step fails with an error that does not clearly point at this setting as the cause. The `github-pages` environment is created automatically after the first successful run; no need to create it beforehand.

#### Step 2: Copy the Caller Workflow

Copy [`examples/deploy-docs.yml`](https://github.com/scottoffen/workflows/blob/main/examples/deploy-docs.yml) from the workflows repository into your project at `.github/workflows/deploy-docs.yml`. Always use that page as the source, as it is kept up to date as the workflow evolves.

For a standard setup with Docusaurus in the `docs/` folder, no edits are needed. If you used a different folder name, update the `paths` filter in the `on:` block and add a `with: docs-path:` input under the `deploy` job.

#### How the Workflow Operates

The workflow triggers on any push to `main` that touches files under `docs/` or the workflow file itself, and can also be run manually from the **Actions** tab. It installs dependencies, runs `npm run build`, and deploys the `build/` output to GitHub Pages. Concurrency and permissions constraints are defined inside the reusable workflow, so the caller file stays minimal. Pull requests do not trigger a deploy.

#### Verifying the Deployment

After the first successful run, the live URL is visible under **Settings > Pages**. It follows the pattern `https://[organizationName].github.io/[projectName]/`, matching the `url` and `baseUrl` values in `docusaurus.config.js`. If the site loads but assets or links are broken, the most common cause is a mismatch between `baseUrl` and the actual repository name.

---

## 2. Versioning

When your package has breaking API changes across releases, Docusaurus versioning lets you maintain a frozen copy of the docs for each released version while `docs/docs/` always represents the next unreleased version under active development.

### How Versioning Works

Docusaurus stores versioned docs in two places:

- `docs/versioned_docs/version-X.Y/`: frozen snapshot of the docs for version X.Y
- `docs/versioned_sidebars/version-X.Y-sidebars.json`: frozen sidebar for that version

`docs/versions.json` tracks all known versions. `docs/docs/` is always treated as the unreleased "next" version. Visitors see a version dropdown in the navbar and can switch between any published version.

### Enable Versioning in `docusaurus.config.js`

The plain `docs` config has no awareness of versions. Expanding it adds three new keys. `lastVersion` tells Docusaurus which version to serve at the root URL; set it to `'current'` until your first release, then update it to the latest tagged version. The `versions` map configures each version individually: `path: 'next'` serves the in-development docs at `/next/` and `banner: 'unreleased'` adds a warning bar. Once you tag the first version, add an entry for it here too.

```js
presets: [
  [
    'classic',
    {
      docs: {
        routeBasePath: '/',
        sidebarPath: './sidebars.js',
        editUrl: 'https://github.com/[organizationName]/[projectName]/tree/main/docs/',
        lastVersion: 'current',
        versions: {
          current: {
            label: 'Next (Unreleased)',
            path: 'next',
            banner: 'unreleased',
          },
        },
      },
      blog: false,
    },
  ],
],
```

### Add the Version Dropdown to the Navbar

The `docsVersionDropdown` component reads your `versions.json` and `versions` config and renders a dropdown automatically. Add it to `navbar.items`; placing it on `'right'` keeps it grouped with the GitHub link.

```js
navbar: {
  items: [
    // ... your existing items ...
    {
      type: 'docsVersionDropdown',
      position: 'right',
    },
  ],
},
```

### Tagging a New Version

Run this from inside the `docs/` folder when you are ready to freeze the current docs as a release:

```bash
cd docs
npm run docusaurus docs:version 1.0.0
```

Replace `1.0.0` with the version you are releasing. Docusaurus will:

1. Copy `docs/docs/` to `docs/versioned_docs/version-1.0.0/`
2. Copy the current sidebar to `docs/versioned_sidebars/version-1.0.0-sidebars.json`
3. Add `"1.0.0"` to `docs/versions.json`

Then update `docusaurus.config.js`. `path: '/'` on the tagged version promotes it to the root URL, so visitors land on the released docs by default. The in-development docs remain accessible at `/next/`.

```js
lastVersion: '1.0.0',
versions: {
  current: {
    label: 'Next (Unreleased)',
    path: 'next',
    banner: 'unreleased',
  },
  '1.0.0': {
    label: '1.0.0',
    path: '/',  // serves this version at the root URL
  },
},
```

### Tagging Subsequent Versions

Repeat the same command for each new release, then move `lastVersion` and `path: '/'` to the new version. The previous release loses its `path` override and is served automatically at its version-prefixed URL (e.g., `/1.0.0/`).

```bash
npm run docusaurus docs:version 2.0.0
```

```js
lastVersion: '2.0.0',
versions: {
  current: {
    label: 'Next (Unreleased)',
    path: 'next',
    banner: 'unreleased',
  },
  '2.0.0': {
    label: '2.0.0',
    path: '/',
  },
  '1.0.0': {
    label: '1.0.0',
    // served at /1.0.0/ automatically
  },
},
```

### Controlling Which Versions Appear in the Dropdown

By default every version in `versions.json` appears in the dropdown. `onlyIncludeVersions` limits the list without removing older versions from the site; they remain accessible via direct URL and continue to be indexed by search engines.

```js
docs: {
  onlyIncludeVersions: ['current', '2.0.0', '1.0.0'],
},
```

### Editing a Frozen Version

Versioned files live under `docs/versioned_docs/version-X.Y/` and can be edited like any other Markdown file. The `editUrl` in your config ensures the "Edit this page" link on versioned pages still works. Changes must be applied to each versioned copy individually; there is no propagation between versions.

### Recommended Commit Strategy {#recommended-commit-strategy-versioning}

The files generated by `docs:version` are not build artifacts. They are the authoritative source for each version's content and cannot be recovered without re-tagging. Commit them to source control and do not add any of the following to `.gitignore`:

```
docs/versioned_docs/
docs/versioned_sidebars/
docs/versions.json
```

### Full `docusaurus.config.js` Versioning Example

```js
const config = {
  title: 'Your Project Name',
  tagline: 'A short description of your project.',
  favicon: 'img/favicon.ico',

  url: 'https://[organizationName].github.io',
  baseUrl: '/[projectName]/',
  organizationName: 'your-github-username',
  projectName: 'your-repo-name',

  presets: [
    [
      'classic',
      {
        docs: {
          routeBasePath: '/',
          sidebarPath: './sidebars.js',
          editUrl: 'https://github.com/[organizationName]/[projectName]/tree/main/docs/',
          lastVersion: '2.0.0',
          onlyIncludeVersions: ['current', '2.0.0', '1.0.0'],
          versions: {
            current: {
              label: 'Next (Unreleased)',
              path: 'next',
              banner: 'unreleased',
            },
            '2.0.0': {
              label: '2.0.0',
              path: '/',
            },
            '1.0.0': {
              label: '1.0.0',
            },
          },
        },
        blog: false,
      },
    ],
  ],

  themeConfig: {
    titleDelimiter: '|',
    titleTemplate: 'Your Project Name | %s',
    navbar: {
      title: 'Your Project Name',
      logo: {
        alt: 'Your Project Name Logo',
        src: 'img/logo.png',
      },
      items: [
        {
          type: 'docSidebar',
          sidebarId: 'docsSidebar',
          position: 'left',
          label: 'Documentation',
        },
        {
          type: 'docsVersionDropdown',
          position: 'right',
        },
        {
          href: 'https://github.com/[organizationName]/[projectName]',
          label: 'GitHub',
          position: 'right',
        },
      ],
    },
    // ... footer config unchanged ...
  },
};
```

---

## 3. Internationalization (i18n)

When your audience spans multiple languages, Docusaurus i18n lets you maintain translated copies of your docs for each locale while `docs/docs/` remains the authoritative source. Each locale gets its own URL prefix (e.g., `/fr/`, `/de/`) and visitors can switch languages from a dropdown in the navbar.

### How i18n Works

Translated content lives under `i18n/` inside your `docs/` directory, organized by locale code:

```
docs/
  i18n/
    fr/
      docusaurus-plugin-content-docs/
        current/          <-- translated Markdown files (mirrors docs/docs/)
        current.json      <-- translated sidebar category labels
      docusaurus-theme-classic/
        navbar.json       <-- translated navbar labels
        footer.json       <-- translated footer labels
      code.json           <-- translated UI strings (Next, Previous, etc.)
```

Docusaurus does not auto-translate anything. It scaffolds the files and you (or a contributor) fill them in. The source files in `docs/docs/` are never modified.

### Declare Locales in `docusaurus.config.js`

Add an `i18n` block at the top level of the config, not inside `presets`. `defaultLocale` is served at the root URL with no prefix; all other locales get a path prefix matching their code. `label` is what visitors see in the locale switcher; use the language's native name. Set `direction: 'rtl'` for right-to-left languages such as Arabic or Hebrew.

```js
i18n: {
  defaultLocale: 'en',
  locales: ['en', 'fr', 'de'],
  localeConfigs: {
    en: {
      label: 'English',
    },
    fr: {
      label: 'Français',
      direction: 'ltr',
    },
    de: {
      label: 'Deutsch',
      direction: 'ltr',
    },
  },
},
```

### Add the Locale Dropdown to the Navbar

The `localeDropdown` component reads your `i18n` config and renders a language switcher automatically using the `label` values from `localeConfigs`. Add it to `navbar.items`; the right side is conventional for locale and version switchers.

```js
navbar: {
  items: [
    // ... your existing items ...
    {
      type: 'localeDropdown',
      position: 'right',
    },
  ],
},
```

### Scaffold the Translation Files

The `write-translations` command generates the JSON files that hold translatable UI strings: navbar labels, footer text, sidebar category names, and theme chrome. It pre-populates each file with the source strings as a starting point. Run it from inside the `docs/` folder for each locale:

```bash
cd docs
npm run docusaurus write-translations -- --locale fr
```

```bash
npm run docusaurus write-translations -- --locale de
```

Run this once per locale. If you add new UI strings later (e.g., a new navbar item), run it again; it merges new keys without overwriting existing translations.

### Add Explicit Heading IDs to Your Markdown

By default, heading anchor IDs are derived from the heading text. Once headings are translated, the derived IDs will differ between locales and any cross-links to specific headings will break. Explicit IDs are stable across all languages.

Run this once from inside `docs/` before beginning any translation work:

```bash
npm run docusaurus write-heading-ids
```

This rewrites headings like `## Getting Started` to:

```md
## Getting Started {#getting-started}
```

Commit these changes before copying files for translation and re-run whenever you add new headings to the source docs.

### Copy and Translate the Docs

Copy the source docs into each locale's content folder:

```bash
cp -r docs/docs/* i18n/fr/docusaurus-plugin-content-docs/current/
```

Then replace the English content with the translation. Keep all frontmatter keys (like `sidebar_position` and explicit heading IDs) unchanged. Only translate the prose, headings, and code comments.

### Translate the JSON Files

Each scaffolded JSON file holds translatable strings for a specific surface area. Fill in the `message` field for each key; leave `description` as-is (it is context for translators, not displayed content).

`navbar.json` covers navbar item labels. Proper nouns like "GitHub" typically need no translation; just copy the source value into `message`.

`i18n/fr/docusaurus-theme-classic/navbar.json`:

```json
{
  "item.label.Documentation": {
    "message": "Documentation",
    "description": "Navbar item with label Documentation"
  },
  "item.label.GitHub": {
    "message": "GitHub",
    "description": "Navbar item with label GitHub"
  }
}
```

`current.json` covers sidebar category labels. The key format is `sidebar.[sidebarId].category.[CategoryName]`; if you rename a category in the source structure, update the key here to match.

`i18n/fr/docusaurus-plugin-content-docs/current.json`:

```json
{
  "sidebar.docsSidebar.category.Getting Started": {
    "message": "Premiers pas",
    "description": "The label for category Getting Started in sidebar docsSidebar"
  }
}
```

`code.json` covers theme UI strings that appear on every page. The full key list is generated by `write-translations`. You do not need to translate every key; any key left with an empty `message` falls back to the default locale value.

`i18n/fr/code.json`:

```json
{
  "theme.common.editThisPage": {
    "message": "Modifier cette page",
    "description": "The link label to edit the current page"
  },
  "theme.common.backToTopButton": {
    "message": "Retour en haut",
    "description": "The label for the back to top button"
  }
}
```

### Preview a Locale Locally

The standard dev server only runs the default locale. To preview a specific translation:

```bash
npm run start -- --locale fr
```

Only one locale runs at a time in development. To verify all locales together before deploying, do a full build and serve it locally:

```bash
npm run build
npm run serve
```

### Recommended Commit Strategy {#recommended-commit-strategy-i18n}

Like versioned docs files, i18n files are not build artifacts. They are the actual content of the translated site and are irreplaceable if lost. Keeping them in source control also makes it easy for contributors to submit translation improvements via pull request.

Commit the entire `docs/i18n/` folder and do not add it to `.gitignore`.

### Full `docusaurus.config.js` i18n Example

```js
const config = {
  title: 'Your Project Name',
  tagline: 'A short description of your project.',
  favicon: 'img/favicon.ico',

  url: 'https://[organizationName].github.io',
  baseUrl: '/[projectName]/',
  organizationName: 'your-github-username',
  projectName: 'your-repo-name',

  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'fr', 'de'],
    localeConfigs: {
      en: { label: 'English' },
      fr: { label: 'Français', direction: 'ltr' },
      de: { label: 'Deutsch', direction: 'ltr' },
    },
  },

  presets: [
    [
      'classic',
      {
        docs: {
          routeBasePath: '/',
          sidebarPath: './sidebars.js',
          editUrl: 'https://github.com/[organizationName]/[projectName]/tree/main/docs/',
        },
        blog: false,
      },
    ],
  ],

  themeConfig: {
    titleDelimiter: '|',
    titleTemplate: 'Your Project Name | %s',
    navbar: {
      title: 'Your Project Name',
      logo: {
        alt: 'Your Project Name Logo',
        src: 'img/logo.png',
      },
      items: [
        {
          type: 'docSidebar',
          sidebarId: 'docsSidebar',
          position: 'left',
          label: 'Documentation',
        },
        {
          type: 'localeDropdown',
          position: 'right',
        },
        {
          href: 'https://github.com/[organizationName]/[projectName]',
          label: 'GitHub',
          position: 'right',
        },
      ],
    },
    // ... footer config unchanged ...
  },
};
```

### Using Versioning and i18n Together

Versioning and i18n are fully composable. Each combination of version and locale gets its own translated, frozen snapshot of the docs.

#### Directory Structure

```
docs/
  docs/                                    <-- current (unreleased), default locale
  versioned_docs/
    version-2.0.0/                         <-- frozen v2.0.0, default locale
    version-1.0.0/                         <-- frozen v1.0.0, default locale
  versioned_sidebars/
    version-2.0.0-sidebars.json
    version-1.0.0-sidebars.json
  versions.json
  i18n/
    fr/
      docusaurus-plugin-content-docs/
        current/                           <-- current (unreleased), French
        version-2.0.0/                     <-- frozen v2.0.0, French
        version-1.0.0/                     <-- frozen v1.0.0, French
        current.json
        version-2.0.0.json
        version-1.0.0.json
    de/
      docusaurus-plugin-content-docs/
        current/
        version-2.0.0/
        version-1.0.0/
        current.json
        version-2.0.0.json
        version-1.0.0.json
```

You do not need to translate every version. Docusaurus falls back to the default locale for any version that has no translated content yet.

#### Setup Order

1. Configure and test versioning first, with no locales.
2. Add locale declarations and scaffold translation files.
3. Translate the current (unreleased) docs for each locale.
4. Translate versioned snapshots as time and contributors allow.

This order keeps each concern isolated while you are getting the setup right.

#### Scaffolding Translations for Versioned Docs

After tagging a version, run `write-translations` once per locale. The command is version-aware: when versioned docs exist, it generates a separate JSON file for each version's sidebar labels alongside `current.json`. Running it again after adding new sidebar categories is safe; it merges without overwriting.

```bash
cd docs
npm run docusaurus write-translations -- --locale fr
```

Then copy the frozen English docs into the locale's versioned folder manually and translate in place:

```bash
cp -r docs/versioned_docs/version-2.0.0/* \
      i18n/fr/docusaurus-plugin-content-docs/version-2.0.0/
```

Repeat for each additional locale.

#### Version Banner Translation

When both versioning and i18n are active, the version banners (the bars that appear on unreleased or archived version pages) also need to be translated. These keys are not generated by `write-translations` because they are only relevant when versioning is also enabled. Add them manually to each locale's `code.json`. The `{link}`, `{versionLabel}`, and `{siteTitle}` placeholders are interpolated at render time and must be preserved exactly.

`i18n/fr/code.json`:

```json
{
  "theme.docs.versions.latestVersionSuggestion": {
    "message": "Vous consultez la documentation de {link} ({versionLabel}).",
    "description": "The banner message suggesting the latest version"
  },
  "theme.docs.versions.unmaintainedVersionLabel": {
    "message": "{siteTitle}: version {versionLabel}, non maintenue.",
    "description": "The label for an unmaintained version banner"
  },
  "theme.docs.versions.unreleasedVersionLabel": {
    "message": "{siteTitle}: documentation non publiée ({versionLabel}).",
    "description": "The label for an unreleased version banner"
  }
}
```

#### Navbar Dropdowns When Both Features Are Active

Place the version dropdown on the left near the docs link (it is a documentation navigation concern) and the locale dropdown on the right near GitHub (it is a site-wide preference). This gives the navbar a logical left-to-right flow.

```js
navbar: {
  items: [
    {
      type: 'docSidebar',
      sidebarId: 'docsSidebar',
      position: 'left',
      label: 'Documentation',
    },
    {
      type: 'docsVersionDropdown',
      position: 'left',
    },
    {
      type: 'localeDropdown',
      position: 'right',
    },
    {
      href: 'https://github.com/[organizationName]/[projectName]',
      label: 'GitHub',
      position: 'right',
    },
  ],
},
```

#### Full `docusaurus.config.js` Combined Example

```js
const config = {
  title: 'Your Project Name',
  tagline: 'A short description of your project.',
  favicon: 'img/favicon.ico',

  url: 'https://[organizationName].github.io',
  baseUrl: '/[projectName]/',
  organizationName: 'your-github-username',
  projectName: 'your-repo-name',

  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'fr', 'de'],
    localeConfigs: {
      en: { label: 'English' },
      fr: { label: 'Français', direction: 'ltr' },
      de: { label: 'Deutsch', direction: 'ltr' },
    },
  },

  presets: [
    [
      'classic',
      {
        docs: {
          routeBasePath: '/',
          sidebarPath: './sidebars.js',
          editUrl: 'https://github.com/[organizationName]/[projectName]/tree/main/docs/',
          lastVersion: '2.0.0',
          onlyIncludeVersions: ['current', '2.0.0', '1.0.0'],
          versions: {
            current: {
              label: 'Next (Unreleased)',
              path: 'next',
              banner: 'unreleased',
            },
            '2.0.0': {
              label: '2.0.0',
              path: '/',
            },
            '1.0.0': {
              label: '1.0.0',
            },
          },
        },
        blog: false,
      },
    ],
  ],

  themeConfig: {
    titleDelimiter: '|',
    titleTemplate: 'Your Project Name | %s',
    navbar: {
      title: 'Your Project Name',
      logo: {
        alt: 'Your Project Name Logo',
        src: 'img/logo.png',
      },
      items: [
        {
          type: 'docSidebar',
          sidebarId: 'docsSidebar',
          position: 'left',
          label: 'Documentation',
        },
        {
          type: 'docsVersionDropdown',
          position: 'left',
        },
        {
          type: 'localeDropdown',
          position: 'right',
        },
        {
          href: 'https://github.com/[organizationName]/[projectName]',
          label: 'GitHub',
          position: 'right',
        },
      ],
    },
    // ... footer config unchanged ...
  },
};
```