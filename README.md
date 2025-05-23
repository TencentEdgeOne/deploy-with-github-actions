# Next.js Project Deployment to EdgeOne Pages

This is a Next.js template project that uses GitHub Actions for building and deployment to [EdgeOne Pages](https://edgeone.ai/products/pages).

## Project Overview

This project provides a starter template for Next.js applications, integrating GitHub Actions workflows and automatic deployment functionality.

Complete `.github/workflows/deploy.yml` configuration is as follows:

```yml
name: Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
      
      - name: Install dependencies
        run: npm install
      
      - name: Build project
        run: npm run build
      
      - name: Deploy to EdgeOne Pages
        run: npx edgeone pages deploy ./out --name my-edgeone-pages-project --token ${{ secrets.EDGEONE_API_TOKEN }}
        env:
          EDGEONE_API_TOKEN: ${{ secrets.EDGEONE_API_TOKEN }}
```

## Step 1: GitHub Actions Build

This project uses [GitHub Actions](https://docs.github.com/en/actions) for automated building. When code is pushed to the main branch, it triggers the following build process:

1. Checkout the repository code
2. Setup Node.js 20 environment
3. Install project dependencies
4. Build the project using Next.js

## Step 2: EdgeOne Pages Deployment

After the build completes, the project is automatically deployed to [EdgeOne Pages](https://edgeone.ai/products/pages) through the following process:

1. The build phase generates the `./out` directory
2. The EdgeOne command line tool is used for deployment:
   ```bash
   npx edgeone pages deploy ./out --name my-edgeone-pages-project --token ${{ secrets.EDGEONE_API_TOKEN }}
   ```

## Setting Up the Repository Secret

To deploy to EdgeOne Pages, you need to add your `EDGEONE_API_TOKEN` as a repository secret in GitHub:

1. Navigate to your GitHub repository
2. Go to "Settings" > "Secrets and variables" > "Actions"
3. Click "New repository secret"
4. Name: `EDGEONE_API_TOKEN`
5. Value: Your EdgeOne API token

Obtain your `EDGEONE_API_TOKEN` from EdgeOne Pages by visiting: https://edgeone.ai/document/177158578324279296

# References

- EdgeOne Pages Documentation: https://edgeone.ai/docs/pages
- GitHub Actions Documentation: https://docs.github.com/en/actions
