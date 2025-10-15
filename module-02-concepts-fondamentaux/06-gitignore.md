🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 2 : Concepts fondamentaux

## 6. .gitignore : ignorer des fichiers

### Introduction : Tous les fichiers ne doivent pas être versionnés

Dans votre projet, tous les fichiers ne méritent pas d'être suivis par Git. Certains fichiers sont :
- **Générés automatiquement** (fichiers compilés, builds)
- **Temporaires** (caches, logs)
- **Personnels** (configurations locales, IDE)
- **Sensibles** (mots de passe, clés API)
- **Trop volumineux** (bases de données, médias)

Le fichier `.gitignore` permet de dire à Git : "Ignore ces fichiers, ne les suit pas, ne les propose pas pour un commit".

### Pourquoi utiliser .gitignore ?

#### 1. Garder un dépôt propre

Sans `.gitignore`, votre `git status` serait pollué par des dizaines de fichiers non pertinents :

```bash
git status
```

**Sans .gitignore** :
```
Untracked files:
  .DS_Store
  node_modules/
  .env
  *.log
  .vscode/
  dist/
  __pycache__/
  .pytest_cache/
  ... (des centaines de fichiers)
```

**Avec .gitignore** :
```
On branch main
nothing to commit, working tree clean
```

#### 2. Éviter les erreurs

Prévenir le commit accidentel de :
- Mots de passe et clés secrètes
- Fichiers de configuration personnelle
- Données sensibles

#### 3. Réduire la taille du dépôt

Les fichiers ignorés ne sont pas versionnés, ce qui :
- Réduit la taille du dépôt
- Accélère les opérations Git
- Facilite le clonage

#### 4. Éviter les conflits inutiles

Les fichiers générés automatiquement (builds, caches) peuvent causer des conflits entre développeurs alors qu'ils n'ont aucune valeur dans Git.

---

## Créer un fichier .gitignore

### Étape 1 : Créer le fichier

À la racine de votre projet :

```bash
# Créer le fichier
touch .gitignore

# Ou sur Windows
type nul > .gitignore
```

Vous pouvez aussi le créer avec votre éditeur de texte.

**Important** :
- Le nom est exactement `.gitignore` (commence par un point)
- Il doit être à la **racine** du projet (là où se trouve `.git`)
- Il peut y en avoir dans les sous-dossiers pour des règles spécifiques

### Étape 2 : Ajouter des patterns

Ouvrez `.gitignore` dans votre éditeur et ajoutez les fichiers à ignorer :

```gitignore
# Fichier de configuration locale
config.local.js

# Dossier des dépendances
node_modules/

# Fichiers de log
*.log

# Fichiers système macOS
.DS_Store
```

### Étape 3 : Sauvegarder et commiter

Le fichier `.gitignore` lui-même **doit être versionné** :

```bash
git add .gitignore
git commit -m "Ajout du fichier .gitignore"
```

Ainsi, tous les membres de l'équipe bénéficieront des mêmes règles d'ignorance.

---

## Syntaxe et patterns

### Règles de base

#### Ignorer un fichier spécifique

```gitignore
# Ignorer un fichier précis
secret.txt
config.local.json
```

#### Ignorer tous les fichiers d'un certain type

```gitignore
# Tous les fichiers .log
*.log

# Tous les fichiers .tmp
*.tmp
```

L'astérisque `*` est un **joker** qui remplace n'importe quelle séquence de caractères.

#### Ignorer un dossier complet

```gitignore
# Ignorer le dossier node_modules
node_modules/

# Ignorer le dossier dist
dist/

# Important : Le slash final indique que c'est un dossier
```

#### Commentaires

```gitignore
# Ceci est un commentaire
# Les lignes commençant par # sont ignorées

*.log  # Commentaire en fin de ligne
```

### Patterns avancés

#### Négation (ne PAS ignorer)

```gitignore
# Ignorer tous les .log
*.log

# SAUF important.log
!important.log
```

Le point d'exclamation `!` inverse la règle.

#### Jokers

##### * (astérisque)

