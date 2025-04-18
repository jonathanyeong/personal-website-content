---
pubDate: 2024-05-30
updatedDate: 2024-05-30
title: You know what they say about assumptions
description: I should know better then to assume my code works
featured: false
draft: false
topics:
  - career
---
Last year, I added a [comments feature to my blog](https://jonathanyeong.com/changelog-added-comments-to-blog/). During development, I verified the change by checking that the comments section existed on a post. It did, #shipit. After almost a year, someone told me that their comments and reactions weren't showing. It turns out that I had set up [Giscus](https://giscus.app/) incorrectly from the start 😫! And if that person hadn't said anything, I would've been clueless.

The fix was trivial, I changed `data-categories` in my Giscus setup to point to the correct discussion category `"Blog Post Discussions"`, not `"Announcements"` ([here's the commit](https://github.com/jonathanyeong/personal-website/commit/6adde2be2b30b28869367dde2045abe6c942d893)).

I took one lesson away from this experience. Something I probably should've already known.

> Just because it's a personal project doesn't mean I should take shortcuts with testing

Here's the test cases I should've run through:

- Does the comment section load when I view a blog post?
- Can I post a comment?
	- Does the comment section show the new comment?
	- Can I put images/gifs in the comment?
- Can I add a reaction?
	- Does the reaction show?
- If a comment is posted, do I get a notification?
- Can I reply to a comment?
	- Does the reply show up with the comment?

Writing out the test cases, it feels obvious that I should've gone through them. I shouldn't release something that's untested. **But these things happen**. Especially for projects where I'm rushing, tired, or overconfident. Maybe a combination of all three. At least now the comments work, I manually verified the test cases. Happy days.