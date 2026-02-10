🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 2 : Concepts fondamentaux

## 4. Exploration de l'historique : git log, git show, git diff

### Introduction : Voyager dans le temps

L'un des plus grands avantages de Git est sa capacité à conserver un **historique complet** de votre projet. Chaque modification, chaque décision, chaque évolution est enregistrée et accessible à tout moment.

Dans cette section, nous allons découvrir trois commandes essentielles pour explorer cet historique :

- **git log** : Afficher la liste des commits
- **git show** : Examiner un commit en détail
- **git diff** : Comparer différentes versions

Ces commandes vous permettent de comprendre comment votre projet a évolué, qui a fait quoi, et pourquoi certaines décisions ont été prises.

---

## git log : Votre machine à remonter le temps

### Qu'est-ce que git log ?

`git log` affiche l'**historique des commits** de votre dépôt, du plus récent au plus ancien. C'est comme un journal de bord qui liste toutes les étapes de votre projet.

### Utilisation de base

```bash
git log
```

Résultat typique :

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0  
Author: Marie Dupont <marie@email.com>  
Date:   Wed Oct 15 14:30:00 2025 +0200

    Ajout de la fonctionnalité de recherche

commit d5e6f7g8h9i0j1k2l3m4n5o6p7q8r9s0t1u2v3w4  
Author: Jean Martin <jean@email.com>  
Date:   Wed Oct 15 10:15:00 2025 +0200

    Correction du bug de pagination

commit x5y6z7a8b9c0d1e2f3g4h5i6j7k8l9m0n1o2p3q4  
Author: Marie Dupont <marie@email.com>  
Date:   Tue Oct 14 16:45:00 2025 +0200

    Refactorisation du code CSS
```

**Explication** :
- Chaque commit est affiché avec son hash complet (40 caractères)
- L'auteur et son email
- La date et l'heure du commit
- Le message de commit

Pour **quitter** l'affichage de git log, tapez `q` (quit).

### Options essentielles de git log

#### --oneline : Format compact

```bash
git log --oneline
```

Résultat :

```
a1b2c3d Ajout de la fonctionnalité de recherche  
d5e6f7g Correction du bug de pagination  
x5y6z7a Refactorisation du code CSS  
e8f9g0h Mise à jour de la documentation
```

**Avantages** :
- Une ligne par commit
- Hash court (7 premiers caractères)
- Seulement le titre du message
- Beaucoup plus facile à lire pour avoir une vue d'ensemble

C'est probablement le format que vous utiliserez le plus souvent.

#### -n ou --max-count : Limiter le nombre de commits

```bash
# Afficher seulement les 5 derniers commits
git log -5

# Ou
git log -n 5

# Avec --oneline
git log --oneline -5
```

Résultat :

```
a1b2c3d Ajout de la fonctionnalité de recherche  
d5e6f7g Correction du bug de pagination  
x5y6z7a Refactorisation du code CSS  
e8f9g0h Mise à jour de la documentation  
i1j2k3l Ajout des tests unitaires
```

**Cas d'usage** : Quand vous voulez juste voir les commits récents sans scroller pendant des heures.

#### --since et --until : Filtrer par date

```bash
# Commits depuis une date
git log --since="2025-10-01"

# Commits depuis 2 semaines
git log --since="2 weeks ago"

# Commits depuis hier
git log --since="yesterday"

# Commits jusqu'à une date
git log --until="2025-10-15"

# Combiner les deux
git log --since="2025-10-01" --until="2025-10-15"

# Format relatif
git log --since="3 days ago"  
git log --since="1 month ago"
```

**Cas d'usage** : Voir ce qui a été fait sur une période spécifique (par exemple, pendant votre sprint de deux semaines).

#### --author : Filtrer par auteur

```bash
# Commits d'un auteur spécifique
git log --author="Marie"

# Fonctionne avec une partie du nom ou de l'email
git log --author="marie@email.com"

# Plusieurs auteurs (expression régulière)
git log --author="Marie\|Jean"
```

**Cas d'usage** : Voir tous vos propres commits ou ceux d'un collègue.

#### --grep : Rechercher dans les messages de commit

```bash
# Chercher un mot dans les messages
git log --grep="bug"

