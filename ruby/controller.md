# Debuging pour les controllers

## FATAL : Unable to monitor directories for changes
Sous Linux on peut rencontre une limite du système sur le nombre de fichiers que l'on peut modifier. Pour contourner l'erreur : 
```
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```
