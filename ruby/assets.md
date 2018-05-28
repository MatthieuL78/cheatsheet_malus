# Debugging pour assets pipeline

## Erreur 404 - Fichier CSS et JS non reconnu
Ajouter et modifier dans le fichier config/environments/production.rb les lignes suivantes :
```
config.assets.compile = true
config.assets.precompile = ['*.js', '*.css', '*.css.erb']
```
Ensuite on lance les compilations de la manière suivante :
```
RAILS_ENV=production bundle exec rake assets:precompile
```
Les fichiers CSS et JS devraient être prit en compte.

## L'asset:production ne fonctionne pas
Si vous rencontrez une erreur en compilant vos assets, il se peut qu'il manque l'application 'yarn'. Vous pouvez vérifier cela en observant votre terminal.

* https://yarnpkg.com/en/

