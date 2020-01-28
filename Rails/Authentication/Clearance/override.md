# Overriding Clearance

## Routes

After you run: ```rails generate clearance:routes``` the routes file is modified and you have full controle of clearance routes.

In `initializers/clearance.rb` set `config.routes` to `false`

```
Clearance.configure do |config|
  ...
  config.routes = false
  ...
end
```

## Controller

The controller that we need to override are:

```
class PasswordsController < Clearance::PasswordsController
class SessionsController < Clearance::SessionsController
class UsersController < Clearance::UsersController
```

The default behavior can be found [here](https://github.com/thoughtbot/clearance/tree/master/app/controllers/clearance)

Command for creating those files:

```
touch app/controllers/passwords_controller.rb
touch app/controllers/sessions_controller.rb
touch app/controllers/users_controller.rb
```
Example of file for users

```
class UsersController < Clearance::UsersController
  before_action :require_login, only: [:create]

end
```

## Modify sign_up route

**routes.rb**

```
get "/sign_up" => "users#new", as: "sign_up"
```

After the `new` template is showed the data are send to `create` controller in clearance/users:

```
resources :users, controller: "clearance/users", only: [:create] do
  resource :password,
    controller: "clearance/passwords",
    only: [:edit, :update]
end
```

To modify this change the route to:

```
resources :users, only: [:create] do
    resource :password,
      controller: "clearance/passwords",
      only: [:edit, :update]
  end
```

## Modify sign_out route

**routes.rb**

```
delete "/sign_out" => "sessions#destroy", as: "sign_out"
```

## Modify sign_in route

**routes.rb**

```
get "/sign_in" => "sessions#new", as: "sign_in"
```

After the `new` template is showed the data are send to `create` controller in clearance/sessions:

`resource :session, controller: "clearance/sessions", only: [:create]`

To modify this change the route to:

```
resource :session, only: [:create]
```

## Remove sign_up if **Clearance.config.allow_sign_up = false**

In `config/initializers/clearance.rb` add:
```
Clearance.configure do |config|
  ...
  config.allow_sign_up = false
  ...
end
```
In routes add:
```
if Clearance.configuration.allow_sign_up?
    get "/sign_up" => "users#new", as: "sign_up"
end
```

# TODO: Check route constraints provided by clearance
