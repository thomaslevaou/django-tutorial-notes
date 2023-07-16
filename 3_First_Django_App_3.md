# Writing your first Django app, part 3

En Django, on appelle **vue** (view) une page Web qui répond à un besoin donné, et qui a son template associé.

Dans notre application de sondages (du projet _mysite_), on souhaite avoir 4 vues:

- la page d'_index des questions_, qui va afficher un peu des dernières questions posées;
- la page de _détails d'une question_, qui va afficher une question, avec un formulaire de vote associé (sans les résultats);
- la page de _résultats d'une question_, qui va afficher les résultats pour une question donnée;
- l'_action de vote_, qui va s'occuper de traiter un choix d'une question.

Les vues sont représentées par des fonction (ou méthodes de classe) Python. La vue est choisie à partir de l'URL (ça on le sait déjà).

On peut alors ajouter 3 vues Django, prenant chacune différents arguments, via le code suivant :

```Python
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)


def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)


def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

Et en Django 4, on précise les paths associé dans `urls.py` (celui de `polls` hein) de la manière suivante :

```Python
from django.urls import path
from . import views

urlpatterns = [
    # ex: /polls/
    path("", views.index, name="index"),
    # ex: /polls/5/
    path("<int:question_id>/", views.detail, name="detail"),
    # ex: /polls/5/results/
    path("<int:question_id>/results/", views.results, name="results"),
    # ex: /polls/5/vote/
    path("<int:question_id>/vote/", views.vote, name="vote"),
]
```
