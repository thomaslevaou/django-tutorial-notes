# Premières éditions de fichiers Django

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

## Templates

