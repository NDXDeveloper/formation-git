# 1.6. Premier projet Git : création d'un dépôt local

Maintenant que Git est installé et configuré sur votre ordinateur, il est temps de créer votre premier projet Git ! Dans cette section, nous allons vous guider pas à pas pour créer un dépôt local et commencer à suivre vos fichiers.

## Qu'est-ce qu'un dépôt Git ?

Un **dépôt** (ou "repository" en anglais, souvent abrégé en "repo") est simplement un projet suivi par Git. Techniquement, c'est un dossier contenant vos fichiers de projet ainsi qu'un sous-dossier spécial nommé `.git` qui contient toutes les informations nécessaires à Git pour suivre les modifications.

## Deux façons de créer un dépôt local

Il existe deux scénarios principaux pour créer un dépôt Git :

1. **Transformer un dossier existant en dépôt Git**
2. **Créer un nouveau dossier et l'initialiser comme dépôt Git**

Nous allons voir les deux approches.

## Méthode 1 : Transformer un dossier existant en dépôt Git

Si vous avez déjà un projet (un dossier avec des fichiers) que vous souhaitez commencer à suivre avec Git, voici comment procéder :

### Étape 1 : Ouvrez votre terminal ou invite de commande

- **Sur Windows** : Ouvrez Git Bash ou l'invite de commande
- **Sur macOS** : Ouvrez Terminal
- **Sur Linux** : Ouvrez votre terminal préféré

### Étape 2 : Naviguez vers le dossier de votre projet

Utilisez la commande `cd` (change directory) pour naviguer vers votre dossier :

```bash
cd chemin/vers/votre/projet
```

Par exemple :
- Sur Windows : `cd C:\Users\VotreNom\Documents\MonProjet`
- Sur macOS/Linux : `cd ~/Documents/MonProjet`

> **Astuce pour débutants** : Si vous ne savez pas comment naviguer dans le terminal, voici quelques commandes utiles :
> - `pwd` affiche le dossier actuel (Print Working Directory)
> - `ls` (ou `dir` sur Windows) liste les fichiers et dossiers
> - `cd ..` remonte d'un niveau dans l'arborescence

### Étape 3 : Initialisez le dépôt Git

Une fois dans le dossier de votre projet, exécutez la commande suivante :

```bash
git init
```

Vous devriez voir un message comme celui-ci :
```
Initialized empty Git repository in /chemin/vers/votre/projet/.git/
```

Félicitations ! Votre dossier est maintenant un dépôt Git. Git a créé un sous-dossier caché `.git` qui contient toute l'infrastructure nécessaire pour suivre vos modifications.

## Méthode 2 : Créer un nouveau projet Git à partir de zéro

Si vous souhaitez commencer un nouveau projet, voici comment créer un dépôt Git vide :

### Étape 1 : Ouvrez votre terminal ou invite de commande

### Étape 2 : Naviguez vers l'emplacement où vous voulez créer le projet

Par exemple, pour créer votre projet dans le dossier Documents :

```bash
cd ~/Documents
```

### Étape 3 : Créez un nouveau dossier pour votre projet

```bash
mkdir MonNouveauProjet
```

### Étape 4 : Déplacez-vous dans ce nouveau dossier

```bash
cd MonNouveauProjet
```

### Étape 5 : Initialisez le dépôt Git

```bash
git init
```

Vous avez maintenant un tout nouveau dépôt Git vide, prêt à accueillir vos fichiers !

## Vérifier l'état de votre dépôt

Après avoir initialisé votre dépôt, il est bon de prendre l'habitude de vérifier son état avec la commande :

```bash
git status
```

Pour un nouveau dépôt, vous devriez voir quelque chose comme :

