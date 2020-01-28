## Add a new method to user_controller

Let's add an **index** method that returns all users. This route can be accessed only by sign-in users.

**app/controllers/users_controller.rb**

```
...
before_action :require_login, only: [:index]
...

def index
  @users = User.all
end
```
Let's add this **route** as root

**routes.rb**
```
...
  root 'users#index'
...
```
Next we need to **create the view template for index method/action**.

```
touch app/views/users/index.html.erb
```

```
<p id="notice"><%= notice %></p>

<h1>Users</h1>

<table>
  <thead>
    <tr>
      <th>Email</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @users.each do |user| %>
      <tr>
        <td><%= user.email %></td>
        <td><%# link_to 'Show', user_path(user) %></td>
      </tr>
    <% end %>
  </tbody>
</table>
<br>
```

## Add a show option to display one user

**users_controller.rb**

```
before_action :require_login, only: [:index, :show]
before_action :set_user, only: [:show]
...
def show
end
...

private

def set_user
  @user = User.find(params[:id])
end
```

**routes.rb**

```
resources :users, except: [:create]
```
Create the show template:

```
touch app/views/users/show.html.erb
```

And add those line for the moment

```
<p id="notice"><%= notice %></p>

<p>
  <strong >Email:</strong>
  <%= @user.email %>
</p>

<%# link_to 'Edit', edit_user_path(@user), class: 'btn btn-primary' %>
<%= link_to 'Back', users_path, class: 'btn btn-primary'%>
```