Remplace n'importe quelle séquence de caractères **sauf le slash** :

```gitignore
# Tous les .txt
*.txt

# Tous les fichiers commençant par "test"
test*

# Tous les fichiers finissant par ".old"
*.old
```

##### ** (double astérisque)

Correspond à n'importe quel nombre de répertoires :

```gitignore
# Tous les .log dans tous les sous-dossiers
**/*.log

# Équivalent à
*.log
```

**Cas d'usage** : Ignorer un type de fichier profondément imbriqué.

##### ? (point d'interrogation)

Remplace **un seul** caractère :

```gitignore
# Fichiers comme file1.txt, file2.txt, fileA.txt
file?.txt

# Mais pas file10.txt (deux caractères)
```

##### [] (crochets)

Ensemble de caractères :

```gitignore
# Fichiers file1.txt, file2.txt, file3.txt
file[123].txt

# Fichiers fileA.txt, fileB.txt, fileC.txt
file[A-C].txt

# Fichiers avec chiffres
file[0-9].txt
```

#### Patterns de chemins

##### Chemins relatifs

```gitignore
# Ignore config.txt seulement à la racine
/config.txt

# Ignore config.txt partout
config.txt
```

Le slash initial `/` signifie "à la racine du dépôt".

##### Ignorer dans un dossier spécifique

```gitignore
# Ignorer tous les .log dans le dossier logs/
logs/*.log

# Ignorer tous les .log dans logs/ et ses sous-dossiers
logs/**/*.log
```

---

## Exemples pratiques par langage/framework

### JavaScript / Node.js

```gitignore
# Dépendances
node_modules/
npm-debug.log
yarn-error.log
package-lock.json  # Optionnel, selon la politique de l'équipe

# Build
dist/
build/
out/

# Cache
.npm/
.eslintcache
.cache/

# Variables d'environnement
.env
.env.local

# Fichiers de configuration IDE
.vscode/
.idea/

# Fichiers système
.DS_Store
Thumbs.db

# Logs
*.log
logs/

# Tests
coverage/
.nyc_output/
```

### Python

```gitignore
# Bytecode
__pycache__/
*.py[cod]
*$py.class

# Distribution / packaging
build/
dist/
*.egg-info/
.eggs/

# Virtual environments
venv/
env/
ENV/
.venv/

# Tests
.pytest_cache/
.coverage
htmlcov/
.tox/

# IDE
.vscode/
.idea/
*.swp
*.swo

# Jupyter Notebook
.ipynb_checkpoints/

# Variables d'environnement
.env
.env.local

# Fichiers système
.DS_Store
```

### Java

```gitignore
# Compiled class files
*.class

# Package Files
*.jar
*.war
*.ear
*.zip
*.tar.gz
*.rar

# Maven
target/
pom.xml.tag
pom.xml.releaseBackup

# Gradle
.gradle/
build/

# IDE
.idea/
*.iml
.eclipse/
.classpath
.project
.settings/

# Logs
*.log

# Fichiers système
.DS_Store
```

### PHP

```gitignore
# Dépendances Composer
vendor/
composer.lock  # Optionnel

# Variables d'environnement
.env
.env.local

# Cache
cache/
storage/logs/

# IDE
.vscode/
.idea/

# Fichiers système
.DS_Store
Thumbs.db
```

### C / C++

```gitignore
# Fichiers compilés
*.o
*.obj
*.exe
*.out
*.so
*.dll
*.a
*.lib

# Build
build/
cmake-build-*/

# Débogage
*.dSYM/

# IDE
.vscode/
.idea/

# Fichiers système
.DS_Store
```

### Projet web général

```gitignore
# Dépendances
node_modules/
vendor/

# Build / Compilation
dist/
build/
public/build/

# Cache
.cache/
.tmp/

# Variables d'environnement
.env
.env.local
.env.*.local

# Logs
*.log
logs/

# IDE
.vscode/
.idea/
*.sublime-*

# Fichiers système
.DS_Store
Thumbs.db
desktop.ini

# Fichiers temporaires
*.swp
*.swo
*~
```

