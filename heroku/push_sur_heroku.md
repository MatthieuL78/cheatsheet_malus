# Debugging pour push sur heroku

## Gemfile lambda fonctionnelle :
Gemfile basique pour lancer sur heroku :
```
source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '2.5.1'

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '~> 5.2.0'
# Use sqlite3 as the database for Active Record
gem 'sqlite3'
# Use Puma as the app server
gem 'puma', '~> 3.11'
# Use SCSS for stylesheets
gem 'sass-rails', '~> 5.0'
# Use Uglifier as compressor for JavaScript assets
gem 'uglifier', '>= 1.3.0'
# See https://github.com/rails/execjs#readme for more supported runtimes
# gem 'mini_racer', platforms: :ruby

# Use CoffeeScript for .coffee assets and views
gem 'coffee-rails', '~> 4.2'
# Turbolinks makes navigating your web application faster. Read more: https://github.com/turbolinks/turbolinks
gem 'turbolinks', '~> 5'
# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
gem 'jbuilder', '~> 2.5'
# Use Redis adapter to run Action Cable in production
# gem 'redis', '~> 4.0'
# Use ActiveModel has_secure_password
# gem 'bcrypt', '~> 3.1.7'

# Use ActiveStorage variant
# gem 'mini_magick', '~> 4.8'

# Use Capistrano for deployment
# gem 'capistrano-rails', group: :development

# Reduces boot times through caching; required in config/boot.rb
gem 'bootsnap', '>= 1.1.0', require: false

# Use to check ruby good behaviour
gem 'rubocop'

# For the model user
gem 'devise'

group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
end

group :development do
  # Access an interactive console on exception pages or by calling 'console' anywhere in the code.
  gem 'web-console', '>= 3.3.0'
  gem 'listen', '>= 3.0.5', '< 3.2'
  # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
  gem 'spring'
  gem 'spring-watcher-listen', '~> 2.0.0'
end

group :test do
  # Adds support for Capybara system testing and selenium driver
  gem 'capybara', '>= 2.15', '< 4.0'
  gem 'selenium-webdriver'
  # Easy installation and use of chromedriver to run system tests with Chrome
  gem 'chromedriver-helper'
end

group :production do
  # use to manage database on heroku
  gem 'pg', '0.20.0'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]

```
Il faut lancer le bundle sans prendre en compte la production :
```
bundle install --without production
```

## Fail a cause de la gem pg 
Il peut arriver que la gem pg crée un problème pendant le push sur heroku (voir le warning dans le log au début du lancement du push)
Il faut retirer le 'without' de notre bundle qui persiste :
```
bundle config --delete without
```

## Fail a cause de la gem sqlite3
La gem sqlite3 n'est pas lue par Heroku, il faut la mettre dans le group 'development' pour fonctionner. La gem pg prend le relais sur Heroku pour faire fontionner la database :
```
group :development, :test do
  # Use sqlite3 as the database for Active Record
  gem 'sqlite3'
end
```
## Fail a cause de la gem devise
```
Devise.secret_key was not set
```
Il faut ajouter la clé présentée dans le log dans le fichier config/environments/production.rb :
```
config.secret_key_base = ENV["SECRET_KEY_BASE"]
```
En appelant la valeur de la clé depuis un fichier .env et avec l'aide de la gem dotenv.
