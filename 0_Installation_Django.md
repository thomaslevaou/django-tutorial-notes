# Installation de Django sur mon PC

## Mise à jour de Python (de 3.9 à 3.11)

```bash
python -V # 3.9 avant d'avoir installé 3.11
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
python -m django --version # Affiche 4.2.2
```