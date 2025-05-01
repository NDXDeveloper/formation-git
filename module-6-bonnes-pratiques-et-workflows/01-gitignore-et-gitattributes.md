# 6.1. .gitignore et .gitattributes

## Le fichier .gitignore : gardez votre dépôt propre

### Qu'est-ce que .gitignore ?

Le fichier `.gitignore` est un outil essentiel qui vous permet de spécifier quels fichiers ou dossiers Git doit **ignorer**. En d'autres termes, ces fichiers ne seront jamais ajoutés à votre dépôt, même si vous exécutez `git add .` pour ajouter tous les fichiers.

### Pourquoi utiliser .gitignore ?

Voici pourquoi un fichier `.gitignore` bien configuré est indispensable :

1. **Éviter de versionner des fichiers inutiles** : fichiers temporaires, logs, caches...
2. **Ne pas inclure d'informations sensibles** : mots de passe, clés API, tokens...
3. **Réduire la taille du dépôt** : fichiers binaires volumineux, dossiers de dépendances...
4. **Éviter les conflits** liés à des fichiers spécifiques à votre environnement

### Comment créer un fichier .gitignore ?

Créez simplement un fichier nommé `.gitignore` à la racine de votre dépôt :

```bash
touch .gitignore
```

Ensuite, éditez-le pour y ajouter les motifs des fichiers à ignorer.

### Syntaxe de base

```
# Ceci est un commentaire
fichier.txt     # Ignore un fichier spécifique
*.log           # Ignore tous les fichiers .log
build/          # Ignore tout le dossier build
/config.json    # Ignore config.json à la racine du projet
```

### Règles courantes à inclure

Voici quelques règles couramment utilisées dans un fichier `.gitignore` :

```
# Fichiers système
.DS_Store
Thumbs.db

# Éditeurs et IDE
.idea/
.vscode/
*.swp
*.swo

# Dépendances et packages
node_modules/
vendor/
packages/

# Fichiers de build
dist/
build/
*.o
*.exe

# Fichiers de configuration locale
.env
.env.local
config.local.json

# Logs et données temporaires
*.log
logs/
tmp/
.cache/
```

### Astuces pour .gitignore

1. **Utilisez des modèles existants** : GitHub propose un [référentiel de modèles .gitignore](https://github.com/github/gitignore) pour différents langages et frameworks.
2. **Le site [gitignore.io](https://www.toptal.com/developers/gitignore)** permet de générer des fichiers `.gitignore` adaptés à votre stack technologique.
3. **Ignorez après coup** : Si vous avez déjà commité un fichier que vous souhaitez maintenant ignorer, utilisez :
   ```bash
   git rm --cached fichier.txt
   ```
   Puis ajoutez-le à votre `.gitignore`.

## Le fichier .gitattributes : contrôler le comportement de Git

### Qu'est-ce que .gitattributes ?

Le fichier `.gitattributes` permet de définir des attributs pour les fichiers de votre dépôt. Il contrôle comment Git gère certains fichiers, notamment en ce qui concerne les fins de ligne, les fusions, et le traitement des fichiers binaires.

### Pourquoi utiliser .gitattributes ?

1. **Normaliser les fins de ligne** entre différents systèmes d'exploitation
2. **Définir comment Git doit gérer certains types de fichiers** (texte ou binaire)
3. **Configurer des stratégies de fusion** pour certains fichiers
4. **Améliorer la collaboration** entre développeurs utilisant différents OS

### Comment créer un fichier .gitattributes ?

Créez un fichier nommé `.gitattributes` à la racine de votre dépôt :

```bash
touch .gitattributes
```

### Syntaxe de base

```
pattern attr1 attr2 ...
```

Par exemple :
```
*.txt text eol=lf
*.jpg binary
```

### Exemples courants

```
# Configuration des fins de ligne
*.txt text eol=lf       # Force LF (Unix) pour les fichiers texte
*.bat text eol=crlf     # Force CRLF (Windows) pour les fichiers batch

# Définition des types de fichiers
*.png binary            # Traite les PNG comme binaires
*.jpg binary            # Traite les JPG comme binaires

# Stratégies de fusion
*.md merge=union        # Utilise la stratégie union pour les fichiers Markdown
database.xml merge=ours # Garde notre version en cas de conflit
```

### Exemple de .gitattributes pour un projet web

```
# Auto detect text files and perform normalization
* text=auto

# Documents
*.md text diff=markdown
*.txt text
*.pdf binary

# Scripts
*.sh text eol=lf
*.bat text eol=crlf

# Web
*.css text diff=css
*.html text diff=html
*.js text
*.json text

# Images
*.png binary
*.jpg binary
*.gif binary
*.svg text

# Fonts
*.woff binary
*.woff2 binary
```

### Conseils pour .gitattributes

1. **Commencez par `* text=auto`** pour que Git détecte automatiquement les fichiers texte et binaires
2. **Soyez cohérent** avec votre équipe sur les configurations choisies
3. **Pensez aux différents systèmes d'exploitation** de votre équipe
4. **Configurez-le en début de projet** pour éviter les problèmes plus tard

## En résumé

- `.gitignore` vous permet d'exclure certains fichiers du contrôle de version
- `.gitattributes` vous permet de configurer comment Git traite vos fichiers

Ces deux fichiers sont essentiels pour maintenir un dépôt propre et assurer une collaboration fluide, particulièrement dans les équipes travaillant sur différents systèmes d'exploitation.

N'hésitez pas à les configurer dès le début de vos projets pour éviter des problèmes futurs !
