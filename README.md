README - Mise en place d'un mini labo de cybersécurité
Objectif

L'objectif de ce laboratoire est de créer un environnement de test permettant d'apprendre les bases de la détection d'événements de sécurité sur une machine Linux.

L'environnement est composé :

d'une machine attaquante : Kali Linux
d'une machine cible : Ubuntu Server (srv-cible2)
d'un service SSH
d'une application web vulnérable (OWASP Juice Shop) déployée avec Docker
1. Création d'un utilisateur de test

Afin de simuler un compte vulnérable sans impacter mon compte principal, j'ai créé un utilisateur dédié :

Shell
sudo useradd -m -s /bin/bash testuser
sudo passwd testuser
Ce que j'ai appris
Créer un nouvel utilisateur Linux.
Associer un mot de passe à un utilisateur.
Isoler les tests sur un compte spécifique.
2. Installation et vérification du serveur SSH

J'ai vérifié l'état du service SSH :

Shell
sudo systemctl status ssh

Le service n'était pas installé ou actif.

J'ai alors installé OpenSSH Server :

Shell
sudo apt update
sudo apt install openssh-server -y
 

Puis démarré le service :

Shell
sudo systemctl start ssh
sudo systemctl enable ssh
Vérification
Shell
sudo systemctl status ssh

Résultat attendu :

Plain Text
Active: active (running)
Ce que j'ai appris
À quoi sert SSH.
Comment installer un service sous Ubuntu.
Comment démarrer et vérifier un service avec systemctl.
3. Installation de Docker

Docker n'était pas disponible par défaut.

Installation :

Shell
sudo snap install docker

Vérification :

Shell
docker --version
Problème rencontré

Erreur :

Plain Text
permission denied while trying to connect to the Docker API

Solution temporaire :

Shell
sudo docker ...
Ce que j'ai appris
Docker fonctionne grâce à un service système.
Certains utilisateurs n'ont pas les droits nécessaires sur Docker.
Le préfixe sudo permet d'exécuter une commande avec les privilèges administrateur.
4. Déploiement d'OWASP Juice Shop

Lancement du conteneur :

Shell
sudo docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop

Vérification :

Shell
sudo docker ps

Résultat observé :

Plain Text
0.0.0.0:3000->3000/tcp
Test de fonctionnement
Shell
curl http://localhost:3000

Le code HTML de l'application est retourné.

Ce que j'ai appris
Télécharger et exécuter une image Docker.
Comprendre le mapping de ports :
Plain Text
3000:3000
Vérifier qu'un conteneur fonctionne correctement.
5. Test de connexion SSH

Depuis Kali Linux :

Shell
ssh testuser@IP_DE_SRV_CIBLE2

J'ai volontairement saisi plusieurs mauvais mots de passe.

Ensuite j'ai utilisé le bon mot de passe.

Ce que j'ai appris
Établir une connexion SSH.
Comprendre le mécanisme d'authentification.
6. Observation des logs de sécurité

Sur la machine cible :

Shell
sudo tail -f /var/log/auth.log

Pendant les tentatives SSH, plusieurs événements sont apparus.

Exemple de logs

Échec :

Plain Text
Failed password for testuser from 192.168.x.x

Succès :

Plain Text
Accepted password for testuser from 192.168.x.x
Ce que j'ai appris

Je sais maintenant identifier :

la date et l'heure d'un événement ;
l'adresse IP source ;
l'utilisateur ciblé ;
une authentification réussie ;
une authentification échouée.
Notions retenues
SSH

Permet la connexion distante sécurisée à un serveur.

Docker

Permet d'exécuter des applications dans des conteneurs isolés.

Logs

Les logs enregistrent les événements qui se produisent sur une machine.

Brute Force

Une attaque brute force consiste à tester de nombreux mots de passe jusqu'à trouver le bon.

Détection

Une attaque brute force peut souvent être repérée grâce à de nombreuses lignes :

Plain Text
Failed password

dans les journaux d'authentification.

État actuel du laboratoire

✅ Ubuntu Server installé

✅ Kali Linux opérationnel

✅ Communication réseau fonctionnelle

✅ SSH actif

✅ Utilisateur de test créé

✅ Docker installé

✅ OWASP Juice Shop déployé

✅ Observation des logs SSH maîtrisée
