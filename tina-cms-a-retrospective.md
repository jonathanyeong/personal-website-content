---
pubDate: 2024-06-08
updatedDate: 2024-06-08
title: "Tina CMS: a retrospective"
description: A retrospective on trying to implement Tina CMS into my workflow
featured: false
draft: false
topics:
  - reflection
---
I decided to try [TinaCMS](https://tina.io/) (aka Tina). It's another headless CMS that supports Git. Perfect for my workflow! It has built in media management, it can validate metadata fields on a post, and it can push directly to your Git repo. Unfortunately, it didn't fit with my flow, as much as I wanted it to. Here are the challenges I faced, and why I ultimately decided not to use Tina.
## Separate content repo woes

This blog consists of two repos; a website repo, and a content repo. The content repo is a flat folder with all my blog posts in Markdown. Luckily, TinaCMS works with a [separate content repo](https://tina.io/guides/tinacms/separate-content-repo/guide/). When you set up Tina, you add a `tina` folder that contains configuration for Tina cloud to read. This configuration is where you specify the metadata of your blog posts, the path to your content, among a few other settings.

Something that the docs don't mention is the requirement to have a `tina` folder in both the website repo, and the content repo. When you run `tinacms build` it will generate some files in your `tina` folder in the content repo. Unfortunately, this folder does not play nicely with AstroJS if you're using [content collections](https://docs.astro.build/en/guides/content-collections/). Including this folder caused the following error:

![AstroJS complaining about mixing content and data entries because the Tina config folder lived in my content repo](https://res.cloudinary.com/jonathan-yeong/image/upload/v1717901753/unsigned_obsidian_uploads/tnet5xinqxvhowxgmzxt.png)


I was able to get around this issue by removing the folder during my Netlify build step. But it was major pain point. With a hacky solution.

## Failing build on deploy previews

I didn't realise at the time, but out of the box, Tina doesn't support multiple branches. It comes as a [feature for the paid editorial workflow](https://tina.io/docs/tina-cloud/branching/). It wasn't a dealbreaker, but it was an inconvenience. Anytime I wanted to push a new feature, I couldn't test it out in a deploy preview. I would have to test in production.

## Flat file structure issues

After the separate content repo woes, and failed deploy previews, I was finally able to get Tina up and running. I could see, and edit my posts! But then I faced another blocker. I couldn't create posts. Whenever I pressed save, I would get the following error:

![Problem creating new documents in Tina cloud. It was due to a forward slash in front of my blog post slug](https://res.cloudinary.com/jonathan-yeong/image/upload/v1717901821/unsigned_obsidian_uploads/jmvokcplie5lazy1amwx.png)


The reason was because I set my `path` in Tina config to an empty string. I needed to set it as an empty string, so I could display my posts in the flat folder. However, Tina will add a `/` when saving a post, which will cause the call to GitHub to fail.

This blocker was the final nail in the coffin. [I opened an issue for it.](https://github.com/tinacms/tinacms/issues/4543). I'm hoping they'll accept my suggestion, or come up with a better approach in the future!

## Deciding not to use TinaCMS

The culmination of all the issues above, made me realise that Tina wasn't for me. I really wanted to get TinaCMS to work. I had a few back and forth's on Tina Discord in an attempt to resolve my issues. But last week, I decided to not continue with it. I doubled down on Obsidian as I started writing more. And I found the Obsidian Cloudinary plugin, which solved my media management problems. I had many learnings during this process. And I'm pretty confident saying, if you have an AstroJS site with a separate content repo, TinaCMS might not be for you.