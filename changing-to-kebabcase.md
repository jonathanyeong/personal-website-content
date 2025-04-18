---
pubDate: 2024-06-01
updatedDate: 2024-06-01
title: Making a custom kebab case method for pretty URLs
description: I was surprised that Rails and Ruby didn't have a built in kebab case method. Adding a gem seemed overkill for me. So I created a custom method to serve pretty URLs.
featured: true
draft: false
topics:
  - tutorial
---
Recently, I was working on a Rails app where I wanted to make some pretty URLs for it. I have a route where you can view a Dog resource `GET /dog/1`. Having an ID in the URL is not what I want. People could increment the ID to potentially see other dogs. But more importantly, it's not personal! I wanted the URL to show your dog name instead, like `/dog/albie` or `/dog/st-george-bernard-the-3rd`. I needed a way to make pretty kebab case URLs.

## Pretty kebab case URLs
To get a URL that's not the default `id`, I added the [`to_param` method](https://edgeapi.rubyonrails.org/classes/ActiveRecord/Integration/ClassMethods.html#method-i-to_param) to my model. Where `slug` was a column on my Model:

```ruby
def to_param
  slug
end
```

Since I wanted the slug to be auto-generated, I had to transform the `name` column to kebab case (see below). Much to my surprise, there wasn't a Ruby / Rails `kebabcase` method:

```ruby
class Dog < ApplicationRecord
  before_create ->(dog) { dog.slug = dog.name.kebabcase } # .kebabcase doesn't exist

  def to_param
    slug
  end
end
```

Looking at the docs, I found `.underscore` and `.dasherize`. Unfortunately, they only work for camel case names:

```ruby
"DogName".underscore.dasherize
# => "dog-name"
```

But who names their dog in camel case? Maybe people do, but I'm assuming most people use spaces. Unfortunately, `.underscore` only works on camel case strings.

```ruby
"Dog Name".underscore.dasherize
# => "dog name"
```

## Searching for a Gem

If Ruby and Rails didn't have a kebab case method. I'm sure there's a gem out there that does. In comes [Strings::Case](https://github.com/piotrmurach/strings-case/tree/master) to the rescue:

```ruby
strings.kebabcase("PostgreSQL adapter")
# => "postgre-sql-adapter"
```

I could've called it a day here, but looking at the gem, it has so many other methods that I wouldn't use. Honestly, it seems like overkill for my tiny side project. It's also another library I would have to maintain and make security updates to. In the end, I decided to roll a custom kebab case method.

## Rolling a custom kebab case method

Before jumping into a solution, I wanted some inspiration from Strings::Case.

Under the hood, the gem scans through a string, building words determined by whether there is a specific delimiter or if there's an uppercase. "DogName" would be two words "Dog" and "Name" since there's an upper case in the middle. And "Dog Name" would be two words, since there's a space in the middle. These words get put into an array, and after the string has been scanned, a `.join` method is called with a separator. In kebab case, the separator would be `-`. I'm oversimplifying this approach. If you're curious, about the source code, you can view it [here](https://github.com/piotrmurach/strings-case/blob/master/lib/strings/case.rb#L365).

For the gem, this process makes total sense. It handles many use cases as well as includes multiple string manipulation functions. Since I only wanted kebab case, I decided to go with a regex approach:

```ruby
dog.name.downcase.gsub(/[^\w\d\s]/, "").gsub(/\s/u, "-")
```

Let's break down what this is doing:
1. `.downcase` - A built-in function that makes the string lowercase.
2. `.gsub(/[^\w\d\s]/, "")` - Regex that matches anything that's not a character, digit or space, i.e. any special characters and removes it.
3. `.gsub(/\s/u, "-")` - Regex that matches a space and replaces with a dash

```ruby
"St. George Bernard the 3rd".downcase.gsub(/[^\w\d\s]/, "").gsub(/\s/u, "-")
# => st-george-bernard-the-3rd
```

The biggest limitation with my approach is the regex. I make plenty of assumptions with what I think names should be, and it's focused primarily on English. But what if someone uses a character language like Japanese?

```ruby
"餅".downcase.gsub(/[^\w\d\s]/, "").gsub(/\s/u, "-")
# => ""
```

If I wanted to make this more robust, I should handle non-English characters properly. If I can, I could transliterate it. Like `é` becomes `e`. If it's in a character language, I could leave it because non-English characters can be used in URLs.

However, for a small side project, what I have is enough. And I didn't need to pull in a new gem to achieve it.

---

*Using Ruby `3.2.0` and Rails `7.1.3.3` at time of writing*