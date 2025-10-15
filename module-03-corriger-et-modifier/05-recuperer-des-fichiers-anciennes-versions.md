🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 3 : Corriger et modifier
## 5. Récupérer des fichiers d'anciennes versions

### Introduction

L'un des grands avantages de Git est qu'il conserve l'historique complet de votre projet. Vous pouvez donc voyager dans le temps et récupérer des fichiers tels qu'ils étaient à n'importe quel moment du passé. Que vous ayez supprimé un fichier par erreur, besoin d'une ancienne version pour comparaison, ou que vous vouliez retrouver du code supprimé, Git vous permet de tout récupérer.

**Analogie** : Imaginez que Git est une machine à remonter le temps pour vos fichiers. Vous pouvez ouvrir n'importe quel "instantané" du passé et en extraire exactement ce dont vous avez besoin, sans affecter votre présent.

---

### Différence importante : Fichier vs Projet complet

Avant de commencer, clarifions une distinction essentielle :

**Récupérer un fichier :** Vous ne voulez qu'un ou quelques fichiers d'une ancienne version, sans toucher au reste de votre projet.

**Revenir à un commit entier :** Vous voulez remettre tout votre projet dans l'état d'un ancien commit (nous avons vu ça avec `git reset`).

Cette section se concentre sur la **récupération de fichiers individuels**.

---

### Méthode 1 : `git restore --source` (Recommandé)

C'est la méthode moderne et la plus claire pour récupérer des fichiers d'anciennes versions.

#### Syntaxe de base

```bash
git restore --source=<commit> <fichier>
```

#### Exemple simple

```bash
# Voir l'historique pour identifier le commit
git log --oneline
# d4e5f6g (HEAD -> main) Version 4
# c3d4e5f Version 3
# b2c3d4e Version 2
# a1b2c3d Version 1

# Récupérer fichier.txt tel qu'il était au commit b2c3d4e (Version 2)
git restore --source=b2c3d4e fichier.txt

# Le fichier est maintenant restauré dans votre Working Directory
# Il apparaît comme "modifié" dans git status
```

#### Récupérer depuis HEAD (commits relatifs)

```bash
# Récupérer depuis le commit précédent
git restore --source=HEAD~1 fichier.txt

# Récupérer depuis 3 commits en arrière
git restore --source=HEAD~3 fichier.txt

# Récupérer depuis 5 commits en arrière
git restore --source=HEAD~5 fichier.txt
```

#### Récupérer plusieurs fichiers

```bash
# Plusieurs fichiers spécifiques
git restore --source=a1b2c3d fichier1.txt fichier2.js

# Tous les fichiers d'un dossier
git restore --source=a1b2c3d src/

# Tous les fichiers .css
git restore --source=a1b2c3d "*.css"
```

---

### Méthode 2 : `git checkout` (Ancienne méthode)

Bien que `git restore` soit recommandé, vous verrez souvent `git checkout` dans les tutoriels plus anciens.

#### Syntaxe

```bash
git checkout <commit> -- <fichier>
```

**Important :** Le `--` est crucial. Il indique à Git que ce qui suit est un nom de fichier, pas une branche.

#### Exemples

```bash
# Récupérer un fichier depuis un commit
git checkout a1b2c3d -- fichier.txt

# Récupérer depuis le commit précédent
git checkout HEAD~1 -- fichier.txt

# Plusieurs fichiers
git checkout b2c3d4e -- fichier1.txt fichier2.js
```

---

### Méthode 3 : Voir le contenu sans restaurer (`git show`)

Parfois, vous voulez juste **consulter** le contenu d'un ancien fichier sans le restaurer immédiatement.

#### Afficher le contenu dans le terminal

```bash
# Voir le contenu d'un fichier dans un commit spécifique
git show a1b2c3d:fichier.txt

# Format : git show <commit>:<chemin/vers/fichier>
```

#### Exemple concret

```bash
# Afficher l'ancien contenu
git show HEAD~5:src/index.html

# Comparer avec la version actuelle
cat src/index.html           # version actuelle
git show HEAD~5:src/index.html  # version d'il y a 5 commits
```

#### Sauvegarder dans un nouveau fichier

```bash
# Exporter le contenu ancien dans un nouveau fichier
git show a1b2c3d:fichier.txt > fichier_ancien.txt

# Maintenant vous avez :
# - fichier.txt (version actuelle)
# - fichier_ancien.txt (version du commit a1b2c3d)
# Vous pouvez les comparer manuellement
```

---

### Trouver dans quel commit se trouvait une version spécifique

Souvent, vous ne savez pas exactement quel commit contient la version que vous cherchez. Voici comment la trouver.

#### Voir l'historique d'un fichier spécifique

