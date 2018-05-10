# Debuging pour les controllers

## FATAL : Unable to monitor directories for changes
Sous Linux on peut rencontrer une limite du système sur le nombre de fichiers que l'on peut modifier. Pour augmenter le nombre de fichier modifiable : 
```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
