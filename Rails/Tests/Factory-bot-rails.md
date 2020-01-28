# Factory-Bot-Rails

[factory-bot-rails](https://github.com/thoughtbot/factory_bot_rails)
[factory-bot-rails config](https://medium.com/@lukepierotti/setting-up-rspec-and-factory-bot-3bb2153fb909)

```
gem 'factory_bot_rails'
```

```
bundle install
```

## Create support folder and factory_bot.rb file

```
mkdir spec/support && touch spec/support/factory_bot.rb
```

## app/spec/support/factory_bot.rb

```
RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end
```

## app/spec/rails_helper.rb

```
require 'support/factory_bot'
```

## create spec/factories folder 

```
mkdir spec/factories
```
