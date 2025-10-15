🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 2 : Concepts fondamentaux

## 1. Les 3 états de Git : Working Directory, Staging Area, Repository

### Introduction : Comprendre le flux de travail Git

Dans le Module 1, nous avons brièvement évoqué que Git fonctionne avec trois zones distinctes. Maintenant que vous avez créé votre premier dépôt et effectué vos premiers commits, il est temps de comprendre en profondeur comment Git gère vos fichiers.

Cette compréhension est **cruciale** pour maîtriser Git. Une fois que vous aurez assimilé ces trois états, tout le reste deviendra beaucoup plus clair et intuitif.

### Vue d'ensemble : Les trois états

Git organise votre travail en trois zones principales :

1. **Working Directory** (Répertoire de travail)
2. **Staging Area** (Zone de préparation / Index)
3. **Repository** (Dépôt / Historique)

Chaque fichier de votre projet se trouve dans l'un de ces états, et vous faites transiter les fichiers d'un état à l'autre avec des commandes Git spécifiques.

---

## État 1 : Working Directory (Répertoire de travail)

### Qu'est-ce que le Working Directory ?

Le **Working Directory** est simplement le dossier de votre projet tel que vous le voyez dans votre explorateur de fichiers. C'est là où vous travaillez au quotidien : vous créez, modifiez et supprimez des fichiers normalement.

### Caractéristiques

- **C'est votre espace de travail personnel** : Vous pouvez faire tout ce que vous voulez
- **Modifications non enregistrées** : Les changements ici ne sont pas encore sauvegardés dans Git
- **Totalement flexible** : Vous pouvez expérimenter librement sans affecter l'historique

### Analogie

Imaginez que vous écrivez un livre. Le Working Directory, c'est votre bureau avec tous vos brouillons, notes et pages en cours d'écriture. Vous pouvez griffonner, rayer, recommencer autant que vous voulez. Rien n'est encore "officiel".

### Exemple concret

Quand vous ouvrez votre projet dans VS Code (ou tout autre éditeur) et que vous modifiez un fichier, vous travaillez dans le Working Directory.

```bash
# Vous modifiez un fichier
echo "Nouvelle ligne" >> fichier.txt

# Le fichier est modifié dans le Working Directory
# Mais Git n'a encore rien enregistré
```

### États des fichiers dans le Working Directory

Les fichiers peuvent avoir différents statuts dans le Working Directory :

#### 1. Fichiers non suivis (Untracked)

Ce sont des **nouveaux fichiers** que Git ne connaît pas encore. Git les voit mais ne les surveille pas.

```bash
# Créer un nouveau fichier
echo "Contenu" > nouveau.txt

# Vérifier le statut
git status
```

Résultat :
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	nouveau.txt
```

#### 2. Fichiers modifiés (Modified)

Ce sont des **fichiers déjà suivis par Git** que vous avez modifiés depuis le dernier commit.

```bash
# Modifier un fichier existant
echo "Modification" >> README.md

# Vérifier le statut
git status
```

Résultat :
```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
	modified:   README.md
```

#### 3. Fichiers supprimés (Deleted)

Des **fichiers suivis que vous avez supprimés** de votre Working Directory.

```bash
# Supprimer un fichier
rm ancien.txt

# Git détecte la suppression
git status
```

Résultat :
```
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
	deleted:    ancien.txt
```

---

## État 2 : Staging Area (Zone de préparation)

### Qu'est-ce que la Staging Area ?

La **Staging Area** (aussi appelée "Index") est une zone intermédiaire entre votre Working Directory et le Repository. C'est là que vous **préparez** les fichiers que vous voulez inclure dans votre prochain commit.

### Pourquoi cette zone intermédiaire ?

Cette étape supplémentaire peut sembler étrange au début, mais elle est extrêmement puissante. Elle vous permet de :

- **Sélectionner précisément** ce que vous voulez commiter
- **Construire votre commit progressivement** : Vous pouvez ajouter les fichiers un par un
- **Séparer différentes modifications** : Même si vous avez modifié 10 fichiers, vous pouvez ne commiter que 3 fichiers
- **Réviser avant de valider** : Vous pouvez vérifier ce qui va être enregistré

### Analogie

Reprenons l'analogie du livre. La Staging Area, c'est comme une **table de relecture** où vous placez les pages que vous considérez prêtes à être publiées. Vous pouvez :
- Ajouter des pages à cette table
- En retirer si vous changez d'avis
- Vérifier que tout est correct avant l'envoi à l'éditeur

### Comment ajouter des fichiers à la Staging Area ?

La commande `git add` déplace les fichiers du Working Directory vers la Staging Area.

```bash
# Ajouter un fichier spécifique
git add fichier.txt

