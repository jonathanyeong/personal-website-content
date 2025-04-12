---
pubDate: 2024-06-12
updatedDate: 2024-06-12
title: Beginning my Developer Experience journey
description: In this post, I share my definition of Developer experience, how we measure it, and what we measure, as well as a note on survey building
featured: false
draft: false
topics:
  - DevX
---
For once, I'm excited about my job. I'm working on the Developer Experience (DevEx) team. I get to help other developers for a living! One of my first tasks is to understand the vibes with our current developer experience through a survey. But before I could start the survey, I had to learn some things.

## What is good DevEx
I know a good developer experience when I see it. But I've never defined it. Here's my stab:

> Good DevEx reduces friction on a developer's daily tasks.

It can look like better tooling, CI/CD, build time improvements, apps to help with pairing etc. A good developer experience makes for a productive developer.

## How do we measure DevEx
We can measure DevEx both qualitatively and quantitatively. It's important to have both. For example, we can pull objective data about build time. However, that account for any flaky test reruns, or merge queue times. It's easier and faster to get a measure of DevEx through qualitative data (i.e. surveys).

## What do we measure?
The two metric frameworks I found were [DORA](https://docs.gitlab.com/ee/user/analytics/dora_metrics.html) (DevOps Research and Assessment) and [SPACE](https://queue.acm.org/detail.cfm?id=3454124). SPACE stands for:

- Satisfaction and well-being
- Performance
- Activity
- Communication and collaboration
- Efficiency and flow

 DORA is focused on DevOps measures like deployment frequency, lead time for changes, change failure rate, and time to restore services. SPACE is all encompassing. DORA can be considered an instance of SPACE.

From a recent [Engineering Enablement, podcast episode](https://getdx.com/podcast/dora-space-devex-choosing-framework/),

> "DORA presents at least a starting point... I see DORA metrics having been really successful is for organizations that are focused on DevOps transformation or who are going from batched releases to continuous delivery"

For my DevEx measures, I'd rather not focus on DORA. There were other functions at my company that had these metrics. Instead, I wanted to answer these set of questions, inspired by SPACE.

- What is the engineering onboarding experience like? Can we reduce errors with setup?
- What does collaboration look like to you? Is it pairing, mentorship, both, or none?
- How do you test your changes? Are you confident that what goes into production will work as expected?
- How long does it take for a change to get pushed to production? How long is the feedback loop for you?
- What is the experience working in a monolith? Have you started committing changes to other teams domains?
- What tooling are you missing? What prevents you from answering positively to the above questions?

## Building the survey
The survey is segmented out by team, function (front end or back end), tenure, and level. I expanded the questions I shared above and gave them options based on the [Likert Scale](https://en.wikipedia.org/wiki/Likert_scale). Here's an example:

I have the tools necessary to get my work done
*1 - strongly disagree, 2 - disagree, 3 - neither agree nor disagree, 4 - agree, 5 - strongly agree*

Something I didn't realise about surveys was how hard they are to write. I want the result of the survey to help answer the questions I have in mind, that will be actionable, but also not be biased due to leading questions. I found [CultureAmp's Guide to Designing a Survey](https://support.cultureamp.com/en/articles/7048658-guide-to-designing-a-survey) a great resource.

---

I'm only beginning my DevEx journey, so there's a lot to learn. If you have any resources, please share! Here's a list of all the resources in this post and more:

- [Github DevEx](https://github.blog/2023-06-08-developer-experience-what-is-it-and-why-should-you-care/)
- [Microsoft DevEx](https://microsoft.github.io/code-with-engineering-playbook/developer-experience/)
- [Engineering Enablement Podcast](https://getdx.com/podcast/)
- [Gitlab DORA metrics](https://docs.gitlab.com/ee/user/analytics/dora_metrics.html)
- [The SPACE of developer productivity](https://queue.acm.org/detail.cfm?id=3454124)
- [Likert Scale](https://en.wikipedia.org/wiki/Likert_scale)
- [CultureAmp's Guide to Designing a Survey](https://support.cultureamp.com/en/articles/7048658-guide-to-designing-a-survey)


