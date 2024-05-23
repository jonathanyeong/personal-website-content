---
pubDate: 2023-06-25
title: "Writing blog posts with Obsidian and ChatGPT"
description: "Sharing my blog post workflow using Obsidian and Chat GPT"
featured: false
draft: true
---

I usually have many challenges writing blog posts but there are two that I think are low hanging fruit

1. Starting with a blank page is scary
2. Writing in pure markdown is distracting

If you're anything like me and have a blog that has content based on markdown then you might have run into similar problems.

## The Editor
1. Explore the distraction-free writing environment in Obsidian 
2. Use of the live preview feature to focus on content rather than markdown syntax

I really enjoy markdown formatting and how expressive it is but what I dislike is all the extra noise when writing. I've played around with a few different options before like VSCode with a Markdown mirror or using an online markdown editor like StackEdit https://stackedit.io/. I've also used apps to help write markdown before and that's where Obsidian comes in.

The step that I wanted to save was trying to copy and paste posts from whatever source into my `posts` directory. And that's where symlinking comes in:

```bash
ln -s ~/personal-website-content BlogPosts
# I ran the above command inside my Obsidian folder
```

I can symlink an entire folder to my website content folder. Which means any new files that I create in the Obsidian `BlogPosts` folder will automatically appear in my `personal-website-content` folder. 

I use the template plugin and natural date in Obsidian to help me standardize the front matter across posts.

## Using Chat GPT
1. Overcoming writers block
2. Refinement and editing
I've also been using Chat GPT regularly. For example in my last post about testing I asked Chat GPT to find any inconsistencies or contradictions. I've also used ChatGPT to generate rough outlines. I also asked it for some prompts:

1. "Can you suggest some ideas for a blog post on [topic]?"
2. "What are the key benefits of [concept/technology] that I can highlight in a blog post?"
3. "I'm writing a blog post about [subject]. Could you help me brainstorm some subtopics to cover?"
4. "Can you provide an outline for a blog post about [theme]?"
5. "I need an engaging introduction for a blog post about [topic]. Any ideas?"
6. "What are some relevant statistics or data points I can include in a blog post about [subject]?"
7. "I'm struggling with organizing my thoughts for a blog post. How can I structure it effectively?"
8. "Can you help me come up with a catchy title for a blog post about [topic]?"
9. "I'm looking for some unique angles or perspectives to explore in a blog post about [theme]. Any suggestions?"
10. "Could you provide me with a compelling conclusion for a blog post about [subject]?"

I also ask Chat GPT to rewrite large chunks of my post with my tone. This is not a copy and paste effort. I end up dismissing much of the rewrite because it sounds nothing like me. But it highlights the areas that I need to clarify. Chat GPT is like the untrained intern. It will do a good job with some menial tasks but you always want to double check its work.

Come up with a conclusion

Finally, I also use Chat GPT to come up with social media snippets that I could use. Again these sound nothing like me but it gives a good starting point.

Starting with a blank page is scary. Chat GPT helps get around that.

## Conclusion

Synthesizing your thoughts into a blog post is already a energy intensive action. Hope these tips make it easier for you.

> "In conclusion, the combination of Obsidian and Chat GPT offers an innovative and efficient solution to two common challenges faced by writers: the fear of starting with a blank page and the distraction of writing in pure markdown. Obsidian's graph-based organization and live preview feature empower writers to overcome the daunting task of beginning with an empty canvas while staying focused on content creation. Chat GPT serves as a valuable writing companion, providing ideas, suggestions, and alternative perspectives to refine and expand your blog post. By integrating these powerful tools into your writing workflow, you can unlock new levels of creativity, overcome writer's block, and streamline the writing process. Embrace the power of Obsidian and Chat GPT to enhance your writing journey, making it more productive, enjoyable, and fulfilling. Don't let the fear of a blank page or the distractions of markdown syntax hold you back. Embrace this innovative approach, experience the transformative impact it can have on your writing, and unleash your full potential as a writer."