# MailCatcher

[mailcatcher](https://mailcatcher.me/)

Install the gem

```
gem install mailcatcher
```

To set up your rails app, I recommend adding this to your environments/development.rb:

```
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = { :address => "localhost", :port => 1025 }
```

Start the server
```
mailcatcher
```

To access the email go to:

```
http://localhost:1080/
```
Send mail through

```
smtp://localhost:1025
```
