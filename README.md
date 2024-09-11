<!-- PROJECT LOGO -->
<br />
<p align="center">
    <h1 align="center"> GitHub to GitBucket Action </h1>
    <h6 align="center"> September 2024 </h6>

  <p align="center">
   Here is a GitHub action for pushing commits made on a GitHub repository to a GitBucket repository. 
    <br />
  </p>
</p>

___

[![Mirror to GitBucket](https://github.com/JamesRobionyRogers/GitHub-to-GitBucket-Action/actions/workflows/push-to-gitbucket.yml/badge.svg)](https://github.com/JamesRobionyRogers/GitHub-to-GitBucket-Action/actions/workflows/push-to-gitbucket.yml)


## Overview 

This action is designed to push commits made on a GitHub repository to a GitBucket repository whilst maintaining the original commit author and history. 

## Usage

Implementing this action is simple. Here is a step by step guide to get you started:

1. Create a new workflow file named `push-to-gitbucket.yml` in your `.github/workflows` directory.
```yaml
name: Mirror to GitBucket

on:
  push:
    branches:
      - "*"

jobs:
  mirror:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Push to GitBucket
        env:
          # Set the GitBucket URL, username and password as secrets in your repository settings
          GITBUCKET_URL: "gitbucket.example.com/username/repo.git"
          GITBUCKET_USERNAME: ${{ secrets.GITBUCKET_USERNAME }}
          GITBUCKET_PASSWORD: ${{ secrets.GITBUCKET_PASSWORD }}
        run: |
          git config --global user.name "GitHub Actions"
          git config --global user.email "actions@github.com"

          git remote add gitbucket https://$GITBUCKET_USERNAME:$GITBUCKET_PASSWORD@$GITBUCKET_URL
          git push --mirror gitbucket
```

2. Replace the `GITBUCKET_URL` with the URL of your GitBucket repository. 

> [!NOTE]  
> Ensure that the URL does NOT contain the `https://` prefix. as this is added on line 27 of the workflow file.

3. Add the `GITBUCKET_USERNAME` and `GITBUCKET_PASSWORD` as secrets in your repository settings.
     - Navigate to your repository on GitHub.
     - Click on `Settings` tab.
     - Click on `Secrets and variables` in the left sidebar, then `Actions`.
     - Click on the green `New repository secret` button.
     -  Add the `GITBUCKET_USERNAME` and `GITBUCKET_PASSWORD` as secrets, with the values being your GitBucket username and password respectively.

4. Push the the new `push-to-gitbucket.yml` file to your GitHub repository and watch as your commits are mirrored to your GitBucket repository.