# Clearance

[clearance-gem](https://github.com/thoughtbot/clearance)

## Gemfile

```
gem 'clearance'
```

```
bundle install
```

## Run the generator

```
rails generate clearance:install
```

Add in **config/environments/{development,test}.rb**

```
config.action_mailer.default_url_options = { host: 'localhost:3000' }
```

Add in **app/views/layouts/application.html.erb**

```ruby
<% if signed_in? %>
  Signed in as: <%= current_user.email %>
  <%= button_to 'Sign out', sign_out_path, method: :delete %>
<% else %>
  <%= link_to 'Sign in', sign_in_path %>
<% end %>
```
and

```ruby
<div id="flash">
  <% flash.each do |key, value| %>
    <div class="flash <%= key %>"><%= value %></div>
  <% end %>
</div>
```

The file should looks like this
```ruby
<!DOCTYPE html>
<html>
  <head>
    <title>LiteRed</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>
    <% if signed_in? %>
      Signed in as: <%= current_user.email %>
      <%= button_to 'Sign out', sign_out_path, method: :delete %>
    <% else %>
      <%= link_to 'Sign in', sign_in_path %>
    <% end %>
  <body>
    <div id="flash">
      <% flash.each do |key, value| %>
        <div class="flash <%= key %>"><%= value %></div>
      <% end %>
    </div>

    <%= yield %>
  </body>
</html>
```

## Importnat
Until we run ```rails db:migrate```  we can add new columns to **User** table.

Find the migration and add new column.

If all looks good, run
```
rails db:migrate
```

## Generate views

```
rails generate clearance:views
```

## Generate routes

```
rails generate clearance:routes
```
Now we can modify the views and change urls.

## Display the console in page

## Just add <% console %> in views/application.html.erb file at the end of the file
