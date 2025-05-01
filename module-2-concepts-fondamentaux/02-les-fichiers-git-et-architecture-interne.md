# 2.2. Les fichiers .git et l'architecture interne

Vous avez peut-être remarqué qu'un dossier mystérieux nommé `.git` apparaît à la racine de votre projet lorsque vous initialisez un dépôt Git. Ce dossier est au cœur du fonctionnement de Git et contient toute la magie qui permet à Git de suivre vos fichiers.

Dans cette section, nous allons explorer ce qui se cache à l'intérieur de ce dossier pour mieux comprendre comment Git fonctionne en coulisses. Ne vous inquiétez pas, nous garderons les explications simples et accessibles !

## Pourquoi comprendre l'architecture interne ?

Vous pourriez vous demander : "Pourquoi devrais-je me soucier de ce qui se passe dans le dossier `.git` ? Je veux juste utiliser Git !"

Voici quelques bonnes raisons de comprendre les bases de l'architecture interne de Git :

1. **Mieux comprendre les commandes Git** : Savoir ce qui se passe en coulisses vous aide à comprendre pourquoi certaines commandes fonctionnent comme elles le font
2. **Résoudre des problèmes** : Quand quelque chose ne va pas, comprendre l'architecture peut vous aider à diagnostiquer le problème
3. **Éviter les erreurs coûteuses** : Savoir ce qui est important dans le dossier `.git` vous aidera à éviter de supprimer des éléments essentiels
4. **Satisfaire votre curiosité** : C'est fascinant de voir comment un outil aussi puissant est construit !

## Le dossier .git : le cerveau de votre dépôt

Lorsque vous exécutez `git init` dans un dossier, Git crée le dossier `.git` qui contient tout ce dont Git a besoin pour fonctionner. Ce dossier est le cerveau de votre dépôt : sans lui, votre projet serait juste un dossier ordinaire sans aucune des fonctionnalités de suivi des versions.

> **Note** : Le dossier `.git` commence par un point, ce qui le rend "caché" dans de nombreux systèmes d'exploitation. Pour le voir dans l'explorateur de fichiers, vous devrez peut-être activer l'affichage des fichiers cachés.

## Explorer le contenu du dossier .git

Pour voir ce que contient le dossier `.git`, vous pouvez utiliser la commande :

```bash
ls -la .git
```

Vous verrez probablement quelque chose comme ceci :

```
drwxr-xr-x  15 user  group   480 Jan 1 12:34 .
drwxr-xr-x   9 user  group   288 Jan 1 12:34 ..
-rw-r--r--   1 user  group    23 Jan 1 12:34 HEAD
-rw-r--r--   1 user  group   137 Jan 1 12:34 config
-rw-r--r--   1 user  group    73 Jan 1 12:34 description
drwxr-xr-x  13 user  group   416 Jan 1 12:34 hooks
drwxr-xr-x   3 user  group    96 Jan 1 12:34 info
drwxr-xr-x   4 user  group   128 Jan 1 12:34 objects
drwxr-xr-x   4 user  group   128 Jan 1 12:34 refs
```

Ne vous laissez pas intimider par tous ces fichiers et dossiers ! Examinons les éléments les plus importants pour comprendre comment Git fonctionne.

## Les composants principaux du dossier .git

### 1. Le fichier HEAD

Le fichier `HEAD` est comme un signet qui indique à Git où vous vous trouvez actuellement dans l'historique de votre projet. Il contient généralement une référence à la branche sur laquelle vous travaillez.

Si vous regardez son contenu avec :

```bash
cat .git/HEAD
```

Vous verrez probablement quelque chose comme :

```
ref: refs/heads/main
```

Cela signifie que vous êtes actuellement sur la branche "main". Lorsque vous changez de branche avec `git checkout` ou `git switch`, le contenu de ce fichier change pour pointer vers la nouvelle branche.

### 2. Le dossier objects : la base de données de Git

Le dossier `objects` est la base de données où Git stocke tout le contenu de votre projet. C'est le cœur du système de stockage de Git.

Git utilise un système de stockage basé sur le contenu, ce qui signifie que :
- Chaque fichier, dossier, commit, ou tout autre élément est stocké comme un "objet"
- Chaque objet reçoit un identifiant unique (une clé SHA-1) basé sur son contenu
- Les objets identiques partagent le même identifiant, ce qui rend le stockage très efficace

#### Types d'objets dans Git

Il existe quatre types d'objets principaux dans la base de données Git :

1. **Blobs (Binary Large Objects)** : Le contenu de vos fichiers
2. **Trees (Arbres)** : Représentent les dossiers et la structure de votre projet
3. **Commits** : Un instantané complet de votre projet à un moment donné
4. **Tags** : Des pointeurs nommés vers des commits spécifiques

Pour voir les objets dans votre dépôt, vous pouvez explorer le dossier :

```bash
ls -la .git/objects
```

Vous verrez des dossiers avec des noms de deux caractères et un dossier "pack". Les objets sont stockés dans ces dossiers en utilisant les deux premiers caractères de leur identifiant SHA-1 comme nom de dossier.

