---
pubDate: 2024-06-03
updatedDate: 2024-06-03
title: "Changelog: Scheduling Posts with AstroJS"
description: Quick demo on how I schedule posts with AstroJS
featured: false
draft: true
topics:
  - Astrojs
  - changelog
---
I had a writing marathon recently because I was going away over the weekend. Yet I still wanted to release posts because I didn't want to break my writing streak. To release posts, I implemented scheduling. [AstroJS](https://astro.build/) makes this process relatively straightforward. But the biggest gotcha is timezones. 

*You can view the [commit here](https://github.com/jonathanyeong/personal-website/commit/d9e779cf6a298b22ad0b5738305a91114a80847f).*

---

Currently, my blog posts are displayed on my homepage if they are not in draft:

```js
// src/pages/index.astro
const posts = (await getCollection('blog')).sort(
	(a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
).filter((post) => showDraftPosts(post));
```

`showDraftPosts` is defined:

```js
// src/utilities/showDraftPosts.ts
import type { CollectionEntry } from 'astro:content';

const showDraftPosts = (post: CollectionEntry<"blog">) => {
	return import.meta.env.DEV || !post.data.draft
};

export default showDraftPosts
```

To set up a filter for publish date, I added a new method `showScheduledPosts` to `index.astro`

```js
// src/pages/index.astro
const posts = (await getCollection('blog')).sort(
	(a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
).filter((post) => showDraftPosts(post) && showScheduledPosts(post));
```

`showScheduledPosts` is defined:

```js
// src/utilities/showScheduledPosts.ts
import type { CollectionEntry } from 'astro:content';
import { formatInTimeZone } from 'date-fns-tz'

const formatDate = (date: Date): string => {
	return formatInTimeZone(date, 'Etc/UTC', 'yyyy-MM-dd');
}

const hasPubDatePassed = (pubDate: Date): boolean => {
	let todaysDate = new Date()
	todaysDate.setHours(0,0,0,0)
	return formatDate(pubDate) <= formatDate(todaysDate);
}

const showScheduledPosts = (post: CollectionEntry<"blog">) => {
	return import.meta.env.DEV || hasPubDatePassed(post.data.pubDate);
};

export default showScheduledPosts
```

Let's break down what's happening in `showScheduledPosts.ts`.

After a [bug with timezones](https://jonathanyeong.com/fixing-blog-timezone-bug/) sometime last year, I knew it wasn't enough to do a date comparison like below:

```js
pubDate <= new Date()
```

To avoid dealing with timezones, I set the time of todays date to `00:00`. This time matches the time of the `pubDate` on the blog posts. After setting the time, I convert the date into UTC timezone, and output strings in a format like `2024-05-30`. Finally, I do a string comparison that will return all posts with pubDate today or in the past. 