```bash
# Afficher tous les commits qui ont modifié ce fichier
git log -- fichier.txt

# Version condensée
git log --oneline -- fichier.txt

# Avec les différences
git log -p -- fichier.txt
```

**Exemple de résultat :**

```bash
git log --oneline -- index.html
# d4e5f6g Ajout menu navigation
# c3d4e5f Correction responsive
# b2c3d4e Création page d'accueil
```

#### Rechercher un contenu spécifique dans l'historique

```bash
# Trouver dans quels commits apparaît une chaîne de caractères
git log -S "fonction_importante" -- fichier.js

# -S recherche dans les ajouts/suppressions
# Utile pour retrouver quand du code a été ajouté ou supprimé
```

**Cas pratique :**

```bash
# Vous avez supprimé une fonction et voulez la retrouver
git log -S "calculateTotal" -- calculator.js

# Résultat montre les commits où cette fonction a été modifiée
# Vous pouvez ensuite récupérer le bon commit
```

#### Rechercher par expression régulière

```bash
# Recherche avec regex (plus puissant)
git log -G "function.*calculate" -- fichier.js

# Recherche dans les messages de commit
git log --grep="bug fix" --oneline
```

---

### Cas pratiques détaillés

#### Scénario 1 : Récupérer un fichier supprimé

```bash
# Vous avez supprimé un fichier
rm important.txt
git add important.txt
git commit -m "Nettoyage fichiers"

# Plus tard, vous réalisez que ce fichier était important
# Trouver le dernier commit où il existait
git log --all --full-history -- important.txt

# Résultat :
# a1b2c3d Nettoyage fichiers (suppression)
# h7i8j9k Dernière modification du fichier

# Récupérer depuis le commit avant la suppression
git restore --source=h7i8j9k important.txt

# Ou plus simple : récupérer depuis HEAD~1 si c'était le dernier commit
git restore --source=HEAD~1 important.txt
```

#### Scénario 2 : Comparer plusieurs versions

```bash
# Vous voulez comparer 3 versions différentes d'un fichier
# Version actuelle
cat config.json

# Version d'il y a 2 commits
git show HEAD~2:config.json > config_v1.json

# Version d'il y a 5 commits
git show HEAD~5:config.json > config_v2.json

# Maintenant comparer avec un outil de diff
diff config.json config_v1.json
```

#### Scénario 3 : Récupérer une partie d'un ancien fichier

```bash
# Voir l'ancien contenu
git show HEAD~3:style.css

# Copier manuellement juste la partie qui vous intéresse
# Pas besoin de tout restaurer

# Ou sauvegarder pour référence
git show HEAD~3:style.css > old_style.css
# Extraire ce que vous voulez de old_style.css
```

#### Scénario 4 : Récupérer depuis une branche ou un tag

```bash
# Récupérer un fichier depuis une autre branche
git restore --source=develop fichier.txt

# Récupérer depuis un tag (version release)
git restore --source=v1.0.0 config.json

# Récupérer depuis une branche distante
git restore --source=origin/main index.html
```

#### Scénario 5 : Récupérer tout un dossier

```bash
# Récupérer tous les fichiers d'un dossier
git restore --source=HEAD~5 src/components/

# Récupérer de manière récursive
git restore --source=a1b2c3d docs/

# Attention : cela restaure TOUS les fichiers du dossier
```

---

### Voir les différences avant de récupérer

Avant de récupérer un fichier, il est sage de voir ce qui va changer.

#### Comparer version actuelle vs ancienne

```bash
# Voir les différences entre maintenant et le commit ancien
git diff HEAD a1b2c3d -- fichier.txt

# Format : git diff <nouveau> <ancien> -- <fichier>
```

#### Exemple de lecture de diff

```diff
git diff HEAD HEAD~3 -- index.html

diff --git a/index.html b/index.html
--- a/index.html
+++ b/index.html
@@ -1,5 +1,3 @@
 <html>
-  <head>
-    <title>Nouveau titre</title>
+  <title>Ancien titre</title>
   </head>
```

- Lignes `-` (rouges) : ce qui sera perdu (version actuelle)
- Lignes `+` (vertes) : ce qui sera ajouté (version ancienne)

---

### Récupération avec staging area

Par défaut, quand vous restaurez un fichier, il apparaît comme "modifié" dans votre Working Directory. Vous pouvez contrôler s'il va directement dans la staging area.

#### Restaurer seulement dans Working Directory (défaut)

```bash
# Le fichier est restauré mais pas ajouté
git restore --source=HEAD~3 fichier.txt

git status
# Changes not staged for commit:
#   modified: fichier.txt
```

#### Restaurer et ajouter à la staging area

