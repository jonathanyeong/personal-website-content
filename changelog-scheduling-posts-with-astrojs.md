---
pubDate: 2024-06-03
updatedDate: 2024-06-03
title: Scheduling Posts with AstroJS
description: Quick demo on how I schedule posts with AstroJS
featured: false
draft: false
topics:
  - Astrojs
  - changelog
---
Before going on a trip this weekend, I wrote multiple posts so I didn't need to write on my trip. However, to keep my writing streak, I had to manually release posts by setting their draft property to `false`. That sucked. Today I released scheduling posts so I don't need to go through this pain again. Here's how I did it.

## Implementing scheduled posts

A scheduled post on my site is any post that is NOT in draft, with a `pubDate` that is in the future. I want to display a scheduled post when the `pubDate` is in the past. With the previous sentence in mind, I created a file that exported a method `hasPubDatePassed`. 

```js
import type { CollectionEntry } from 'astro:content';
import { formatInTimeZone } from 'date-fns-tz'

const formatDate = (date: Date): string => {
	return formatInTimeZone(date, 'Etc/UTC', 'yyyy-MM-dd');
}

const hasPubDatePassed = (post: CollectionEntry<"blog">) => {
	let todaysDate = new Date()
	todaysDate.setHours(0, 0, 0, 0)
	return formatDate(todaysDate) >= formatDate(post.data.pubDate);
};

export default hasPubDatePassed
```

I used this method in my `filterPublishPosts` helper ([source](https://github.com/jonathanyeong/personal-website/blob/main/src/utilities/filterPublishedPosts.ts))
```js
const filterPublishedPosts = (allPosts: CollectionEntry<"blog">[]): CollectionEntry<"blog">[] => {
  return allPosts.sort(
    (a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf()
  ).filter((post) => import.meta.env.DEV || !isDraftPost(post) && hasPubDatePassed(post));
}
```

Where this `filterPublishedPosts` method sorts the posts and then displays only those that are not in draft and who's `pubDate` is in the past.

Note: `import.meta.env.DEV` is used to display all posts in local development.

### Timezone Gotcha's

After a [bug with timezones](https://jonathanyeong.com/fixing-blog-timezone-bug/) sometime last year, I knew it wasn't enough to do a date comparison like below. `todaysDate` would have a timestamp but `pubDate` doesn't have a timestamp. If we applied timezones to these two dates we could publish a post earlier then expected.

```js
// ❌: 🕛 Timezone issues
const hasPubDatePassed = (post: CollectionEntry<"blog">) => {
	let todaysDate = new Date()
	return todaysDate >= post.data.pubDate;
};
```

To avoid dealing with timezones, I set the time of today's date to `00:00`. This time matches the time of `pubDate`. After setting the time, I convert both dates into UTC timezone, and output strings in a format like `2024-05-30`. Finally, I do a string comparison that will return all posts with pubDate today or in the past. 

```js
// ✅: 🕛 Sets both times to UTC
const hasPubDatePassed = (post: CollectionEntry<"blog">) => {
	let todaysDate = new Date()
	todaysDate.setHours(0, 0, 0, 0)
	return formatDate(todaysDate) >= formatDate(post.data.pubDate);
};
```

---
I have a scheduled post for tomorrow to test this feature 🤞 it all works. *Note to self: automated tests would be nice*.