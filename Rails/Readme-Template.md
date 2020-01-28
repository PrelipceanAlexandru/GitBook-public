# [Project_Name]

## Install

### Clone the repository

```shell
git clone [git-repo]   
cd [project_name]
```

### Check your Ruby version

```shell
ruby -v
```

The ouput should start with something like e.g. [`ruby 2.5.1` ]

If not, install the right ruby version using [rbenv](https://github.com/rbenv/rbenv) (it could take a while):

```shell
rbenv install [rails-version]
```
### Check your Rails version

```shell
rails -v
```

The ouput should start with something like e.g [`Rails 5.2.3` ]

### Install dependencies

Using [Bundler](https://github.com/bundler/bundler) and [Yarn](https://github.com/yarnpkg/yarn):

```shell
bundle && yarn
```

### Set environment variables

```
cp config/samples/local_env.yml config
```

### Database 
 [**MySQL**]

```shell
rails db:create db:migrate db:seed
```

## Serve

```shell
rails s
```

## Deploy