# Recherche insensible à la casse
git log --grep="BUG" -i

# Chercher plusieurs termes
git log --grep="bug" --grep="fix"

# Chercher avec une expression régulière
git log --grep="bug-[0-9]+"
```

**Cas d'usage** : Retrouver tous les commits liés à un bug ou une fonctionnalité spécifique.

#### --graph : Visualisation graphique

```bash
git log --graph --oneline
```

Résultat :

```
* a1b2c3d Ajout de la fonctionnalité de recherche
*   d5e6f7g Merge branch 'feature-x'
|\
| * x5y6z7a Développement de feature-x
| * e8f9g0h Tests pour feature-x
|/
* i1j2k3l Correction du bug de pagination
* m4n5o6p Refactorisation du code CSS
```

Les lignes et astérisques montrent visuellement comment les branches ont divergé et fusionné.

**Options complémentaires** :

```bash
# Graph avec plus de détails
git log --graph --oneline --all

# Graph avec décoration (branches, tags)
git log --graph --oneline --all --decorate
```

#### --all : Tous les commits de toutes les branches

Par défaut, `git log` affiche seulement l'historique de la branche actuelle.

```bash
# Voir tous les commits de toutes les branches
git log --oneline --all

# Souvent combiné avec --graph
git log --graph --oneline --all
```

#### --stat : Statistiques des modifications

```bash
git log --stat
```

Résultat :

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0  
Author: Marie Dupont <marie@email.com>  
Date:   Wed Oct 15 14:30:00 2025 +0200

    Ajout de la fonctionnalité de recherche

 src/search.js      | 45 +++++++++++++++++++++++++++++++++
 src/index.html     |  8 +++++-
 tests/search.test.js | 32 ++++++++++++++++++++++
 3 files changed, 84 insertions(+), 1 deletion(-)
```

Montre :
- Quels fichiers ont été modifiés
- Combien de lignes ajoutées/supprimées dans chaque fichier
- Résumé total

**Cas d'usage** : Comprendre l'ampleur des changements de chaque commit.

#### -p ou --patch : Voir les différences complètes

```bash
git log -p
```

Affiche le **diff complet** de chaque commit (toutes les lignes ajoutées et supprimées).

**Attention** : Peut être très long pour de gros commits. Utilisez plutôt avec `-n` :

```bash
# Voir le diff des 3 derniers commits
git log -p -3
```

### Combinaisons puissantes

Vous pouvez combiner plusieurs options :

```bash
# Commits d'une personne sur les 2 dernières semaines en format compact
git log --oneline --author="Marie" --since="2 weeks ago"

# Historique graphique complet des 10 derniers commits
git log --graph --oneline --all -10

# Commits liés aux bugs avec statistiques
git log --stat --grep="bug" -i

# Commits d'aujourd'hui par tous les auteurs
git log --since="today" --all
```

### Options de formatage personnalisé

#### --pretty=format : Format complètement personnalisé

```bash
# Format personnalisé
git log --pretty=format:"%h - %an, %ar : %s"
```

Résultat :

```
a1b2c3d - Marie Dupont, 2 hours ago : Ajout de la fonctionnalité de recherche  
d5e6f7g - Jean Martin, 5 hours ago : Correction du bug de pagination  
x5y6z7a - Marie Dupont, 1 day ago : Refactorisation du code CSS
```

**Placeholders utiles** :
- `%H` : Hash complet
- `%h` : Hash court
- `%an` : Nom de l'auteur
- `%ae` : Email de l'auteur
- `%ad` : Date de l'auteur
- `%ar` : Date relative (il y a X jours)
- `%s` : Sujet (titre du message)
- `%b` : Corps du message

**Exemple avancé** :

```bash
git log --pretty=format:"%C(yellow)%h%C(reset) - %C(green)%ar%C(reset) - %s %C(blue)(%an)%C(reset)"
```

Ajoute des couleurs pour une meilleure lisibilité.

#### --date : Format de date

```bash
# Format relatif (par défaut)
git log --date=relative

# Format ISO
git log --date=iso

# Format court
git log --date=short

# Format personnalisé
git log --date=format:"%Y-%m-%d %H:%M"
```

