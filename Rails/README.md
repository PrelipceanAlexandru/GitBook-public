# Rails-setup

## Create a new project:

```
rails new [project_name]
```

## Create local_env file

```
mkdir config/samples && touch config/samples/local_env.rb
```

**config/samples/local_env.rb**
```ruby
ENV['USERNAME'] = 'YOUR_USERNAME'
ENV['PASSWORD'] = 'YOUR_PASSWORD'

ENV['DEV_DATABASE'] = 'YOUR_DEV_DATABASE'
ENV['TEST_DATABASE'] = 'YOUR_TEST_DATABASE'
ENV['PROD_DATABASE'] = 'YOUR_PROD_DATABASE'
```

**.gitignore**

```
/config/local_env.rb
```

**Load the local_env.rb file**

https://docs.microsoft.com/en-us/graph/tutorials/ruby?tutorial-step=3

In `config/environment.rb` add the following line 

```ruby
# Load the Rails application.
require_relative 'application'

# ...
# Load local_evn file
local_env_variable = File.join(Rails.root, 'config', 'local_env.rb')
load(local_env_variable) if File.exists?(local_env_variable)
#...

# Initialize the Rails application.
Rails.application.initialize!

```

## Update the README

[Readme-Template-Raw](https://raw.githubusercontent.com/PrelipceanAlexandru/GitBook-public/master/Rails/Readme-Template.md)

---
git push heroku master

heroku run rake db:migrate

heroku ps:scale web=1