# Ajouter plusieurs fichiers
git add fichier1.txt fichier2.txt

# Ajouter tous les fichiers modifiés
git add .

# Ajouter tous les fichiers d'un certain type
git add *.js
```

### Exemple détaillé

Imaginons que vous avez modifié trois fichiers : `index.html`, `style.css` et `script.js`, mais vous voulez les commiter séparément.

```bash
# Vérifier l'état initial
git status
```

Résultat :
```
Changes not staged for commit:
	modified:   index.html
	modified:   style.css
	modified:   script.js
```

Tous les fichiers sont modifiés dans le Working Directory, mais aucun n'est staged.

```bash
# Ajouter seulement index.html à la Staging Area
git add index.html

# Vérifier l'état
git status
```

Résultat :
```
Changes to be committed:
	modified:   index.html

Changes not staged for commit:
	modified:   style.css
	modified:   script.js
```

Maintenant, `index.html` est dans la Staging Area (prêt à être commité), tandis que les deux autres sont toujours dans le Working Directory.

### Visualiser ce qui est staged

Pour voir exactement ce qui est dans la Staging Area :

```bash
# Voir les fichiers staged
git diff --staged
```

Ou :

```bash
git diff --cached
```

Ces commandes montrent les différences entre la Staging Area et le dernier commit.

### Retirer des fichiers de la Staging Area

Si vous changez d'avis et voulez retirer un fichier de la Staging Area **sans perdre vos modifications** :

```bash
# Retirer un fichier de la Staging Area (Git 2.23+)
git restore --staged fichier.txt

# Ancienne syntaxe (toujours valide)
git reset HEAD fichier.txt
```

Le fichier revient dans le Working Directory avec vos modifications intactes.

---

## État 3 : Repository (Dépôt / Historique)

### Qu'est-ce que le Repository ?

Le **Repository** (dépôt) est la base de données Git qui contient l'**historique complet** de votre projet. C'est là que sont stockés tous vos commits de manière permanente.

### Caractéristiques

- **Permanence** : Une fois qu'un commit est créé, il fait partie de l'historique
- **Intégrité** : Chaque commit est identifié par un hash unique et cryptographiquement sécurisé
- **Traçabilité complète** : Vous pouvez remonter à n'importe quel moment de l'historique
- **Immuabilité** : L'historique ne peut pas être modifié sans que cela soit détectable

### Analogie

Dans notre analogie du livre, le Repository c'est la **bibliothèque officielle** où toutes les éditions publiées de votre livre sont archivées. Une fois qu'une version est publiée (commitée), elle reste dans les archives pour toujours.

### Comment enregistrer dans le Repository ?

La commande `git commit` déplace ce qui est dans la Staging Area vers le Repository.

```bash
# Créer un commit avec les fichiers staged
git commit -m "Message descriptif du commit"
```

### Exemple complet

Reprenons l'exemple précédent où nous avons staged `index.html` :

```bash
# Commiter le fichier staged
git commit -m "Amélioration de la structure HTML"
```

Résultat :
```
[main 8a7b9c4] Amélioration de la structure HTML
 1 file changed, 5 insertions(+), 2 deletions(-)
```

Maintenant, `index.html` est dans le Repository (dans l'historique), mais `style.css` et `script.js` sont toujours modifiés dans le Working Directory.

```bash
# Vérifier l'état
git status
```

Résultat :
```
Changes not staged for commit:
	modified:   style.css
	modified:   script.js
```

---

## Le cycle complet : schéma et flux de travail

### Schéma conceptuel

Voici comment les fichiers circulent entre les trois états :

```
┌─────────────────────┐
│  Working Directory  │  ← Vous travaillez ici
│   (Modifications)   │
└──────────┬──────────┘
           │
           │  git add
           ↓
┌─────────────────────┐
│   Staging Area      │  ← Préparation du commit
│   (Index/Cache)     │
└──────────┬──────────┘
           │
           │  git commit
           ↓