### Filtrer par fichier ou dossier

```bash
# Historique d'un fichier spécifique
git log fichier.txt

# Historique d'un dossier
git log src/

# Avec le diff pour ce fichier
git log -p fichier.txt

# Statistiques pour ce fichier
git log --stat fichier.txt
```

**Cas d'usage** : Comprendre l'évolution d'un fichier spécifique, qui l'a modifié et pourquoi.

### Rechercher dans le contenu des commits

```bash
# Trouver les commits qui ont ajouté ou supprimé un certain texte
git log -S "fonction_recherche"

# Avec expression régulière
git log -G "fonction_.*"
```

**Différence avec --grep** :
- `--grep` cherche dans les **messages** de commit
- `-S` cherche dans le **contenu** des fichiers modifiés

**Cas d'usage** : "Quand est-ce que cette fonction a été ajoutée ?"

---

## git show : Examiner un commit en détail

### Qu'est-ce que git show ?

`git show` affiche les **détails complets** d'un commit spécifique : métadonnées, message complet, et diff de toutes les modifications.

### Utilisation de base

#### Voir le dernier commit

```bash
git show
```

Résultat :

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0  
Author: Marie Dupont <marie@email.com>  
Date:   Wed Oct 15 14:30:00 2025 +0200

    Ajout de la fonctionnalité de recherche

    Implémentation d'une barre de recherche avec suggestions
    en temps réel. Utilise l'API de recherche du backend.

diff --git a/src/search.js b/src/search.js  
new file mode 100644  
index 0000000..e5f6a8b
--- /dev/null
+++ b/src/search.js
@@ -0,0 +1,45 @@
+function initSearch() {
+  // Code de la fonction
+}
...
```

#### Voir un commit spécifique

```bash
# Avec le hash complet
git show a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0

# Avec le hash court (7 premiers caractères suffisent)
git show a1b2c3d
```

### Références relatives

Git permet d'utiliser des références relatives pour désigner des commits :

```bash
# Le commit précédent
git show HEAD~1

# Le commit d'il y a 2 commits
git show HEAD~2

# Le commit d'il y a 3 commits
git show HEAD~3

# Autre notation équivalente
git show HEAD^   # Parent immédiat (= HEAD~1)  
git show HEAD^^  # Grand-parent (= HEAD~2)
```

**HEAD** représente le commit actuel (là où vous êtes).

**Analogie** : Imaginez l'historique comme une ligne de temps. `HEAD` est "maintenant", `HEAD~1` est "hier", `HEAD~2` est "avant-hier", etc.

### Voir seulement certaines informations

#### --stat : Statistiques sans le diff complet

```bash
git show --stat a1b2c3d
```

Résultat :

```
commit a1b2c3d...  
Author: Marie Dupont <marie@email.com>  
Date:   Wed Oct 15 14:30:00 2025 +0200

    Ajout de la fonctionnalité de recherche

 src/search.js      | 45 +++++++++++++++++++++
 src/index.html     |  8 +++-
 tests/search.test.js | 32 ++++++++++++++
 3 files changed, 84 insertions(+), 1 deletion(-)
```

#### --name-only : Seulement les noms de fichiers

```bash
git show --name-only a1b2c3d
```

Résultat :

```
commit a1b2c3d...  
Author: Marie Dupont <marie@email.com>
...

src/search.js  
src/index.html  
tests/search.test.js
```

#### --name-status : Noms et statut

```bash
git show --name-status a1b2c3d
```

Résultat :

```
commit a1b2c3d...
...

A	src/search.js  
M	src/index.html  
A	tests/search.test.js
```

Légende :
- `A` : Ajouté (Added)
- `M` : Modifié (Modified)
- `D` : Supprimé (Deleted)
- `R` : Renommé (Renamed)

### Voir un fichier à un commit spécifique

```bash
# Voir le contenu d'un fichier à un commit donné
git show a1b2c3d:src/index.html

