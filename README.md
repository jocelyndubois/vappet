Installation
=============

Etape 1 - Installations
---------------------

Téléchargez et installez les softwares suivants :

1. Vagrant : 
    - http://www.vagrantup.com/downloads.html
2. VirtualBox
    - https://www.virtualbox.org/wiki/Downloads
    
Etape 2 - Créer l’environement
---------------------

Créez simplement un dossier sur la machine host et se placer dedans. Ce sera ici que seront tout les fichiers de config.

Etape 3 - Cloner le repo git.
---------------------

Lancez la commande suivante : 

     git clone git@github.com:jocelyndubois/vappet.git

Ceci va télécharger tous les fichiers de config.
Aller sur la branche pgsql :

     git checkout -t origin/pgsql

Etape 4 - Personnalisation
---------------------

Avant de lancer la machine, pensez a personnaliser votre environnement.

A savoir configurer mysql et votre BDD ainsi que votre dossier partagé.

### Etape 4a - Dossier partagé

Allez dans le fichier Vagrantfile à la racine du projet.

 Par défaut le dossier partagé sur la machine hôte est /var/www

### Etape 4b - SQL

Allez dans le fichier puphpet/config.yaml

Vous pouvez dans la partie pgsql personnaliser un utilisateur, ajouter par défaut une BDD et utiliser un fichier SQL pour la peupler.

Etape 5 - Lancer la machine
---------------------

Lancez la commande : 
    - vagrant up
Ceci a pour effet de télécharger, lancer et configurer la machine virtuelle.

Etape 6 - Programmez !
---------------------

Pour accéder à la machine virtuelle, tapez “vagrant ssh”. 

Votre serveur virtuel est pret et accessible à : 
http://192.168.56.103/
Et pgsql à : 
http://192.168.56.103/adminer
- dans "Serveur" entrez **localhost**
- dans "Utilisateur" entrez **postgres**
- dans "Mot de passe" entrez **123**

Explications
=============

Créer facilement sa config
---------------------

Le site https://puphpet.com/ permet de créer facilement sa config, il suffit de suivre les différentes étapes.

A la fin le site propose le téléchargement d’un zip. Dézippez le simplement dans votre dossier et lancer vagrant up.

Modifier la config
---------------------

Pour modifier l’interface web (l’adresse ip par exemple), éditez le fichier “Vagrantfile”.


Informations
=============

MySql
---------------------

De base les infos sont les suivants :
```
Login : root
Mdp : 123
```
PgSql
---------------------

De base les infos sont les suivants :
```
serveur: localhost
Login : postgres
Mdp : 123
```