---

## Cas d'usage spécifiques

### Ignorer tous les fichiers sauf certains

```gitignore
# Ignorer tout
*

# Mais garder ces fichiers
!.gitignore
!README.md
!src/
!src/**
```

**Attention** : Cette approche est délicate. Il faut ré-inclure explicitement ce qu'on veut garder.

### Ignorer un fichier mais suivre son template

```gitignore
# Ignorer le fichier de configuration réel
config.json

# Mais suivre le template
!config.json.example
```

**Usage** : Les développeurs copient `config.json.example` vers `config.json` et le personnalisent.

### Ignorer par environnement

```gitignore
# Variables d'environnement
.env
.env.local
.env.development
.env.test
.env.production

# Mais suivre l'exemple
!.env.example
```

### Ignorer les fichiers volumineux

```gitignore
# Fichiers médias
*.mp4
*.mov
*.avi
*.mkv

# Bases de données
*.db
*.sqlite
*.sql

# Archives
*.zip
*.tar.gz
*.rar
```

**Note** : Pour les gros fichiers nécessaires, envisagez **Git LFS** (Large File Storage).

---

## Fichiers système courants à ignorer

### macOS

```gitignore
.DS_Store
.AppleDouble
.LSOverride
._*
.Spotlight-V100
.Trashes
```

### Windows

```gitignore
Thumbs.db
ehthumbs.db
Desktop.ini
$RECYCLE.BIN/
*.lnk
```

### Linux

```gitignore
*~
.directory
.Trash-*
```

---

## Règles d'IDE à ignorer

### VS Code

```gitignore
.vscode/
*.code-workspace
```

**Option alternative** : Ne pas ignorer `.vscode/` si vous voulez partager la configuration de l'équipe.

### IntelliJ IDEA / PyCharm / WebStorm

```gitignore
.idea/
*.iml
*.iws
*.ipr
```

### Eclipse

```gitignore
.classpath
.project
.settings/
```

### Vim

```gitignore
*.swp
*.swo
*~
```

### Sublime Text

```gitignore
*.sublime-project
*.sublime-workspace
```

---

## Erreurs courantes et solutions

### Erreur 1 : Fichier déjà suivi

**Problème** : Vous ajoutez une règle dans `.gitignore` mais le fichier apparaît toujours dans `git status`.

**Cause** : Si un fichier est **déjà suivi** par Git, `.gitignore` ne l'affecte pas.

**Solution** : Désindexer le fichier :

```bash
# Arrêter de suivre le fichier (sans le supprimer localement)
git rm --cached fichier_a_ignorer.txt

# Ou pour un dossier
git rm -r --cached dossier/

# Commiter le changement
git commit -m "Arrêt du suivi de fichier_a_ignorer.txt"
```

Maintenant, `.gitignore` s'appliquera.

### Erreur 2 : Oublier le slash pour les dossiers

**Mauvais** :
```gitignore
node_modules
```

Cela ignore aussi un éventuel **fichier** nommé `node_modules`.

**Bon** :
```gitignore
node_modules/
```

Le slash final précise qu'il s'agit d'un dossier.

### Erreur 3 : Patterns trop larges

**Problème** :
```gitignore
*.json
```

Cela ignore **tous** les fichiers JSON, y compris `package.json` qui devrait être versionné !

**Solution** : Être plus précis :
```gitignore
# Ignorer les configs locales
config.local.json
*.local.json

# Mais pas package.json
```

### Erreur 4 : Commiter des secrets par erreur

**Prévention** : Toujours créer `.gitignore` **avant** le premier commit.

**Si c'est trop tard** : Voir le Module 8 sur comment nettoyer l'historique (attention, opération délicate).

### Erreur 5 : .gitignore dans le mauvais dossier

Le `.gitignore` principal doit être à la **racine du projet** (là où se trouve `.git/`).

---

## Techniques avancées

### .gitignore globaux

Vous pouvez avoir un `.gitignore` global pour tous vos projets :

