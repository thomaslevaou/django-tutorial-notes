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

## Création d'une vue

Dans le dossier `my_tennis_club`, on a donc maintenant entre autres un dossier `members`, et un autre `my_tennis_club`.

Dans l'app members, on peut alors initier le code du fichier `views.py` avec le code ci-dessous :

```Python
from django.shortcuts import render
from django.http import HttpResponse

def members(request):
    return HttpResponse("Hello world!")
```

## Création d'une URL

Dans l'app `members` (donc son dossier), on peut créer un nouveau fichier `urls.py`, dans lequel on va insérer le code ci-dessous :

```Python
from django.urls import path
from . import views

urlpatterns = [
    path('members/', views.members, name='members'),
]
```

Et pour indiquer au projet que, pour le moment, la page d'accueil du projet sera la page d'accueil de members, on va insérer le code suivant dans le fichier
`urls.py` dans le **dossier my_tennis_club/my_tennis_club** cette fois, en utilisant le module `include` dans le fichier `urls.py` déjà existant :  

```Python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('members.urls')),
    path('admin/', admin.site.urls),
]
```

La page `http://localhost:8000/members/` affichera alors un "Hello World".  