# Voir un fichier à 3 commits en arrière
git show HEAD~3:README.md
```

**Cas d'usage** : "Comment était ce fichier il y a 5 commits ?"

### Comparer deux commits

Bien que `git diff` soit plus adapté (voir plus bas), vous pouvez utiliser :

```bash
# Comparer deux commits
git show a1b2c3d..d5e6f7g
```

---

## git diff : Comparer les versions

### Qu'est-ce que git diff ?

`git diff` montre les **différences** entre deux versions de vos fichiers. C'est l'outil essentiel pour voir exactement ce qui a changé.

### Les différents types de diff

Git a plusieurs "zones" entre lesquelles vous pouvez comparer :

```
Working Directory ←→ Staging Area ←→ Repository (commits)
```

### Utilisation de base

#### Voir les modifications non stagées

```bash
git diff
```

Compare le **Working Directory** avec la **Staging Area**.

Résultat :

```diff
diff --git a/README.md b/README.md  
index e5f6a8b..d1e2f3g 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,4 @@
 # Mon Projet

 Description du projet
+Nouvelle ligne ajoutée
```

**Interprétation** :
- `---` : Version ancienne (dans la Staging Area)
- `+++` : Version nouvelle (dans le Working Directory)
- Lignes avec `-` : Supprimées
- Lignes avec `+` : Ajoutées
- `@@ -1,3 +1,4 @@` : Numéros de lignes (ancien range → nouveau range)

#### Voir les modifications stagées

```bash
git diff --staged
```

Ou :

```bash
git diff --cached
```

Compare la **Staging Area** avec le **dernier commit**.

**Cas d'usage** : Vérifier exactement ce qui sera dans le prochain commit.

#### Voir toutes les modifications locales

```bash
git diff HEAD
```

Compare le **Working Directory** avec le **dernier commit** (staged + non staged).

### Comparer des commits

#### Comparer avec un commit spécifique

```bash
# Comparer le Working Directory avec un commit
git diff a1b2c3d

# Comparer deux commits
git diff a1b2c3d d5e6f7g

# Comparer avec le commit précédent
git diff HEAD~1
```

#### Comparer des branches

```bash
# Comparer la branche actuelle avec main
git diff main

# Comparer deux branches
git diff branche1 branche2

# Comparer avec l'origine distante
git diff origin/main
```

### Comparer un fichier spécifique

```bash
# Modifications non stagées d'un fichier
git diff fichier.txt

# Modifications stagées d'un fichier
git diff --staged fichier.txt

# Comparer un fichier entre deux commits
git diff a1b2c3d d5e6f7g fichier.txt

# Comparer un fichier avec sa version d'il y a 3 commits
git diff HEAD~3 fichier.txt
```

### Options utiles de git diff

#### --stat : Vue statistique

```bash
git diff --stat
```

Résultat :

```
 README.md     | 1 +
 src/index.js  | 15 +++++++++------
 src/style.css | 3 +--
 3 files changed, 11 insertions(+), 8 deletions(-)
```

Montre seulement les statistiques, pas les détails ligne par ligne.

#### --name-only : Seulement les noms

```bash
git diff --name-only
```

Résultat :

```
README.md  
src/index.js  
src/style.css
```

#### --name-status : Noms avec statut

```bash
git diff --name-status
```

Résultat :

```
M	README.md  
M	src/index.js  
M	src/style.css
```

#### --color-words : Différences mot par mot

```bash
git diff --color-words
```

Au lieu de montrer les lignes complètes, montre seulement les mots qui ont changé (colorés).

**Cas d'usage** : Quand vous avez fait de petites modifications dans de longues lignes.

#### --word-diff : Différences mot par mot en texte

```bash
git diff --word-diff
```

Similaire à `--color-words` mais fonctionne sans couleurs.

#### --ignore-all-space : Ignorer les espaces

```bash
git diff -w
```

Ignore les changements d'espaces blancs (utile si vous avez reformaté le code).

#### --unified : Contexte autour des changements

```bash
# Plus de contexte (10 lignes au lieu de 3)
git diff -U10

# Moins de contexte
git diff -U1
```

Par défaut, Git montre 3 lignes de contexte autour de chaque changement.

### Comparer des plages de commits

```bash
# Tous les commits depuis a1b2c3d jusqu'à d5e6f7g
git diff a1b2c3d..d5e6f7g

