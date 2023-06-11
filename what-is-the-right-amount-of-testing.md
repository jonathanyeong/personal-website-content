---
pubDate: 2023-06-11
title: "What is the right amount of testing?"
description: "My opinions on good defaults to keep in mind when trying to understand the right amount of testing"
featured: false
draft: false
---

Over the years my answer to this question has changed drastically. From fully embracing integration testing and requiring code coverage numbers on Pull Requests (PRs). To writing only unit tests covering the critical paths of the application. Leaving much of code untested.

Of course there's no definitive answer for the right amount of testing. If there was, everyone would be doing it. Instead, I'd like to share what I consider a good default amount of testing. But before diving into that, let's discuss the problem we're trying to solve.

The primary goal of testing is to prevent bugs from reaching production. Too little testing leads to a brittle application. As more features get pushed out, more fires need to be fought. And eventually you're worried that any change might cause something unrelated to break.

In response, we swing the pendulum to the other side. We write extensive suites of integration tests that get run on every PR. Code coverage numbers need to be met before a PR can be approved. We aim to test every possible path, leaving no room for bugs to slip through. Unfortunately, our feedback loops begins to slow down. And meaningful features take longer to be released.

Both of these are extreme ends of the spectrum. So what is a happy middle ground? Here's what I think we should default to.

- Write unit tests for all your changes. Testing error paths but not every combination of errors/inputs. Pick your edge cases. Here's some examples:
  - Test paths that have a complex outcome and/or an error you display to the user.
  ```ruby
  if condition
    complex_outcome
  else
    error_to_user
  end
  ```
  - Don't test expectations of libraries. Expect the libraries to already have tested them.
  ```ruby
  # Don't test that objects gets saved to the DB.
  model = Model.create
  # Don't test the `do_something` library function.
  libraryObject = library.do_something
  ```
  - Test for public not private methods. Unexpected changes to private methods should be caught in the public method tests.
  ```ruby
  # Test this method
  def public_method
    something = do_something
    do_something_with(something)
  end

  private

  # Don't test these methods
  def do_something
    ...
  end

  def do_something_with
    ...
  end
  ```

- Test suites should run fast enough that by the time you make a coffee your build is complete (faster is better). That means unit tests over integration tests.

- If possible, always manually test your change. Keep the process lightweight and repeatable by writing down your steps. Don't test the entire app behaviour on every change. But allow someone on your team to vet your change by running through the testing steps you wrote down.

- Manually testing is especially important if testing a third party service/api. Mocking in unit tests is needed for fast tests but you need to validate your mocks in the real world.

- Bonus points if you have a cloud developer environment or a developer preview on your PR that allows someone to validate your changes without having to run your code on their machine.

- Finally, have a robust CI/CD pipeline that lets you rollback bad builds, or feature flags to quickly turn off a breaking change. Having the right monitors in place will tell you if something is broken before it causes too much damage. Expect that bugs will happen even with a proper amount of testing.

While there's no one size fits all solution for the right amount of testing. I've found that these defaults have worked well in the past. They strike a balance between maintaining healthy developer feedback loops while minimizing game breaking bugs that make it to production.