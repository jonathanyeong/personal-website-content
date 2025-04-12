---
pubDate: 2024-05-26
updatedDate: 2024-05-26
title: What is Zeitwerk?
description: A note on Zeitwerk, a code loader for Ruby.
featured: false
draft: false
topics:
  - ruby
---

[Zeitwerk](https://github.com/fxn/zeitwerk) is a gem that loads Ruby files. From Rails 6, Zeitwerk is the default loader. Loading a Ruby file means making that file available in your code. There are a few ways to do this with Ruby, `require`, `require_relative`, `load`. Zeitwerk removes the need to add these special keywords and does the loading for you. It can load files on demand (autoloading) or upfront (eager loading).

Zeitwerk works by following naming conventions. The idea is that file paths match the constant name.  From Zeitwerk's README:

```
lib/my_gem.rb         -> MyGem
lib/my_gem/foo.rb     -> MyGem::Foo
lib/my_gem/bar_baz.rb -> MyGem::BarBaz
lib/my_gem/woo/zoo.rb -> MyGem::Woo::Zoo
```

One reason you would use Zeitwerk in your Ruby projects is to get rid of using `require`. Manually loading files is brittle because you might forget to load certain files or load files in an incorrect order. Zeitwerk also removes the need for `require_dependency` since Zeitwerk also supports code reloading.




