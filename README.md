# PhpBase

## Présentation

Ce dépôt constitue un **environnement de développement PHP prêt à l'emploi**.

Il utilise **GitHub Codespaces** et Docker afin de disposer d'un environnement Web complet dans le cloud.

Aucune installation locale de PHP, Apache ou MySQL n'est nécessaire.

> **Aucune application PHP n'est installée dans ce dépôt modèle.**

Chaque dépôt créé à partir de `PhpBase` permet de développer sa propre application PHP.

---

# Architecture de l'environnement

Le Codespace utilise trois conteneurs Docker :

```text
GitHub Codespace
│
├── app
│   ├── Apache
│   ├── PHP 8.4
│   ├── PDO MySQL
│   ├── Sodium
│   ├── Git
│   ├── zip
│   └── unzip
│
├── mysql
│   └── MySQL 8.4
│
└── phpmyadmin
    └── Interface Web de gestion MySQL
```

---

# Conteneur `app`

Le conteneur `app` constitue l'environnement principal de développement.

VS Code est connecté directement à ce conteneur.

Il contient :

- Apache ;
- PHP 8.4 ;
- PDO MySQL ;
- Sodium ;
- Git ;
- zip ;
- unzip ;
- un terminal Linux.

Le dossier de travail est :

```text
/workspace/projetPhp
```

Ce dossier est destiné à accueillir les fichiers de l'application PHP.

---

# Serveur Web Apache

Apache est installé et démarré automatiquement dans le conteneur `app`.

Le dossier Web configuré dans Apache est :

```text
/workspace/projetPhp
```

L'application Web est accessible depuis le port :

```text
8000
```

Le fonctionnement est le suivant :

```text
Navigateur
    │
    ▼
Port 8000 du Codespace
    │
    ▼
Port 80 du conteneur app
    │
    ▼
Apache
    │
    ▼
/workspace/projetPhp
```

Il n'est pas nécessaire de lancer manuellement un serveur Web.

Apache assure automatiquement le service Web.

---

# PHP

PHP 8.4 est installé dans le conteneur `app`.

La version de PHP peut être vérifiée avec :

```bash
php -v
```

L'environnement permet notamment d'utiliser les fonctions PHP natives :

```php
filter_input()
password_hash()
password_verify()
```

Ces fonctions ne nécessitent aucune bibliothèque supplémentaire.

---

# PDO MySQL

Le pilote PHP :

```text
pdo_mysql
```

est installé automatiquement lors de la construction du conteneur.

Il permet à PHP de communiquer avec le serveur MySQL.

Sa présence peut être vérifiée avec :

```bash
php -m | grep pdo_mysql
```

Résultat attendu :

```text
pdo_mysql
```

Exemple de connexion PDO :

```php
<?php

$connexion = new PDO(
    "mysql:host=mysql;dbname=nomBase",
    "root",
    "root"
);

$connexion->setAttribute(
    PDO::ATTR_ERRMODE,
    PDO::ERRMODE_EXCEPTION
);

?>
```

Le nom :

```text
mysql
```

correspond au service MySQL défini dans `docker-compose.yml`.

---

# Sodium

La bibliothèque Sodium est disponible dans PHP.

Elle permet notamment de réaliser des opérations de chiffrement et de déchiffrement.

Sa présence peut être vérifiée avec :

```bash
php -m | grep sodium
```

Résultat attendu :

```text
sodium
```

L'environnement peut notamment utiliser les fonctions PHP Sodium.

Exemple :

```php
sodium_crypto_secretbox()
sodium_crypto_secretbox_open()
```

---

# Gestion des fichiers

L'environnement permet l'utilisation des fonctions PHP de gestion des fichiers.

Exemples :

```php
fopen()
fgets()
fwrite()
fputs()
fclose()
```

Apache exécute PHP avec l'utilisateur système :

```text
www-data
```

Afin de permettre aux scripts PHP de créer et modifier des fichiers dans le dossier de développement, des droits spécifiques sont automatiquement configurés sur :

```text
/workspace/projetPhp
```

Le système utilise les ACL Linux.

L'utilisateur :

```text
www-data
```

dispose des droits nécessaires pour lire, écrire et créer des fichiers.

Les nouveaux fichiers et dossiers peuvent également hériter de ces droits.

La configuration peut être vérifiée avec :

```bash
getfacl /workspace/projetPhp
```

---

# MySQL

Le conteneur `mysql` héberge le serveur de bases de données **MySQL 8.4**.

Le nom du serveur sur le réseau Docker est :

```text
mysql
```

Le port interne utilisé par MySQL est :

```text
3306
```

Les paramètres de connexion sont :

```text
Serveur      : mysql
Utilisateur  : root
Mot de passe : root
```

Aucune base de données applicative n'est créée automatiquement.

Les bases nécessaires aux projets doivent être créées manuellement.

---

# Persistance des bases de données

Les données MySQL sont stockées dans le volume Docker :

```text
mysql-data
```

Ce volume permet de conserver les bases de données lors du redémarrage des conteneurs.

```text
Conteneur MySQL
       │
       ▼
Volume mysql-data
       │
       ▼
Données persistantes
```

---

# phpMyAdmin

Le conteneur `phpmyadmin` fournit une interface Web d'administration du serveur MySQL.

phpMyAdmin est accessible depuis le port :

```text
8080
```

Paramètres de connexion :

```text
Serveur      : mysql
Utilisateur  : root
Mot de passe : root
```

Le port `8080` donne accès à phpMyAdmin.

