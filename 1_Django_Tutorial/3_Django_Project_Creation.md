# Création d'un projet Django

Le projet de ce tuto va s'appeler `my_tennis_club`:  

```bash
cd ~/Documents/Informatique/Django/myworld/
django-admin startproject my_tennis_club
cd my_tennis_club
python manage.py runserver
```

On peut alors voir un projet de base être affiché dans le navigateur sur http://localhost:8000/.  
Le message des 18 "unapplied migrations" est pour le moment normal.  

## Création d'une App

En Django projet et app sont deux notions différentes.  
Une application est une appli web qui est une partie de mon projet.
On va créer ici on application s'appelant `members`.

Pour ce faire, on exécute les commandes suivantes :

```bash
cd ~/Documents/Informatique/Django/myworld/my_tennis_club
python manage.py startapp members
```

Dans le dossier `my_tennis_club`, on a donc maintenant entre autres un dossier `members`, et un autre `my_tennis_club`.  