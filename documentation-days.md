---
pubDate: 2023-07-14
updatedDate: 2023-04-05
title: "Documentation Days"
description: "Documentation days or Doc Days is time you carve out with your team to find stale documentation and update it"
featured: false
draft: true
topics:
  - career
---

I cry a little inside every time someone finds a piece of [[five-reasons-stale-documentation|stale documentation]]. Keeping documentation up to date seems like a problem at every company I've been at. I want to propose an approach to tackle this problem. Documentation (or doc) days!

Doc days is a regularly occurring meeting with the entire team. We start in the morning with a synchronous meeting. In this meeting, we comb through our existing documentation looking for anything outdated. As people find outdated pieces they call dibs and let others know that they're going to be updating it. Once everyone has something to work on, we break and let people update async. At the end of the day, everyone gives an update on their progress.

Basically, updating documentation is the focus for the day!

Having this structure to doc days offers two benefits:

1. It spreads the load of documentation; breaking knowledge silos. You don't have one person who writes all the documentation.
2. It holds people accountable for updating the documentation. You're socially committing to doing something.

My team and I have run this a few times to some success. One of the challenges we faced has been the time investment. Can you really spend an entire day with a team working only on documentation? As a dev, I'd argue yes. But as a business, I'm not sure they would agree. Fighting the good fight is a bit out of scope here, but there have been some ways we've been able to mitigate this problem *(pitch the idea of doc days)*.

### Keep track of stale documentation.
We started tagging our docs with a "last updated" date that someone needs to manually change. This date should only change if there has actually been an update and not if someone unintentionally added white space. You can then pull the meta data of the document and generate a list of docs that haven't been touched in a while. If they're still relevant, you bump the date. This approach helps with the discovery of stale docs so you can get to updating them sooner.

### Channel the digital garden ethos
Like a digital garden, documentation is ever changing. It's okay if you don't finish updating every single piece of documentation in a day. Slapping a WIP on a piece of documentation and noting it down for next time can help mitigate the time commitment. And if someone needs that piece of documentation outside of doc days they can ask the team and you can prioritize updating it. Having this ethos takes the pressure of doc days and if absolutely needed lets you vary the time people need to commit.

### Celebrate the wins together
We try to be super vocal about documentation that we've updated. Sometimes we'll have a coworking room where people can hop in and out and share what they're currently working on. Or we'll share a list of all the docs we updated at the end of the day giving kudos to people who updated each one. It's morale boosting as a team to see how far we've come and how our codebase has evolved.

---

The doc days idea isn't perfect, but it's been the most effective way I've seen to keep documentation up to date. Having "update documentation" tickets or trying to carve out time to update documentation by myself at the end of the project just doesn't seem to work. It needs to be a coordinated team effort. Documentation will always change and we need to keep up with that change.