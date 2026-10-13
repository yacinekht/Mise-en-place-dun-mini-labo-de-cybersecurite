# 🔐 Mini Labo de Cybersécurité

> **Projet :** Mise en place d'un laboratoire virtuel pour l'apprentissage de l'administration Linux, de la conteneurisation et de l'analyse de logs.  
> **Auteur :** Yacine KHIAT 💻🔐

---

## 📖 Présentation

Ce projet a pour objectif de concevoir un laboratoire pratique de cybersécurité afin de maîtriser les fondamentaux des domaines suivants :
* **Administration Linux**
* **Gestion des services réseau**
* **Conteneurisation (Docker)**
* **Analyse de logs de sécurité**
* **Détection d'incidents**

### 💻 Composants du laboratoire

| Machine / Service | Rôle / Description |
| :--- | :--- |
| 🐉 **Kali Linux** | Machine attaquante (Audit & Tests) |
| 🐧 **Ubuntu Server** | Machine cible (`srv-cible2`) |
| 🔑 **OpenSSH** | Service d'accès distant sécurisé |
| 🧃 **OWASP Juice Shop** | Application Web délibérément vulnérable |
| 🐳 **Docker** | Gestionnaire de conteneurs |

---

## 🏗️ Architecture du Laboratoire

```text
Kali Linux (Attaquante)
  │
  ▼
Ubuntu Server (srv-cible2)
  ├── 🔑 OpenSSH
  ├── 🐳 Docker
  └── 🧃 OWASP Juice Shop (Port 3000)
```

---

## 🛠️ Étapes de Réalisation

### 1️⃣ Création d'un utilisateur de test
Création d'un compte dédié afin de simuler un utilisateur vulnérable et isoler les tests.

```bash
sudo useradd -m -s /bin/bash testuser
sudo passwd testuser
```

> **✅ Ce que j'ai appris :**
> * Créer un utilisateur Linux avec son répertoire personnel (`/home/testuser`).
> * Attribuer et gérer les mots de passe.
> * Isoler les activités de test sur un compte restreint.

---

### 2️⃣ Installation & Configuration du serveur SSH

```bash
# Mise à jour et installation
sudo apt update && sudo apt install openssh-server -y

# Démarrage et activation au boot
sudo systemctl start ssh
sudo systemctl enable ssh

# Vérification du statut
sudo systemctl status ssh
```

> **Statut du service :** `Active: active (running)`

> **✅ Ce que j'ai appris :**
> * Installer un service sous Linux via `apt`.
> * Utiliser `systemctl` pour gérer les démons système.
> * Comprendre le fonctionnement de l'authentification SSH.

---

### 3️⃣ Installation de Docker

```bash
sudo snap install docker

# Vérification de l'installation
docker --version
```

> **✅ Ce que je me suis approprié :**
> * Comprendre le concept d'isolation par conteneurs.
> * Installer et vérifier le bon fonctionnement de Docker.
> * Gérer les privilèges d'exécution.

---

### 4️⃣ Déploiement d'OWASP Juice Shop

```bash
# Lancement du conteneur en arrière-plan
sudo docker run -d \
  --name juice-shop \
  -p 3000:3000 \
  bkimminich/juice-shop

# Vérification du conteneur
sudo docker ps
```

> **Redirection de port :** `0.0.0.0:3000 -> 3000/tcp`

> **✅ Ce que j'ai appris :**
> * Télécharger une image depuis Docker Hub.
> * Exécuter un conteneur en mode détaché (`-d`).
> * Rediriger les flux réseau d'un port hôte vers un port conteneur (`-p`).

---

### 5️⃣ Simulation de connexions SSH depuis Kali Linux

Test d'accès SSH à distance depuis la machine attaquante avec tentative d'échec puis de succès :

```bash
ssh testuser@IP_SERVEUR
```

> **✅ Ce que j'ai appris :**
> * Utiliser le client SSH en ligne de commande.
> * Identifier visuellement une réponse d'authentification réussie ou échouée.

---

### 6️⃣ Analyse des logs de sécurité

Surveillance des tentatives d'accès en temps réel sur la machine cible :

```bash
sudo tail -f /var/log/auth.log
```

* **Exemple d'échec d'authentification :**
  ```text
  Failed password for testuser from 192.168.x.x port 54321 ssh2
  ```
* **Exemple de connexion réussie :**
  ```text
  Accepted password for testuser from 192.168.x.x port 54321 ssh2
  ```

> **✅ Ce que j'ai appris :**
> * Identifier l'adresse IP source et le port d'une tentative d'accès.
> * Extraire le nom d'utilisateur ciblé.
> * Analyser les journaux d'événements Linux (`/var/log/auth.log`) pour détecter des comportements suspect (ex: Brute Force).

---

## 🎯 Synthèse des Notions Retenues

* **SSH :** Protocole d'accès distant sécurisé chiffré.
* **Docker :** Solution de conteneurisation légère pour isoler des applications.
* **Logs :** Traçabilité essentielle pour l'investigation et l'analyse post-incident.
* **Brute Force :** Attaque par essais/erreurs détectable via la répétition d'échecs de mots de passe.

---

## ✅ État du Projet

- [x] Ubuntu Server installé et configuré
- [x] Kali Linux opérationnelle
- [x] Interconnexion réseau établie
- [x] Service SSH fonctionnel
- [x] Compte utilisateur de test créé
- [x] Docker déployé
- [x] Application Juice Shop opérationnelle
- [x] Analyse et interprétation des logs effectuées

---

## 🚀 Prochaine Étape

Mettre en place une solution **SIEM** afin de centraliser, corréler et alerter automatiquement sur les événements de sécurité :
* 🔹 **Wazuh** (Solution open-source EDR/SIEM)
* 🔹 **Microsoft Sentinel**
* 🔹 **Splunk**