┌─────────────────────┐
│    Repository       │  ← Historique permanent
│   (.git/objects)    │
└─────────────────────┘
```

### Flux de travail typique

Voici le cycle que vous répéterez constamment :

#### 1. Modifier des fichiers (Working Directory)

```bash
# Vous éditez vos fichiers normalement
# Par exemple, modifier README.md dans votre éditeur
```

#### 2. Vérifier l'état

```bash
git status
```

Vous voyez quels fichiers ont été modifiés.

#### 3. Voir les modifications exactes

```bash
git diff
```

Cela affiche ligne par ligne ce qui a changé dans le Working Directory.

#### 4. Ajouter à la Staging Area

```bash
# Ajouter des fichiers spécifiques
git add README.md

# Ou tout ajouter
git add .
```

#### 5. Vérifier ce qui est staged

```bash
git status
git diff --staged
```

#### 6. Commiter dans le Repository

```bash
git commit -m "Description claire de ce qui a été fait"
```

#### 7. Vérifier l'historique

```bash
git log --oneline
```

---

## Comprendre les commandes de transition

Voici un récapitulatif des commandes qui font transiter les fichiers entre les états :

### Du Working Directory vers la Staging Area

```bash
git add <fichier>      # Ajouter un fichier spécifique
git add .              # Ajouter tous les fichiers
git add *.txt          # Ajouter tous les fichiers .txt
git add -A             # Ajouter tous les fichiers (y compris suppressions)
git add -u             # Ajouter seulement les fichiers déjà suivis
```

### De la Staging Area vers le Repository

```bash
git commit -m "Message"           # Commit avec message court
git commit                         # Ouvre l'éditeur pour un message long
git commit -am "Message"           # Add + commit pour fichiers suivis
```

### Retour arrière : de la Staging Area vers le Working Directory

```bash
git restore --staged <fichier>    # Unstage un fichier (moderne)
git reset HEAD <fichier>           # Unstage un fichier (classique)
```

### Retour arrière : annuler les modifications du Working Directory

```bash
git restore <fichier>              # Restaurer la version du dernier commit
git checkout -- <fichier>          # Ancienne syntaxe
```

**Attention** : Cette commande **supprime définitivement** vos modifications non commitées !

---

## Cas pratiques : scénarios courants

### Scénario 1 : Commit partiel

Vous avez modifié plusieurs fichiers, mais vous voulez les commiter séparément pour garder un historique clair.

```bash
# Modifier plusieurs fichiers
echo "Fix bug" >> bug.js
echo "New feature" >> feature.js

# Ajouter et commiter le fix séparément
git add bug.js
git commit -m "Correction du bug d'authentification"

# Puis la feature
git add feature.js
git commit -m "Ajout de la fonctionnalité de recherche"
```

### Scénario 2 : Changement d'avis après staging

Vous avez ajouté un fichier à la Staging Area, mais vous réalisez que vous voulez encore le modifier.

```bash
# Ajouter un fichier
git add fichier.txt

# Changer d'avis
git restore --staged fichier.txt

# Le fichier est de retour dans le Working Directory
# Vous pouvez le modifier à nouveau
```

### Scénario 3 : Annuler des modifications locales

Vous avez fait des modifications dans un fichier, mais vous voulez revenir à la version du dernier commit.

```bash
# Vous avez modifié fichier.txt et vous voulez annuler
git restore fichier.txt

# Ou pour tous les fichiers
git restore .
```

**Attention** : Vos modifications seront perdues définitivement !

### Scénario 4 : Vérifier avant de commiter

Vous voulez être sûr de ce que vous allez commiter.

```bash
# Modifier des fichiers
# ...

# Ajouter à la staging area
git add .

# Vérifier exactement ce qui va être commité
git diff --staged

# Si tout est OK
git commit -m "Message"