```
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Cette sortie vous indique que :
- Vous êtes sur la branche principale (main)
- Aucun commit n'a encore été fait
- Il n'y a rien à committer pour l'instant

## Ajouter des fichiers à votre projet

Maintenant, créons quelques fichiers pour notre projet. Vous pouvez utiliser n'importe quel éditeur de texte pour cela.

### Exemple : Créer un fichier README.md

Un fichier README est souvent le premier fichier d'un projet. Il explique ce qu'est le projet et comment l'utiliser.

1. Créez un fichier nommé `README.md` dans votre dossier de projet
2. Ajoutez-y du texte simple, par exemple :
   ```
   # Mon Premier Projet Git

   C'est mon premier projet utilisant Git pour le contrôle de version.
   ```
3. Sauvegardez le fichier

### Vérifier l'état après avoir ajouté un fichier

Exécutez à nouveau :

```bash
git status
```

Vous devriez maintenant voir quelque chose comme :

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

Git vous indique qu'il a détecté un nouveau fichier `README.md`, mais qu'il ne le suit pas encore (untracked).

## Suivre vos premiers fichiers

Pour que Git commence à suivre un fichier, vous devez l'ajouter à la "zone de préparation" (staging area) avec la commande `git add` :

```bash
git add README.md
```

Pour vérifier que le fichier a bien été ajouté, exécutez :

```bash
git status
```

Vous devriez maintenant voir :

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

Cela signifie que le fichier est maintenant dans la zone de préparation, prêt à être enregistré dans l'historique du projet.

> **Note pour débutants** : Pour ajouter tous les fichiers modifiés en une seule commande, vous pouvez utiliser `git add .`

## Créer votre premier commit

Un "commit" est comme une photographie de votre projet à un moment donné. Pour créer votre premier commit :

```bash
git commit -m "Premier commit : ajout du fichier README"
```

Le paramètre `-m` vous permet d'ajouter un message décrivant ce que contient ce commit. Ce message est important pour comprendre plus tard les modifications apportées.

Après cette commande, vous devriez voir quelque chose comme :

```
[main (root-commit) f7d1e17] Premier commit : ajout du fichier README
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
```

Félicitations ! Vous venez de créer votre premier commit. Le code `f7d1e17` (ou similaire) est l'identifiant unique de ce commit.

## Vérifier l'historique des commits

Pour voir l'historique des commits de votre projet, utilisez :

```bash
git log
```

Vous devriez voir votre commit avec son identifiant, l'auteur (vous), la date et le message :

```
commit f7d1e17b9c48d4aacbd748f88b501e8a27c47b32 (HEAD -> main)
Author: Votre Nom <votre.email@exemple.com>
Date:   Jeu Mai 1 13:37:01 2025 +0200

    Premier commit : ajout du fichier README
```

## Cycle de travail typique avec Git

Maintenant que vous avez créé votre premier dépôt et commit, voici le cycle de travail typique avec Git :

1. **Modifiez** vos fichiers dans votre éditeur
2. **Ajoutez** les modifications à la zone de préparation avec `git add`
3. **Committez** ces modifications avec `git commit`
4. Répétez !

### Exemple pratique : ajouter un deuxième fichier

1. Créez un nouveau fichier, par exemple `index.html` :
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Mon Premier Projet Git</title>
   </head>
   <body>
       <h1>Bienvenue sur mon projet</h1>
       <p>Je suis en train d'apprendre Git !</p>
   </body>
   </html>
   ```

2. Vérifiez l'état :
   ```bash
   git status
   ```

3. Ajoutez le fichier à la zone de préparation :
   ```bash
   git add index.html
   ```

4. Committez le changement :
   ```bash
   git commit -m "Ajout de la page d'accueil HTML"
   ```

5. Vérifiez à nouveau l'historique :
   ```bash
   git log
   ```

Vous verrez maintenant vos deux commits dans l'historique.

## Comprendre la structure d'un dépôt Git

Si vous êtes curieux, vous pouvez voir le dossier spécial `.git` que Git a créé :

```bash
ls -la .git
```

Vous verrez plusieurs fichiers et dossiers, comme `HEAD`, `config`, `objects`, etc. Ne vous inquiétez pas si cela semble complexe pour l'instant - vous n'avez pas besoin de comprendre ces fichiers pour utiliser Git efficacement.

> **Note importante** : Ne supprimez jamais manuellement le dossier `.git` sauf si vous voulez délibérément supprimer tout l'historique Git !

## Récapitulatif des commandes apprises

- `git init` : Initialise un nouveau dépôt Git
- `git status` : Affiche l'état actuel du dépôt
- `git add` : Ajoute des fichiers à la zone de préparation
- `git commit` : Enregistre les modifications dans l'historique
- `git log` : Affiche l'historique des commits

## Exercice pratique

Pour vous entraîner, essayez de :

1. Modifier le fichier README.md en ajoutant une nouvelle ligne
2. Vérifier l'état avec `git status`
3. Ajouter la modification à la zone de préparation avec `git add`
4. Créer un nouveau commit avec `git commit`
5. Vérifier l'historique avec `git log`

## Conclusion

Dans cette section, vous avez appris à créer votre premier dépôt Git et à enregistrer des modifications dans l'historique. Vous connaissez maintenant les bases pour commencer à utiliser Git dans vos projets personnels.

Dans le prochain module, nous explorerons plus en détail les concepts fondamentaux de Git, notamment les trois états de Git et comment explorer l'historique des modifications.
