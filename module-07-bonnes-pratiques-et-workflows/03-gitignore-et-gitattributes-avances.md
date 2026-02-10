🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 3. .gitignore et .gitattributes avancés

### Introduction

Tous les fichiers de votre projet n'ont pas vocation à être versionnés dans Git. Certains sont générés automatiquement, d'autres contiennent des informations sensibles, d'autres encore sont spécifiques à votre environnement local.

Les fichiers `.gitignore` et `.gitattributes` sont vos outils pour :
- **`.gitignore`** : dire à Git quels fichiers **ignorer** (ne pas suivre)
- **`.gitattributes`** : définir des **attributs spéciaux** pour certains fichiers

Maîtriser ces deux fichiers est essentiel pour maintenir un dépôt propre et professionnel.

---

## Partie 1 : Le fichier .gitignore

### Qu'est-ce que .gitignore ?

Le fichier `.gitignore` contient une liste de patterns (motifs) qui indiquent à Git quels fichiers ou dossiers ignorer.

**Pourquoi ignorer des fichiers ?**

1. **Fichiers générés automatiquement** : logs, fichiers compilés, caches
2. **Dépendances** : `node_modules/`, `vendor/`, librairies téléchargeables
3. **Fichiers système** : `.DS_Store` (macOS), `Thumbs.db` (Windows)
4. **Configuration locale** : variables d'environnement, IDE settings
5. **Données sensibles** : clés API, mots de passe (même si ça ne devrait jamais être dans le code !)
6. **Fichiers temporaires** : fichiers de build, fichiers de test

**⚠️ Important** : `.gitignore` n'affecte que les fichiers **non suivis**. Si un fichier est déjà commité, l'ajouter à `.gitignore` ne le supprimera pas de l'historique.

---

### Créer un fichier .gitignore

Le fichier `.gitignore` se place à la **racine de votre projet**.

```bash
# Créer le fichier
touch .gitignore

# L'éditer
nano .gitignore
# ou
code .gitignore
```

**Exemple simple** :

```gitignore
# Fichiers de logs
*.log

# Dossier node_modules
node_modules/

# Fichiers de configuration locale
.env

# Fichiers système macOS
.DS_Store
```

Puis commitez-le :

```bash
git add .gitignore  
git commit -m "chore: ajoute le fichier .gitignore"
```

---

### Syntaxe de base

#### 1. Lignes vides et commentaires

```gitignore
# Ceci est un commentaire

# Les lignes vides sont ignorées
```

#### 2. Ignorer un fichier spécifique

```gitignore
# Ignore le fichier secret.txt à la racine
secret.txt

# Ignore secret.txt partout dans le projet
**/secret.txt
```

#### 3. Ignorer tous les fichiers d'un type

```gitignore
# Tous les fichiers .log
*.log

# Tous les fichiers .tmp
*.tmp

# Tous les fichiers .pdf
*.pdf
```

#### 4. Ignorer un dossier

```gitignore
# Le dossier temp/ à la racine
temp/

# Tous les dossiers nommés temp
**/temp/

# Alternative (équivalent)
temp
```

**⚠️ Attention** : toujours terminer les dossiers par `/` pour être explicite.

#### 5. Ne PAS ignorer (exception)

Le préfixe `!` crée une **exception**.

```gitignore
# Ignore tous les fichiers .log
*.log

# SAUF important.log
!important.log
```

**Exemple pratique** :

```gitignore
# Ignore tout le dossier build/
build/

# Mais garde le fichier README dans build/
!build/README.md
```

---

### Syntaxe avancée

#### 1. Le wildcard `*`

`*` correspond à **n'importe quoi** sauf `/`.

```gitignore
# Tous les fichiers .txt
*.txt

# Tous les fichiers commençant par "test"
test*

# Tous les fichiers se terminant par "backup"
*backup

# Tous les fichiers contenant "temp"
*temp*
```

#### 2. Le wildcard `**`

`**` correspond à **tous les dossiers et sous-dossiers**.

```gitignore
# Tous les fichiers .log dans n'importe quel sous-dossier
**/*.log

# Équivalent plus simple (comportement par défaut)
*.log
```

**Différence importante** :

```gitignore
# Ignore temp/ seulement à la racine
/temp/

# Ignore tous les dossiers temp/ partout
**/temp/
# ou simplement
temp/
```

#### 3. Le wildcard `?`

`?` correspond à **exactement un caractère**.

