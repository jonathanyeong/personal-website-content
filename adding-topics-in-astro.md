---
pubDate: 2023-07-11
title: "Adding Topics (aka tags) in Astro"
description: "A short how to guide on adding topics to your Astro site. Part of Virtual Coffee Build in Public challenge"
featured: false
draft: true
topics: ["Tutorials", "Astrojs"]
---

This month's Virtual Coffee challenge is [Build in Public](https://dev.to/virtualcoffee/join-virtual-coffee-for-the-build-in-public-the-power-of-daily-standup-and-demo-challenge-35kb). And I wanted to kick things off with some website updates. I've been meaning to add tags to my site for a long time. For anyone who's peeked at my source code, I've had a `_tag` folder as well as a header link setup but commented out. 
![Hiding my tag header link in HTML](../../assets/hidden-tag-html.png)
After poking around at other people's sites, I decided to switch from the concept of tagging to the concept of topics. While the naming is different the process to adding topics was the same. In this post I want to share how I added topics, what I learnt, and what's next. 

## How I added topics to my site
I mainly followed the [Astro guide on building a tag index page](https://docs.astro.build/en/tutorial/5-astro-api/3/](https://docs.astro.build/en/tutorial/5-astro-api/3/).

Firstly, I created this structure in my project:

```
pages/
	topics/
		[topic].astro
		index.astro
```

`index.astro` is rendered when you go to `/topics` ([example link](https://jonathanyeong.com/topics/)).

`[topic].astro` is rendered when you go to `/topics/:topic`. Where `:topic` is something I define in my blog posts. Example, `topics/api%20design/` ([example link](https://jonathanyeong.com/topics/api%20design/)).

I then added the `topics` field to my front matter yml in my blog posts:
```yml
---
pubDate: 2023-04-05
updatedDate: 2023-04-05
title: "template"
description: "A template"
draft: true
topics: []
---
```

Added `topics` key in my `config.ts` file ([source code](https://github.com/jonathanyeong/personal-website/blob/main/src/content/config.ts#L21))
```javascript
topics: z
	.array(z.string())
	.optional()
	.default([])
```

Finally, to display the topics on my blog post, I made a component that is pulled into the Blog Post Layout:

[BlogPost.astro](https://github.com/jonathanyeong/personal-website/blob/main/src/layouts/BlogPost.astro#L25)
```javascript
...
{topics && topics.length > 0 && <TopicList topics={topics} />}
...
```

[TopicList.astro](https://github.com/jonathanyeong/personal-website/blob/main/src/components/TopicList.astro)
```html
---
const { topics } = Astro.props;
---  

<p class="flex space-x-3">
{topics.map((topic: string) => (
	<a href={`/topics/${topic}`}>{topic}</a>
))}
</p>
```

### Deep dive (aka what did I learn)

Digging into the `index.astro` and `[topic].astro` files a bit more.

#### index.astro
The way we get a list of topics to display on the `/topic` page is through this code ([source code](https://github.com/jonathanyeong/personal-website/blob/main/src/pages/topics/index.astro#L9)):

```javascript
const posts = (await getCollection('blog'))
const topics = [...new Set(posts.flatMap((post) => post.data.topics))];
```

This line will get all my posts from my blog folder, pull out all of the topics, and return an array of topics with no duplicates. 

#### [topic].astro
To generate the corresponding topic endpoints like `/topic/api%20design/` I needed to modify the `getStaticPaths()` function in the `[topic].astro`. ([source code](https://github.com/jonathanyeong/personal-website/blob/main/src/pages/topics/%5Btopic%5D.astro#L7)). I forgot to modify it originally, and I was very confused when I kept getting a 404 not found error when trying to navigate to a topic page 🤦🏻!

```javascript
export async function getStaticPaths() {
	const posts = (await getCollection('blog'))
	const topics = [...new Set(posts.flatMap((post) => post.data.topics))];
	return topics.map((topic) => ({
		params: { topic }
	}));
}
```

[getStaticPaths()](https://docs.astro.build/en/reference/api-reference/#getstaticpaths) is an Astro function that Astro uses to generate each page at build time. It returns an array that looks something like this:

```javascript
[
	{ params: {topic: "API Design"} },
	{ params: {topic: "Productivity"} }
]
```


## What's next
I'm not super happy with how my topics are styled. But I'm also not sure how I would style them - I'm open to suggestions! Next up for me is writing an About me page. I don't know why but I find it incredibly hard to write! I'll be chugging away at it while trying to come up with a better styling in the meantime.