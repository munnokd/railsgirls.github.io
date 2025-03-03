---
layout: guide
title: Adding Authentication with Devise
permalink: devise
---

# Adding Authentication with Devise

*Created by Piotr Steininger, [@polishprince](https://twitter.com/polishprince). Updated by Ernesto Jimenez, [@ernesto_jimenez](https://twitter.com/ernesto_jimenez)*

**This guide assumes that you have already built a Rails Girls app by** [**following the app development guide**](/app).

## *1.* Add devise gem

Open up your `Gemfile` and add this line

{% highlight ruby %}
gem "devise"
{% endhighlight %}
and run
{% highlight sh %}
bundle install
{% endhighlight %}
to install the gem. **Also remember to restart the Rails server**.

## *2.* Set up devise in your app

Run the following command in the terminal.

{% highlight sh %}
rails g devise:install
{% endhighlight %}

## *3.* Configure Devise

Ensure you have defined default url options in your environments files. Open up `config/environments/development.rb` and add this line:
{% highlight ruby %}
config.action_mailer.default_url_options = { host: "localhost", port: 3000 }
{% endhighlight %}

before the `end` keyword.

Open up `app/views/layouts/application.html.erb` and add:

{% highlight erb %}
<% if notice %>
  <p class="alert alert-success"><%= notice %></p>
<% end %>
<% if alert %>
  <p class="alert alert-danger"><%= alert %></p>
<% end %>
{% endhighlight %}
right above
{% highlight ruby %}
  <%= yield %>
{% endhighlight %}

Open up `app/views/ideas/show.html.erb` and remove the line that says:

{% highlight erb %}
<p id="notice"><%= notice %></p>
{% endhighlight %}

Do the same for `app/views/comments/show.html.erb`. These lines are not necessary as we've put the notice in the `app/views/layouts/application.html.erb` file.

## *4.* Setup the User model

We'll use a bundled generator script to create the User model.
{% highlight sh %}
rails g devise user
rails db:migrate
{% endhighlight %}

{% coach %}
Explain what user model has been generated. What are the fields?
{% endcoach %}

## *5.* Create your first user

Now that you have set everything up you can create your first user. Devise creates all the code and routes required to create accounts, log in, log out, etc.

Make sure your rails server is running, open <http://localhost:3000/users/sign_up> and create your user account.

## *6.* Add sign-up and login links

All we need to do now is to add appropriate links or notice about the user being logged in in the top right corner of the navigation bar.

In order to do that, edit `app/views/layouts/application.html.erb` add:
{% highlight erb %}
<p class="navbar-text float-right">
<% if user_signed_in? %>
  Logged in as <strong><%= current_user.email %></strong>.
  <%= link_to "Edit profile", edit_user_registration_path, class: "navbar-link" %> |
  <%= link_to "Logout", destroy_user_session_path, method: :delete, class: "navbar-link"  %>
<% else %>
  <%= link_to "Sign up", new_user_registration_path, class: "navbar-link"  %> |
  <%= link_to "Login", new_user_session_path, class: "navbar-link"  %>
<% end %>
</p>
{% endhighlight %}
right after
{% highlight erb %}
  <ul class="navbar-nav mr-auto">
    <li class="nav-item active">
      <a class="nav-link" href="/ideas">Ideas</a>
    </li>
    ...
  </ul>
{% endhighlight %}

Finally, force the user to redirect to the login page if the user was not logged in. Open up `app/controllers/application_controller.rb` and add:

{% highlight ruby %}
before_action :authenticate_user!
{% endhighlight %}

after `class ApplicationController < ActionController::Base`.

Open your browser and try logging in and out from.

{% coach %}
Talk about the `user_signed_in?` and `current_user` helpers. Why are they useful?
{% endcoach %}

## What next?

* Add extra fields to the User model
* Add relationships between users and ideas
* Restrict users to only be able to edit their own ideas and delete their own comments
* Expand to use roles or permissions (use one of the popular authorization gem like CanCan)
