# Installation de Django sur mon PC

## Mise à jour de Python (de 3.9 à 3.11)

```bash
python3 -V # Je suis en 3.9.2 au moment où j'écris cette ligne
sudo apt install build-essential libssl-dev libffi-dev libsqlite3-devel python3-dev # Exécution des commandes pour faire compiler du Python, d'après https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-debian-11
```

Je télécharge Python3.11 en .tgz sur [le site officiel de Python](https://www.python.org/downloads/release/python-3112/)

Puis je le décompresse :

```bash
cd ~/Téléchargements
tar -xzf Python-3.11.2.tgz
cd Python-3.11.2
./configure --enable-optimizations --enable-loadable-sqlite-extensions
sudo make altinstall # La commande peut durer une dizaine de minutes
ls /usr/local/bin/python*
sudo update-alternatives --install /usr/bin/python python /usr/local/bin/python3.11 1
ls /usr/local/bin/python*
python -V # Affiche alors Python 3.11.2
```

## Installation de pip

PIP est le gestionnaire de paquets Python grosso modo équivalent à Composer en PHP.  

```bash
/usr/local/bin/python3.11 -m pip install --upgrade pip
ls /usr/local/bin/pip*
sudo update-alternatives --install /usr/bin/pip pip /usr/local/bin/pip3.11 1
pip -V # Affiche alors pip 23.0.1
```

## Installation de Django

```bash
python -m pip install Django
python -m django --version # Affiche 4.1.7
(Si environnement virtuel déjà activé) django-admin --version # Affiche DJano 4.1.7 (Le cours est en 4.1.2 mais osef)
(Facultatif) pip install --upgrade pip # Bizarrement, ma version de pip a un peu diminué dans l'environnement virtuel, mais bon c'est corrigé
```

## Installation d'un environnement virtuel Python

Pour chaque projet Django, on va configurer un **environnement virtuel** Python qui permettra d'avoir une configuration propre à chaque projet (un peu à la manière d'un composer.php pour chaque projet PHP en Symfony).  
On peut appeler chacun de nos environnements virtuels de la manière qu'on veut. Ici, je vais vouloir l'appeler `myworld`, ce qui va être possible via les commandes suivantes :

```bash
cd ~/Documents/Informatique/Django/
python -m venv myworld # la création de l'environnement implique la création d'un dossier "myworld" dans le dossier courant
source myworld/bin/activate # Cette commande devra être activée manuellement à chaque fois qu'on travaillera sur ce projet. Je pense que ça n'existe pas sur Django 1
```


## Création d'un projet Django

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