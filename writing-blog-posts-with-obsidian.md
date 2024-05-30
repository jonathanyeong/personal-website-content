---
pubDate: 2024-05-31
title: Writing blog posts with Obsidian
description: How I use Obisidan to make my Markdown look pretty
featured: false
draft: true
---

I've been writing a lot of Markdown lately. And I have two thoughts:

1. I love writing Markdown
2. I hate looking at Markdown

When I write pure Markdown, I feel like it's too noisy. I've tried some editors that mirror Markdown like [VSCode with the Markdown preview](https://code.visualstudio.com/docs/languages/markdown#_markdown-preview), or using an online Markdown editor like [StackEdit](https://stackedit.io/). I couldn't deal with the split, my eyes didn't know what to look at! There's probably live Markdown previews online as well. But copy-and-pasting content from the browser to a file was too many steps. 

Then I found [Obsidian](https://obsidian.md/). It's a writing app that's popular in the PKM (personal knowledge management) space. You can use it to organise your thoughts and link together different concepts. It fits perfectly in my workflow because it provides a distraction-free environment, live preview, and page templates.

*Pic here*
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

```
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

## What's next?

My main gripe with my workflow is media management. It's non-existent. I'd love to add more images or GIFs to my posts. But there are many steps to insert an image. There are also a few hoops I have to go through to publish a post. I added some automation to help, planning to write about that soon! Finally, I've been looking at using [TinaCMS](https://tina.io/), a headless CMS that can push to Git, more on that to come as well. 