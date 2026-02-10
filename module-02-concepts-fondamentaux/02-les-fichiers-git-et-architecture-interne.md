🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 2 : Concepts fondamentaux

## 2. Les fichiers .git et l'architecture interne

### Introduction : Regarder sous le capot

Vous savez maintenant utiliser Git pour gérer vos fichiers, mais comment Git fonctionne-t-il réellement en coulisses ? Que se passe-t-il quand vous faites `git add` ou `git commit` ? Où Git stocke-t-il toutes ces informations ?

Dans cette section, nous allons explorer l'architecture interne de Git. Ne vous inquiétez pas : vous n'avez pas besoin de tout comprendre pour utiliser Git efficacement, mais cette connaissance vous aidera à mieux comprendre ce que vous faites et à résoudre des problèmes plus facilement.

**Note importante** : Cette section est un peu plus technique. Si certains concepts vous semblent abstraits, ce n'est pas grave ! Revenez-y plus tard après avoir pratiqué davantage.

---

## Le dossier .git : le cœur de votre dépôt

### Qu'est-ce que le dossier .git ?

Rappelez-vous : quand vous faites `git init`, Git crée un dossier caché appelé `.git` dans votre projet. **Ce dossier contient absolument tout** ce dont Git a besoin pour fonctionner :

- L'historique complet de votre projet
- Les configurations locales
- Les références aux branches
- Les objets Git (fichiers, commits, etc.)

**Règle d'or** : Ne modifiez jamais manuellement le contenu de `.git` ! Git gère tout automatiquement. Comprendre ce qu'il contient est utile, mais le modifier peut corrompre votre dépôt.

### Visualiser le dossier .git

Dans votre terminal, positionnez-vous dans un dépôt Git et tapez :

```bash
# Lister le contenu de .git
ls -la .git
```

Sur Windows (PowerShell) :
```bash
ls -Force .git
```

Vous verrez quelque chose comme :

```
.git/
├── HEAD
├── config
├── description
├── hooks/
├── info/
├── objects/
├── refs/
└── index
```

Explorons chacun de ces éléments.

---

## Structure du dossier .git

### 1. Le fichier HEAD

**Emplacement** : `.git/HEAD`

Le fichier `HEAD` est un pointeur qui indique **où vous êtes actuellement** dans votre dépôt. C'est la référence à votre position actuelle dans l'historique.

Regardons son contenu :

```bash
cat .git/HEAD
```

Résultat typique :
```
ref: refs/heads/main
```

Cela signifie : "Vous êtes actuellement sur la branche `main`".

**Analogie** : Imaginez un livre avec un marque-page. Le fichier HEAD est ce marque-page : il vous indique quelle page (branche) vous lisez actuellement.

### 2. Le fichier config

**Emplacement** : `.git/config`

Ce fichier contient la **configuration locale** de votre dépôt (souvenez-vous des trois niveaux de configuration : system, global, local).

```bash
cat .git/config
```

Vous verrez quelque chose comme :

```ini
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://github.com/utilisateur/projet.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
	remote = origin
	merge = refs/heads/main
```

C'est ici que sont stockées les configurations spécifiques à ce projet (dépôts distants, branches, etc.).

### 3. Le fichier description

**Emplacement** : `.git/description`

Ce fichier contient une description du dépôt. Il est rarement utilisé et principalement utile pour les dépôts hébergés sur des serveurs Git.

```bash
cat .git/description
```

Par défaut :
```
Unnamed repository; edit this file 'description' to name the repository.
```

### 4. Le fichier index

**Emplacement** : `.git/index`

C'est un fichier binaire qui représente votre **Staging Area** ! C'est ici que Git stocke les informations sur ce qui sera dans votre prochain commit.