```gitignore
# Fichiers comme file1.txt, fileA.txt, mais pas file12.txt
file?.txt

# Fichiers comme log1.txt, log2.txt, logA.txt
log?.txt
```

#### 4. Les crochets `[]`

Les crochets permettent de spécifier un **ensemble de caractères**.

```gitignore
# Fichiers file1.txt, file2.txt, file3.txt
file[123].txt

# Fichiers commençant par a, b ou c
[abc]*.txt

# Fichiers avec n'importe quel chiffre
file[0-9].txt

# Fichiers avec n'importe quelle lettre minuscule
[a-z]*.txt
```

#### 5. Négation avec `!`

```gitignore
# Ignore tous les .txt
*.txt

# Sauf readme.txt
!readme.txt

# MAIS attention à l'ordre !
# Si le dossier est ignoré, les exceptions ne fonctionnent pas
build/
!build/important.txt  # ❌ Ne marchera PAS

# Solution :
build/*               # Ignore le CONTENU du dossier
!build/important.txt  # ✅ Maintenant ça marche
```

#### 6. Ancrage avec `/`

Le `/` au début ou à la fin a un sens spécial.

```gitignore
# /au_debut signifie "à la racine uniquement"
/temp           # Ignore temp/ à la racine seulement

# Sans / au début, c'est partout
temp            # Ignore temp/ partout

# Avec / à la fin, c'est un dossier
temp/           # Ignore le dossier temp

# Sans / à la fin, c'est fichier OU dossier
temp            # Ignore le fichier ET le dossier temp
```

---

### Patterns courants par langage/framework

#### JavaScript / Node.js

```gitignore
# Dépendances
node_modules/  
npm-debug.log*  
yarn-debug.log*  
yarn-error.log*

# Fichiers de build
dist/  
build/
.cache/

# Variables d'environnement
.env
.env.local
.env.*.local

# Fichiers de test
coverage/
.nyc_output/

# Fichiers d'éditeur
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
```

#### Python

```gitignore
# Byte-compiled / optimized
__pycache__/
*.py[cod]
*$py.class

# Distribution / packaging
dist/  
build/
*.egg-info/
.eggs/

# Virtual environment
venv/  
env/  
ENV/
.venv

# PyCharm
.idea/

# Jupyter Notebook
.ipynb_checkpoints

# pytest
.pytest_cache/
.coverage
htmlcov/

# mypy
.mypy_cache/
```

#### Java

```gitignore
# Compiled class files
*.class

# Package files
*.jar
*.war
*.ear

# Maven
target/  
pom.xml.tag

# Gradle
.gradle/
build/

# IntelliJ IDEA
.idea/
*.iml
*.iws

# Eclipse
.classpath
.project
.settings/

# Logs
*.log
```

#### PHP

```gitignore
# Composer
vendor/  
composer.lock

# Laravel specific
.env
storage/framework/cache/*  
storage/framework/sessions/*  
storage/framework/views/*  
storage/logs/*

# Symfony specific
var/cache/  
var/logs/  
var/sessions/
```

#### Ruby / Rails

```gitignore
# Bundler
vendor/bundle/

# Rails
/log/*
/tmp/*
/db/*.sqlite3

# Environment variables
.env

# IDE
.idea/
*.swp
```

#### C / C++

```gitignore
# Compiled Object files
*.o
*.obj
*.ko

# Executables
*.exe
*.out
*.app

# Libraries
*.lib
*.a
*.so
*.dylib
*.dll

# CMake
CMakeCache.txt  
CMakeFiles/  
cmake_install.cmake
```

---

### .gitignore à plusieurs niveaux

Vous pouvez avoir **plusieurs fichiers `.gitignore`** dans votre projet.

**Structure exemple** :

```
mon-projet/
├── .gitignore              # Règles globales
├── backend/
│   └── .gitignore          # Règles spécifiques au backend
├── frontend/
│   └── .gitignore          # Règles spécifiques au frontend
└── docs/
    └── .gitignore          # Règles spécifiques aux docs
```

Les règles des `.gitignore` dans les sous-dossiers s'**ajoutent** à celles de la racine.

**Exemple** :

**`.gitignore` (racine)** :
```gitignore
# Ignorer partout
.DS_Store
*.log
```

**`backend/.gitignore`** :
```gitignore
# Ignorer seulement dans backend/
node_modules/
.env
```

---

