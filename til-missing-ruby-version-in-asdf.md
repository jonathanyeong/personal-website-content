---
pubDate: 2024-06-07
updatedDate: 2024-06-07
title: Fixing missing Ruby versions in asdf
description: "TIL: how to fix missing Ruby versions in asdf"
featured: false
draft: false
topics:
  - ruby
---
I use [asdf](https://asdf-vm.com/) to manage my Ruby versions. It's a version manager that can manage multiple languages. Meaning I don't need to install a separate Ruby version manager with a Node version manager, for example.

I ran into an issue today where `asdf` wasn't showing the latest Ruby version. To fix it I ran:

```bash
asdf plugin update ruby
```

Then to install the latest ruby version, and set it as the default Ruby version:

```bash
asdf install ruby latest
asdf global ruby latest
```

You can also use `.tool-versions` to specify which Ruby version you want at the project level (see [asdf docs for more info](https://asdf-vm.com/manage/configuration.html#tool-versions)).

