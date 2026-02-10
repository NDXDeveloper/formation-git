🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 1 : Introduction à Git

## 6. Premier projet Git : création d'un dépôt local

### Qu'est-ce qu'un dépôt Git ?

Un **dépôt** (ou "repository" en anglais, souvent abrégé "repo") est un espace où Git stocke toutes les informations sur votre projet :

- L'historique complet de toutes les modifications
- Les différentes versions de vos fichiers
- Les métadonnées (qui a fait quoi et quand)
- La configuration spécifique au projet

Pensez à un dépôt comme à un **livre d'histoire** de votre projet : chaque page représente un moment précis de votre travail, et vous pouvez revenir à n'importe quelle page quand vous voulez.

### Les deux façons de créer un dépôt

Il existe deux manières d'obtenir un dépôt Git :

1. **Initialiser un nouveau dépôt** : Créer un dépôt Git dans un dossier existant ou nouveau
2. **Cloner un dépôt existant** : Copier un dépôt qui existe déjà ailleurs (nous verrons cela dans un module ultérieur)

Dans cette section, nous allons nous concentrer sur la première méthode : créer un tout nouveau dépôt.

---

## Créer votre premier dépôt local

### Étape 1 : Créer un dossier pour votre projet

Commençons par créer un dossier qui contiendra notre projet. Ouvrez votre terminal et tapez :

```bash
# Créer un nouveau dossier
mkdir mon-premier-projet

# Se déplacer dans ce dossier
cd mon-premier-projet
```

**Explication** :
- `mkdir` (make directory) crée un nouveau dossier
- `cd` (change directory) vous déplace dans ce dossier

**Alternative** : Vous pouvez aussi créer le dossier via votre explorateur de fichiers, puis naviguer dedans avec le terminal.

### Vérifier où vous êtes

Pour vérifier que vous êtes bien dans le bon dossier :

```bash
pwd
```

Cette commande affiche le chemin complet de votre dossier actuel (par exemple : `/Users/marie/mon-premier-projet`).

### Étape 2 : Initialiser le dépôt Git

Maintenant, transformons ce simple dossier en un dépôt Git :

```bash
git init
```

Vous devriez voir un message comme :

```
Initialized empty Git repository in /Users/marie/mon-premier-projet/.git/
```

**Félicitations !** Vous venez de créer votre premier dépôt Git. 🎉

### Que s'est-il passé ?

La commande `git init` a créé un dossier caché appelé `.git` dans votre projet. C'est là que Git stocke toutes les informations sur votre dépôt.

Pour voir ce dossier caché :

```bash
# Sur macOS/Linux
ls -la

# Sur Windows (PowerShell)
ls -Force

# Sur Windows (Git Bash)
ls -la
```

Vous verrez un dossier `.git` dans la liste. **Ne touchez jamais à ce dossier manuellement** - Git s'en occupe pour vous.

---

## Explorer la structure du dépôt

### Le dossier .git

Jetons un coup d'œil à ce que contient le dossier `.git` :

```bash
ls .git
```

Vous verrez plusieurs fichiers et dossiers, notamment :

