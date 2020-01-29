# Lograge

[Lograge](https://github.com/roidrage/lograge)

Simplify rails log

Lograge is an attempt to bring sanity to Rails' noisy and unusable, unparsable and, in the context of running multiple processes and servers, unreadable default logging output. Rails' default approach to log everything is great during development, it's terrible when running it in production. It pretty much renders Rails logs useless to me.


**Gemfile**
```ruby
gem 'lograge'
```

**config/application.rb**
```ruby
config.lograge.enabled = true
```

Check official documentation for more information
