# Writing your first Django app, part 1

Je crée un projet en faisant `django-admin startproject mysite`.

Je me retrouve alors avec un projet s'appelant `mysite`, dans leuqel je trouve également un projet appelé `mysite`.

```bash
django-admin startproject mysite
cd mysite/
python manage.py runserver
```

On peut alors voir le résultat être affiché à l'adresse <http://localhost:8000/>.

Je peux aussi changer le port si je le souhaite en paramètre du `runserver`, par exemple en faisant `python manage.py runserver 8080` pour que le projet soit accessible sur le port 8080.

On vient donc de mettre en place un projet, qui va faire office _d'environnement_.

En Django, on distingue les **projets** des **apps**. Un projet rassemble différentes applications Web, appelées ici apps.

Toujours dans le dossier `mysite`, on va créer une application `polls` via la commande suivante :

```bash
python manage.py startapp polls
```

Parfois les apps sont créées dans le dossier `mysite` du projet, mais osef.