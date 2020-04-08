**problem** 

rails db:migrate  TypeError: no implicit conversion of nil into String

**solution**
application.yml

```
...
test:
  APP_URL: 'http://YOUR_HOST/api/v1'
  DEVISE_KEY: <rake secret>
  CORS_DOMAINS: CORS_DOMAINS  
...
```
**problem** 

```
rails aborted!
NameError: undefined local variable or method `​' for main:Object
/home/alex/work/tutorial/my_space/config/local_env.rb:3:in `<main>'
```

**solution**

rewrite local_env.rm DON'T copy/paste

**problem** 
```
PG::ConnectionBad: FATAL:  Peer authentication failed for user "postgres"
```

**solution**

Add in `database.yml` 

```host: localhost```
