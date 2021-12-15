<p align="center">
  <img src="https://i.ibb.co/W0N0vr0/white-nextjs-1.png" alt="Neverbland Logo"/>
</p>
<a href="https://github.com/kelvinakinolaajayi/blog-starter-ts-devops" title="Go to GitHub repo"><img src="https://img.shields.io/static/v1?label=kelvinakinolaajayi&message=blog-starter-ts-devops&color=red&logo=github" alt="kelvinakinolaajayi - blog-starter-ts-devops"></a>

 # Neverbland's Junior DevOps Challenge Documentation


## Basic Overview
This project involves building and deploying a statically generated Next.js blog site. The project also includes using Amazon Web Services as the destination for the blog site and making a choice on the best service for the task.

# Contents

- Prerequisites
- Arguments
- GitHub Actions Workflow
- AWS Service Considerations

# GitHub Actions Workflow
```yaml
# This is a basic workflow to help you get started with Actions

name: Static Site S3 Deploy

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 12.22.0
      - run: npm install -g yarn
      - run: yarn install --frozen-lockfile
      - run: yarn build
      - run: yarn next export
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-west-2
      - run: aws s3 sync ./out s3://junior-devops-challenge --acl public-read
```
The link to the YAML file can be located [here](https://github.com/kelvinakinolaajayi/blog-starter-ts-devops/blob/main/.github/workflows/deploy.yml)

## AWS Service Considerations
