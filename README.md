# Elata Documentation

[![Docs Status](https://img.shields.io/badge/docs-ready-brightgreen)]() [![License](https://img.shields.io/badge/license-MIT-blue)]()

Welcome to the Elata Biosciences documentation repository. This repo contains the user, developer, and product documentation that powers our public and internal docs sites.

Table of contents
- [About](#about)
- [Audience](#audience)
- [Getting started](#getting-started)
  - [Clone the repo](#clone-the-repo)
  - [Preview locally (common options)](#preview-locally-common-options)
- [Repository structure](#repository-structure)
- [How to contribute](#how-to-contribute)
  - [Editing pages](#editing-pages)
  - [Branching and PRs](#branching-and-prs)
  - [Style guide & frontmatter](#style-guide--frontmatter)
- [Publishing & CI](#publishing--ci)
  - [Example GitHub Actions (preview & publish)](#example-github-actions-preview--publish)
- [Versioning docs](#versioning-docs)
- [Support & contact](#support--contact)
- [License & acknowledgements](#license--acknowledgements)

---

## About
This repository holds all documentation content for Elata Biosciences including:
- Product user guides
- Developer guides and APIs
- Release notes and change logs
- Internal onboarding and process docs

The docs are intended to be rendered into a static site and hosted (GitHub Pages, Netlify, or similar).

## Audience
- End users looking for product guidance
- Engineers and integrators using our API/SDKs
- Internal team members (onboarding, SOPs)
- Partners and regulators (as applicable)

## Getting started

### Clone the repo
```bash
git clone https://github.com/Elata-Biosciences/docs.git
cd docs
```

### Preview locally (common options)
This repo aims to be renderer-agnostic. Below are commands for the most common doc site tools. If your project already uses one, follow that section; otherwise pick one and I can help wire it up.

- MkDocs (Python)
```bash
pip install mkdocs mkdocs-material
mkdocs serve
# open http://127.0.0.1:8000
```

- Docusaurus (Node.js)
```bash
npm install
npm start
# open http://localhost:3000
```

- Static HTML (Jekyll / Hugo / plain)
Follow the project's README or the specific generator's docs.

If your repository includes a `package.json`, `mkdocs.yml`, `docusaurus.config.js`, or similar, use the corresponding commands above. If you want, I can inspect the repo and give exact commands for the chosen generator.

## Repository structure
A typical structure (adjust to this repo's actual layout):
```
/docs                # markdown content, organized by topic
/site                # generated site (usually in .gitignore)
/assets              # images, diagrams, downloads
/.github/workflows   # CI workflows for preview & publish
mkdocs.yml           # or docusaurus.config.js
README.md
CONTRIBUTING.md
```

Key conventions:
- Keep each page focused on a single topic.
- Use folders for logical grouping (e.g., /user-guides, /developer, /api).
- Store images under `/assets/images/<topic>/` and reference them with relative links.

## How to contribute

### Editing pages
- Create a new branch for your change: `git checkout -b docs/<short-description>`
- Edit or add Markdown files under `docs/` (or the repo's docs folder)
- Keep images under `/assets/images/` and reference them relatively:
  ```md
  ![Diagram](../assets/images/featureX/flow.png)
  ```
- Add meaningful frontmatter if your static site generator requires it (examples below).

### Branching and PRs
- Branch naming: `docs/<feature|bug|typo>-<short-desc>` (e.g., `docs/typo-installation`)
- Open a pull request against `main` (or the repository's default branch).
- Add reviewers from the docs and product teams.
- Include screenshots or a link to a preview if available.

### Style guide & frontmatter
Tone & style
- Use plain language and active voice.
- Prefer short paragraphs, bullet lists, and examples.
- Use consistent casing for headings and labels.
- Use code blocks for commands and snippets.

Frontmatter examples:

- MkDocs / Material (YAML frontmatter is optional; use if you need per-page metadata):
```yaml
---
title: "Getting started"
description: "How to install and start using the product"
sidebar: "main"
---
```

- Docusaurus (support for frontmatter in docs):
```yaml
---
id: getting-started
title: Getting Started
sidebar_position: 1
---
```

Accessibility & images
- Add alt text to images.
- Prefer SVG for diagrams where possible.
- Optimize image file sizes.

## Publishing & CI
Recommended approach:
- Use GitHub Actions to build and publish the site on push to `main` (or on release tags).
- Optionally use separate preview deployments for PRs (Netlify, Vercel, or GitHub Pages preview).

Example GitHub Actions notes:
- Build steps: install dependencies, run the generator (mkdocs build or npm run build)
- Publish steps: push the generated site to `gh-pages` or deploy to your hosting provider.

### Example GitHub Actions (preview & publish)
Below is a simple example that builds MkDocs and deploys to GitHub Pages. Adapt for Docusaurus or other tools as needed.

```yaml
name: Build & Deploy Docs (MkDocs)
on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - name: Install dependencies
        run: pip install mkdocs mkdocs-material
      - name: Build site
        run: mkdocs build
      - name: Upload artifact (for previews)
        if: github.event_name == 'pull_request'
        uses: actions/upload-artifact@v4
        with:
          name: site
          path: site/
      - name: Deploy to GitHub Pages
        if: github.ref == 'refs/heads/main'
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

If you use Docusaurus, replace the build/install steps accordingly (Node, `npm install`, `npm run build`, publish `build` directory).

## Versioning docs
- For breaking changes, create a new docs version (e.g., `/v2/`) or use the generator's built-in versioning features (Docusaurus has docs versioning).
- Keep a short, human-readable changelog in `CHANGELOG.md` or `docs/release-notes/`.

## Support & contact
- For doc content changes: @docs-owner (or list actual GitHub usernames)
- For technical or API clarifications: @engineering-lead
- For legal/regulatory content: @compliance

If you're unsure who to contact, open a PR and request a "docs" review; the team will triage.

## License & acknowledgements
- This repository is licensed under the MIT License (or insert appropriate license).
- Acknowledge third-party assets and tools used to build and host the docs.

---

Thank you for helping keep Elata Biosciences documentation accurate and useful. Contributions are welcome — please read [CONTRIBUTING.md](./CONTRIBUTING.md) for more info.