### 3. Le dossier refs : les références

Le dossier `refs` contient des pointeurs vers des commits. Il y a principalement trois types de références :

1. **heads** : Pointe vers la pointe de chaque branche
2. **tags** : Pointe vers des tags que vous avez créés
3. **remotes** : Pointe vers l'état des branches sur les dépôts distants

Pour voir vos branches locales :

```bash
ls -la .git/refs/heads
```

Chaque fichier dans ce dossier représente une branche, et son contenu est l'identifiant SHA-1 du commit le plus récent de cette branche.

### 4. Le fichier config

Le fichier `config` contient les paramètres spécifiques à ce dépôt. Par exemple :
- Les URLs des dépôts distants
- Les paramètres de branche par défaut
- Les configurations spécifiques au projet

Vous pouvez voir son contenu avec :

```bash
cat .git/config
```

C'est le même fichier que celui modifié par la commande `git config` lorsque vous l'utilisez sans l'option `--global`.

### 5. Le dossier hooks

Le dossier `hooks` contient des scripts que Git peut exécuter à certains moments (avant un commit, après un push, etc.). Ces scripts sont désactivés par défaut, mais vous pouvez les personnaliser pour automatiser des tâches.

### 6. Le dossier info

Le dossier `info` contient des informations supplémentaires, comme le fichier `exclude` qui fonctionne comme un `.gitignore` local au dépôt mais n'est pas partagé.

## Comment Git stocke vos données

Pour comprendre vraiment Git, il est utile de savoir comment il stocke vos données. Contrairement à d'autres systèmes de contrôle de version qui stockent des "différences" entre les fichiers, Git stocke des "instantanés" (snapshots) complets.

### Le modèle de stockage d'instantanés

Voici comment fonctionne le stockage dans Git :

1. Lorsque vous faites un commit, Git prend un "instantané" de tous les fichiers dans votre projet à ce moment précis
2. Les fichiers inchangés ne sont pas dupliqués - Git utilise simplement une référence au fichier identique stocké précédemment
3. Ces instantanés sont liés entre eux pour former un historique

Imaginez que vous prenez une photo de votre projet à chaque commit. Ces photos sont liées comme un film qui raconte l'histoire de votre projet.

### L'intégrité des données avec SHA-1

Git utilise un algorithme de hachage appelé SHA-1 pour générer un identifiant unique de 40 caractères pour chaque objet. Par exemple :

```
24b9da6552252987aa493b52f8696cd6d3b00373
```

Cet identifiant est calculé à partir du contenu de l'objet, ce qui signifie que :
- Deux fichiers identiques auront toujours le même identifiant SHA-1
- Si le contenu change, même d'un seul caractère, l'identifiant change complètement
- Il est pratiquement impossible que deux contenus différents aient le même identifiant

Cette propriété garantit l'intégrité des données dans Git : la moindre corruption ou modification serait immédiatement détectable.

## Une analogie pour mieux comprendre

Pour rendre cela plus concret, imaginons que le dossier `.git` est une bibliothèque :

- Le fichier **HEAD** est le signet dans le livre que vous êtes en train de lire
- Le dossier **objects** est l'ensemble des livres de la bibliothèque
- Le dossier **refs** est le catalogue qui vous aide à trouver rapidement les livres
- Le fichier **config** est le règlement de la bibliothèque
- Les **commits** sont des chapitres du livre de l'histoire de votre projet
- Les **branches** sont différentes histoires alternatives qui peuvent éventuellement fusionner

## Est-ce que je peux modifier le contenu du dossier .git ?

En règle générale, vous ne devriez **jamais** modifier manuellement le contenu du dossier `.git`. Laissez Git gérer ce dossier pour vous. Modifier directement ces fichiers peut corrompre votre dépôt et vous faire perdre des données.

> **Avertissement** : Ne supprimez jamais le dossier `.git` sauf si vous voulez délibérément supprimer tout l'historique et les informations de suivi de version de votre projet !

## Taille du dossier .git et performances

Avec le temps, le dossier `.git` peut devenir assez volumineux, surtout pour les projets qui existent depuis longtemps ou qui contiennent de gros fichiers. Git offre des commandes pour optimiser ce stockage :

```bash
# Nettoyage et optimisation de la base de données
git gc

# Vérification de la taille
du -sh .git
```

## Conclusion

Le dossier `.git` est le cœur et l'âme de Git. Il contient tout l'historique de votre projet, toutes les branches, tous les tags, et toute la magie qui permet à Git de fonctionner si efficacement.

Bien que vous n'ayez pas besoin de comprendre tous les détails pour utiliser Git efficacement, avoir une idée générale de son fonctionnement interne peut vous aider à mieux comprendre les commandes que vous utilisez au quotidien.

Dans la prochaine section, nous allons nous concentrer sur les commandes pratiques pour suivre vos fichiers avec Git : `git status`, `git add`, et `git commit`.
