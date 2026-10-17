🔐 Mini Labo de Cybersécurité
Projet : Mise en place d'un laboratoire virtuel pour l'apprentissage de l'administration Linux, de la conteneurisation, de l'analyse de logs et de la centralisation SIEM. Auteur : Yacine KHIAT 💻🔐

📖 Présentation
Ce projet a pour objectif de concevoir un laboratoire pratique de cybersécurité afin de maîtriser les fondamentaux des domaines suivants :

Administration Linux
Gestion des services réseau
Conteneurisation (Docker)
Analyse de logs de sécurité
Détection d'incidents
Centralisation et corrélation d'événements via un SIEM (Wazuh)
💻 Composants du laboratoire
Machine / Service	Rôle / Description
🐉 Kali Linux	Machine attaquante (Audit & Tests)
🐧 Ubuntu Server	Machine cible (srv-cible2)
🔑 OpenSSH	Service d'accès distant sécurisé
🧃 OWASP Juice Shop	Application Web délibérément vulnérable
🐳 Docker	Gestionnaire de conteneurs
🛡️ Wazuh	Solution SIEM/EDR open-source (Manager, Indexer, Dashboard)
🏗️ Architecture du Laboratoire
Kali Linux (Attaquante)
  │
  ▼
Ubuntu Server (srv-cible2)
  ├── 🔑 OpenSSH
  ├── 🐳 Docker
  ├── 🧃 OWASP Juice Shop (Port 3000)
  └── 🛡️ Wazuh (Manager + Indexer + Dashboard, Port 443)
         └── Agent Wazuh (collecte /var/log/auth.log + logs Docker)
🛠️ Étapes de Réalisation
1️⃣ Création d'un utilisateur de test
Création d'un compte dédié afin de simuler un utilisateur vulnérable et isoler les tests.

bash
sudo useradd -m -s /bin/bash testuser
sudo passwd testuser
✅ Ce que j'ai appris :

Créer un utilisateur Linux avec son répertoire personnel (/home/testuser).
Attribuer et gérer les mots de passe.
Isoler les activités de test sur un compte restreint.
2️⃣ Installation & Configuration du serveur SSH
bash
# Mise à jour et installation
sudo apt update && sudo apt install openssh-server -y

# Démarrage et activation au boot
sudo systemctl start ssh
sudo systemctl enable ssh

# Vérification du statut
sudo systemctl status ssh
Statut du service : Active: active (running)

✅ Ce que j'ai appris :

Installer un service sous Linux via apt.
Utiliser systemctl pour gérer les démons système.
Comprendre le fonctionnement de l'authentification SSH.
3️⃣ Installation de Docker
bash
sudo snap install docker

# Vérification de l'installation
docker --version
✅ Ce que je me suis approprié :

Comprendre le concept d'isolation par conteneurs.
Installer et vérifier le bon fonctionnement de Docker.
Gérer les privilèges d'exécution.
4️⃣ Déploiement d'OWASP Juice Shop
bash
# Lancement du conteneur en arrière-plan
sudo docker run -d \
  --name juice-shop \
  -p 3000:3000 \
  bkimminich/juice-shop

# Vérification du conteneur
sudo docker ps
Redirection de port : 0.0.0.0:3000 -> 3000/tcp

✅ Ce que j'ai appris :

Télécharger une image depuis Docker Hub.
Exécuter un conteneur en mode détaché (-d).
Rediriger les flux réseau d'un port hôte vers un port conteneur (-p).
5️⃣ Simulation de connexions SSH depuis Kali Linux
Test d'accès SSH à distance depuis la machine attaquante avec tentative d'échec puis de succès :

bash
ssh testuser@IP_SERVEUR
✅ Ce que j'ai appris :

Utiliser le client SSH en ligne de commande.
Identifier visuellement une réponse d'authentification réussie ou échouée.
6️⃣ Analyse manuelle des logs de sécurité
Surveillance des tentatives d'accès en temps réel sur la machine cible :

bash
sudo tail -f /var/log/auth.log
Exemple d'échec d'authentification :

Failed password for testuser from 192.168.x.x port 54321 ssh2
Exemple de connexion réussie :

Accepted password for testuser from 192.168.x.x port 54321 ssh2
✅ Ce que j'ai appris :

Identifier l'adresse IP source et le port d'une tentative d'accès.
Extraire le nom d'utilisateur ciblé.
Analyser les journaux d'événements Linux (/var/log/auth.log) pour détecter des comportements suspects (ex: Brute Force).
7️⃣ Installation de Wazuh (Manager + Indexer + Dashboard)
Déploiement de la stack SIEM complète en mode "all-in-one" sur srv-cible2 afin de centraliser la collecte et l'analyse des événements.