```bash
# Créer un fichier global
touch ~/.gitignore_global

# Le configurer dans Git
git config --global core.excludesfile ~/.gitignore_global
```

**Contenu typique** :
```gitignore
# Fichiers système
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/

# Fichiers temporaires
*~
*.swp
```

**Avantage** : Règles personnelles qui s'appliquent partout sans polluer les `.gitignore` des projets.

### .gitignore dans les sous-dossiers

Vous pouvez créer des `.gitignore` dans les sous-dossiers pour des règles spécifiques :

```
projet/
├── .gitignore          (règles globales)
├── frontend/
│   └── .gitignore      (règles spécifiques au frontend)
└── backend/
    └── .gitignore      (règles spécifiques au backend)
```

Les règles se combinent (les sous-dossiers héritent et ajoutent).

### .git/info/exclude

Pour des règles **locales** qui ne doivent pas être partagées :

```bash
# Éditer
nano .git/info/exclude
```

**Contenu** :
```gitignore
# Ma base de données locale de test
test_local.db

# Mes notes personnelles
NOTES.md
```

**Différence avec .gitignore** :
- `.gitignore` est versionné et partagé
- `.git/info/exclude` est local et non partagé

---

## Vérifier si un fichier est ignoré

### Tester un fichier

```bash
# Vérifier si un fichier est ignoré
git check-ignore -v fichier.txt
```

Résultat (si ignoré) :
```
.gitignore:5:*.txt	fichier.txt
```

Cela montre :
- Quel fichier `.gitignore` contient la règle
- À quelle ligne
- Le pattern qui correspond

### Lister tous les fichiers ignorés

```bash
# Voir tous les fichiers ignorés dans le répertoire
git status --ignored
```

### Déboguer les règles

```bash
# Pourquoi ce fichier est-il ignoré ?
git check-ignore -v node_modules/package.json

# Afficher tous les fichiers ignorés correspondant à un pattern
git check-ignore -v *
```

---

## Templates .gitignore

### Utiliser GitHub

GitHub fournit des templates pour différents langages :

