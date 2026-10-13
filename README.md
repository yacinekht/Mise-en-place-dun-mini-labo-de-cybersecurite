<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Labo de Cybersécurité</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #0b0f19;
            --card-bg: #111827;
            --card-border: #1f293d;
            --accent-blue: #38bdf8;
            --accent-purple: #818cf8;
            --text-main: #f3f4f6;
            --text-muted: #9ca3af;
            --success: #34d399;
            --code-bg: #030712;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            line-height: 1.6;
            padding: 40px 20px;
        }

        .container {
            max-width: 1000px;
            margin: 0 auto;
        }

        /* Header */
        header {
            text-align: center;
            margin-bottom: 50px;
            position: relative;
        }

        header h1 {
            font-size: 2.5rem;
            font-weight: 700;
            background: linear-gradient(135deg, var(--accent-blue), var(--accent-purple));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 10px;
        }

        header p {
            color: var(--text-muted);
            font-size: 1.1rem;
        }

        /* Card */
        .card {
            background: var(--card-bg);
            border: 1px solid var(--card-border);
            border-radius: 16px;
            padding: 28px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.5);
            transition: transform 0.2s ease, border-color 0.2s ease;
        }

        .card:hover {
            border-color: rgba(56, 189, 248, 0.4);
            transform: translateY(-2px);
        }

        h2 {
            font-size: 1.4rem;
            color: #ffffff;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        h3 {
            font-size: 1rem;
            color: var(--accent-blue);
            margin: 20px 0 10px 0;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        /* Lists */
        ul {
            list-style: none;
            padding-left: 0;
        }

        ul li {
            position: relative;
            padding-left: 24px;
            margin-bottom: 10px;
            color: #d1d5db;
        }

        ul li::before {
            content: "▹";
            position: absolute;
            left: 0;
            color: var(--accent-blue);
            font-weight: bold;
        }

        /* Table */
        .table-container {
            overflow-x: auto;
            margin-top: 15px;
            border-radius: 8px;
            border: 1px solid var(--card-border);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
        }

        table th {
            background-color: #1f2937;
            color: var(--accent-blue);
            font-weight: 600;
            padding: 14px 16px;
            border-bottom: 1px solid var(--card-border);
        }

        table td {
            padding: 14px 16px;
            border-bottom: 1px solid var(--card-border);
            color: #d1d5db;
        }

        table tr:last-child td {
            border-bottom: none;
        }

        table tr:hover td {
            background-color: rgba(255, 255, 255, 0.02);
        }

        /* Terminal / Code block */
        .terminal {
            background: var(--code-bg);
            border: 1px solid var(--card-border);
            border-radius: 10px;
            overflow: hidden;
            margin: 15px 0;
        }

        .terminal-header {
            background: #1f2937;
            padding: 8px 15px;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .dot {
            width: 10px;
            height: 10px;
            border-radius: 50%;
            display: inline-block;
        }

        .dot-red { background: #ef4444; }
        .dot-yellow { background: #f59e0b; }
        .dot-green { background: #10b981; }

        pre {
            font-family: 'Fira Code', monospace;
            padding: 16px;
            overflow-x: auto;
            color: #38bdf8;
            font-size: 0.9rem;
            line-height: 1.5;
        }

        .status-badge {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            background: rgba(52, 211, 153, 0.1);
            color: var(--success);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 600;
            margin-top: 10px;
            border: 1px solid rgba(52, 211, 153, 0.2);
        }

        /* Architecture schema */
        .architecture-box {
            background: var(--code-bg);
            border: 1px dashed var(--accent-blue);
            border-radius: 12px;
            padding: 25px;
            text-align: center;
            font-family: 'Fira Code', monospace;
            color: #e5e7eb;
            line-height: 1.8;
        }

        /* Footer */
        footer {
            text-align: center;
            margin-top: 60px;
            padding-top: 20px;
            border-top: 1px solid var(--card-border);
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        footer strong {
            color: var(--text-main);
        }
    </style>
</head>
<body>

<div class="container">

    <header>
        <h1>🔐 Mini Labo de Cybersécurité</h1>
        <p>Documentation d'installation, configuration et analyse de sécurité</p>
    </header>

    <!-- Présentation -->
    <div class="card">
        <h2>📖 Présentation</h2>
        <p>Ce projet a pour objectif de mettre en place un laboratoire de cybersécurité afin d'apprendre les bases de :</p>
        <ul style="margin-top: 15px;">
            <li>Administration Linux</li>
            <li>Gestion des services réseau</li>
            <li>Conteneurisation (Docker)</li>
            <li>Analyse de logs de sécurité</li>
            <li>Détection d'incidents</li>
        </ul>

        <div class="table-container">
            <table>
                <thead>
                    <tr>
                        <th>Machine / Techno</th>
                        <th>Rôle</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>🐉 <strong>Kali Linux</strong></td>
                        <td>Machine attaquante</td>
                    </tr>
                    <tr>
                        <td>🐧 <strong>Ubuntu Server</strong></td>
                        <td>Machine cible</td>
                    </tr>
                    <tr>
                        <td>🔑 <strong>OpenSSH</strong></td>
                        <td>Accès distant sécurisé</td>
                    </tr>
                    <tr>
                        <td>🧃 <strong>OWASP Juice Shop</strong></td>
                        <td>Application web vulnérable</td>
                    </tr>
                    <tr>
                        <td>🐳 <strong>Docker</strong></td>
                        <td>Gestion des conteneurs</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

    <!-- Architecture -->
    <div class="card">
        <h2>🏗️ Architecture du laboratoire</h2>
        <div class="architecture-box">
            <span style="color: #f87171;">Kali Linux</span> (Attaquante)<br>
            │<br>
            ▼<br>
            <span style="color: var(--accent-blue);">Ubuntu Server</span> (srv-cible2)<br>
            ├── 🔑 OpenSSH<br>
            ├── 🐳 Docker<br>
            └── 🧃 OWASP Juice Shop
        </div>
    </div>

    <!-- Étape 1 -->
    <div class="card">
        <h2>1️⃣ Création d'un utilisateur de test</h2>
        <p>Création d'un compte dédié afin de simuler un utilisateur vulnérable.</p>
        
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo useradd -m -s /bin/bash testuser
sudo passwd testuser</pre>
        </div>

        <h3>✅ Ce que j'ai appris</h3>
        <ul>
            <li>Créer un utilisateur Linux avec son répertoire personnel</li>
            <li>Attribuer et sécuriser un mot de passe</li>
            <li>Isoler les tests sur un compte dédié sans privilèges globaux</li>
        </ul>
    </div>

    <!-- Étape 2 -->
    <div class="card">
        <h2>2️⃣ Installation du serveur SSH</h2>
        
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo apt update && sudo apt install openssh-server -y
sudo systemctl start ssh
sudo systemctl enable ssh</pre>
        </div>

        <p>Vérification du statut du service :</p>
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo systemctl status ssh</pre>
        </div>
        <div class="status-badge">✔ Active (running)</div>

        <h3>✅ Ce que j'ai appris</h3>
        <ul>
            <li>Installer et mettre à jour un service Linux</li>
            <li>Gérer les services au démarrage avec <code>systemd</code></li>
            <li>Comprendre les principes fondamentaux du protocole SSH</li>
        </ul>
    </div>

    <!-- Étape 3 -->
    <div class="card">
        <h2>3️⃣ Installation de Docker</h2>
        
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo snap install docker
docker --version</pre>
        </div>

        <h3>✅ Ce que j'ai appris</h3>
        <ul>
            <li>Comprendre le principe et l'intérêt des conteneurs</li>
            <li>Installer et configurer l'environnement Docker</li>
            <li>Gérer les permissions utilisateur pour Docker</li>
        </ul>
    </div>

    <!-- Étape 4 -->
    <div class="card">
        <h2>4️⃣ Déploiement d'OWASP Juice Shop</h2>
        
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo docker run -d \
  --name juice-shop \
  -p 3000:3000 \
  bkimminich/juice-shop</pre>
        </div>

        <p>Vérification du conteneur actif :</p>
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo docker ps
# Output: 0.0.0.0:3000 -> 3000/tcp</pre>
        </div>

        <h3>✅ Ce que j'ai appris</h3>
        <ul>
            <li>Télécharger une image depuis Docker Hub</li>
            <li>Lancer un conteneur en arrière-plan (mode détaché)</li>
            <li>Rediriger et exposer un port réseau de l'hôte vers un conteneur</li>
        </ul>
    </div>

    <!-- Étape 5 -->
    <div class="card">
        <h2>5️⃣ Test de connexion SSH depuis Kali</h2>
        
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>ssh testuser@IP_SERVEUR</pre>
        </div>
        <p><em>Simulation : Plusieurs tentatives de connexion échouées avec mot de passe erroné, suivies d'une connexion réussie.</em></p>

        <h3>✅ Ce que j'ai appris</h3>
        <ul>
            <li>Utiliser le client SSH en ligne de commande</li>
            <li>Comprendre le flux d'authentification distante</li>
            <li>Différencier le comportement lors d'un échec et d'un succès de connexion</li>
        </ul>
    </div>

    <!-- Étape 6 -->
    <div class="card">
        <h2>6️⃣ Analyse des logs de sécurité</h2>
        
        <div class="terminal">
            <div class="terminal-header">
                <span class="dot dot-red"></span>
                <span class="dot dot-yellow"></span>
                <span class="dot dot-green"></span>
            </div>
            <pre>sudo tail -f /var/log/auth.log</pre>
        </div>

        <p>Échec d'authentification (Brute Force / Erreur) :</p>
        <div class="terminal">
            <pre style="color: #f87171;">Failed password for testuser from 192.168.x.x port 54321 ssh2</pre>
        </div>

        <p>Connexion réussie :</p>
        <div class="terminal">
            <pre style="color: #34d399;">Accepted password for testuser from 192.168.x.x port 54321 ssh2</pre>
        </div>

        <h3>✅ Ce que j'ai appris</h3>
        <ul>
            <li>Identifier l'adresse IP source d'une tentative d'accès</li>
            <li>Repérer le compte utilisateur ciblé</li>
            <li>Lire et interpréter les journaux système Linux en temps réel</li>
        </ul>
    </div>

    <!-- Notions & État -->
    <div class="card">
        <h2>🎯 Notions clés & État du projet</h2>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-top: 15px;">
            <div>
                <h3>📌 Notions retenues</h3>
                <ul>
                    <li><strong>SSH :</strong> Accès distant sécurisé.</li>
                    <li><strong>Docker :</strong> Isolation d'applications.</li>
                    <li><strong>Logs :</strong> Traçabilité des événements.</li>
                    <li><strong>Brute Force :</strong> Attaque par dictionnaire/tentatives.</li>
                </ul>
            </div>
            <div>
                <h3>📋 État actuel</h3>
                <ul>
                    <li><span style="color: var(--success);">✔</span> Infrastructure opérationnelle</li>
                    <li><span style="color: var(--success);">✔</span> SSH & Docker configurés</li>
                    <li><span style="color: var(--success);">✔</span> Juice Shop fonctionnel</li>
                    <li><span style="color: var(--success);">✔</span> Logs analysés</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Prochaine étape -->
    <div class="card" style="border-left: 4px solid var(--accent-purple);">
        <h2>🚀 Prochaine étape</h2>
        <p>Mettre en place une solution <strong>SIEM</strong> afin de centraliser, analyser et alerter sur les événements de sécurité :</p>
        <ul style="margin-top: 10px;">
            <li><strong>Wazuh</strong> (Open Source EDR/SIEM)</li>
            <li><strong>Microsoft Sentinel</strong></li>
            <li><strong>Splunk</strong></li>
        </ul>
    </div>

    <footer>
        Projet réalisé par <strong>Yacine KHIAT</strong> 💻🔐
    </footer>

</div>

</body>
</html>
