# Rails-setup

## Create a new project:

```
rails new [project_name]
```

## Create local_env file

```
mkdir config/samples && touch config/samples/local_env.yml
```

**config/samples/local_env.yml**
```ruby
USERNAME: 'YOUR_USERNAME'
PASSWORD: 'YOUR_PASSWORD'

DEV_DATABASE: 'YOUR_DEV_DATABASE'
TEST_DATABASE: 'YOUR_TEST_DATABASE'
PROD_DATABASE: 'YOUR_PROD_DATABASE'
```

**.gitignore**

```
/config/local_env.yml
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

[Readme-Template-Raw](https://raw.githubusercontent.com/PrelipceanAlexandru/Rails-setup/master/Readme-Template.md)

---