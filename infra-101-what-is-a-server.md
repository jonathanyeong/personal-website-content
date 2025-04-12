---
pubDate: 2024-06-18
updatedDate: 2024-06-18
title: "Infrastructure 101: What is a server?"
description: Beginning my infrastructure journey from the very basics. Let's talk about servers.
featured: false
draft: false
topics:
  - infrastructure
---
You've been working hard on a Rails application. It's living on your local machine, but now you're ready to show the world. What's next?

We need a server! A piece of hardware which can download your Rails app, run it, and allow people to access it. There are many kinds of servers. *When I was doing research, the term "server" was losing all meaning.*

- Cloud Servers
	- A virtual server hosted on a cloud computing environment like AWS, GCP, Azure etc. Examples include, AWS EC2 or Digital Ocean Droplets.
	- Server is managed by the provider.
	- Pricing is on demand, can be dynamically scaled up or down.
- Virtual Private Servers (VPS)
	- A virtual server that runs on top of a physical server. The physical server may have multiple virtual servers running on it. It can be hosted on on-premises physical servers or within data centres.
	- Server managed by you.
- On-premise (on-prem) Servers
	- Physical machines in a data centre (or even a dedicated machine at home) that will run your application.

We'll focus on cloud servers, since that's what most hobby projects would use. You can spin up a cloud server through the provider UI, or through an Infrastructure as Code tool like Terraform. Once the server is up and running, the next steps will be to load your application. I'll leave that for the next post!

---

P.S. In most cases, especially if you're a solo dev trying to get something running quickly, I'd recommend [Fly.io](https://fly.io/). Fly.io is an infrastructure as a service (IaaS) platform. It has a simple deploy process, `fly launch && fly deploy`, and is cheaper than Heroku. Here's the [guide to getting Rails up and running with Fly.io](https://fly.io/docs/rails/getting-started/).