[https://github.com/github/gitignore](https://github.com/github/gitignore)

Vous y trouverez des `.gitignore` prêts à l'emploi pour :
- Python
- Node.js
- Java
- C++
- Ruby
- Go
- Et des dizaines d'autres

### Générer un .gitignore

Site web pratique : [gitignore.io](https://www.toptal.com/developers/gitignore)

1. Entrez votre stack (par exemple : "Node, VisualStudioCode, macOS")
2. Le site génère un `.gitignore` complet
3. Copiez-le dans votre projet

### Dans VS Code

Extension **gitignore** : Ajoute automatiquement des templates dans l'éditeur.

---

## Bonnes pratiques

### 1. Créer .gitignore dès le début

**Avant** le premier commit, créez votre `.gitignore`. Cela évite des problèmes plus tard.

```bash
# Ordre recommandé
git init
touch .gitignore
# Configurer .gitignore
git add .gitignore
git commit -m "Initial commit avec .gitignore"
```

### 2. Utiliser des templates

Ne réinventez pas la roue. Utilisez des templates existants et adaptez-les.

### 3. Commenter vos règles

```gitignore
# === Dépendances ===
node_modules/
vendor/

# === Build ===
dist/
build/

# === Configuration locale ===
.env
*.local.json
```

Les commentaires aident à comprendre pourquoi certains fichiers sont ignorés.

### 4. Être spécifique plutôt que générique

**Mauvais** :
```gitignore
*.json
```

**Bon** :
```gitignore
# Fichiers de configuration locale
config.local.json
settings.json
```

### 5. Documenter les exceptions

```gitignore
# Ignorer tous les .env
.env*

# Sauf le template
!.env.example
```

### 6. Réviser régulièrement

Au fil du projet, vérifiez que votre `.gitignore` est toujours adapté :
- Nouveaux outils utilisés ?
- Nouveaux fichiers générés ?
- Règles obsolètes ?

### 7. Ne jamais ignorer le code source

Ne mettez **jamais** dans `.gitignore` :
- Votre code source
- Les fichiers de configuration essentiels (`package.json`, `requirements.txt`, etc.)
- La documentation

### 8. Protéger les secrets

**Toujours** ignorer :
- `.env` (variables d'environnement)
- Fichiers contenant des clés API
- Certificats et clés privées
- Fichiers de configuration avec mots de passe

**Alternative** : Utiliser des gestionnaires de secrets (Vault, AWS Secrets Manager, etc.)

---

## Que faire si vous avez commité un secret ?

### Prévention

Le mieux est de ne jamais commiter de secrets. Mais si c'est arrivé :

### Solution 1 : Si pas encore pushé

```bash
# Désindexer le fichier
git rm --cached .env

# Ajouter à .gitignore
echo ".env" >> .gitignore

# Amender le dernier commit
git add .gitignore
git commit --amend
```

### Solution 2 : Si déjà pushé

C'est plus complexe et nécessite de réécrire l'historique. Voir le Module 8.

**Actions immédiates** :
1. **Révoquer immédiatement** les secrets (changer les mots de passe, régénérer les clés)
2. Contacter votre équipe
3. Nettoyer l'historique (opération avancée)

---

## Exemples de .gitignore complets

### Projet React

```gitignore
# Dépendances
node_modules/
.pnp/
.pnp.js

# Testing
coverage/

# Production
build/
dist/

# Misc
.DS_Store
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*

# IDE
.vscode/
.idea/

# Cache
.eslintcache
```

### Projet Django (Python)

```gitignore
# Python
*.py[cod]
__pycache__/
*.so

# Virtual environment
venv/
env/
ENV/

# Django
*.log
db.sqlite3
/media
/static
local_settings.py

# Tests
.coverage
htmlcov/
.pytest_cache/

# IDE
.vscode/
.idea/

# Environnement
.env
.env.local

# Fichiers système
.DS_Store
```

### Projet Laravel (PHP)

```gitignore
# Laravel
/node_modules
/public/hot
/public/storage
/storage/*.key
/vendor
.env
.env.backup
.phpunit.result.cache
Homestead.json
Homestead.yaml
npm-debug.log
yarn-error.log

# IDE
.vscode/
.idea/

# Fichiers système
.DS_Store
Thumbs.db
```

---

## Commandes utiles récapitulatives

```bash
# Créer .gitignore
touch .gitignore

# Vérifier si un fichier est ignoré
git check-ignore -v fichier.txt

# Voir tous les fichiers ignorés
git status --ignored

# Arrêter de suivre un fichier déjà suivi
git rm --cached fichier.txt

# Arrêter de suivre un dossier
git rm -r --cached dossier/

# Configurer un .gitignore global
git config --global core.excludesfile ~/.gitignore_global

# Forcer l'ajout d'un fichier ignoré (déconseillé)
git add -f fichier_ignore.txt
```

---

## En résumé

Le fichier `.gitignore` est essentiel pour :

1. **Garder un dépôt propre** : Ignorer les fichiers non pertinents
2. **Protéger les secrets** : Ne jamais versionner les mots de passe et clés
3. **Optimiser les performances** : Réduire la taille du dépôt
4. **Éviter les conflits** : Ne pas versionner les fichiers générés

**Points clés** :
- Créer `.gitignore` **dès le début** du projet
- Le **versionner** (git add, git commit)
- Utiliser des **templates** adaptés à votre technologie
- Être **spécifique** dans les patterns
- **Documenter** avec des commentaires
- **Tester** avec `git check-ignore`

**Pattern de base** :
```gitignore
# Dépendances
node_modules/

# Build
dist/

# Environnement
.env

# IDE
.vscode/

# Système
.DS_Store
```

Avec un bon `.gitignore`, votre `git status` restera toujours clair et vous éviterez de nombreux problèmes courants.

---

*Le Module 2 est maintenant terminé ! Vous maîtrisez les concepts fondamentaux de Git. Dans le Module 3, nous verrons comment corriger et modifier vos commits.*

⏭️ [Module 3 : Corriger et modifier](/module-03-corriger-et-modifier/README.md)
