# CI/CD Pipeline for Static Website with GitHub Actions

This repository contains a simple static website deployed automatically to GitHub Pages using a GitHub Actions CI/CD pipeline.

---

## Project Overview

- The website is a basic HTML page (`index.html`) that displays a welcome message.
- The goal was to automate deployment so that every time code is pushed to the `main` branch, the website updates automatically on GitHub Pages.
- The deployment is handled by a GitHub Actions workflow defined in `.github/workflows/deploy.yml`.

## Technologies Used
HTML

CSS

GitHub Actions (CI/CD)

GitHub Pages (Static Hosting)

---

## Development and Deployment Steps

### Step 1: Create a Web App

- Created a simple `index.html` with a heading message.👇
- Placed all files in the root directory of the repository.

### Step 2: Push Code to GitHub

- Initialized Git repository locally.
- Added and committed files.
- Pushed to GitHub remote repository on the `main` branch.
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/your-repo-name.git
git push -u origin main (follow the above steps)


### Step 3: Set Up GitHub Actions Workflow

- Created `.github/workflows/deploy.yml` with the following structure:
  
  ```yaml
  name: Deploy to GitHub Pages

  on:
    push:
      branches:
        - main

  jobs:
    deploy:
      runs-on: ubuntu-latest

      steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./  # root directory containing HTML files
## Errors Faced and Debugging
Initial Error: Workflow failed to run due to YAML syntax errors in the workflow file.

How I fixed: Carefully corrected indentation and syntax in .yml file.

Error: exit code 128 related to Git commands in workflow.

Cause: The default GitHub token permissions or repository settings caused permission issues for pushing deployment commits.

How I fixed: Updated workflow to use the official actions/deploy-pages@v2 action which handles permissions more reliably.

Error: Workflow failed with exit code 254 during build steps.

Cause: The workflow included unnecessary steps like npm install and npm run build for a static HTML site.

How I fixed: Removed build steps and simplified workflow to only checkout and deploy files.

Error: GitHub Pages source settings misconfigured.

How I fixed: Set GitHub Pages source to deploy from gh-pages branch via GitHub repository settings.

## Final Solution
Used this GitHub Actions workflow which works reliably for static sites:
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Pages
        uses: actions/configure-pages@v3

      - name: Upload HTML files
        uses: actions/upload-pages-artifact@v2
        with:
          path: .  # Upload everything in the root (HTML, CSS, etc.)

      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v2
## This workflow triggered automatically on each push to main and deployed the site successfully.