### Ignorer un fichier déjà suivi

**Problème** : Vous avez commité un fichier par erreur et vous voulez maintenant l'ignorer.

**Solution** : Retirer le fichier de Git (mais pas du disque) :

```bash
# Retirer le fichier du suivi Git
git rm --cached fichier.txt

# Ou un dossier entier
git rm --cached -r dossier/

# Ajouter à .gitignore
echo "fichier.txt" >> .gitignore

# Commiter
git add .gitignore  
git commit -m "chore: retire fichier.txt du suivi et l'ajoute à .gitignore"
```

**⚠️ Important** : `git rm --cached` ne supprime **pas** le fichier de votre disque, seulement du suivi Git.

**Si le fichier contient des secrets** :

Retirer le fichier de l'historique est plus complexe. Il faut utiliser `git filter-branch` ou `BFG Repo-Cleaner` (voir section dépannage du module 8).

---

### Vérifier ce qui est ignoré

#### Voir si un fichier est ignoré

```bash
git check-ignore -v fichier.txt
```

Si ignoré, Git affichera la règle qui l'ignore.

**Exemple** :
```bash
$ git check-ignore -v node_modules/
.gitignore:5:node_modules/    node_modules/
```

→ Le fichier `.gitignore`, ligne 5, avec la règle `node_modules/`

#### Lister tous les fichiers ignorés

```bash
git status --ignored
```

Affiche les fichiers non suivis ET les fichiers ignorés.

---

### Templates .gitignore

Plutôt que de créer votre `.gitignore` de zéro, utilisez des templates !

#### gitignore.io

Site web : https://www.toptal.com/developers/gitignore

Générez un `.gitignore` pour votre stack :

```bash
# Exemple avec curl
curl -L https://www.toptal.com/developers/gitignore/api/node,visualstudiocode,macos > .gitignore
```

#### GitHub templates

GitHub propose des templates officiels :  
https://github.com/github/gitignore