```text
Navigateur
    │
    ▼
Port 8080
    │
    ▼
phpMyAdmin
    │
    ▼
MySQL
```

---

# Git

Git est installé dans le conteneur `app`.

Il permet de versionner le projet et d'enregistrer le travail dans le dépôt GitHub.

Commandes principales :

```bash
git status
```

```bash
git add .
```

```bash
git commit -m "Description des modifications"
```

```bash
git push
```

---

# Compression et décompression

Les outils :

```text
zip
unzip
```

sont installés dans le conteneur `app`.

Ils permettent de créer et d'extraire des archives ZIP.

Exemple de création d'une archive :

```bash
zip -r projetPhp.zip projetPhp
```

Exemple de décompression :

```bash
unzip projetPhp.zip
```

---

# Fichiers de configuration

L'environnement repose sur trois fichiers principaux :

```text
PhpBase
│
├── .devcontainer
│   ├── devcontainer.json
│   └── Dockerfile
│
├── docker-compose.yml
│
└── README.md
```

---

## `devcontainer.json`

Le fichier :

```text
.devcontainer/devcontainer.json
```

configure GitHub Codespaces et VS Code.

Il définit notamment :

- le fichier `docker-compose.yml` utilisé ;
- le conteneur `app` comme environnement de développement ;
- le dossier `/workspace/projetPhp` comme dossier de travail ;
- les ports `8000` et `8080` ;
- les extensions VS Code installées automatiquement.

Les extensions suivantes sont installées :

- Intelephense ;
- PHP Debug.

Après la création du Codespace, l'environnement vérifie automatiquement :

- la version de PHP ;
- la présence de `pdo_mysql` ;
- la présence de Sodium.

Les droits d'écriture de l'utilisateur Apache `www-data` sont également configurés sur le dossier de développement.

---

## `docker-compose.yml`

Le fichier :

```text
docker-compose.yml
```

définit et orchestre les trois conteneurs :

- `app` ;
- `mysql` ;
- `phpmyadmin`.

Il définit également :

- les ports ;
- les volumes ;
- les paramètres MySQL ;
- les relations entre les conteneurs.

Le port Apache est publié de la manière suivante :

```text
8000:80
```

Le port `8000` permet ainsi d'accéder au port `80` d'Apache dans le conteneur `app`.

Le port phpMyAdmin est publié de la manière suivante :

```text
8080:80
```

---

## `Dockerfile`

Le fichier :

```text
.devcontainer/Dockerfile
```

construit et personnalise le conteneur `app`.

Il utilise comme image de base :

```text
php:8.4-apache
```

Il ajoute :

- Git ;
- zip ;
- unzip ;
- ACL ;
- PDO MySQL.

Apache est configuré pour utiliser :

```text
/workspace/projetPhp
```

comme dossier Web.

Le module Apache :

```text
rewrite
```

est également activé.

---

# Rôle des fichiers de configuration

```text
devcontainer.json
        │
        │ Configure Codespaces et VS Code
        ▼
docker-compose.yml
        │
        │ Définit et relie les conteneurs
        ▼
Dockerfile
        │
        ├── Construit le conteneur app
        ├── Installe Apache et PHP
        ├── Installe PDO MySQL
        ├── Installe Git
        ├── Installe zip / unzip / ACL
        └── Configure Apache
```

---

# État actuel du dépôt modèle

Les composants suivants sont installés et configurés :

- GitHub Codespaces ;
- Docker Compose ;
- Apache ;
- PHP 8.4 ;
- PDO MySQL ;
- Sodium ;
- MySQL 8.4 ;
- phpMyAdmin ;
- Git ;
- zip ;
- unzip ;
- ACL ;
- extensions VS Code pour le développement PHP ;
- droits d'écriture Apache sur le dossier de développement.

Les fonctionnalités PHP suivantes ont été testées :

```text
PDO MySQL
Sodium
filter_input()
password_hash()
password_verify()
fopen()
fgets()
fwrite()
fputs()
```

> **Aucune application PHP n'est installée dans le dépôt modèle.**

Le dossier :

```text
/workspace/projetPhp
```

est destiné à accueillir le futur projet PHP.

---

# Créer un nouveau projet

`PhpBase` est configuré comme dépôt modèle GitHub.

Pour créer un nouveau projet :

1. Ouvrir le dépôt `PhpBase`.
2. Cliquer sur `Use this template`.
3. Choisir `Create a new repository`.
4. Sélectionner son compte GitHub comme propriétaire.
5. Donner un nom au nouveau dépôt.
6. Créer le dépôt.
7. Créer un Codespace depuis ce nouveau dépôt.

GitHub Codespaces construit automatiquement l'environnement.

Les trois conteneurs sont démarrés :

```text
app
mysql
phpmyadmin
```

Apache démarre automatiquement.

---

# Créer une première application PHP

Dans le dossier :

```text
/workspace/projetPhp
```

créer un fichier :

```text
index.php
```

Exemple :

```php
<?php

echo "<h1>Mon application PHP</h1>";

?>
```

Ouvrir ensuite le port :

```text
8000
```

L'application PHP est directement accessible dans le navigateur.

---

# Résumé

```text
PhpBase
    │
    │ Use this template
    ▼
Dépôt personnel
    │
    ▼
Create Codespace
    │
    ▼
Apache + PHP + MySQL
+ phpMyAdmin
+ PDO MySQL
+ Sodium
    │
    ▼
Développement PHP
    │
    ▼
Commit + Push
```
