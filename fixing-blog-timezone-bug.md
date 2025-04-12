---
pubDate: 2023-07-17
updatedDate: 2024-05-30
title: Fixing a Timezone bug on my blog
description: I had a bug on my blog where the published date displayed was different then the published date I added in my markdown. It came down to a timezone issue!
featured: false
draft: false
topics:
  - Astrojs
	- javascript
---

I noticed a weird bug on my blog the other day. The dates that I set in my blog post metadata didn't line up with the dates that I saw on the page. It was always a day off. For example, the front matter for my blog post showed days:

```yaml
---
pubDate: 2023-07-11
updatedDate: 2023-07-10
---
```

But the dates displayed on my blog post was:

```
Published Jul 10, 2023 | Updated Jul 9, 2023
```

This bug felt like a timezone error. It's weird that it was off by one day. But I wasn't sure, so I went through my debug steps.

I console logged `pubDate` and `updatedDate` everywhere and I got these values:

```
updatedDate is: 2023-07-10T00:00:00.000Z
pubDate is: 2023-07-11T00:00:00.000Z
```

Which meant I wasn't doing some weird conversion when pulling out the metadata from my blog post.

The only place where it was changing the date was in this snippet:

```javascript
{
	date.toLocaleDateString('en-us', {
		year: 'numeric',
		month: 'short',
		day: 'numeric',
	})
}
```

So I dug into the `toLocalDateString` docs. It turns out `toLocaleDateString` was using my local timezone. I had totally missed this line from the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString):

> The **`toLocaleDateString()`** method returns a string with a language-sensitive representation of the date portion of the **specified date in the user agent's timezone**

Unfortunately, there didn't seem like an easy way to keep the UTC timezone when displaying the dates with `toLocaleDateString()`. Since my date formatting needs were pretty basic, I could use `toUTCString()` instead:

```javascript
const formatDate = (date: Date) => {
	const dateParts = date.toUTCString().split(" ");
	const monthIndex = 2;
	const dayIndex = 1;
	const yearIndex = 3;

	return `${dateParts[monthIndex]} ${dateParts[dayIndex]}, ${dateParts[yearIndex]}`
}
```

It's not pretty but it works without needing to pull in another library. If I wanted to have fancier date formatting I could use something like [date-fns](https://date-fns.org/). But I found a gotcha with date-fns

> Since `date-fns` always returns a plain JS Date, which implicitly has the current system's time zone, helper functions are needed for handling common time zone related use cases. ([source](https://date-fns.org/v2.30.0/docs/Time-Zones))

So if I wanted to display a UTC time on my current system which is set to EST. Then I would need to pull in [date-fns-tz](https://github.com/marnusw/date-fns-tz) instead. This library lets me specify which IANA timezone and uses [the Intl API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl). Here's what that change would look like:

```javascript
import { formatInTimeZone } from 'date-fns-tz'

const formatDate = (date: Date) => {
	return formatInTimeZone(date, 'Etc/UTC', 'MMM dd, yyyy');
}
```

This bug was a reminder that timezones are the worst.

---

P.S: I had this post sitting in draft for about a year, and I have no idea why.