# Mysql2

[Mysql2-gem](https://github.com/brianmario/mysql2)

## Gemfile

```
gem 'mysql2'
```

```
bundle install
```
## Configuration
```
cp config/samples/local_env.yml config
```
Update `config/local_env.yml` using your corresponding values


## config/database.yml
```
default: &default
  adapter: mysql2
  database: <%= ENV['DEV_DATABASE'] %>
  username: <%= ENV['USERNAME'] %>
  password: <%= ENV['PASSWORD'] %>
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  <<: *default
  database: <%= ENV['TEST_DATABASE'] %>

production:
  <<: *default
  database: <%= ENV['PROD_DATABASE'] %>
```







