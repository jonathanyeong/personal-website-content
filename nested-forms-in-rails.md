---
pubDate: 2024-06-09
updatedDate: 2024-06-09
title: Nested forms in Rails
description: Quick tutorial on how to build nested forms in Rails
featured: false
draft: false
topics:
  - tutorial
---
## What I'm trying to achieve

I have two models:
- Training Steps
- Training Session which has many Training Steps

When a user creates a Training Session, I want a form that contains fields for Training Steps inside the Training Session form. When I submit the form, I will create a Training Session object along with the related Training Steps.

## 1. Accepting nested attributes

First, I added `accepts_nested_attributes_for` to my Training Session model.

```ruby
class TrainingStep < ApplicationRecord
  belongs_to :training_session
end

class TrainingSession < ApplicationRecord
  has_many :training_steps
  accepts_nested_attributes_for :training_steps, reject_if: lambda { |attributes| attributes['description'].blank? }
end
```

By default, nested attribute updating is turned off. With `accepts_nested_attributes_for` I'm able to create a Training Session and the Training Steps in one go. With this method, `TrainingSession` will now accept a key `training_steps_attributes` as a value. With `reject_if`,  it will also silently ignore any records that don't pass the lambda.

## 2. Adding the controller `new` method

```ruby
class TrainingSessionsController < ApplicationController
  def new
    @training_session = TrainingSession.new
    2.times { @training_session.training_steps.build }
  end
end
```

In my `new` method, I'll initialise a new Training Session model assigned to `@training_session`, and build two `training_steps` models along with it. `@training_session` will be used in the view.

## 3. Adding a nested form in the view

```ruby
<%= form_with model: @training_session do |form| %>
  Session Steps:
  <ul>
    <%= form.fields_for :training_steps do |training_steps_form| %>
      <li>
        <%= training_steps_form.label :description %>
        <%= training_steps_form.text_field :description %>
      </li>
    <% end %>
  </ul>
  <%= form.submit "Create New Session" %>
<% end %>
```

In the view, `new.html.erb`, we're using `form_with` to build a form that routes to the correct `create` path. Rails will infer the correct routes based on the resource, in this case `@training_session`. `fields_for` is then used to refer to the nested `training_steps` objects. This method is used to refer to a specific model object, `training_steps` in this case, but it yields to the `form_with` element. In other words, you would use `fields_for` inside a `form_with`.

TIL - `form_with` is the new way to build forms. I used `form_for` in the past but that's now obselete ([here's the PR with the change to form_with](https://github.com/rails/rails/pull/26976)).
## 4. Creating a nested object

```ruby
class TrainingSessionsController < ApplicationController
  def new
    @training_session = TrainingSession.new
    2.times { @training_session.training_steps.build }
  end

  def create
    training_session = TrainingSession.new(training_session_params)
    if training_session.save
      training_session.save
      redirect_to training_sessions_path
    else
      render "new"
    end
  end

  private

  def training_session_params
    params.require(:training_session).permit(training_steps_attributes: [:id, :description])
  end
end
```

In the controller, we can add the `create` method to handle creating the Training Session object. Notice that we use the `training_steps_attributes` in `training_session_params`. We'll pass these parameters directly into `TrainingSession.new` and when we `save` Rails will create both the TrainingSession object and the associated Training Steps objects.