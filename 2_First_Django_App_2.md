# Writing your first Django app, part 2

## Base de données et gestion des modèles

En plus de l'application `polls` que nous venons de créer, la création d'un projet Django entraîne automatiquement l'installation des app de contrib (celles visibles dans le `INSTALLED_APPS` de `mysite/settings`), qu'on va bientôt avoir l'occasion d'utiliser.

Pour utiliser ces apps, on va avoir besoin de créer et d'utiliser une base de données. Pour la création, on l'exécute via la commande `python manage.py migrate`. Cette commande va créer les tables nécessaires au bon fonctionnement des INSTALLED_APPS, en se basant sur les réglages de `DATABASES`.

Par défaut, les bases de données Django sont faites avec SQLite, et ça suffira pour le tuto.
À changer en cas de vraie utilisation en prod pour le SGBD adapté, bien entendu.
Liens pour changer le SGBD disponible dans le tuto: <https://docs.djangoproject.com/en/4.1/intro/tutorial02/>.
EN SQLite, les bases de données sont stockées dans un fichier, dont le nom est donné dans le paramètre `NAME`.
Avec d'autres SGBD, on peut devoir renseigner ci-dessous un USER, PASSWORD, HOST, etc
Ici le fichier db.sqlite3 se situe à la racine du projet mysite/.

Après avoir exécuté la commande migrate, je peux installer sqlite3 via `sudo apt install sqlite3`, puis faire un `sqlite3 db.sqlite3` à la racine du projet, puis un `.tables` dans la commande sqlite3 pour voir les tables nouvellement créées.

En Django, les modèles représentent l'organisation en base de données, avec éventuellement quelques métadonnées supplémentaires.
Le modèle Django est censé être la seule et définitive source d'information sur la donnée. Django suit le principe du DRY (Don't Repeat Yourself),
et donc va éviter toute redondance (comme j'ai souvent eu l'habitude de voir en SQL).

En Django, les migrations dépendent complètement des modèles (alors que ce n'est pas le cas en Ruby on Rails par exemple).

Tous les modèles Django héritent de la classe `models.Model`.

Les déclarations de modèle ci-dessous suffisent à Django pour pouvoir faire ensuite des CREATE TABLE, et créer une API de connexion à la base de données pour
que les modifications des modèles ci-dessous se synchronisent avec les tables en base.

Pour que l'app soit incluse dans notre projet, on doit le préciser explicitement en indiquant `'polls.apps.PollsConfig'` dans le `INSTALLED_APPS` des settings de `mysite`.

Avec ce champ, on indique où se trouve la config de l'application `polls` nouvellement créée, ce qui permet au projet `mysite` d'inclure des migrations.

On peut alors avertir Django d'une mise à jour dans notre modèle de données via un `python manage.py makemigrations polls`
Les changements de base de données sont alors stockées dans une **migration**, qu'on peut voir dans le fichier migrations/0001_initial.py ici.
Comme en Symfony, on pourrait parfois être amené à le modifier si besoin.
Avant d'exécuter la migration, on peut savoir quelles requêtes SQL seront lancées via `python manage.py sqlmigrate polls 0001`.
Django se permet quelques conventions de nommage par défaut, qu'on peut modifier / surcharger si besoin.
On peut aussi faire `python manage.py check` pour que Django puisse faire quelques vérifications pour s'assurer que l'exécution des commandes sera safe.
Si tout est ok, alors on peut faire `python manage.py migrate` pour exécuter la migration.
L'historique des migrations appliqué est stocké dans Django dans la table `django_migrations`.

On peut lancer une commande Python avec la commande `python manage.py shell`, qui a pour particularité (par rapport à l'interpréteur Python) de pré-définir la valeur de DJANGO_SETTINGS_MODULE, ce qui nous permet d'importer des trucs listés dans notre fichier `settings.py` ici (comme l'app Polls par exemple).

Ainsi, dans ce shell Django, on peut faire `Question.objects.all()`, après avoir importé le modèle `Question` de `polls.models`, ce qui permet de faire des requêtes avec les modèles Django sans trop de souci.

On peut même s'amuser à insérer une donnée dans `Question` avec la suite de commandes suivante :

```Python
from polls.models import Choice, Question
Question.objects.all()
from django.utils import timezone
q.save()
q.id # 1
q.question_text # "What's new?"
q.pub_date #datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=datetime.timezone.utc)
q = Question(question_text="What's new?", pub_date=timezone.now())
q.question_text = "What's up?"
q.save()
Question.objects.all()
```

Cependant, le truc c'est qu'en l'état, le résultat de la commande `Question.objects.all()` est un `<QuerySet [<Question: Question object (1)>]>`, ce qui n'est pas très lisible.
Pour changer ce résultat, on peut ajouter des fonctions `__str__` aux modèles `Question` et `Choice`, pour y ajouter par exemple des textes de questions.
Ce n'est pas important que pour a fenêtre de commande, mais aussi pour les pages d'administration de Django.

On profite de ce moment pour ajouter la méthode `was_published_recently` à `Question`, qui vérifie si la question a été publiée il y a moins de 24 heures.

On peut alors relancer la commande python, et on en profite pour informer de l'existence de nouvelles méthodes qui existent :

```Python
Question.objects.filter(id=1)
Question.objects.filter(question_text__startswith="What")
from django.utils import timezone
current_year = timezone.now().year
Question.objects.get(pub_date__year=current_year)
Question.objects.get(pk=1) # "primary key = 1", ici dans ce contexte la même chose que id=1
q = Question.objects.get(pk=1)
q.was_published_recently() # True or Flase, depending on the day the command was executed
q.choice_set.all() # À partir du moment où un `choice` est un attribut de Question, Django crée automatiquement un `choice_set` représentant la liste des choix de la question
# On peut alors ajouter les choices d'une question
q.choice_set.create(choice_text="Not much", votes=0)
q.choice_set.create(choice_text="The sky", votes=0)
c = q.choice_set.create(choice_text="Just hacking again", votes=0)
c.question # On voit alors la question "What's up" associée au dernier choix
q.choice_set.all() # Là, le set n'est bien plus vite, mais comprend les trois questions précédentes
c = q.choice_set.filter(choice_text__startswith="Just hacking")
c.delete()
q.choice_set.all() # La question a bien été supprimée
```

## Les interfaces d'administration Django

Pour éviter d'avoir à coder ce genre de chose soi-même, Django dispose de ses propres pages d'administration pour permettre la gestion de contenus sur le site.

La création d'un super-utilisateur a lieu en suivant les commandes suivantes :

```bash
python manage.py createsuperuser
```

En suivant l'invite de commande qui en résulte, on renseigne le nom de cet utilisateur en `admin`, son adresse e-mail en `admin@example.com`, son mot de passe en `admin`. Vu le contexte, on se balek des warnings de sécurité signalés par la console.

En relançant alors le serveur (`python manage.py runserver`), on peut alors accéder à la page `http://127.0.0.1:8000/admin/login/?next=/admin/` en saisissant les identifiants que nous venons de créer.

Les contenus affichés à l'écran `groups` et `users`, sont fournis par le framework d'authentification de Django, appelé `django.contrib.auth`.

Pour ajouter du contenu de notre application `polls` de notre projet `mysite`, on doit le préciser dans le fichier `polls/admin.py`, via la commande `admin.site.register(Question)`. Une nouvelle ligne de gestion de la bdd s'affiche alors automatiquement sur `http://127.0.0.1:8000/admin/`.

On a alors l'équivalent d'un PHPmyAdmin en Django.
