# Debugging pour push sur heroku

## Gemfile lambda fonctionnelle :
Gemfile basique pour lancer sur heroku :
```
source 'https://rubygems.org'

# authentification
#gem 'devise'
#gem 'pundit'

gem 'rails'
gem 'puma'
gem 'sass-rails'
gem 'uglifier'
gem 'coffee-rails'
gem 'jquery-rails'
gem 'turbolinks'
gem 'jbuilder'
gem 'bootsnap', '>= 1.1.0', require: false

group :development, :test do
  gem 'sqlite3'
  gem 'byebug',  '9.1', platform: :mri
  gem 'rspec-rails'
  gem 'pry-rails'
  gem 'pry-byebug'
  gem 'database_cleaner'
  gem "factory_bot_rails"
  gem 'simplecov'
end

group :development do
  gem "better_errors"
  gem "binding_of_caller"
  gem 'letter_opener'
  gem 'web-console'
  gem 'listen'
  gem 'spring'
  gem 'spring-watcher-listen'
end

group :test do
  gem 'capybara', '1.1.0'
  gem 'poltergeist'
  gem 'shoulda-matchers'
end

group :production do
  gem 'pg', '0.20.0'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```
Il faut lancer le bundle sans prendre compte la production :
```
bundle install --without production
```

## Fail a cause de la gem pg 
Il peut arriver que la gem pg crée un problème pendant le push sur heroku (voir le warning dans le log au début du lancement du push)
Il faut retirer le 'without' de notre bundle qui persiste :
```
bundle config --delete without
```

## Fail d'une migration a cause de la gem pg
Il peut arriver que la gem pg crée un problème pendant une migration de database sur heroku (voir le warning dans le log au début du lancement de la migration)
Il faut retirer le 'without' de notre bundle qui persiste :
```
bundle config --delete without
```
Refaire ensuite un push vers le heroku en faisant les add et commit et la migration devrait fonctionner.
