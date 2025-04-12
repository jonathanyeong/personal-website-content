---
pubDate: 2024-06-05
updatedDate: 2024-06-05
title: Using Github Actions to fetch content
description: My content lives in a private repository separated from the main website repository. It got annoying to manually keep changes in sync. Instead, I used Github Actions to automate publishing posts.
featured: true
draft: false
topics:
  - automation
---
I recently added more automation around my blog post publishing workflow. Previously, I shared how I use [Obsidian to write blog posts](https://jonathanyeong.com/writing-blog-posts-with-obsidian/). In this post, I want to show you what automation I've added to help publish a post.

Before I dive in, I need to provide some context. This site is separated into two repos. A [website repo](https://github.com/jonathanyeong/personal-website), that's where all the AstroJS goodness lives. And then a [content repo](https://github.com/jonathanyeong/personal-website-content), that's where all my posts live. The content repo is a Git submodule inside the website repo. These two are separated for a few reasons:

- I can keep my content commits separate from my website commits.
- I have dedicated version control over my content.
- I have visibility controls over my content. Meaning, I can set my content to provide if I ever wanted to.

Unfortunately, with two separate repos, I have a few extra steps to publish. I need to push the new post to the content repo. Then pull in those changes to the website repo, before pushing again.
## Automation introduced
I used two Github Action scripts to automate fetching the latest content changes, and pushing to main.

In my content repo, I have a [script that runs whenever I push to main](https://github.com/jonathanyeong/personal-website-content/blob/main/.github/workflows/main.yml):
```yaml
name: Trigger deploy
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger parent repository update
        run: |
          curl -XPOST https://api.github.com/repos/jonathanyeong/personal-website/dispatches \
          -H 'Accept: application/vnd.github+json' \
          -u jonathanyeong:${{ secrets.ACCESS_TOKEN }} \
          --data '{"event_type": "deploy content changes"}'
```

This script will dispatch an event to my website repository whenever there's a push to main. The data field doesn't matter, and could be whatever event type you want. GitHub has more docs on [creating a repository dispatch event](https://docs.github.com/en/rest/repos/repos?apiVersion=2022-11-28#create-a-repository-dispatch-event).

In my website repo, I have a [script that listens to dispatch events](https://github.com/jonathanyeong/personal-website/blob/main/.github/workflows/fetch-content-updates.yml) and runs when it receives one.
```yaml
name: "Fetch Content Updates"
on: [repository_dispatch, workflow_dispatch]
jobs:
  pull_content_updates:
    name: pull_content_updates
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          token: ${{ secrets.ACCESS_TOKEN }}
          submodules: true
      - name: Pull submodule
        run: |
          git pull --recurse-submodules
          git submodule update --init --recursive --remote
      - name: Push changes
        run: |
          git config --global user.email "hey@jonathanyeong.com"
          git config --global user.name "Jonathan Yeong"
          git add .
          git commit -m "[Workflow] Updating website content"
          git push

```

The `repository_dispatch` handler will watch for dispatch events. When it gets one, it will pull down the latest changes from the submodule, and then push the updates to main. Here are the [GitHub docs on `repository_dispatch`](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#repository_dispatch). After pushing to main branch on my website repo, Netlify will build my site and publish it.

Note: I included `workflow_dispatch` so I can manually trigger a content fetch, and push for testing.

## What's next?

These GitHub Actions were my first foray into automating my workflow. And I'd love to do more. It'd be great to have a linter that would catch any markdown issues before I pushed. I also want to automate pushing posts to the content repo. It would be cool if I had a "publish" button in Obsidian that would do a commit and push for me.