Vous ne pouvez pas le lire directement (c'est du binaire), mais vous pouvez voir son contenu avec :

```bash
git ls-files --stage
```

Résultat :
```
100644 a1b2c3d4... 0	README.md
100644 e5f6g7h8... 0	index.html
```

Cela montre tous les fichiers actuellement dans la Staging Area avec leur mode (permissions) et leur hash.

---

## Le dossier objects : la base de données de Git

### Qu'est-ce que objects/ ?

**Emplacement** : `.git/objects/`

C'est le **cœur de Git** ! Ce dossier contient tous vos fichiers, tous vos commits, toute l'histoire de votre projet. Tout est stocké ici sous forme d'objets.

```bash
ls -la .git/objects/
```

Vous verrez des dossiers avec des noms à deux caractères (00, 01, 02... ff) et un dossier `pack/` :

```
objects/
├── 00/
├── 01/
├── a7/
│   └── b8c9d0e1f2g3h4i5j6k7l8m9n0o1p2q3r4s5t6
├── pack/
└── info/
```

### Les trois types d'objets Git

Git stocke tout dans trois types d'objets principaux :

#### 1. Blob (Binary Large Object)

Un **blob** représente le **contenu d'un fichier**. C'est simplement les données du fichier, sans le nom ni les métadonnées.

**Analogie** : Imaginez une bibliothèque où chaque livre est identifié uniquement par son contenu, pas par son titre. Deux livres avec exactement le même contenu auraient le même identifiant.

**Exemple** :

```bash
# Créer un fichier
echo "Hello, Git!" > hello.txt

# L'ajouter
git add hello.txt

# Git a créé un blob avec le contenu "Hello, Git!"
```

Si vous créez deux fichiers avec le même contenu dans votre projet, Git ne stockera qu'**un seul blob** ! C'est très efficace.

#### 2. Tree (Arbre)

Un **tree** représente un **répertoire**. Il contient des références aux blobs (fichiers) et à d'autres trees (sous-dossiers).

**Analogie** : C'est comme la table des matières d'un livre. Elle liste les chapitres (fichiers) et leurs sections (sous-dossiers).

Un tree contient :
- Le nom des fichiers
- Le mode (permissions)
- Le type (blob ou tree)
- Le hash de l'objet

**Exemple de structure d'un tree** :

```
tree
├── README.md (blob a1b2c3d4...)
├── src/ (tree e5f6g7h8...)
│   ├── index.js (blob i9j0k1l2...)
│   └── utils.js (blob m3n4o5p6...)
└── package.json (blob q7r8s9t0...)
```

#### 3. Commit

Un **commit** est un objet qui regroupe :
- Un pointeur vers un tree (l'état complet de votre projet à ce moment)
- Le(s) parent(s) (le ou les commits précédents)
- L'auteur et la date
- Le message de commit

**Analogie** : Un commit est comme une photo instantanée de votre projet avec une note explicative et la date.

**Structure d'un commit** :

```
commit
├── tree: f1a2b3c4...         (l'état du projet)
├── parent: d5e6f7g8...        (le commit précédent)
├── author: Marie <marie@...>  (qui a créé)
├── committer: Marie <...>     (qui a enregistré)
├── date: 2025-10-15 14:30
└── message: "Ajout de la page d'accueil"
```

### Visualiser les objets

Pour voir le type d'un objet :

```bash
# Remplacez a1b2c3d par un vrai hash de votre dépôt
git cat-file -t a1b2c3d
```

Résultat : `blob`, `tree`, ou `commit`

Pour voir le contenu d'un objet :

```bash
git cat-file -p a1b2c3d
```

**Exemple pratique** :

```bash
# Voir le dernier commit
git cat-file -p HEAD
```

Résultat :
```
tree f1a2b3c4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0  
parent d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4  
author Marie Dupont <marie@email.com> 1729000000 +0200  
committer Marie Dupont <marie@email.com> 1729000000 +0200

Ajout de la page d'accueil
```

### Comment Git génère les identifiants (hash)

Chaque objet Git a un identifiant unique appelé **hash SHA-1** (40 caractères hexadécimaux).

Git calcule ce hash en fonction du **contenu** de l'objet. Cela signifie :
- Deux objets avec le même contenu auront le même hash
- Si le contenu change, le hash change
- Impossible de modifier un objet sans changer son hash

**Exemple** :

```bash
# Le contenu "Hello" donnera toujours le même hash
echo "Hello" | git hash-object --stdin
# Résultat : e965047ad7c57865823c7d992b1d046ea66edf78
```

C'est grâce à ce système que Git garantit l'**intégrité** de votre historique.

---

## Le dossier refs : les références

### Qu'est-ce que refs/ ?

**Emplacement** : `.git/refs/`

Ce dossier contient les **pointeurs vers des commits**. Les références sont des noms lisibles qui pointent vers des hash de commits.

Structure typique :

```
refs/
├── heads/          # Les branches locales
│   ├── main
│   └── feature-x
├── remotes/        # Les branches distantes
│   └── origin/
│       └── main
└── tags/           # Les tags
    └── v1.0.0
```

### Les branches (refs/heads/)

Chaque fichier dans `refs/heads/` est une branche. Le contenu du fichier est simplement le hash du commit sur lequel pointe la branche.

```bash
cat .git/refs/heads/main
```

Résultat :
```
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
```

**Concept clé** : Une branche n'est qu'un **pointeur mobile** vers un commit ! Quand vous créez un nouveau commit sur une branche, le pointeur avance automatiquement.

### Les branches distantes (refs/remotes/)

Stocke les références aux branches sur les dépôts distants (comme GitHub).

```bash
cat .git/refs/remotes/origin/main
```

Cela vous montre sur quel commit se trouve la branche `main` sur le dépôt distant.

### Les tags (refs/tags/)

Les tags sont comme les branches, mais ils **ne bougent pas**. C'est un pointeur permanent vers un commit spécifique (souvent utilisé pour marquer des versions).

---

## Le dossier hooks : scripts automatiques

### Qu'est-ce que hooks/ ?

**Emplacement** : `.git/hooks/`

Ce dossier contient des **scripts** qui peuvent s'exécuter automatiquement à certains moments (avant un commit, après un push, etc.).

```bash
ls .git/hooks/
```

Vous verrez des fichiers avec `.sample` :

```
applypatch-msg.sample  
commit-msg.sample  
pre-commit.sample  
pre-push.sample
...
```

Ces scripts sont désactivés par défaut (`.sample`). Pour en activer un, enlevez le `.sample` et rendez-le exécutable.

**Exemple d'utilisation** : Vous pourriez créer un hook `pre-commit` qui vérifie que votre code respecte certaines règles avant de permettre le commit.

Nous reviendrons sur les hooks dans un module avancé.

---

## Le dossier info : informations diverses

### Qu'est-ce que info/ ?

**Emplacement** : `.git/info/`

Contient des fichiers d'information supplémentaires :

- **exclude** : Comme `.gitignore` mais local au dépôt (non partagé)
- **refs** : Cache de certaines références

```bash
cat .git/info/exclude
```

Vous pouvez ajouter des patterns pour ignorer des fichiers localement sans modifier le `.gitignore` du projet.

---

## Comment Git stocke efficacement les données

### La déduplication automatique

Git est très efficace en termes de stockage grâce à plusieurs mécanismes :

#### 1. Stockage basé sur le contenu

Si deux fichiers ont le même contenu, Git ne les stocke qu'**une seule fois**.

**Exemple** :

```bash
# Créer deux fichiers identiques
echo "Même contenu" > fichier1.txt  
echo "Même contenu" > fichier2.txt

# Les ajouter
git add fichier1.txt fichier2.txt

# Git ne créera qu'un seul blob !
```

#### 2. Les objets compressés

Tous les objets Git sont **compressés** avec zlib. Cela réduit considérablement la taille.

#### 3. Les fichiers pack

Quand votre dépôt grossit, Git **optimise** le stockage en regroupant les objets dans des fichiers "pack" (`.git/objects/pack/`).

Au lieu de stocker chaque version complète d'un fichier, Git stocke :
- Une version complète
- Des deltas (différences) pour les autres versions

**Exemple** : Si vous modifiez légèrement un fichier de 1 Mo sur 10 commits, Git ne stockera pas 10 Mo, mais plutôt 1 Mo + quelques Ko de différences.

Vous pouvez déclencher cette optimisation manuellement :

```bash
git gc
```

(`gc` = garbage collection, nettoyage)

---

## Architecture globale : comment tout fonctionne ensemble

Résumons comment tous ces éléments interagissent :

### Quand vous faites git add

1. Git calcule le hash SHA-1 du contenu du fichier
2. Git compresse le contenu
3. Git crée un **blob** dans `.git/objects/`
4. Git met à jour l'**index** (`.git/index`) avec la référence au blob

### Quand vous faites git commit

1. Git crée des objets **tree** représentant la structure de vos dossiers
2. Git crée un objet **commit** qui pointe vers le tree racine
3. Git met à jour la référence de la branche actuelle (dans `.git/refs/heads/`)
4. Git met à jour **HEAD** pour pointer vers le nouveau commit

### Visualisation du processus

```
Fichiers modifiés
       ↓
   git add
       ↓
Création de blobs → .git/objects/
       ↓
Mise à jour index → .git/index
       ↓
   git commit
       ↓
Création de trees → .git/objects/
       ↓
Création commit → .git/objects/
       ↓
Mise à jour branche → .git/refs/heads/main
       ↓
Mise à jour HEAD → .git/HEAD
```

---

## Exemple concret : tracer un commit

Faisons un petit exercice pour voir comment tout est lié.

### 1. Créer un commit

```bash
# Créer un fichier
echo "Contenu de test" > test.txt

# L'ajouter et commiter
git add test.txt  
git commit -m "Test d'architecture"
```

### 2. Explorer le commit

```bash
# Obtenir le hash du dernier commit
git log --oneline -1
```

Résultat :
```
a1b2c3d Test d'architecture
```

### 3. Voir le contenu du commit

```bash
git cat-file -p a1b2c3d
```

Résultat :
```
tree e4f5g6h7...  
parent i8j9k0l1...  
author Marie Dupont <marie@email.com>
...
Test d'architecture
```

### 4. Voir le tree

```bash
git cat-file -p e4f5g6h7
```

Résultat :
```
100644 blob m2n3o4p5...    test.txt
100644 blob q6r7s8t9...    README.md
...
```

### 5. Voir le blob (fichier)

```bash
git cat-file -p m2n3o4p5
```

Résultat :
```
Contenu de test
```

**Vous venez de suivre toute la chaîne** : Commit → Tree → Blob → Contenu !

---

## Pourquoi cette architecture est brillante

### 1. Intégrité garantie

Grâce aux hash SHA-1, il est **impossible** de modifier l'historique sans que cela soit détectable. Toute modification change le hash, ce qui casse la chaîne.

### 2. Efficacité de stockage

La déduplication et la compression font que même de gros projets avec un long historique restent raisonnablement petits.

### 3. Rapidité

Comme Git stocke tout localement et utilise des hash pour identifier les objets, la plupart des opérations sont ultra-rapides.

### 4. Branches peu coûteuses

Une branche n'est qu'un fichier de 41 octets (un pointeur) ! Créer une branche est donc instantané, contrairement aux anciens systèmes où c'était une copie complète du projet.

### 5. Distribution facile

L'architecture à base d'objets rend facile la synchronisation entre dépôts. Git n'a qu'à envoyer les objets manquants.

---

## Commandes utiles pour explorer l'architecture

Voici des commandes qui vous permettent de "regarder sous le capot" :

```bash
# Voir le type d'un objet
git cat-file -t <hash>

# Voir le contenu d'un objet
git cat-file -p <hash>

# Voir la taille d'un objet
git cat-file -s <hash>

# Lister tous les objets du dépôt
git rev-list --all --objects

# Voir l'arbre d'un commit
git ls-tree <hash>

# Voir les fichiers dans l'index
git ls-files --stage

# Calculer le hash d'un fichier
git hash-object <fichier>

# Compter les objets
git count-objects -v

# Vérifier l'intégrité du dépôt
git fsck
```

---

## Ce qu'il n'est pas nécessaire de retenir

**Bonne nouvelle** : Vous n'avez pas besoin de mémoriser tous ces détails pour utiliser Git efficacement !

Ce qui est important :
- Comprendre que Git stocke tout dans `.git`
- Savoir que les commits, fichiers et arbres sont des objets
- Comprendre que les branches sont des pointeurs
- Savoir que Git garantit l'intégrité par les hash

Le reste est "bon à savoir" mais pas indispensable au quotidien.

---

## Analogie finale : la bibliothèque Git

Imaginez `.git` comme une **bibliothèque très organisée** :

- **objects/** : Les rayonnages avec tous les livres (votre contenu)
  - Chaque livre (objet) a un code unique (hash)
  - Les livres sont organisés par leurs deux premiers caractères

- **refs/heads/** : Les signets qui marquent où en est chaque histoire (branches)
  - Chaque signet indique le dernier chapitre lu

- **HEAD** : Le signet spécial qui montre quel livre vous lisez actuellement

- **index** : La liste de courses des livres à acheter (fichiers à commiter)

- **config** : Les règles de fonctionnement de votre bibliothèque

Cette bibliothèque est si bien organisée que même avec des milliers de livres, vous pouvez instantanément retrouver n'importe quelle version de n'importe quel livre !

---

## En résumé

L'architecture interne de Git repose sur quelques concepts simples :

1. **Tout est un objet** : blobs (fichiers), trees (dossiers), commits (instantanés)
2. **Les objets sont identifiés par leur hash** : impossible de les modifier sans changer leur identifiant
3. **Les branches sont des pointeurs** : légers et rapides
4. **Tout est local** : dans le dossier `.git`
5. **Stockage intelligent** : déduplication, compression, pack files

Cette architecture rend Git :
- **Rapide** : tout est local
- **Fiable** : intégrité cryptographique
- **Efficace** : stockage optimisé
- **Flexible** : branches peu coûteuses

Vous n'avez pas besoin de tout comprendre en détail, mais avoir cette vue d'ensemble vous aide à mieux saisir ce que fait Git quand vous utilisez ses commandes.

---

*Dans la section suivante, nous verrons en détail comment suivre les fichiers avec git status, git add et git commit.*

⏭️ [Suivi des fichiers : git status, git add, git commit](/module-02-concepts-fondamentaux/03-suivi-des-fichiers.md)