**Exemples** :
- [Node.gitignore](https://github.com/github/gitignore/blob/main/Node.gitignore)
- [Python.gitignore](https://github.com/github/gitignore/blob/main/Python.gitignore)
- [Java.gitignore](https://github.com/github/gitignore/blob/main/Java.gitignore)

---

### Bonnes pratiques .gitignore

#### ✅ À faire

1. **Commiter `.gitignore` dans le dépôt**
   - Tout le monde profite des mêmes règles

2. **Créer `.gitignore` au début du projet**
   - Évite les erreurs d'ajout de fichiers inutiles

3. **Grouper et commenter**
   ```gitignore
   # === Dépendances ===
   node_modules/
   vendor/

   # === Build ===
   dist/
   build/

   # === Configuration locale ===
   .env
   .env.local
   ```

4. **Être spécifique**
   - Préférez `config.local.js` à `*.js` (trop large)

5. **Ignorer les fichiers IDE/OS au niveau global**
   - Voir section .gitignore global ci-dessous

#### ❌ À éviter

1. **Ne jamais ignorer des fichiers de code source**
   - Évitez d'ignorer `*.js` ou `*.py` globalement

2. **Ne pas ignorer les fichiers de configuration partagés**
   - `package.json`, `requirements.txt` doivent être suivis

3. **Éviter les règles trop larges**
   ```gitignore
   # ❌ Trop large
   *

   # ✅ Spécifique
   *.log
   ```

---

### .gitignore global (personnel)

Pour ignorer des fichiers spécifiques à **votre environnement** (IDE, OS), créez un `.gitignore` global.

```bash
# Créer le fichier
touch ~/.gitignore_global

# Configurer Git pour l'utiliser
git config --global core.excludesfile ~/.gitignore_global
```

**Exemple `~/.gitignore_global`** :

```gitignore
# === macOS ===
.DS_Store
.AppleDouble
.LSOverride

# === Windows ===
Thumbs.db  
ehthumbs.db  
Desktop.ini

# === Linux ===
*~
.directory

# === Visual Studio Code ===
.vscode/
*.code-workspace

# === IntelliJ IDEA ===
.idea/
*.iml

# === Vim ===
*.swp
*.swo
*.swn

# === Emacs ===
*~
\#*\#
.\#*
```

**Avantage** : Ces règles s'appliquent à tous vos projets sans polluer les `.gitignore` de chaque projet.

---

## Partie 2 : Le fichier .gitattributes

### Qu'est-ce que .gitattributes ?

Le fichier `.gitattributes` permet de définir des **attributs spéciaux** pour certains fichiers :
- Comment Git doit gérer les **fins de lignes** (LF vs CRLF)
- Quels fichiers sont **binaires**
- Comment effectuer les **diff** et **merge**
- Quels fichiers inclure dans les **archives** (`git archive`)
- Gestion de **Git LFS** (Large File Storage)

---

### Créer un fichier .gitattributes

```bash
# À la racine du projet
touch .gitattributes
```

Puis commitez-le :

```bash
git add .gitattributes  
git commit -m "chore: ajoute .gitattributes"
```

---

### Syntaxe de base

**Format** :
```
pattern attr1 attr2 attr3
```

**Exemple** :
```gitattributes
# Tous les fichiers .txt sont des fichiers texte
*.txt text

# Les fichiers .jpg sont binaires
*.jpg binary
```

---

### Gestion des fins de lignes (EOL)

**Problème** : Windows utilise `CRLF` (`\r\n`), Unix/Linux/macOS utilisent `LF` (`\n`).

Sans configuration, Git peut créer des conflits de merge inutiles.

#### L'attribut `text`

**`text`** : Git normalise les fins de lignes.

```gitattributes
# Tous les fichiers texte : LF dans le dépôt, auto sur le disque
* text=auto
```

**`text=auto`** : Git détecte automatiquement les fichiers texte et normalise.

#### Configurations recommandées

**Configuration moderne (recommandée)** :

```gitattributes
# Normalisation automatique des fins de lignes
* text=auto

# Fichiers qui doivent toujours être LF
*.sh text eol=lf
*.bash text eol=lf
Makefile text eol=lf

# Fichiers qui doivent toujours être CRLF (Windows)
*.bat text eol=crlf
*.cmd text eol=crlf
```

#### Types de fichiers communs

**Fichiers source** :

```gitattributes
# Langages de programmation
*.c text diff=c
*.cpp text diff=cpp
*.cs text diff=csharp
*.css text diff=css
*.html text diff=html
*.java text diff=java
*.js text diff=javascript
*.json text
*.php text diff=php
*.py text diff=python
*.rb text diff=ruby
*.sql text
*.xml text

# Scripts
*.sh text eol=lf
*.bash text eol=lf
*.ps1 text eol=crlf
```

**Fichiers de configuration** :

```gitattributes
.gitattributes text
.gitignore text
.editorconfig text
*.yml text
*.yaml text
*.toml text
*.ini text
*.cfg text
.env* text
```

**Documentation** :

```gitattributes
*.md text diff=markdown
*.txt text
*.adoc text
LICENSE text  
README text
```

---

### Gestion des fichiers binaires

#### L'attribut `binary`

Les fichiers binaires ne doivent **jamais** être modifiés par Git.

```gitattributes
# Raccourci pour -text -diff
*.png binary
*.jpg binary
*.jpeg binary
*.gif binary
*.ico binary
*.pdf binary
*.zip binary
*.tar.gz binary
*.woff binary
*.woff2 binary
*.ttf binary
*.eot binary
*.mp4 binary
*.mp3 binary
*.mov binary
*.avi binary
*.exe binary
*.dll binary
*.so binary
*.dylib binary
```

**`binary`** est équivalent à `-text -diff` (pas de normalisation EOL, pas de diff).

---

### Améliorer les diffs

#### Drivers de diff spécialisés

Git peut utiliser des algorithmes de diff spécialisés pour certains langages.

```gitattributes
*.c text diff=c
*.cpp text diff=cpp
*.cs text diff=csharp
*.css text diff=css
*.html text diff=html
*.java text diff=java
*.js text diff=javascript
*.php text diff=php
*.py text diff=python
*.rb text diff=ruby
```

**Effet** : Les diffs sont plus lisibles en affichant les noms de fonctions.

#### Diff pour des fichiers spéciaux

**Images** :

Git peut montrer les métadonnées des images dans les diffs avec `exiftool` :

```gitattributes
*.png diff=exif
*.jpg diff=exif
*.jpeg diff=exif
```

Puis configurez :
```bash
git config diff.exif.textconv exiftool
```

**Documents Office** :

Convertir des .docx en texte pour les diffs :

```gitattributes
*.docx diff=word
```

Configurez :
```bash
git config diff.word.textconv "pandoc --to=plain"
```

---

### Stratégies de merge personnalisées

#### L'attribut `merge`

Définir comment Git doit merger certains fichiers.

**Exemple : fichiers de changelog**

```gitattributes
CHANGELOG.md merge=union
```

**Stratégies disponibles** :
- `merge=union` : Garde les deux versions (utile pour changelogs)
- `merge=ours` : Garde toujours notre version
- `merge=theirs` : Garde toujours leur version (custom)

---

### Filtres personnalisés (clean/smudge)

Les filtres transforment les fichiers lors du `git add` (clean) et `git checkout` (smudge).

**Cas d'usage** : Nettoyer automatiquement des données sensibles.

```gitattributes
*.secret filter=remove-secrets
```

Configuration :
```bash
git config filter.remove-secrets.clean "sed 's/password=.*/password=REDACTED/'"  
git config filter.remove-secrets.smudge cat
```

---

### Exportation et archivage

#### L'attribut `export-ignore`

Exclure des fichiers de `git archive` (pour créer des releases).

```gitattributes
# Ne pas inclure ces fichiers dans les archives
.gitattributes export-ignore
.gitignore export-ignore
.github/ export-ignore
tests/ export-ignore
*.test.js export-ignore
.editorconfig export-ignore
.eslintrc export-ignore
```

**Utilisation** :
```bash
git archive --format=zip HEAD > release.zip
```

Les fichiers marqués `export-ignore` ne seront pas dans `release.zip`.

#### L'attribut `export-subst`

Remplacer des placeholders dans les fichiers lors de l'export.

```gitattributes
version.txt export-subst
```

Dans `version.txt` :
```
Version: $Format:%H$  
Date: $Format:%ci$
```

Lors de l'archive, `$Format:%H$` sera remplacé par le hash du commit.

---

### Git LFS (Large File Storage)

Git LFS permet de gérer de **gros fichiers** (vidéos, assets 3D, datasets).

```gitattributes
# Utiliser LFS pour ces fichiers
*.psd filter=lfs diff=lfs merge=lfs -text
*.ai filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
*.zip filter=lfs diff=lfs merge=lfs -text
```

**Installation de Git LFS** :
```bash
git lfs install  
git lfs track "*.psd"
```

---

### Template .gitattributes complet

Voici un template moderne et complet :

```gitattributes
# ===== Normalisation automatique =====
* text=auto

# ===== Code source =====
*.c text diff=c
*.cc text diff=cpp
*.cpp text diff=cpp
*.cs text diff=csharp
*.css text diff=css
*.h text diff=c
*.hpp text diff=cpp
*.html text diff=html
*.java text diff=java
*.js text diff=javascript
*.jsx text diff=javascript
*.json text
*.php text diff=php
*.py text diff=python
*.rb text diff=ruby
*.rs text diff=rust
*.sql text
*.ts text diff=typescript
*.tsx text diff=typescript
*.xml text

# ===== Scripts =====
*.bash text eol=lf
*.sh text eol=lf
*.bat text eol=crlf
*.cmd text eol=crlf
*.ps1 text eol=crlf

# ===== Configuration =====
*.cnf text
*.conf text
*.config text
*.ini text
*.toml text
*.yaml text
*.yml text
.editorconfig text
.env* text
.gitattributes text
.gitignore text
Dockerfile text  
Makefile text eol=lf

# ===== Documentation =====
*.md text diff=markdown
*.markdown text diff=markdown
*.txt text
*.adoc text
AUTHORS text  
CHANGELOG text  
CHANGES text  
CONTRIBUTING text  
COPYING text  
COPYRIGHT text  
INSTALL text  
LICENSE text  
NEWS text  
README text  
TODO text

# ===== Templates =====
*.hbs text
*.mustache text
*.twig text
*.ejs text

# ===== Graphics =====
*.ai binary
*.bmp binary
*.eps binary
*.gif binary
*.gifv binary
*.ico binary
*.jng binary
*.jp2 binary
*.jpg binary
*.jpeg binary
*.jpx binary
*.jxr binary
*.pdf binary
*.png binary
*.psb binary
*.psd binary
*.svg text
*.svgz binary
*.tif binary
*.tiff binary
*.wbmp binary
*.webp binary

# ===== Audio =====
*.kar binary
*.m4a binary
*.mid binary
*.midi binary
*.mp3 binary
*.ogg binary
*.ra binary

# ===== Video =====
*.3gpp binary
*.3gp binary
*.as binary
*.asf binary
*.asx binary
*.avi binary
*.fla binary
*.flv binary
*.m4v binary
*.mng binary
*.mov binary
*.mp4 binary
*.mpeg binary
*.mpg binary
*.ogv binary
*.swc binary
*.swf binary
*.webm binary

# ===== Archives =====
*.7z binary
*.gz binary
*.jar binary
*.rar binary
*.tar binary
*.zip binary

# ===== Fonts =====
*.ttf binary
*.eot binary
*.otf binary
*.woff binary
*.woff2 binary

# ===== Executables =====
*.exe binary
*.pyc binary
*.dll binary
*.so binary
*.dylib binary

# ===== Exclusions export =====
.gitattributes export-ignore
.gitignore export-ignore
.github/ export-ignore
.vscode/ export-ignore
tests/ export-ignore
*.test.js export-ignore
*.spec.js export-ignore
```

---

### Vérifier les attributs

```bash
# Voir les attributs d'un fichier
git check-attr -a fichier.txt

# Voir un attribut spécifique
git check-attr text fichier.txt
```

---

### Bonnes pratiques .gitattributes

#### ✅ À faire

1. **Ajouter `.gitattributes` au début du projet**
   - Évite les problèmes de fins de lignes

2. **Utiliser `* text=auto`**
   - Normalisation automatique pour tous

3. **Définir explicitement les fichiers binaires**
   - Évite que Git les traite comme du texte

4. **Commiter `.gitattributes`**
   - Tout le monde utilise les mêmes règles

5. **Utiliser `export-ignore` pour les fichiers de dev**
   - Archives plus propres

#### ❌ À éviter

1. **Ne pas spécifier EOL quand ce n'est pas nécessaire**
   - `text=auto` suffit dans la plupart des cas

2. **Ne pas surcharger avec trop de règles**
   - Restez simple si le projet est simple

---

### Cas d'usage avancés

#### 1. Projet multi-plateforme

```gitattributes
# Normalisation globale
* text=auto

# Scripts Unix : toujours LF
*.sh text eol=lf
*.bash text eol=lf
Makefile text eol=lf

# Scripts Windows : toujours CRLF
*.bat text eol=crlf
*.cmd text eol=crlf
*.ps1 text eol=crlf
```

#### 2. Projet avec assets lourds

```gitattributes
# Code source
*.js text diff=javascript
*.css text diff=css

# Assets avec Git LFS
*.psd filter=lfs diff=lfs merge=lfs -text
*.ai filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
*.mov filter=lfs diff=lfs merge=lfs -text

# Binaires normaux
*.png binary
*.jpg binary
```

#### 3. Projet avec données sensibles

```gitattributes
# Appliquer un filtre aux fichiers de config
config/*.secret filter=clean-secrets
```

Configuration du filtre :
```bash
git config filter.clean-secrets.clean "sed 's/password=.*/password=REDACTED/'"  
git config filter.clean-secrets.smudge cat
```

---

### Résumé

**`.gitignore`** :
- Indique à Git quels fichiers **ne pas suivre**
- Essentiel pour garder un dépôt propre
- Utiliser des templates et `.gitignore` global

**`.gitattributes`** :
- Définit le **comportement** de Git pour certains fichiers
- Normalise les fins de lignes
- Améliore les diffs et merges
- Gère les fichiers binaires correctement

**Les deux ensemble** :
- Créent un environnement de développement cohérent
- Évitent les conflits inutiles
- Améliorent la collaboration

**À retenir** :
- Ajoutez ces fichiers **dès le début** du projet
- Commitez-les pour que toute l'équipe en profite
- Utilisez des templates comme base
- Adaptez selon vos besoins spécifiques

---

### Ressources

- [gitignore.io](https://www.toptal.com/developers/gitignore) - Générateur de .gitignore
- [GitHub gitignore templates](https://github.com/github/gitignore) - Templates officiels
- [Git Attributes Documentation](https://git-scm.com/docs/gitattributes) - Documentation officielle
- [Git LFS](https://git-lfs.github.com/) - Large File Storage

**Prochaine étape** : Maintenant que votre dépôt est bien configuré, découvrons les différents workflows collaboratifs avec Git !

⏭️ [Workflows collaboratifs (Git Flow, GitHub Flow, Trunk-Based)](/module-07-bonnes-pratiques-et-workflows/04-workflows-collaboratifs.md)
