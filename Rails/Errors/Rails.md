rails db:migrate  TypeError: no implicit conversion of nil into String

application.yml

```
...
test:
  APP_URL: 'http://YOUR_HOST/api/v1'
  DEVISE_KEY: <rake secret>
  CORS_DOMAINS: CORS_DOMAINS  
...
```
```
rails aborted!
NameError: undefined local variable or method `​' for main:Object
/home/alex/work/tutorial/my_space/config/local_env.rb:3:in `<main>'
```
rewrite local_env.rm DON'T copy/paste