```bash
# Restaurer ET mettre dans staging area
git restore --source=HEAD~3 --staged --worktree fichier.txt

git status
# Changes to be committed:
#   modified: fichier.txt
```

---

### Récupérer des fichiers dans un commit "detached HEAD"

Vous pouvez aussi naviguer vers un ancien commit et copier les fichiers que vous voulez.

```bash
# Se positionner sur un ancien commit
git checkout a1b2c3d

# Vous êtes maintenant en "detached HEAD"
# Vous pouvez voir les fichiers tels qu'ils étaient

# Copier les fichiers qui vous intéressent
cp fichier_important.txt /tmp/sauvegarde.txt

# Revenir à votre branche principale
git checkout main

# Restaurer le fichier sauvegardé
cp /tmp/sauvegarde.txt fichier_important.txt
```

**Note :** Cette méthode est moins élégante que `git restore` mais peut être utile pour des cas complexes.

---

### Rechercher du code supprimé

#### Trouver quand une ligne a été supprimée

```bash
# Rechercher dans l'historique une fonction supprimée
git log -S "maFonctionPerdue" --all

# Résultat montre les commits où cette fonction a été ajoutée/supprimée
```

#### Récupérer du code supprimé

```bash
# Une fois le commit trouvé (disons b2c3d4e)
# Voir ce qui a été supprimé
git show b2c3d4e

# Si vous voulez récupérer tout le fichier tel qu'il était avant
git restore --source=b2c3d4e^ fichier.js
# Le ^ signifie "le commit parent" (donc avant la suppression)
```

---

### Commandes utiles pour explorer l'historique

#### Visualiser l'historique graphique

```bash
# Historique avec graphe
git log --oneline --graph --all

# Avec plus de détails
git log --graph --all --decorate --oneline
```

#### Voir qui a modifié chaque ligne

```bash
# Afficher l'auteur et le commit de chaque ligne
git blame fichier.txt

# Résultat :
# a1b2c3d (John  2024-01-15 10:30:00 +0100  1) première ligne
# c3d4e5f (Alice 2024-01-20 14:20:00 +0100  2) deuxième ligne
```

#### Naviguer dans l'historique d'un fichier

```bash
# Voir toutes les versions d'un fichier
git log --follow -- fichier.txt

# --follow suit le fichier même s'il a été renommé
```

---

### Erreurs courantes et solutions

#### Erreur 1 : "pathspec did not match any file(s)"

```bash
git restore --source=a1b2c3d fichier_inexistant.txt
# error: pathspec 'fichier_inexistant.txt' did not match any file(s)

# Solutions :
# 1. Vérifier l'orthographe du fichier
# 2. Vérifier que le fichier existait dans ce commit
git ls-tree a1b2c3d  # Liste tous les fichiers du commit
```

#### Erreur 2 : Mauvais chemin de fichier

```bash
# Le fichier est dans un sous-dossier
git restore --source=HEAD~3 fichier.txt  # ❌ Erreur

# Correct : spécifier le chemin complet
git restore --source=HEAD~3 src/components/fichier.txt  # ✅
```

#### Erreur 3 : Confusion entre commit et branche

```bash
# "develop" est une branche, pas un commit
git restore --source=develop fichier.txt  # ✅ OK

# Si vous voulez le dernier commit de develop
git restore --source=develop fichier.txt  # C'est la même chose
```

---

### Bonnes pratiques

#### 1. Toujours vérifier avant de restaurer

```bash
# Voir ce que vous allez récupérer
git show a1b2c3d:fichier.txt

# Comparer avec la version actuelle
git diff HEAD a1b2c3d -- fichier.txt

# Puis restaurer
git restore --source=a1b2c3d fichier.txt
```

#### 2. Créer une copie de sécurité

```bash
# Avant de restaurer, faire une copie de la version actuelle
cp fichier.txt fichier_backup.txt

# Restaurer
git restore --source=HEAD~5 fichier.txt

# Si problème, vous pouvez revenir en arrière
cp fichier_backup.txt fichier.txt
```

#### 3. Tester après récupération

```bash
# Après avoir récupéré des fichiers
git restore --source=v1.0.0 config.json

# Tester que tout fonctionne
npm test
npm start

# Si OK, commiter
git add config.json
git commit -m "Restauration config depuis v1.0.0"
```

#### 4. Documenter dans le message de commit

```bash
# Bon message expliquant la récupération
git add fichier_restaure.txt
git commit -m "Restauration fichier_restaure.txt depuis commit a1b2c3d

Le fichier actuel était corrompu. Récupération de la dernière
version fonctionnelle connue (commit a1b2c3d du 2024-01-15)."
```

---

### Outils avancés pour l'exploration

#### `git log` avec options puissantes

