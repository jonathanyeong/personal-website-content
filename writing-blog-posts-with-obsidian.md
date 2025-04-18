---
pubDate: 2024-06-02
updatedDate: 2025-04-12
title: Writing blog posts with Obsidian
description: My end to end writing workflow for my blog using Obsidian as the main editor. From how I setup Obsidian for writing, image management, and automating git pushes.
featured: true
draft: false
topics:
  - tutorial
---

I've been writing a lot of Markdown lately. And I have two thoughts:

1. I love writing Markdown
2. I hate looking at Markdown

When I write pure Markdown, I feel like it's too noisy. I've tried some editors that mirror Markdown like [VSCode with the Markdown preview](https://code.visualstudio.com/docs/languages/markdown#_markdown-preview), or using an online Markdown editor like [StackEdit](https://stackedit.io/). I couldn't deal with the split, my eyes didn't know what to look at! There's probably live Markdown previews online as well. But copy-and-pasting content from the browser to a file was too many steps.

Then I found [Obsidian](https://obsidian.md/). It's a writing app that's popular in the PKM (personal knowledge management) space. You can use it to organise your thoughts and link together different concepts. It fits perfectly in my workflow because it provides a distraction-free environment, live preview, and page templates.

## Setting up the blog post workflow
To set up the workflow, I needed to symlink two folders. One was to my content folder in my Astro project. And the other was to the folder I created for blog posts in Obsidian.

Opening up my terminal, I first got the path to my content folder. Then `cd` into my Obsidian folder. This folder was set up when I created a new "Vault" (see [Obsidian Manage Vaults doc]( https://help.obsidian.md/Files+and+folders/Manage+vaults) for more information). I then created a new folder, and symlinked the two.

```bash
cd <path-to-obsidian-vault>
mkdir BlogPosts
ln -s <path-to-content-folder> BlogPosts
```

Now, any new posts created in the `BlogPosts` folder in Obsidian will also appear in my `path-to-content-folder`.

## Setting up a template
Obsidian has a built-in Template plugin that I use for all my posts. Learn more in the [Obsidian Template documentation](https://help.obsidian.md/Plugins/Templates).

I have a folder named `Templates` that contains a file `blog-post-template`. The template is common front matter I use for all my posts:

```yaml
---
pubDate: {{date}}
updatedDate: {{date}}
title: "template"
description: "A template"
featured: false
draft: true
topics: []
---
```

## Adding images to posts

Initially, I added image by dropping the file into my Astro project than referencing the file in Obsidian. This is clunky for a couple of reasons.

- My content is separate from my website. Meaning I have to sync git pushes to ensure my images aren't breaking on publish.
- I have to jump back and forth between apps to get the image details.

A better alternative is to drop a link to an image. Cloudinary is a popular tool to manage media, and there's a [Cloudinary Uploader plugin](https://jordanhandy.github.io/obsidian-cloudinary-uploader/) that will upload your image to cloudinary and return a link in one step.

You do need a Cloudinary account and an API key, but after some setup it's been working flawlessly.

## Pushing posts to Git

After writing a post, I would switch to my terminal and run a few commands to push the post to the Git repository. Running these commands became anonying quickly. So I built a [small Obsidian plugin](https://github.com/jonathanyeong/obsidian-publish-post) that would run the git push for me!

## What's next?

I've tried using [Tina CMS but unfortunately it didn't fit with my workflow](https://jonathanyeong.com/tina-cms-a-retrospective/). But moving to a CMS would be a logical next step. Media management is still a pain point. I also want to start building components to use in MDX files, and I know Obsidian won't handle that out of the box.