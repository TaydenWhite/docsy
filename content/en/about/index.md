---
title: About Goldydocs
linkTitle: About
menu: {main: {weight: 10}}
---

{{% blocks/cover title="Hugo Docsy Tutorial" image_anchor="bottom" height="auto" %}}
This is a simple up to date tutorial on how to deploy a Docsy themed Hugo website from Github Pages
{.mt-5}

{{% /blocks/cover %}}

{{% blocks/lead %}}

# Step 1: Prerequisites

 Go to the Docsy: <a href="https://www.docsy.dev/docs/get-started/docsy-as-module/installation-prerequisites/" target="_blank">Before You Begin</a> page in order to download the correct prequisites.



{{% /blocks/lead %}}

{{% blocks/lead %}}

# Step 2: Run Your Website Locally

 Go to the Docsy: <a href="https://github.com/google/docsy-example" target="_blank">example-site</a> repository and click "Use this template" in order to make your own docsy repository. Git clone your repository into VS Code or any other IDE. Once there, you can type "hugo server" in the terminal **from the root folder of your project** in order to host your website locally.


{{% /blocks/lead %}}

{{% blocks/lead %}}

# Step 3: Deploy on Github Pages

Follow the instructions in Hugo's <a href="https://gohugo.io/hosting-and-deployment/hosting-on-github/" target="_blank">Host on GitHub Pages</a>. At Step 6, instead of using the yaml file provided, use the one below.

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.127.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Install Docsy
        run: npm install --save-dev docsy
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
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

Okay.
{{% /blocks/lead %}}