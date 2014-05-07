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

     git checkout -t origin/pgsql_slave

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
http://192.168.56.104/
Et pgsql à :
http://192.168.56.104/adminer

(Infos de connexion à la fin du readme)

Configuration slave / master
=============

Lancer les 2 box : pgsql (master) et pgsql_slave (slave)
---------------------

IP MASTER = 192.168.56.103
IP SLAVE = 192.168.56.104

CONFIGURER MASTER
---------------------

     sudo passwd postgres

(entrer 123)

     sudo su - postgres

     ssh-keygen
     ssh-copy-id 192.168.56.104

Créer l'utilisateur qui va s'occuper de la réplication

     psql -c "CREATE USER rep REPLICATION LOGIN CONNECTION LIMIT 1 ENCRYPTED PASSWORD '123';"

Ajouter ces lignes dans pg_hba.conf:

     vim /etc/postgresql/9.3/main/pg_hba.conf

> host    replication     rep     192.168.56.104/32  md5

Ajouter ces lignes dans postgresql.conf:

     vim /etc/postgresql/9.3/main/postgresql.conf

> listen_addresses = '*'
> wal_level = 'hot_standby'
> archive_mode = on
> archive_command = 'cd .'
> max_wal_senders = 1
> hot_standby = on

     service postgresql restart

CONFIGURER SLAVE
---------------------

     service postgresql stop

     sudo passwd postgres

(entrer 123)

     sudo su - postgres

     ssh-keygen
     ssh-copy-id 192.168.56.104

Ajouter ces lignes dans pg_hba.conf:

     vim /etc/postgresql/9.3/main/pg_hba.conf

> host    replication     rep     192.168.56.103/32  md5

Ajouter ces lignes dans postgresql.conf:

     vim /etc/postgresql/9.3/main/postgresql.conf

> listen_addresses = '*'
> wal_level = 'hot_standby'
> archive_mode = on
> archive_command = 'cd .'
> max_wal_senders = 1
> hot_standby = on

INITIALISER LA BASE SUR SLAVE A PARTIR DE MASTER
---------------------

     psql -c "select pg_start_backup('initial_backup');"
     rsync -cva --inplace --exclude=*pg_xlog* /var/lib/postgresql/9.3/main/ 192.168.56.104:/var/lib/postgresql/9.3/main/
     psql -c "select pg_stop_backup();"

CREER LE FICHIER RECOVERY.CONF SUR SLAVE
---------------------

     vim /var/lib/postgresql/9.3/main/recovery.conf

Ajouter :
> standby_mode = 'on'
> primary_conninfo = 'host=192.168.56.103 port=5432 user=rep password=123'
> trigger_file = '/tmp/postgresql.trigger.5432'

toujours sur slave :
     service postgresql start

Et voilà!


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

PgSql
---------------------

De base les infos sont les suivants :
```
serveur: localhost
Login : postgres
Mdp : 123
```


