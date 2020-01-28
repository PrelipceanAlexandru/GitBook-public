## Forgot password/route overriding

In `config/initializer/clearance.rb`

```
Clearance.configure do |config| do
  ...
  config.mailer_sender = "reply@example.com"
  ...
```
where `reply@example.com`is the `form` value in email.
Modify the routes for password as follows:

```
Rails.application.routes.draw do
  ...
  resources :passwords, only: [:create, :new]
  ...
  resources :users, only: [:create] do
    resource :password, only: [:edit, :update]
  end
  ...
end
```
Also be sure that you have the `PasswordsController` overrided in you application.
