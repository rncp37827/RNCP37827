# Ground Control (Version factice)

Repo complet :  `[https://github.com/elpulpo0/RNCP37827](https://github.com/elpulpo0/RNCP37827)`

## Présentation

**Ground Control** est une application centrale conçue pour optimiser la gestion, la saisie, l'organisation et le transfert des données au sein de l'entreprise *Microphyt*. Elle répond aux besoins des départements suivants :
- **Production** : Gestion des cultures.
- **R&D** : Analyse de la productivité.
- **Laboratoire** : Saisie des suivis journaliers.

Une application complémentaire, **Admin_Dashboard**, permet de gérer les utilisateurs (rôles, scopes, etc.) et les bases de données des différentes applications (sauvegardes, vérifications d'intégrité, etc.) Pour les besoins de cette présentation, le backend de cette application sera intégré à ce repo sous le service nommé "Auth"

### Architecture
- **Base de données "rncp37827_db"** : Une base de données unique et commune à tous les services, implémentée avec **PostGreSQL**.
- **Base de donnée "users"** : Une base de données qui sert la gestion des utilsateurs, des rôles, des scopes des différentes applications, implémentée avec **SQLite**.
- **Frontend** : Interface développée avec **Vue.js**.
- **Backend** et **Auth** : APIs développées avec **Python** et **FastAPI**.
- **Données factices** : Pour des raisons de confidentialité, les résultats des requêtes API (supervision des cultures) et les données initiales de la base de données "rncp37827_db" sont factices dans cette version.

### Structure

```sh
RNCP37827/
|── .github/
│   └── workflows/                                      # Dossier contenant les workflows Github
│       ├── Format and test.yml                         # Formatage du code Python et Javascript avec Ruff et ESLint + Tests unitaires
│       ├── Test docker-compose.yml                     # Test de création des différents services et vérification de la bonne santé des API
│       └── Update release.yml                          # Scripts d'automatisation des release sur Github pour un meilleur suivi du versionnage
# Partie Authentification
├── auth/                                               # Code backend pour la gestion de l'authentification (API, logique serveur)
│   ├── backups/                                        # Fichiers backups de la base de donnée générés automatiquement au démarrage et toute les 24h
│   ├── database/                                       # Fichiers de base de données générés
│   ├── logs/                                           # Fichiers de logs (erreurs, débogage, informations)
│   ├── modules/                                        # Modules Python principaux
│   │   ├── api/                                        # API REST (FastAPI, routes, schémas...)
│   │   │   ├── auth/                                   # Authentification (routes, modèles, sécurité)
│   │   │   │   ├── functions.py
│   │   │   │   ├── models.py
│   │   │   │   ├── routes.py
│   │   │   │   ├── schemas.py
│   │   │   │   └── security.py
│   │   │   ├── notifs/                                 # Notifications Telegram
│   │   │   │   ├── bot_telegram.py
│   │   │   │   ├── classes.py
│   │   │   │   ├── functions.py
│   │   │   │   └── routes.py
│   │   │   ├── users/                                  # Gestion des utilisateurs (modèles, routes...)
│   │   │   │   ├── create_db.py
│   │   │   │   ├── functions.py
│   │   │   │   ├── initial_users.yaml                  # Données initiales des utilisateurs
│   │   │   │   ├── models.py
│   │   │   │   ├── routes.py
│   │   │   │   └── schemas.py
│   │   │   └──  main.py                                # Point d'entrée FastAPI
│   │   └── database/                                   # Configuration et session de la base de données
│   │       ├── config.py
│   │       ├── dependencies.py
│   │       └── session.py
│   ├── tests/                                          # Fichiers de configuration et de tests Pytest
│   ├── utils/
│   │   └── logger_config.py                            # Configuration du logger
│   ├── Dockerfile                                      # Dockerfile pour construire l'image auth
│   ├── entrypoint.sh                                   # Script nécessaire au build de l'image auth
│   └── requirements.txt                                # Dépendances Python
# Partie Backend
├── backend/                                            # Code backend pour la gestion de l'authentification (API, logique serveur)
│   ├── backups/                                        # Fichiers backups de la base de donnée générés automatiquement au démarrage et toute les 24h
│   ├── database/                                       # Fichiers de base de données générés
│   ├── logs/                                           # Fichiers de logs (erreurs, débogage, informations)
│   ├── modules/                                        # Modules Python principaux
│   │   ├── api/                                        # API REST (FastAPI, routes, schémas...)
│   │   │   ├── predictions/                            # Api dédiée aux modèles IA (routes, fonctions...)
│   │   │   │   ├── functions.py
│   │   │   │   ├── models.py
│   │   │   │   └── routes.py
│   │   │   ├── production/                             # Gestion de la production (routes, modèles...)
│   │   │   │   ├── analyses/                           # Module pour les analyses
│   │   │   │   │   ├── classes_analyses.py
│   │   │   │   │   ├── models_analyses.py
│   │   │   │   │   └── routes_analyses.py
│   │   │   │   ├── cultures/                           # Module pour les cultures
│   │   │   │   │   ├── classes_cultures.py
│   │   │   │   │   ├── models_cultures.py
│   │   │   │   │   └── routes_cultures.py
│   │   │   │   ├── ensembl_algaebase_geo_wikimedia/    # Module pour les Metadata externes
│   │   │   │   │   ├── classes_ensembl_algaebase_geo_wikimedia.py
│   │   │   │   │   ├── models_ensembl_algaebase_geo_wikimedia.py
│   │   │   │   │   └── routes_ensembl_algaebase_geo_wikimedia.py
│   │   │   │   ├── pbrs_species_strains_std/           # Module pour les PBRs, espèces, souches et startdaycodes
│   │   │   │   │   ├── classes_pbrs_species_std.py
│   │   │   │   │   ├── models_pbrs_species_std.py
│   │   │   │   │   └── routes_pbrs_species_std.py
│   │   │   │   ├── pilotage/                           # Module pour le pilotage
│   │   │   │   │   ├── classes_pilotage.py
│   │   │   │   │   ├── models_pilotage.py
│   │   │   │   │   └── routes_pilotage.py
│   │   │   │   ├── prelevements/                       # Module pour les prélèvements
│   │   │   │   │   ├── classes_prelevements.py
│   │   │   │   │   ├── models_prelevements.py
│   │   │   │   │   └── routes_prelevements.py
│   │   │   │   └── recoltes/                           # Module pour les récoltes
│   │   │   │       ├── classes_recoltes.py
│   │   │   │       ├── models_recoltes.py
│   │   │   │       └── routes_recoltes.py
│   │   │   ├──  main.py                                # Point d'entrée FastAPI
│   │   │   └──  utils.py                               # Utilitaire supplémentaire pour FastAPI
│   │   └── database/
│   │       ├── config_create_db.py                     # Données initiales
│   │       ├── config_database.py                      # Config et sessions DB (engines, locals)
│   │       ├── config_fake_data.py                     # Données factices
│   │       ├── config_indexes.py                       # Création et gestion des index DB
│   │       ├── create_db.py                            # Script principal de création DB
│   │       ├── dependencies.py                         # Dépendances DB
│   │       ├── generate_fake_data.py                   # Génération et insertion de données factices
│   │       ├── import_from_AlgaeBase.py                # Scraping et insertion depuis algaebase.org
│   │       ├── import_from_Ensembl.py                  # Import et insertion depuis base Ensembl
│   │       ├── import_from_excel.py                    # Import des fichiers Excel
│   │       ├── import_from_GEO.py                      # Import datasets GSE via Entrez/Biopython
│   │       └── session.py                              # Gestionnaires de sessions DB
│   ├── utils/
│   │   └── logger_config.py                            # Configuration du logger
│   ├── Dockerfile                                      # Dockerfile pour construire l'image backend
│   ├── entrypoint.sh                                   # Script nécessaire au build de l'image backend
│   └── requirements.txt                                # Dépendances Python
# Partie Frontend
├── frontend/                                           # Code frontend de l'application (Vue.js)
│   ├── public/                                         # Fichiers publics (favicon, assets statiques)
│   ├── scripts/                                        # Script custom pour l'automatisation des release Github
│   ├── src/                                            # Code source Vue.js
│   │   ├── assets/                                     # Images, logos, styles globaux
│   │   ├── components/                                 # Composants Vue réutilisables
│   │   ├── functions/                                  # Fonctions utilitaires frontend (TypeScript)
│   │   ├── main.ts                                     # Point d'entrée frontend Vue
│   │   ├── pages/                                      # Pages Vue (ex. Users.vue)
│   │   ├── router/                                     # Gestion des routes Vue Router
│   │   ├── stores/                                     # Gestion de l'état global (Pinia)
│   │   └── styles/                                     # Fichiers CSS / styles globaux
│   ├── Dockerfile                                      # Dockerfile pour construire l'image frontend
│   ├── index.html                                      # Page HTML principale
│   └── package.json                                    # Dépendances et scripts frontend (npm) 
# Autres
├── scripts/                                      # Scripts utilitaires pour le développement ou la gestion
│   ├── analyse_db.py                                   # Script pour analyser la base de données (tables, types, etc..)
│   ├── check_versions.sh                               # Script pour vérifier les versions des outils prérequis
│   ├── generate_secret_key.py                          # Script pour générer une clé secrète (ex. JWT)
│   ├── postgre_drop_db.py                              # Script permettant de réinitialiser la base de donnée PostGreSQL
│   └── print_tree.py                                   # Script pour afficher la structure de l'arborescence du projet
├── .env.example                                        # Fichier d'exemples des variables d'environnement à compléter
├── .gitignore
├── docker-compose.yml                                  # Orchestration Docker
├── README.md
├── requirements.txt                                    # Dépendances Python (développement local)
├── run_auth.py                                         # Script pour exécuter la gestion de l'authentification (développement local)
├── run_backend.py                                      # Script pour exécuter le backend (développement local)
└── run_frontend.py                                     # Script pour exécuter le frontend (développement local)
```

**Si vous souhaitez simplement tester l'application, visitez `https://rncp37827.elpulpo.xyz` et rendez-vous directement [à l'étape 6](#6-tester-lapplication), sinon continuez votre lecture.**

## Prérequis

Avant de commencer, assurez-vous d'avoir les outils suivants installés sur votre machine :
- **Git** : Pour cloner le dépôt.
- Un éditeur de texte (ex. : **VS Code**) pour modifier le fichier `.env`.
- **Docker** : Pour exécuter l'ensemble de l'application en mode conteneur (optionnel, mais recommandé pour simplifier le lancement sans multiples terminaux).

Si vous préférez exécuter les modules de manière indépendantes, il vous faudra également :
- **Python** (version 3.8 ou supérieure) et **Postgres** (version 16 ou supérieure) : Pour exécuter le backend.
- **Node.js** (version 16 ou supérieure) et **npm** : Pour exécuter le frontend.
- **Java** (JDK 17 ou supérieur) et **Apache Spark** (version 4.0 ou supérieure) : Pour l'utilisation de la partie Big data.

Vérifiez vos versions avec la commande `bash scripts/check_versions.sh`

Pour informations, la stabilité de l'application a été validée avec ces versions :
- **Python**: Python 3.13.2
- **Postgres**: psql (PostgreSQL) 18.0
- **Node.js**: v22.14.0
- **npm**: 11.3.0
- **Java**: openjdk version "17.0.16" 2025-07-15
            OpenJDK Runtime Environment Temurin-17.0.16+8 (build 17.0.16+8)
            OpenJDK 64-Bit Server VM Temurin-17.0.16+8 (build 17.0.16+8, mixed mode, sharing)
- **Spark**: 4.0.1

## Installation

Suivez les étapes ci-dessous pour installer et exécuter l'application.

### 1. Clonez le dépôt

Clonez le dépôt GitHub dans le répertoire de votre choix :

```bash
git clone https://github.com/elpulpo0/RNCP37827.git
cd RNCP37827
```

### 2. Configurez l'environnement

**Créez le fichier d'environnement :**

```bash
cp .env.example .env
```

- Ouvrez le fichier `.env` dans un éditeur de texte et complétez les variables `PORT_*` avec les ports de votre choix. (Il y en a 3 : backend, interface et authentification)
- Créez un utilsateur et un mot de passe pour la base de donnée Postgres.
- Générez et remplissez les variables APY_KEY (Pour protéger les routes du module "predictions" ) et SECRET_KEY (qui sera utilisée pour le hashage des emails et des mots de passe du service "Auth") : 

```bash
python scripts/generate_secret_keys.py
```

### 3. Configurez les utilisateurs initiaux (Facultatif)

**Ajoutez des utilsateurs au fichier de configuration**

`auth/modules/api/users/initial_users.yaml`

### 4. Exécution

### 4.1 Lancez les modules avec Docker (Recommandé)

Si vous utilisez Docker, vous n'aurez pas besoin d'ouvrir plusieurs terminaux. Cela lancera l'ensemble (Auth, Backend, Frontend, Postgres et Spark) en une seule instance via Docker Compose.

```bash
docker compose up -d --build
```

Passez ensuite [à l'étape 5](#5-acceder-à-lapplication).

### 4.2 Lancez les modules séparément (Facultatif)

#### 4.2.1 Préparez votre environnement python

**Créez un environnement virtuel :**

```bash
python -m venv .venv
```

**Activez l'environnement virtuel :**

- Sur **Windows** en shell :
  ```bash
  .venv\Scripts\activate
  ```
- Sur **Windows** en bash :
  ```bash
  source .venv/Scripts/activate
  ```
- Sur **Linux/Mac** :
    ```bash
  source .venv/bin/activate
  ```

**Mettez à jour pip et installez les dépendances :**

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

#### 4.2.2 Lancez le module Auth

Ouvrez un premier terminal et lancez cette commande :

```bash
python run_auth.py
```

**Bonus**

Cette application est livrée avec un bot Telegram permettant de notifier l'administrateur lorsqu'un nouvel utilsateur est crée ou se connecte à l'une des application qu'il controle. 

Pour activer cette partie, visitez `https://t.me/RNCP37827_bot` après avoir démarrer le module "Auth" pour obtenir vote chat_id et remplissez la variable complémentaire dans le fichier `.env`

*_Un redémarrage sera alors nécessaire: `ctrl+c` + `python run_auth.py`_

#### 4.2.3 Lancez le Backend

Ouvrez un second terminal et lancez cette commande :

```bash
python run_backend.py
```

Cette section fonctionnera en local si Java, Spark et PostgreSQL sont installés et que les variables d'environnement sont correctement configurées. Il est particulièrement important d'avoir créé l'utilisateur spécifié dans ces variables (avec les privilèges de superutilisateur) directement dans votre instance PostgreSQL locale.

A défaut, préférez le lancement via Docker.

#### 4.2.4 Lancer le Frontend

Ouvrez un troisième terminal et suivez ces étapes :

```bash
cd frontend
```

**Installez les dépendances :**

```bash
npm install
```

**Lancez le frontend :**

```bash
cd ..
python run_frontend.py
```

### 5. Acceder à l'application

Ouvrez votre navigateur et accédez à l'application via l'URL suivante :  
`http://localhost:<port_front choisi dans .env>`

### 6. Tester l'application

Connectez-vous avec les identifiants suivants* :

- **Utilisateur avec rôle *reader*** :
  - **Nom d'utilisateur** : `john@reader.com`
  - **Mot de passe** : `secret139`

- **Utilisateur avec rôle *editor* sur cette application** :
  - **Nom d'utilisateur** : `bob@editor.com`
  - **Mot de passe** : `secret258`

- **Utilisateur avec rôle *editor* sur une autre application** :
  - **Nom d'utilisateur** : `eve@editor.com`
  - **Mot de passe** : `secret375`

- **Utilisateur avec rôle *admin* sur cette application** :
  - **Nom d'utilisateur** : `alice@admin.com`
  - **Mot de passe** : `secret647`

*_Ou avec les identifiants créés à l'étape 3_

## Remarques

- Assurez-vous que les ports spécifiés dans le fichier `.env` ne sont pas déjà utilisés par d'autres applications.
- Les données factices sont utilisées pour simuler les fonctionnalités de l'application sans divulguer d'informations confidentielles.
- En cas de problème, vérifiez que toutes les dépendances sont correctement installées et que les versions de Python et Node.js sont compatibles.
- Après vos tests, il vous est possible de dropper la base de données crée avec la commande `python -m scripts.postgre_drop_db`.
- Lors de la première execution du backend, la base de donnée s'initialise et un dataset factice est généré. Cette opération prend quelques minutes et il faut donc attendre la fin du processus pour que le service backend devienne opérationnel. Les logs indiqueront clairement la progression de cette étape nécessaire.

## Contact

Pour toute question ou assistance concernant l'installation ou l'utilisation de cette version de présentation, n'hésitez pas à me contacter.