# Commits entre une branche et une autre
git diff main..feature-x

# Différence entre où la branche a divergé
git diff main...feature-x
```

**Différence entre .. et ...** :
- `..` : Différence entre deux points
- `...` : Différence depuis l'ancêtre commun

### Lire un diff

Comprendre la sortie de `git diff` est essentiel. Décortiquons un exemple :

```diff
diff --git a/src/index.js b/src/index.js  
index e5f6a8b..d1e2f3g 100644
--- a/src/index.js
+++ b/src/index.js
@@ -10,7 +10,8 @@ function init() {
   console.log('Initialisation');

-  setupOldFeature();
+  setupNewFeature();
+  enableExperimentalMode();

   render();
 }
```

**Explication** :
1. `diff --git a/src/index.js b/src/index.js` : Fichier comparé
2. `index e5f6a8b..d1e2f3g` : Hash des versions
3. `--- a/src/index.js` : Version ancienne (avant les changements)
4. `+++ b/src/index.js` : Version nouvelle (après les changements)
5. `@@ -10,7 +10,8 @@` : Changement commence à la ligne 10
   - Ancienne version : 7 lignes à partir de la ligne 10
   - Nouvelle version : 8 lignes à partir de la ligne 10
6. Lignes sans préfixe : Contexte (inchangées)
7. `-  setupOldFeature();` : Ligne supprimée (en rouge)
8. `+  setupNewFeature();` : Ligne ajoutée (en vert)
9. `+  enableExperimentalMode();` : Ligne ajoutée (en vert)

---

## Cas pratiques d'utilisation

### Cas 1 : Voir ce que j'ai changé depuis hier

```bash
git log --oneline --since="yesterday"
```

### Cas 2 : Comprendre un bug - qui a modifié cette ligne ?

```bash
# Historique d'un fichier
git log -p fichier_problematique.js

# Ou avec git blame (nous verrons ça plus tard)
git blame fichier_problematique.js
```

### Cas 3 : Vérifier avant de commiter

```bash
# Voir ce qui va être commité
git diff --staged

# Si tout est bon
git commit -m "Message"
```

### Cas 4 : Retrouver quand une fonction a été supprimée

```bash
# Chercher "nomFonction" dans l'historique
git log -S "nomFonction"

# Voir les détails du commit où elle a été supprimée
git show <hash_du_commit>
```

### Cas 5 : Comparer avec la version de production

```bash
# Voir les différences entre votre branche et main
git diff main

# Voir seulement les fichiers modifiés
git diff --name-only main
```

### Cas 6 : Voir l'évolution d'un fichier

```bash
# Historique complet avec les différences
git log -p fichier.txt

# Version compacte avec statistiques
git log --stat fichier.txt
```

### Cas 7 : "Qu'est-ce qui a changé cette semaine ?"

```bash
# Liste des commits
git log --oneline --since="1 week ago"

# Avec statistiques
git log --stat --since="1 week ago"

# Par auteur
git log --oneline --since="1 week ago" --author="Marie"
```

---

## Alias utiles à créer

Pour faciliter votre travail, créez des alias pour les commandes que vous utilisez souvent :

```bash
# Historique compact et graphique
git config --global alias.lg "log --graph --oneline --all --decorate"

# Historique avec statistiques
git config --global alias.ls "log --stat"

# Dernier commit
git config --global alias.last "log -1 HEAD"

# Commits d'aujourd'hui
git config --global alias.today "log --since=midnight --oneline"

# Historique compact des 10 derniers commits
git config --global alias.recent "log --oneline -10"
```

Ensuite, vous pouvez utiliser :

```bash
git lg  
git ls  
git last  
git today  
git recent
```

---

## Outils graphiques pour visualiser l'historique

Bien que la ligne de commande soit puissante, les outils graphiques peuvent être utiles :

### Outils intégrés

```bash
# Outil graphique Git (simple mais efficace)
gitk

