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

