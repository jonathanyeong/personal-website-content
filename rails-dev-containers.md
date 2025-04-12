---
pubDate: 2024-06-20
updatedDate: 2024-06-20
title: Rails Dev Containers
description: "Went to a meetup tonight and learnt a new feature that's coming to Rails: Dev Containers"
featured: false
draft: false
topics:
  - rails
---
I went to a Ruby meetup tonight and watched a talk about a new feature that's coming to Rails 7.2; [dev containers](https://github.com/rails/devcontainer). First off, a dev container isn't a Rails construct. It's been used by [GitHub Codespaces](https://github.com/features/codespaces) and even has an [official spec](https://github.com/devcontainers/spec). However, there hasn't been a dev container made specifically for Rails.

Why is there a specific Rails dev container? There are a few dependencies that Rails has like Ruby version, database (mysql, postgres, sqlite etc) version, and whatever else your app might be running. Rails dev containers will automatically set up all of your dependencies for you.

Why might you want to use a dev container? Ultimately, a dev container is a docker container. Which means to get a Rails app running, all you need is Docker. You don't need to install any dependencies on your machine. The dev container will run, and it will have all dependencies for you.

I'm pretty excited to try out dev containers. But as of time of writing, you can only get them on the Rails main branch.

```
rails new MyApp --database=mysql --devcontainer --main
```

It will be out in Rails 7.2, see [release notes](https://github.com/rails/rails/blob/7606f99649a7688616516ea6dea5718746fe9203/guides/source/7_2_release_notes.md?plain=1#L62).