# Ou
git gui
```

### Outils externes populaires

- **GitKraken** : Interface moderne et intuitive
- **SourceTree** : Gratuit, très complet
- **GitHub Desktop** : Simple pour les débutants
- **Git Graph** (extension VS Code) : Directement dans votre éditeur
- **Fork** : Rapide et élégant (payant)

### Dans votre éditeur

La plupart des éditeurs modernes ont des extensions Git :
- **VS Code** : GitLens, Git Graph
- **IntelliJ/PyCharm** : Git intégré
- **Sublime Text** : GitGutter
- **Atom** : Git-Plus

---

## Bonnes pratiques

### 1. Utilisez git log régulièrement

Consultez l'historique fréquemment pour :
- Comprendre l'évolution du projet
- Voir ce que font vos collègues
- Retrouver quand un bug a été introduit

### 2. Maîtrisez les formats compacts

`git log --oneline` est votre ami pour une vue d'ensemble rapide.

### 3. Vérifiez avec git diff avant de commiter

```bash
git diff --staged
```

C'est votre dernière chance de vérifier avant d'enregistrer !

### 4. Utilisez git show pour explorer

Quand vous voyez un commit intéressant dans `git log`, utilisez `git show` pour en savoir plus.

### 5. Créez des alias pour vos commandes fréquentes

Gagnez du temps avec des raccourcis personnalisés.

### 6. Combinez les options

Les commandes Git sont conçues pour être combinées :

```bash
git log --graph --oneline --author="Marie" --since="1 week ago"
```

### 7. N'ayez pas peur d'expérimenter

Ces commandes sont "en lecture seule" : elles ne modifient rien. Explorez librement !

---

## Commandes récapitulatives

### git log

```bash
git log                              # Historique complet  
git log --oneline                    # Format compact  
git log -n 5                         # 5 derniers commits  
git log --since="2 weeks ago"        # Par date  
git log --author="Marie"             # Par auteur  
git log --grep="bug"                 # Recherche dans messages  
git log --graph --all                # Vue graphique  
git log --stat                       # Avec statistiques  
git log -p                           # Avec diff complet  
git log fichier.txt                  # Historique d'un fichier  
git log -S "texte"                   # Recherche dans contenu
```

### git show

```bash
git show                             # Dernier commit  
git show a1b2c3d                     # Commit spécifique  
git show HEAD~2                      # Commit d'il y a 2 commits  
git show --stat a1b2c3d              # Avec statistiques  
git show --name-only a1b2c3d         # Seulement noms fichiers  
git show a1b2c3d:fichier.txt         # Fichier à un commit donné
```

### git diff

```bash
git diff                             # Modifications non stagées  
git diff --staged                    # Modifications stagées  
git diff HEAD                        # Toutes modifications locales  
git diff a1b2c3d                     # Avec un commit  
git diff a1b2c3d d5e6f7g              # Entre deux commits  
git diff main                        # Avec une branche  
git diff --stat                      # Vue statistique  
git diff --name-only                 # Seulement noms fichiers  
git diff -w                          # Ignorer espaces  
git diff fichier.txt                 # Un fichier spécifique
```

---

## En résumé

L'exploration de l'historique Git repose sur trois commandes complémentaires :

1. **git log** : Naviguer dans l'historique
   - Vue d'ensemble des commits
   - Filtrage puissant (date, auteur, message, contenu)
   - Formats variés pour différents besoins

2. **git show** : Examiner en détail
   - Informations complètes sur un commit
   - Voir l'état d'un fichier à un moment donné
   - Comprendre ce qui a été fait

3. **git diff** : Comparer les versions
   - Voir exactement ce qui a changé
   - Vérifier avant de commiter
   - Comparer entre commits, branches, ou états

**Workflow recommandé** :
```
1. git log --oneline        → Vue d'ensemble
2. git show <commit>        → Détails d'un commit intéressant
3. git diff                 → Vérifier vos modifications actuelles
```

Avec ces trois commandes maîtrisées, vous avez un contrôle total sur l'historique de votre projet. Vous pouvez comprendre son évolution, retrouver des informations, et prendre des décisions éclairées.

---

*Dans la section suivante, nous visualiserons l'historique sous forme de graphe et comprendrons mieux la notion de graphe dans Git.*

⏭️ [Visualiser l'historique : git log --graph et la notion de graphe](/module-02-concepts-fondamentaux/05-visualiser-historique.md)