```bash
# Afficher les modifications sur un fichier avec stats
git log --stat -- fichier.txt

# Afficher avec le diff complet
git log -p -- fichier.txt

# Depuis une date spécifique
git log --since="2024-01-01" -- fichier.txt

# Par auteur
git log --author="Alice" -- fichier.txt

# Derniers 5 commits sur un fichier
git log -5 -- fichier.txt
```

#### Recherche de contenu dans tout l'historique

```bash
# Rechercher "TODO" dans tous les commits
git log -S "TODO" --all

# Rechercher dans les messages de commit
git log --grep="bug" --grep="fix" --all-match

# Rechercher par date
git log --after="2024-01-01" --before="2024-02-01"
```

---

### Workflow complet de récupération

#### Exemple de workflow professionnel

```bash
# Étape 1 : Identifier le problème
# Un fichier a été modifié et ne fonctionne plus

# Étape 2 : Explorer l'historique
git log --oneline -10 -- fichier_problematique.js
# d4e5f6g Refactoring (actuel - ne marche pas)
# c3d4e5f Ajout feature X
# b2c3d4e Fix bug Y (dernière version qui marchait ?)

# Étape 3 : Examiner la version précédente
git show b2c3d4e:fichier_problematique.js

# Étape 4 : Comparer les versions
git diff b2c3d4e HEAD -- fichier_problematique.js

# Étape 5 : Décider de restaurer
git restore --source=b2c3d4e fichier_problematique.js

# Étape 6 : Tester
npm test

# Étape 7 : Si OK, commiter
git add fichier_problematique.js
git commit -m "Restauration fichier depuis b2c3d4e - version stable"

# Étape 8 : Analyser pourquoi la nouvelle version ne marchait pas
git diff b2c3d4e d4e5f6g -- fichier_problematique.js
# Comprendre l'erreur pour la corriger proprement plus tard
```

---

### Aide-mémoire des commandes

```bash
# Restaurer un fichier (méthode moderne)
git restore --source=<commit> <fichier>
git restore --source=HEAD~3 fichier.txt

# Restaurer un fichier (ancienne méthode)
git checkout <commit> -- <fichier>

# Voir le contenu sans restaurer
git show <commit>:<fichier>
git show HEAD~5:fichier.txt

# Sauvegarder dans un nouveau fichier
git show <commit>:<fichier> > nouveau_fichier.txt

# Historique d'un fichier
git log -- <fichier>
git log --oneline -- <fichier>
git log -p -- <fichier>           # Avec différences
git log --follow -- <fichier>     # Suit les renommages

# Rechercher dans l'historique
git log -S "texte" -- <fichier>   # Recherche contenu
git log -G "regex" -- <fichier>   # Recherche regex
git log --grep="message"          # Recherche message commit

# Voir qui a modifié quoi
git blame <fichier>

# Lister les fichiers d'un commit
git ls-tree <commit>
git show <commit> --name-only

# Comparer versions
git diff <commit1> <commit2> -- <fichier>
```

---

### Points clés à retenir

✅ `git restore --source` est la méthode moderne recommandée

✅ `git show commit:fichier` permet de voir sans restaurer

✅ `git log -- fichier` montre l'historique d'un fichier spécifique

✅ `git log -S "texte"` recherche du contenu dans l'historique

✅ Toujours comparer avant de restaurer (`git diff`)

✅ Documenter pourquoi vous restaurez dans le message de commit

✅ Tester après toute restauration

✅ Git conserve tout : rien n'est vraiment perdu (sauf ce qui n'a jamais été commité)

---

### Conclusion

La capacité de récupérer des fichiers d'anciennes versions est l'une des fonctionnalités les plus puissantes de Git. Que vous ayez supprimé du code par erreur, besoin de comparer différentes approches, ou que vous vouliez simplement explorer l'évolution de votre projet, Git conserve précieusement tout l'historique.

**Philosophie Git :**
> "Dans Git, rien n'est jamais vraiment perdu. Tout ce qui a été commité peut être retrouvé."

Cette assurance vous permet d'expérimenter librement, sachant que vous pouvez toujours revenir en arrière si nécessaire. C'est ce qui fait de Git un outil si précieux pour le développement logiciel.

Ce chapitre conclut le Module 3 sur la correction et la modification. Vous maîtrisez maintenant :
- Modifier le dernier commit (`git commit --amend`)
- Annuler des modifications (`git restore`)
- Comprendre et utiliser `git reset`
- Annuler des commits publics (`git revert`)
- Récupérer des fichiers d'anciennes versions

Dans le prochain module, nous explorerons le travail avec les branches, un concept fondamental pour la collaboration et l'organisation du développement.

⏭️ [Module 4 : Travailler avec les branches](/module-04-travailler-avec-les-branches/README.md)
