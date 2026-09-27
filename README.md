# MyTraining — Mini LMS (TP5 DAW)

Plateforme de gestion de formation en ligne, développée en **PHP + MySQL**, prête à être déployée sur **XAMPP** (ou WAMP / MAMP) et adaptable à **Vercel** avec une base MySQL accessible publiquement.

## ✨ Fonctionnalités

- ✅ **Inscription** des étudiants (formulaire HTML/CSS responsive)
- ✅ **Validation côté client** en JavaScript (`Verif()`) :
  - Champs obligatoires
  - Nom/Prénom : lettres uniquement
  - CIN : exactement 8 chiffres
  - Email valide
  - Maximum 2 modules
- ✅ **Validation côté serveur** en PHP (sécurité)
- ✅ **Insertion** dans MySQL via PDO + requêtes préparées (anti-injection SQL)
- ✅ **Liste** dynamique des inscrits (jointures SQL `users` ⨝ `inscriptions` ⨝ `modules`)
- ✅ **Recherche** instantanée (JavaScript)

### 🎁 Bonus implémentés
- 🔐 **Authentification admin** (login + register, mots de passe hachés avec `password_hash`)
- ✖ **Suppression** d'une inscription (admin uniquement)
- ✎ **Modification** des données (admin uniquement)
- 📊 **Statistiques** : nb d'utilisateurs par module + répartition par niveau (graphiques en barres)

## 📂 Structure du projet

```
mytraining/
├── index.php              # Formulaire d'inscription
├── traitement.php         # Traitement POST + insertion BD
├── liste.php              # Liste des inscrits (avec recherche)
├── statistiques.php       # Bonus : statistiques + graphiques
├── login.php              # Bonus : connexion admin
├── register.php           # Bonus : création de compte admin
├── logout.php             # Bonus : déconnexion
├── modifier.php           # Bonus : modification d'une inscription
├── supprimer.php          # Bonus : suppression d'une inscription
├── includes/
│   ├── db.php             # Connexion PDO à formation_db
│   ├── header.php         # En-tête HTML + navigation
│   └── footer.php         # Pied de page
├── assets/
│   ├── css/style.css      # Design responsive
│   └── js/script.js       # Validation Verif() + interactions
├── sql/
│   └── formation_db.sql   # Schéma + modules de départ
└── README.md
```

## 🛠️ Installation sur XAMPP

### 1. Copier le dossier
Copiez le dossier `mytraining/` dans le répertoire **htdocs** de XAMPP :

- **Windows** : `C:\xampp\htdocs\mytraining\`
- **macOS**   : `/Applications/XAMPP/htdocs/mytraining/`
- **Linux**   : `/opt/lampp/htdocs/mytraining/`

### 2. Démarrer XAMPP
Lancez **Apache** + **MySQL** depuis le panneau de contrôle XAMPP.

### 3. Importer la base de données
1. Ouvrez phpMyAdmin : <http://localhost/phpmyadmin>
2. Cliquez sur **Importer**
3. Sélectionnez le fichier `mytraining/sql/formation_db.sql`
4. Cliquez sur **Exécuter**

> La base `formation_db` est créée avec 8 modules de formation et un compte admin par défaut.

### 4. Vérifier la connexion
Le fichier `includes/db.php` est pré-configuré pour XAMPP :
```php
$DB_HOST = 'localhost';
$DB_NAME = 'formation_db';
$DB_USER = 'root';
$DB_PASS = '';   // mot de passe vide par défaut sur XAMPP
```
Modifiez ces valeurs si votre installation utilise d'autres identifiants.

### 5. Ouvrir l'application
Rendez-vous sur :

👉 <http://localhost/mytraining/>

## 🚀 Déploiement sur Vercel

Vercel ne peut pas se connecter au MySQL installé dans votre XAMPP local. Pour
déployer cette application, il faut utiliser une base MySQL hébergée et
accessible depuis Internet, puis configurer ces variables dans **Vercel →
Project Settings → Environment Variables** :

| Variable | Valeur |
|----------|--------|
| `DB_HOST` | Hôte de la base MySQL hébergée |
| `DB_PORT` | Port MySQL, généralement `3306` |
| `DB_NAME` | Nom de la base |
| `DB_USER` | Utilisateur MySQL |
| `DB_PASS` | Mot de passe MySQL |

Importez ensuite `sql/formation_db.sql` dans cette base. Le fichier
`vercel.json` configure le runtime PHP communautaire et route les pages PHP
vers leurs fonctions Vercel. Les fichiers CSS et JavaScript restent servis
comme fichiers statiques.

Dans Vercel, vérifiez aussi que **Root Directory** pointe vers le dossier qui
contient `vercel.json`, `api/`, `assets/` et les fichiers PHP. Après chaque
modification, redéployez le bon commit et la bonne branche GitHub.

## 🔑 Compte de démo

Pour accéder aux fonctionnalités d'administration (modifier / supprimer) :

| Identifiant | Mot de passe |
|-------------|--------------|
| `admin`     | `admin123`   |

Vous pouvez aussi créer votre propre compte via la page **Register**.

## 🗄️ Schéma de la base

- **users** (`id`, `nom`, `prenom`, `cin`, `email`, `niveau`, `cree_le`)
- **modules** (`id`, `nom_module`, `description`, `icone`)
- **inscriptions** (`id`, `user_id` → users, `module_id` → modules, `date_inscription`)
- **comptes** (bonus auth) (`id`, `username`, `password`, `role`)

Les contraintes `ON DELETE CASCADE` suppriment automatiquement les inscriptions d'un utilisateur quand celui-ci est supprimé.

## 🔒 Sécurité

- Toutes les requêtes SQL utilisent des **requêtes préparées (PDO)** → protection contre l'injection SQL
- Échappement systématique des sorties HTML avec `htmlspecialchars()` → protection XSS
- Mots de passe admin **hachés** avec `password_hash` (bcrypt) et vérifiés avec `password_verify`
- Validation **côté client ET côté serveur**

## 📝 Plan sur 3 semaines (couvert)

| Semaine | Contenu                                            | Statut |
|---------|----------------------------------------------------|--------|
| 1       | HTML + CSS + Formulaire + JS validation `Verif()` | ✅ |
| 2       | Création BD MySQL + Connexion PHP + Insertion     | ✅ |
| 3       | Liste dynamique + Jointures SQL + Bonus + UI      | ✅ |

Bon développement ! 🚀
