# Writing your first Django app, part 2

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
Question.objects.all()
from django.utils import timezone
q = Question(question_text="What's new?", pub_date=timezone.now())
q.save()
q.id # 1
q.question_text # "What's new?"
q.pub_date #datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=datetime.timezone.utc)
q.question_text = "What's up?"
q.save()
Question.objects.all()
```