# Sinon, retirer ce qui ne va pas
git restore --staged fichier_problematique.txt
```

---

## États des fichiers : récapitulatif visuel

Voici un tableau qui résume tous les états possibles d'un fichier :

| État | Emplacement | Commande pour y arriver | Commande pour en sortir |
|------|-------------|-------------------------|-------------------------|
| **Untracked** | Working Directory | Créer un nouveau fichier | `git add` |
| **Modified** | Working Directory | Modifier un fichier suivi | `git add` ou `git restore` |
| **Staged** | Staging Area | `git add` | `git commit` ou `git restore --staged` |
| **Committed** | Repository | `git commit` | (permanent dans l'historique) |
| **Unmodified** | Après commit | `git commit` | Modifier le fichier |

---

## Commandes de visualisation de l'état

Git vous donne plusieurs outils pour comprendre dans quel état sont vos fichiers :

### git status : Vue d'ensemble

```bash
git status
```

Affiche :
- Les fichiers dans la Staging Area (en vert)
- Les fichiers modifiés dans le Working Directory (en rouge)
- Les fichiers non suivis

### git status -s : Version courte

```bash
git status -s
```

Affiche un résumé compact :
```
M  fichier_staged.txt
 M fichier_modifié.txt
?? fichier_non_suivi.txt
```

Légende :
- `M ` (espace après) : Modifié et staged
- ` M` (espace avant) : Modifié mais pas staged
- `MM` : Modifié, staged, puis modifié à nouveau
- `??` : Non suivi

### git diff : Différences détaillées

```bash
# Working Directory vs Staging Area
git diff

# Staging Area vs dernier commit
git diff --staged

# Working Directory vs dernier commit
git diff HEAD
```

---

## Comprendre le fichier .git/index

La Staging Area est physiquement stockée dans un fichier binaire appelé `.git/index`. C'est là que Git garde la trace de ce qui sera dans le prochain commit.

Vous n'avez pas besoin de manipuler ce fichier directement (Git s'en charge), mais comprendre son existence vous aide à mieux saisir comment Git fonctionne.

---

## Points clés à retenir

1. **Trois états, pas deux** : Git ne se contente pas de "modifié" et "sauvegardé". Il y a une étape intermédiaire cruciale : la Staging Area.

2. **La Staging Area donne du contrôle** : Elle vous permet de construire des commits propres et atomiques, même si vous avez modifié plein de fichiers.

3. **git status est votre ami** : Utilisez cette commande constamment pour savoir où vous en êtes.

4. **Les transitions sont réversibles** (presque) : Vous pouvez faire des allers-retours entre Working Directory et Staging Area sans problème. Attention seulement avec `git restore` qui supprime les modifications.

5. **Le Repository est permanent** : Une fois commité, c'est dans l'historique. C'est ce qui rend Git si puissant pour la traçabilité.

---

## Erreurs courantes des débutants

### Erreur 1 : Oublier de git add

```bash
# Modifier un fichier
echo "Nouveau contenu" >> fichier.txt

# Essayer de commiter directement
git commit -m "Message"
```

**Résultat** : Rien ne se passe ! Il faut d'abord `git add`.

### Erreur 2 : Confondre git add et git commit

`git add` ne sauvegarde pas dans l'historique, il prépare seulement. Seul `git commit` sauvegarde vraiment.

### Erreur 3 : Utiliser git add . sans vérifier

Ajouter tous les fichiers d'un coup peut inclure des fichiers que vous ne voulez pas commiter (fichiers de configuration locale, mots de passe, etc.).

**Bonne pratique** : Toujours faire `git status` avant `git add .`

### Erreur 4 : Ne pas utiliser .gitignore

Certains fichiers ne doivent jamais être versionnés (logs, fichiers temporaires, dépendances). Utilisez un fichier `.gitignore` (nous verrons cela plus tard).

---

## En résumé

Les trois états de Git constituent le cœur du fonctionnement de l'outil :

1. **Working Directory** : Votre espace de travail, où vous modifiez librement vos fichiers
2. **Staging Area** : Zone de préparation où vous sélectionnez ce qui sera dans le prochain commit
3. **Repository** : L'historique permanent de votre projet

**Le workflow** :
```
Modifier → git add → git commit
   ↓          ↓          ↓
Working    Staging   Repository
Directory   Area
```

Maîtriser ces trois états et les transitions entre eux est fondamental pour utiliser Git efficacement. Une fois que vous aurez intégré ce concept, tout le reste de Git deviendra beaucoup plus logique.

---

*Dans la section suivante, nous explorerons les fichiers .git et l'architecture interne de Git pour comprendre ce qui se passe en coulisses.*

⏭️ [Les fichiers .git et l'architecture interne](/module-02-concepts-fondamentaux/02-les-fichiers-git-et-architecture-interne.md)