bash
# Téléchargement du script d'installation officiel
curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh

# Installation complète (manager + indexer + dashboard)
sudo bash wazuh-install.sh -a

# Récupération des identifiants générés
sudo tar -xvf wazuh-install-files.tar
sudo cat wazuh-install-files/wazuh-passwords.txt
⚠️ Point de vigilance rencontré : l'installation nécessite 4 Go de RAM minimum (recommandation officielle). Une première tentative avec une RAM insuffisante a provoqué un oom-kill sur le processus wazuh-indexer (tué par l'OOM Killer du noyau). La VM a été réallouée à 5 Go de RAM, ce qui a permis à l'indexeur de démarrer correctement sans interruption.

Accès au dashboard : https://192.168.56.102 Identifiants : admin / mot de passe généré dans wazuh-passwords.txt

✅ Ce que j'ai appris :

Architecture d'un SIEM (Manager / Indexer / Dashboard).
Dimensionnement des ressources système pour un service basé sur la JVM (Java).
Diagnostic d'un échec de service via systemctl status et journalctl (identification d'un oom-kill).
Déploiement d'une stack de sécurité centralisée.
Gestion des certificats auto-signés générés par l'installeur.
8️⃣ Déploiement de l'agent Wazuh sur srv-cible2
Installation de l'agent qui collecte et transmet les événements système au manager :

bash
curl -o wazuh-agent.deb https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.9.0-1_amd64.deb

sudo WAZUH_MANAGER='192.168.56.102' dpkg -i ./wazuh-agent.deb

sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
✅ Ce que j'ai appris :

Modèle agent/serveur en cybersécurité.
Transmission sécurisée des événements (ports 1514/1515).
Vérification de l'enrôlement d'un agent côté manager.
9️⃣ Détection automatique du Brute Force SSH via Wazuh
Génération de plusieurs tentatives d'authentification échouées depuis Kali pour valider la détection automatique :

bash
for i in {1..6}; do ssh testuser@192.168.56.102 -o PreferredAuthentications=password; done
Résultat observé dans le dashboard Wazuh (menu Security Events) : déclenchement de la règle 5712 - SSHD brute force trying to get access to the system (niveau de sévérité 10).

✅ Ce que j'ai appris :

Lire et interpréter les règles Wazuh (sshd_rules.xml).
Comprendre les niveaux de sévérité (level 0 à 16).
Exploiter le dashboard pour une investigation visuelle rapide, sans dépendre d'un tail -f manuel.
🔟 Surveillance de Juice Shop via Wazuh
Intégration des logs applicatifs du conteneur Docker dans la collecte Wazuh :

bash
sudo docker logs -f juice-shop > /var/log/juice-shop.log &
Ajout dans la configuration de l'agent (/var/ossec/etc/ossec.conf) :

xml
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/juice-shop.log</location>
</localfile>
✅ Ce que j'ai appris :

Intégrer des logs applicatifs non-système dans un SIEM.
Corréler des événements web (scans, tentatives d'injection) avec l'activité réseau et système.
🎯 Synthèse des Notions Retenues
SSH : Protocole d'accès distant sécurisé chiffré.
Docker : Solution de conteneurisation légère pour isoler des applications.
Logs : Traçabilité essentielle pour l'investigation et l'analyse post-incident.
Brute Force : Attaque par essais/erreurs détectable via la répétition d'échecs de mots de passe.
SIEM : Outil centralisant, corrélant et alertant sur les événements de sécurité de plusieurs sources.
Agent/Manager : Modèle de collecte distribuée des logs.
Règles de détection : Logique permettant de transformer un simple log en alerte exploitable.
Dimensionnement système : Importance d'allouer suffisamment de ressources (RAM/CPU) pour des services critiques basés sur la JVM.
✅ État du Projet
 Ubuntu Server installé et configuré
 Kali Linux opérationnelle
 Interconnexion réseau établie
 Service SSH fonctionnel
 Compte utilisateur de test créé
 Docker déployé
 Application Juice Shop opérationnelle
 Analyse et interprétation des logs effectuées (manuelle)
 Wazuh Manager / Indexer / Dashboard installés
 Agent Wazuh déployé sur srv-cible2
 Détection brute force SSH validée via le dashboard
 Intégration des logs Juice Shop dans Wazuh
 Création de règles de détection personnalisées (local_rules.xml)
 Mise en place de notifications (email/Slack) sur alerte critique
