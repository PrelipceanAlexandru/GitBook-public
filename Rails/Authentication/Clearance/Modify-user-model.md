
## Add new column to user table and display it.

```
rails g migration AddNameToUser name:string
```

Add a validation on user model for name

**app/models/user.rb**  

```
validates :name, length: { minimum: 2, maximum: 30 }
```

**app/controllers/users_controller.rb**

```ruby
  ...
  
  private 
  
  def user_params
    params.require(:user).permit(:name, :email, :password)
  end
  ...
```



