---
title: "Setting up Hugo with GitHub Actions & Blowfish"
date: 2026-03-10
description: "A comprehensive guide on deploying your academic website using Hugo and GitHub Actions."
layout: "post"
tags: ["Hugo", "GitHub Actions", "WebDev", "CI/CD"]
---

### Why GitHub Actions for Hugo?
Manually building and uploading your `public/` folder is a thing of the past. Using **GitHub Actions**, every time you push content, the site is built and deployed automatically. This is especially useful for **Blowfish** theme users because it requires specific Hugo versions and submodule handling.

#### 1. Configure the Workflow
Create a file at `.github/workflows/hugo.yml`. Here's a version compatible with the Blowfish theme (requiring Hugo `0.145.0+`):

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches:
      - master # Adjust this to your branch name (e.g., main or master)
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.145.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb
          sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive # CRITICAL for themes added as submodules
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### 2. Handling Submodules
If you don't use Hugo Modules, you must add your theme as a git submodule:
```bash
git submodule add https://github.com/nunocoracao/blowfish.git themes/blowfish
```
Without this, the GitHub runner will see an empty `themes/` folder and fail the build.

#### 3. Common Troubleshooting
- **Error: function "try" not defined**: Happens if your Hugo version is too old. Upgrade `HUGO_VERSION` in your yaml to at least `0.145.0`.
- **Empty repo/Actions tab**: Ensure your workflow branch (`main` vs `master`) matches your repository's branch.
- **Theme not found**: Ensure `submodules: recursive` is set in the checkout step.

Happy coding!
