---
pubDate: 2024-12-05
updatedDate: 2024-12-05
title: "Articles I Enjoyed #1"
description: Sharing some articles that I've enjoyed reading recently
featured: false
draft: false
topics:
  - personal
---
I'm trying to read more these days, and I wanted to share some posts that I enjoyed recently. If there's an article you think I should read or a blog I should follow, leave a comment or find me on [Bluesky](https://bsky.app/profile/jonathanyeong.com).

---

## [Why I write](https://brittanyellich.com/why-i-write/)
### [Brittany Ellich](https://brittanyellich.com/)
A lot of the reasons why Brittany writes resonate with me. Brittany's also on a posting spree this month with the [#blogvent challenge](https://bsky.app/profile/cassidoo.co/post/3lcbjdiahas2l).

> I write because it helps me to connect dots between the different things that I am reading, watching, and learning. Taking a topic and trying to form coherent sentences so that other folks can understand your thinking around it is a great way to synthesize your ideas.

## [Redesign: Version 7.0: Sidebars, light-dark, and Bluesky](https://www.taniarascia.com/redesign-version-7/)
### [Tania Rascia](https://www.taniarascia.com/)
I'm a huge fan of redesign posts. Reading about the thought process behind design decisions is fascinating. Go check out Tania's redesign if you haven't. It's really well done!

It also led me to another of Tania's posts [Tending to My Digital Garden](https://www.taniarascia.com/digital-gardening/) that comes back to why I write, even if I forget my why sometimes.:

> There are a lot of reasons contributing to less activity from me, but I think one of the major ones is that I've been treating all my blog posts like they have to be precious. They're not posts, I'll think, they're articles.

> It's weird that I feel weird about that, considering it's my blog, so what else could or should it consist of aside from whatever I want to put on it? ... it's just whatever I find interesting and whatever I'm working on

> So I'm writing this post to give myself permission to put whatever I want on the site.

## [Why pipes sometimes get "stuck": buffering](https://jvns.ca/blog/2024/11/29/why-pipes-get-stuck-buffering/)
### [Julia Evans](https://jvns.ca/)
I never thought I needed to know about why pipes get stuck. But it's happened to me before, and now I know why! Julia's posts are the perfect example of I had a problem, I figured out why it was a problem, and now I'll share it with everyone.

TIL how grep decides to buffer its output

>  Here’s how `grep` (and many other programs) decides to buffer its output:
>  - Check if stdout is a terminal or not using the `isatty` function
>    - If it’s a terminal, use line buffering (print every line immediately as soon as you have it)
>    - Otherwise, use “block buffering” – only print data if you have at least 8KB or so of data to print
