# Debugging pour les migrations sur heroku

## Fail d'une migration a cause de la gem pg
Il peut arriver que la gem pg crée un problème pendant une migration de database sur heroku (voir le warning dans le log au début du lancement de la migration)
Il faut retirer le 'without' de notre bundle qui persiste :
```
bundle config --delete without
```
Refaire ensuite un push vers le heroku en faisant les add et commit et la migration devrait fonctionner.

## Fail d'une migration a cause de ETIMEDOUT
Exemple :
```
ETIMEDOUT: connect ETIMEDOUT 50.19.103.36:5000
```
Il semble que c'est un problème du port 5000 (dans ce cas là). Il faut ouvrir le port pour faire la migration.