- **HEAD** : Pointe vers la branche actuelle
- **config** : Configuration spécifique à ce dépôt
- **description** : Description du dépôt (utilisée rarement)
- **hooks/** : Scripts automatiques (nous verrons cela plus tard)
- **objects/** : Base de données de tous les objets Git
- **refs/** : Références aux commits (branches, tags)

Vous n'avez pas besoin de comprendre tout cela maintenant. L'important est de savoir que c'est ici que Git travaille en coulisses.

### Vérifier l'état du dépôt

La commande la plus importante que vous utiliserez au quotidien est :

```bash
git status
```

Résultat :

```
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

**Décryptage** :
- **On branch main** : Vous êtes sur la branche principale
- **No commits yet** : Aucun commit n'a encore été créé
- **nothing to commit** : Aucun fichier à enregistrer pour le moment

---

## Ajouter votre premier fichier

### Créer un fichier

Créons un fichier simple dans notre projet :

```bash
echo "# Mon Premier Projet Git" > README.md
```

Cette commande crée un fichier `README.md` avec le contenu "# Mon Premier Projet Git".

**Alternative** : Vous pouvez aussi créer ce fichier avec votre éditeur de texte préféré.

### Vérifier l'état

Retapons `git status` :

```bash
git status
```

Résultat :

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

**Analyse** :
- Git a détecté le nouveau fichier `README.md`
- Il est marqué comme "Untracked" (non suivi) : Git voit le fichier mais ne le surveille pas encore
- Git nous suggère d'utiliser `git add` pour le suivre

---

## Comprendre le workflow Git en trois étapes

Avant de continuer, il est crucial de comprendre que Git fonctionne en **trois étapes** :

### 1. Working Directory (Répertoire de travail)

C'est votre dossier de projet normal où vous créez et modifiez des fichiers. C'est là où vous travaillez au quotidien.

### 2. Staging Area (Zone de préparation)

C'est une zone intermédiaire où vous préparez les fichiers que vous voulez enregistrer. Imaginez-la comme un panier avant la caisse au supermarché : vous choisissez ce que vous voulez acheter.

### 3. Repository (Dépôt)

C'est l'historique permanent de votre projet. Une fois qu'un fichier est "commité", il est sauvegardé dans l'historique.

**Schéma conceptuel** :

```
Working Directory → Staging Area → Repository
    (modifier)      (git add)     (git commit)
```

---

## Ajouter le fichier à la Staging Area

Pour dire à Git de commencer à suivre notre fichier et de le préparer pour un commit :

```bash
git add README.md
```

Vérifions l'état :

```bash
git status
```

Résultat :

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   README.md
```

**Changements** :
- Le fichier est maintenant dans "Changes to be committed" (changements à commiter)
- Il est en vert (si vous avez activé les couleurs)
- Il est prêt à être enregistré dans l'historique

### Ajouter plusieurs fichiers

Créons quelques fichiers supplémentaires :

```bash
echo "print('Hello, Git!')" > hello.py  
echo "console.log('Hello, Git!');" > hello.js
```

Pour ajouter tous les fichiers d'un coup :

```bash
git add .
```

Le point `.` signifie "tous les fichiers du dossier actuel".

**Alternatives** :
```bash
# Ajouter tous les fichiers
git add --all

# Ajouter tous les fichiers Python
git add *.py

# Ajouter plusieurs fichiers spécifiques
git add file1.txt file2.txt
```

---

## Faire votre premier commit

Un **commit** est comme une photo instantanée de votre projet à un moment donné. C'est un point de sauvegarde dans l'historique.

### Créer le commit

```bash
git commit -m "Premier commit : ajout des fichiers initiaux"
```

**Explication** :
- `git commit` : crée un nouveau commit
- `-m` : permet de spécifier le message directement
- Entre guillemets : le message qui décrit ce que vous avez fait

Résultat :

```
[main (root-commit) a1b2c3d] Premier commit : ajout des fichiers initiaux
 3 files changed, 3 insertions(+)
 create mode 100644 README.md
 create mode 100644 hello.js
 create mode 100644 hello.py
```

**Félicitations !** Vous venez de créer votre premier commit ! 🎊

### Vérifier l'état après le commit

```bash
git status
```

Résultat :

```
On branch main  
nothing to commit, working tree clean
```

**Interprétation** :
- **working tree clean** : Tous vos fichiers sont à jour et sauvegardés
- Aucune modification en attente

---

## Explorer l'historique

### Voir les commits

Pour afficher l'historique de vos commits :

```bash
git log
```

Résultat :

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0  
Author: Votre Nom <votre.email@example.com>  
Date:   Wed Oct 15 14:30:00 2025 +0200

    Premier commit : ajout des fichiers initiaux
```

**Décryptage** :
- **commit a1b2c3d...** : L'identifiant unique du commit (hash SHA-1)
- **Author** : Qui a fait ce commit (c'est pour ça que la configuration du nom/email était importante !)
- **Date** : Quand le commit a été fait
- Le message du commit

### Format plus concis

Pour un affichage plus compact :

```bash
git log --oneline
```

Résultat :

```
a1b2c3d Premier commit : ajout des fichiers initiaux
```

### Affichage graphique

Pour voir l'historique sous forme de graphe :

```bash
git log --oneline --graph --all
```

Pour l'instant, avec un seul commit, ce n'est pas très impressionnant, mais cela deviendra très utile plus tard.

---

## Faire un deuxième commit

Pratiquons en faisant un autre commit. Modifions un fichier :

### 1. Modifier un fichier

Ajoutons du contenu au README :

```bash
echo "" >> README.md  
echo "Ce projet est un exemple pour apprendre Git." >> README.md
```

Ou ouvrez `README.md` dans votre éditeur et ajoutez du texte.

### 2. Vérifier les changements

```bash
git status
```

Résultat :

```
On branch main  
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md
```

Git a détecté que `README.md` a été modifié.

### 3. Voir les modifications exactes

```bash
git diff
```

Cette commande montre exactement ce qui a changé :

```diff
diff --git a/README.md b/README.md  
index e7b5e4c..d5a4b3c 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,3 @@
 # Mon Premier Projet Git
+
+Ce projet est un exemple pour apprendre Git.
```

Les lignes avec `+` sont les ajouts.

### 4. Ajouter et commiter

```bash
git add README.md  
git commit -m "Ajout de la description du projet"
```

### 5. Voir l'historique

```bash
git log --oneline
```

Résultat :

```
b2c3d4e Ajout de la description du projet  
a1b2c3d Premier commit : ajout des fichiers initiaux
```

Vous avez maintenant deux commits dans votre historique !

---

## Raccourci utile

Vous pouvez combiner `git add` et `git commit` pour les fichiers déjà suivis :

```bash
git commit -am "Message du commit"
```

**Attention** : Le `-a` ajoute automatiquement tous les fichiers **déjà suivis** qui ont été modifiés. Il n'ajoute pas les nouveaux fichiers non suivis.

---

## Bonnes pratiques pour les commits

### Messages de commit clairs

Un bon message de commit doit :
- Être concis mais descriptif
- Commencer par un verbe à l'impératif ou à l'infinitif
- Expliquer **quoi** et **pourquoi**, pas **comment**

**Bons exemples** :
- "Ajout de la fonction de connexion utilisateur"
- "Correction du bug d'affichage sur mobile"
- "Mise à jour de la documentation"

**Mauvais exemples** :
- "Update" (trop vague)
- "Modifications" (pas assez précis)
- "J'ai changé des trucs" (pas professionnel)

### Commits atomiques

Faites des commits **petits et ciblés** :
- Un commit = une modification logique
- Évitez les commits qui mélangent plusieurs fonctionnalités
- Plus facile de revenir en arrière si besoin

**Bon** : Trois commits séparés pour "Ajout du formulaire", "Validation des données", "Style du formulaire"

**Moins bon** : Un seul gros commit "Ajout du formulaire complet"

### Commiter régulièrement

N'attendez pas d'avoir terminé toute une fonctionnalité pour commiter. Commitez à chaque étape significative :
- Cela crée un historique détaillé
- Vous pouvez revenir à n'importe quelle étape
- Moins de risque de perdre du travail

---

## Vérifier l'information sur un commit

### Voir les détails d'un commit

```bash
git show
```

Cela affiche les détails du dernier commit (changements inclus).

Pour voir un commit spécifique :

```bash
git show a1b2c3d
```

Remplacez `a1b2c3d` par les premiers caractères du hash du commit qui vous intéresse.

### Voir l'historique avec les modifications

```bash
git log -p
```

Cette commande affiche l'historique avec le détail des modifications de chaque commit.

---

## Résumé du workflow de base

Voici le cycle que vous répéterez constamment :

```bash
# 1. Vérifier l'état
git status

# 2. Faire des modifications
# (éditer vos fichiers)

# 3. Voir ce qui a changé
git diff

# 4. Ajouter à la staging area
git add fichier.txt
# ou
git add .

# 5. Vérifier ce qui est prêt à être commité
git status

# 6. Commiter
git commit -m "Message descriptif"

# 7. Voir l'historique
git log --oneline
```

### Schéma récapitulatif

```
1. Modifier des fichiers (Working Directory)
   ↓
2. git add (vers Staging Area)
   ↓
3. git commit (vers Repository)
   ↓
4. Répéter
```

---

## Commandes essentielles récapitulatives

Voici les commandes que vous venez d'apprendre et que vous utiliserez quotidiennement :

| Commande | Description |
|----------|-------------|
| `git init` | Initialiser un nouveau dépôt Git |
| `git status` | Voir l'état actuel du dépôt |
| `git add <fichier>` | Ajouter un fichier à la staging area |
| `git add .` | Ajouter tous les fichiers modifiés |
| `git commit -m "message"` | Créer un commit avec un message |
| `git log` | Voir l'historique des commits |
| `git log --oneline` | Historique condensé |
| `git diff` | Voir les modifications non stagées |
| `git show` | Voir les détails du dernier commit |

---

## En résumé

Vous avez appris à :

1. **Créer un dépôt Git** avec `git init`
2. **Comprendre les trois états** : Working Directory, Staging Area, Repository
3. **Ajouter des fichiers** avec `git add`
4. **Créer des commits** avec `git commit`
5. **Explorer l'historique** avec `git log`
6. **Vérifier l'état** avec `git status`

Vous maîtrisez maintenant le workflow de base de Git ! C'est la fondation sur laquelle tout le reste repose. Dans les prochains modules, nous approfondirons ces concepts et découvrirons des fonctionnalités plus avancées.

---

*Le Module 1 est terminé ! Dans le Module 2, nous explorerons en détail les concepts fondamentaux de Git et comprendrons mieux son architecture interne.*

⏭️ [Module 2 : Concepts fondamentaux](/module-02-concepts-fondamentaux/README.md)
