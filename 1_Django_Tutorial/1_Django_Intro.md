# Introduction à Django

Je vais noter ici ce que j'apprends en suivant la documentation officielle de Django, pour être à jour le plus rapidement possible avec les technologies du moment.
<https://www.djangoproject.com/>.  

Enfin pour avoir quelque chose de plus stable en termes de tuto, je vais me baser sur le tuto de w3schools <https://www.w3schools.com/django/index.php> qui a l'air à jour en Django 4 aussi. L'avantage d'un tutorial comme celui-ci est le fait d'avoir accès à des exos pour s'entraîner.  

Django suit le **Design Pattern MVT** (Model View Template):

- Le **Modèle** gère les données qu'on veut présenter (habituellement d'une base de données). De la même manière qu'en Symfony, les données sont gérées via un ORM, habituellement dans un fichier `views.py`;
- La **Vue** qui rassemble le modèle et son template, pour répondre aux requêtes des utilisateurs, habituellement dans un fichier `views.py`;
- Le **Template** qui contient l'affichage de la page Web, dans un fichier `.html` mais avec des tags Django pour répondre à certains besoins logiques, habtiuellement dans un dossier appelé `templates`.  

Les navigations par URL sont permises via un fichier `urls.py`.

Mais je préfère quand même me baser sur la vraie doc de Django, même si ce sera plus long, vu que là je suis quand même censé expertiser sur un projet professionnel dessus. Je vais donc utiliser le tuto officiel, bien qu'il soit plus long : <https://docs.djangoproject.com/fr/4.1/intro/tutorial01/>
(Celui de w3schools est vraiment trop léger dans ce contexte selon moi).  
