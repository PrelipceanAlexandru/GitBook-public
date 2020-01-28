## Add edit, update and destroy action
**user_controller.rb**

Add edit, update and destroy in set_user call_back

```ruby
  before_action :set_user, only: [:show, :edit, :update, :destroy]  
```

```ruby
  def edit
  end

  def update
    if @user.update_attributes(name: params[:user][:name])
      flash.now[:notice] = "Success"
      redirect_to user_path(@user)
    else
      flash.now[:notice] = "Error"
      render exit_path(@user)
    end
  end
  
  def destroy
    @user.delete
    flash[:success] = 'User was successfully destroyed.'
    redirect_to users_path
  end
```

Create edit template 
```
touch app/views/users/edit.html.erb
```

```
<h1>Editing Company</h1>

<%= render 'edit', user: @user %>

<br>

<%= link_to 'Show', @user%>
<%= link_to 'Back', users_path %>
```

Create the partial for this `_edit.html.erb`

```
touch app/views/users/_edit.html.erb
```

```
<%= form_for @user do |form| %>
  <div class="text-field">
    <%= form.label :name %>
    <%= form.text_field :name %>
  </div>

  <div class="submit-field">
    <%= form.submit %>
  </div>
<% end %>
```

Add the link for destroy

**app/views/users/index.html.erb**

```ruby
  ...
  <td><%= link_to 'Destroy', user, method: :delete, data: { confirm: 'Are you sure?' } %></td>
  ...
```


