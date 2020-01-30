```
Go to config/routes.rb
```

```
get 'pages/home', to: 'pages#home'

get 'pages/about', to: 'pages#about'
```


```
Under app/controllers create a pages_controller.rb
```


```
class PagesController < ApplicationController

def home

end

def about

end

end
```

```
Under app/views create create two files named home.html.erb and about.html.erb
```

```
add html
```

```
```
