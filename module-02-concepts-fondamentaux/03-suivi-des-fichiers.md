🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 2 : Concepts fondamentaux

## 3. Suivi des fichiers : git status, git add, git commit

### Introduction : Le trio fondamental

Vous avez maintenant compris l'architecture interne de Git et les trois états dans lesquels peuvent se trouver vos fichiers. Il est temps d'approfondir les trois commandes que vous utiliserez le plus souvent dans votre travail quotidien :

- **git status** : pour savoir où vous en êtes
- **git add** : pour préparer vos modifications
- **git commit** : pour enregistrer vos modifications dans l'historique

Ces trois commandes forment le **cœur du workflow Git**. Les maîtriser vous permettra d'être productif et confiant dans votre utilisation de Git.

---

## git status : Votre boussole dans Git

### Qu'est-ce que git status ?

`git status` est probablement la commande que vous utiliserez le plus souvent. Elle vous montre l'**état actuel** de votre dépôt :

- Sur quelle branche vous êtes
- Quels fichiers ont été modifiés
- Quels fichiers sont prêts à être commités (staged)
- Quels fichiers ne sont pas suivis
- Si vous êtes à jour avec le dépôt distant

**Conseil** : Prenez l'habitude d'utiliser `git status` constamment, avant et après chaque commande. C'est votre meilleur ami pour comprendre ce qui se passe.

### Utilisation de base

```bash
git status
```

C'est aussi simple que ça ! Cette commande n'a aucun effet sur votre dépôt, elle ne fait qu'afficher des informations.

### Interpréter la sortie de git status

Voyons différents scénarios et comment git status les affiche :

#### Scénario 1 : Dépôt propre

```bash
git status
```

Résultat :
```
On branch main
nothing to commit, working tree clean
```

**Signification** :
- Vous êtes sur la branche `main`
- Aucune modification en attente
- Tout est à jour et commité

C'est l'état idéal après un commit !

#### Scénario 2 : Nouveaux fichiers non suivis

```bash
# Créer un nouveau fichier
echo "Nouveau contenu" > nouveau.txt

git status
```

Résultat :
```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	nouveau.txt

nothing added to commit but untracked files present (use "git add" to track)
```

**Signification** :
- Un fichier `nouveau.txt` existe mais Git ne le suit pas encore
- Git vous suggère d'utiliser `git add` pour commencer à le suivre
- Ce fichier est en rouge (si les couleurs sont activées)

#### Scénario 3 : Fichiers modifiés

```bash
# Modifier un fichier existant
echo "Modification" >> README.md

git status
```

Résultat :
```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

**Signification** :
- `README.md` a été modifié depuis le dernier commit
- Ces modifications ne sont pas encore dans la Staging Area
- Git vous propose deux options : `git add` pour préparer ou `git restore` pour annuler

#### Scénario 4 : Fichiers dans la Staging Area

```bash
# Ajouter le fichier à la Staging Area
git add README.md

git status
```

Résultat :
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md
```

**Signification** :
- `README.md` est dans la Staging Area
- Il est prêt à être commité
- Le fichier est en vert
- Vous pouvez encore l'unstage avec `git restore --staged`

#### Scénario 5 : Fichier modifié et staged, puis modifié à nouveau

```bash
# Modifier, ajouter
echo "Première modification" >> fichier.txt
git add fichier.txt

# Modifier à nouveau
echo "Deuxième modification" >> fichier.txt

git status
```

Résultat :
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   fichier.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   fichier.txt
```

**Signification** :
- Le fichier apparaît deux fois !
- Une version est staged (première modification)
- Une version est modifiée mais pas staged (deuxième modification)
- Si vous commitez maintenant, seule la première modification sera enregistrée

**Solution** : Refaites `git add fichier.txt` pour inclure la deuxième modification.

#### Scénario 6 : Fichier supprimé

```bash
# Supprimer un fichier
rm ancien.txt

git status
```

Résultat :
```
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    ancien.txt
```

**Signification** :
- Git a détecté la suppression
- Vous devez utiliser `git add ancien.txt` ou `git rm ancien.txt` pour enregistrer cette suppression

### Options utiles de git status

#### Version courte : -s ou --short

Pour un affichage plus compact :

```bash
git status -s
```

Résultat :
```
M  fichier_staged.txt
 M fichier_modifié.txt
MM fichier_modifié_deux_fois.txt
A  nouveau_staged.txt
?? fichier_non_suivi.txt
 D fichier_supprimé.txt
```

**Légende des codes** :
- `M ` (M + espace) : Modifié et staged
- ` M` (espace + M) : Modifié mais pas staged
- `MM` : Modifié, staged, puis modifié à nouveau
- `A ` : Nouveau fichier ajouté (Added)
- `??` : Non suivi (Untracked)
- ` D` : Supprimé (Deleted)
- `R ` : Renommé (Renamed)

La colonne de gauche indique l'état dans la Staging Area, celle de droite l'état dans le Working Directory.

#### Afficher les branches : -b ou --branch

```bash
git status -sb
```

Résultat :
```
## main...origin/main [ahead 2]
M  README.md
```

Cela ajoute des informations sur la branche et son avance/retard par rapport au dépôt distant.

#### Ignorer les fichiers non suivis : -uno

Si vous avez beaucoup de fichiers non suivis qui encombrent l'affichage :

```bash
git status -uno
```

Cela masque les fichiers untracked.

---

## git add : Préparer vos modifications

### Qu'est-ce que git add ?

`git add` est la commande qui déplace les fichiers du Working Directory vers la Staging Area. C'est la première étape avant de créer un commit.

**Concept clé** : `git add` ne sauvegarde pas dans l'historique, il **prépare** seulement ce qui sera dans le prochain commit.

### Utilisation de base

#### Ajouter un fichier spécifique

```bash
git add fichier.txt
```

C'est la forme la plus simple et la plus sûre.

#### Ajouter plusieurs fichiers

```bash
git add fichier1.txt fichier2.txt fichier3.txt
```

#### Ajouter tous les fichiers du répertoire courant

```bash
git add .
```

**Attention** : Le point `.` signifie "répertoire courant". Si vous êtes dans un sous-dossier, seuls les fichiers de ce sous-dossier seront ajoutés.

Pour ajouter tous les fichiers depuis la racine du projet, placez-vous à la racine ou utilisez :

```bash
git add --all
```

#### Ajouter tous les fichiers d'un certain type

```bash
# Tous les fichiers JavaScript
git add *.js

# Tous les fichiers dans un dossier
git add src/

# Tous les fichiers CSS récursivement
git add **/*.css
```

### Les différentes variantes de git add

#### git add -A ou --all

```bash
git add -A
```

Ajoute **tous les changements** du dépôt :
- Nouveaux fichiers
- Fichiers modifiés
- Fichiers supprimés

C'est la forme la plus complète.

#### git add -u ou --update

```bash
git add -u
```

Ajoute **seulement les fichiers déjà suivis** qui ont été modifiés ou supprimés. N'ajoute pas les nouveaux fichiers.

**Cas d'usage** : Quand vous avez créé des fichiers temporaires que vous ne voulez pas ajouter, mais que vous voulez ajouter toutes les modifications des fichiers existants.

#### git add -p ou --patch

```bash
git add -p fichier.txt
```

Mode **interactif** qui vous permet de choisir quelles parties d'un fichier ajouter. Git vous montre chaque modification et vous demande :

```
Stage this hunk [y,n,q,a,d,s,e,?]?
```

Options :
- `y` : Oui, stage ce morceau
- `n` : Non, ne le stage pas
- `q` : Quitter
- `a` : Stage ce morceau et tous les suivants dans ce fichier
- `d` : Ne stage pas ce morceau ni les suivants
- `s` : Diviser ce morceau en morceaux plus petits
- `e` : Éditer manuellement ce morceau
- `?` : Aide

**Cas d'usage** : Quand vous avez fait plusieurs modifications dans un fichier mais voulez les commiter séparément.

#### git add -n ou --dry-run

```bash
git add -n .
```

Mode "simulation" : affiche ce qui serait ajouté **sans réellement l'ajouter**. Très utile pour vérifier avant d'exécuter.

#### git add -v ou --verbose

```bash
git add -v fichier.txt
```

Mode verbeux : affiche les fichiers qui sont ajoutés.

### Ajouter des fichiers supprimés

Quand vous supprimez un fichier du système de fichiers, Git le détecte mais ne l'enregistre pas automatiquement.

```bash
# Supprimer physiquement un fichier
rm fichier.txt

# Git a détecté la suppression
git status
# deleted: fichier.txt

# Méthode 1 : utiliser git add (oui, ça marche !)
git add fichier.txt

# Méthode 2 : utiliser git rm
git rm fichier.txt

# Les deux font la même chose dans ce cas
```

**Différence** :
- `git rm` supprime le fichier ET le stage
- `rm` + `git add` fait la même chose en deux étapes

### Renommer des fichiers

```bash
# Mauvaise méthode : renommer manuellement
mv ancien_nom.txt nouveau_nom.txt
git add ancien_nom.txt nouveau_nom.txt

# Bonne méthode : utiliser git mv
git mv ancien_nom.txt nouveau_nom.txt
```

`git mv` fait tout en une seule commande : renommer ET stage le changement.

### Annuler un git add (unstaging)

Si vous avez ajouté un fichier par erreur :

#### Avec Git 2.23+

```bash
git restore --staged fichier.txt
```

#### Ancienne syntaxe (toujours valide)

```bash
git reset HEAD fichier.txt
```

#### Pour tout unstager

```bash
git restore --staged .
```

ou

```bash
git reset HEAD
```

**Important** : Ces commandes ne suppriment **pas** vos modifications, elles retirent simplement les fichiers de la Staging Area.

---

## git commit : Enregistrer dans l'historique

### Qu'est-ce que git commit ?

`git commit` crée un **instantané permanent** de tous les fichiers qui sont dans la Staging Area. C'est la commande qui enregistre réellement vos modifications dans l'historique de Git.

Un commit contient :
- L'état complet de votre projet à ce moment
- Un message décrivant les changements
- L'auteur et la date
- Un lien vers le(s) commit(s) parent(s)

### Utilisation de base

```bash
git commit -m "Message de commit"
```

Le flag `-m` (message) vous permet de spécifier le message directement en ligne de commande.

**Exemple** :
```bash
git commit -m "Ajout de la page de contact"
```

### Écrire un bon message de commit

Un message de commit doit être :

#### 1. Concis mais descriptif

**Bon** :
```bash
git commit -m "Correction du bug de validation du formulaire"
```

**Mauvais** :
```bash
git commit -m "Fix"
git commit -m "Modifications"
git commit -m "Update"
```

#### 2. À l'impératif (ou infinitif)

Comme si vous donniez un ordre au code.

**Bon** :
```bash
git commit -m "Ajouter la fonctionnalité de recherche"
git commit -m "Corriger l'erreur 404 sur la page produits"
git commit -m "Refactoriser la fonction de calcul"
```

**Acceptable (mais moins conventionnel)** :
```bash
git commit -m "Ajout de la fonctionnalité de recherche"
git commit -m "Correction de l'erreur 404"
```

#### 3. Spécifique au changement

Expliquez **quoi** et **pourquoi**, pas **comment**.

**Bon** :
```bash
git commit -m "Augmenter le timeout de l'API à 30s pour éviter les erreurs réseau"
```

**Moins bon** :
```bash
git commit -m "Changer un nombre dans config.js"
```

### Messages de commit longs

Pour des messages plus détaillés, utilisez `git commit` sans `-m` :

```bash
git commit
```

Cela ouvre votre éditeur de texte configuré. Vous pouvez alors écrire :

```
Titre du commit (50 caractères max)

Description détaillée du commit si nécessaire.
Vous pouvez écrire sur plusieurs lignes.

- Vous pouvez utiliser des listes
- Pour expliquer plusieurs points

Le titre et le corps sont séparés par une ligne vide.
Le corps peut contenir plusieurs paragraphes.
```

**Structure conventionnelle** :
- Ligne 1 : Titre court (50 caractères max)
- Ligne 2 : Ligne vide
- Lignes suivantes : Description détaillée (72 caractères par ligne max)

### Options utiles de git commit

#### -a ou --all : Add et commit en une seule commande

```bash
git commit -am "Message"
```

**Équivaut à** :
```bash
git add -u
git commit -m "Message"
```

**Important** :
- Ne fonctionne que pour les fichiers **déjà suivis**
- N'ajoute pas les nouveaux fichiers
- Pratique pour les petites modifications rapides

#### --amend : Modifier le dernier commit

Si vous venez de faire un commit et que vous réalisez :
- Vous avez oublié un fichier
- Le message est incorrect
- Vous voulez ajouter une petite modification

```bash
# Oublié d'ajouter un fichier
git add fichier_oublie.txt
git commit --amend --no-edit

# Modifier le message du dernier commit
git commit --amend -m "Nouveau message"

# Modifier le message dans l'éditeur
git commit --amend
```

**Attention** : N'utilisez `--amend` que si vous n'avez **pas encore partagé** le commit (pas encore fait de push). Modifier un commit publié peut créer des problèmes pour les autres.

#### -v ou --verbose : Voir les modifications dans l'éditeur

```bash
git commit -v
```

Quand l'éditeur s'ouvre, vous verrez non seulement le template de message mais aussi le diff complet de ce qui va être commité. Utile pour vérifier avant de finaliser.

#### --allow-empty : Créer un commit vide

```bash
git commit --allow-empty -m "Trigger CI/CD"
```

Crée un commit sans aucune modification. Rarement utilisé, sauf pour déclencher des systèmes automatisés.

#### --date : Spécifier une date personnalisée

```bash
git commit --date="2025-01-15 14:30:00" -m "Message"
```

Permet de définir manuellement la date du commit.

### Annuler un commit (avant push)

Si vous venez de faire un commit mais regrettez :

#### Garder les modifications (unstage)

```bash
git reset --soft HEAD~1
```

Le commit est annulé mais les modifications restent dans la Staging Area.

#### Garder les modifications (working directory)

```bash
git reset --mixed HEAD~1
```

ou simplement :

```bash
git reset HEAD~1
```

Le commit est annulé et les modifications retournent dans le Working Directory (non staged).

#### Supprimer complètement les modifications

```bash
git reset --hard HEAD~1
```

**Danger** : Le commit et toutes ses modifications sont définitivement supprimés !

---

## Le workflow complet : status → add → commit

Voici le cycle typique que vous répéterez sans cesse :

### Étape par étape

#### 1. Vérifier l'état de départ

```bash
git status
```

```
On branch main
nothing to commit, working tree clean
```

#### 2. Faire des modifications

Vous travaillez normalement : créez, modifiez, supprimez des fichiers dans votre éditeur.

```bash
# Par exemple
echo "Nouveau contenu" > fichier1.txt
echo "Modification" >> fichier2.txt
```

#### 3. Vérifier ce qui a changé

```bash
git status
```

```
On branch main
Changes not staged for commit:
	modified:   fichier2.txt

Untracked files:
	fichier1.txt
```

#### 4. Voir les différences exactes

```bash
git diff
```

Affiche ligne par ligne ce qui a changé dans les fichiers modifiés.

Pour voir aussi les nouveaux fichiers :

```bash
git diff HEAD
```

#### 5. Ajouter à la Staging Area

```bash
# Option 1 : ajouter fichier par fichier
git add fichier1.txt
git add fichier2.txt

# Option 2 : tout ajouter
git add .
```

#### 6. Vérifier ce qui est staged

```bash
git status
```

```
On branch main
Changes to be committed:
	new file:   fichier1.txt
	modified:   fichier2.txt
```

Vous pouvez aussi utiliser :

```bash
git diff --staged
```

Pour voir exactement ce qui va être commité.

#### 7. Commiter

```bash
git commit -m "Ajout de fichier1 et modification de fichier2"
```

```
[main a1b2c3d] Ajout de fichier1 et modification de fichier2
 2 files changed, 5 insertions(+), 1 deletion(-)
 create mode 100644 fichier1.txt
```

#### 8. Vérifier que tout est propre

```bash
git status
```

```
On branch main
nothing to commit, working tree clean
```

#### 9. Voir l'historique

```bash
git log --oneline
```

```
a1b2c3d Ajout de fichier1 et modification de fichier2
e4f5g6h Commit précédent
...
```

### Schéma récapitulatif

```
1. git status                  → Voir l'état
2. [Modifier des fichiers]     → Travailler
3. git diff                    → Voir les changements
4. git add <fichiers>          → Préparer (Stage)
5. git status                  → Vérifier ce qui est staged
6. git diff --staged           → Vérifier les changements staged
7. git commit -m "Message"     → Enregistrer (Commit)
8. git status                  → Confirmer que c'est propre
9. git log                     → Voir l'historique

Répéter...
```

---

## Bonnes pratiques

### 1. Commiter souvent

**Principe** : Faites des commits petits et fréquents plutôt que de gros commits rares.

**Avantages** :
- Historique plus clair
- Plus facile de revenir en arrière
- Moins de conflits en équipe
- Progression traçable

**Guideline** : Commitez à chaque étape logique (une fonctionnalité, un bug fixé, une amélioration).

### 2. Commits atomiques

Chaque commit devrait représenter **une seule chose**.

**Bon** : Trois commits séparés
```bash
git commit -m "Ajout de la validation côté client"
git commit -m "Ajout de la validation côté serveur"
git commit -m "Ajout des tests de validation"
```

**Moins bon** : Un seul gros commit
```bash
git commit -m "Ajout de toute la validation et des tests"
```

### 3. Toujours vérifier avec git status

Avant chaque `git add` ou `git commit`, utilisez `git status` pour être sûr de ce que vous faites.

```bash
# Toujours faire
git status      # Voir ce qui a changé
git add ...     # Ajouter
git status      # Vérifier ce qui est staged
git commit ...  # Commiter
git status      # Confirmer que c'est propre
```

### 4. Utiliser git diff avant de commiter

```bash
# Voir ce qui va être commité
git diff --staged
```

Cela vous évite de commiter des choses inattendues (fichiers de debug, console.log, TODO temporaires, etc.).

### 5. Éviter git commit -am aveuglément

`git commit -am` est pratique mais dangereux :
- Vous ne voyez pas ce que vous commitez
- Vous risquez d'inclure des modifications indésirables

**Préférez** :
```bash
git add .
git status
git diff --staged
git commit -m "Message"
```

### 6. Messages de commit significatifs

Prenez 10 secondes pour écrire un message clair. Votre futur vous (et vos collègues) vous remercieront.

**Bon** : On comprend ce qui a été fait
```bash
git commit -m "Correction de l'erreur de division par zéro dans le calcul de prix"
```

**Mauvais** : Aucune information utile
```bash
git commit -m "fix"
```

### 7. Ne pas commiter des fichiers sensibles

Avant de commiter, vérifiez que vous n'incluez pas :
- Mots de passe
- Clés API
- Fichiers de configuration personnels
- Fichiers temporaires

Utilisez `.gitignore` pour éviter ça (nous verrons cela dans une prochaine section).

### 8. Un commit = un état fonctionnel

Idéalement, chaque commit devrait laisser le projet dans un **état fonctionnel**.

**Mauvais** : Commit qui casse le code
```bash
git commit -m "WIP: refactoring en cours"
```

Si vous devez absolument commiter du travail incomplet, indiquez-le clairement dans le message : `WIP:` (Work In Progress).

---

## Cas pratiques courants

### Cas 1 : J'ai modifié plusieurs fichiers mais veux les commiter séparément

```bash
# Situation : fichierA.js, fichierB.js et fichierC.js modifiés

# Commit 1
git add fichierA.js
git commit -m "Refactorisation de la fonction A"

# Commit 2
git add fichierB.js
git commit -m "Ajout de la fonction B"

# Commit 3
git add fichierC.js
git commit -m "Correction de bug dans C"
```

### Cas 2 : J'ai commité mais oublié un fichier

```bash
# Commit initial
git commit -m "Ajout de la page de contact"

# Oups, j'ai oublié contact.css
git add contact.css
git commit --amend --no-edit

# Le commit est modifié pour inclure contact.css
```

### Cas 3 : Je veux annuler mes modifications locales

```bash
# J'ai modifié fichier.txt mais veux revenir en arrière
git restore fichier.txt

# Ou pour tout annuler
git restore .
```

**Attention** : Modifications perdues définitivement !

### Cas 4 : J'ai ajouté un fichier par erreur dans la Staging Area

```bash
# Ajouté par erreur
git add fichier_temporaire.txt

# Retirer de la Staging Area
git restore --staged fichier_temporaire.txt

# Le fichier existe toujours mais n'est plus staged
```

### Cas 5 : Je veux voir exactement ce qui a changé dans un fichier

```bash
# Voir les changements non staged
git diff fichier.txt

# Voir les changements staged
git diff --staged fichier.txt

# Voir tous les changements (staged + non staged)
git diff HEAD fichier.txt
```

### Cas 6 : Message de commit trop vague, je veux le changer

```bash
# Dernier commit avec message vague
git commit -m "Update"

# Changer le message
git commit --amend -m "Mise à jour de la configuration du serveur pour supporter HTTPS"
```

---

## Commandes de vérification

Voici les commandes essentielles pour vérifier votre travail à chaque étape :

| Commande | Usage |
|----------|-------|
| `git status` | État global du dépôt |
| `git status -s` | État en format court |
| `git diff` | Changements non staged |
| `git diff --staged` | Changements staged |
| `git diff HEAD` | Tous les changements |
| `git log` | Historique des commits |
| `git log --oneline` | Historique compact |
| `git show` | Détails du dernier commit |
| `git show <hash>` | Détails d'un commit spécifique |

---

## Récapitulatif : Les commandes essentielles

### git status

```bash
git status           # Affichage complet
git status -s        # Affichage court
git status -sb       # Court + info branche
git status -uno      # Sans fichiers untracked
```

### git add

```bash
git add fichier.txt         # Un fichier
git add .                   # Tous les fichiers du dossier courant
git add -A                  # Tous les changements du dépôt
git add -u                  # Seulement fichiers déjà suivis
git add -p                  # Mode interactif
git add *.js                # Tous les .js
```

### git commit

```bash
git commit -m "Message"              # Commit avec message
git commit -am "Message"             # Add + commit (fichiers suivis)
git commit --amend                   # Modifier dernier commit
git commit --amend --no-edit         # Amend sans changer message
git commit -v                        # Voir diff dans l'éditeur
```

### Annulation

```bash
git restore fichier.txt              # Annuler modifications locales
git restore --staged fichier.txt     # Unstage un fichier
git reset HEAD~1                     # Annuler dernier commit (garder modifs)
git reset --hard HEAD~1              # Annuler dernier commit (tout supprimer)
```

---

## En résumé

Le trio `git status`, `git add`, `git commit` forme le cœur de votre workflow Git quotidien :

1. **git status** : Votre boussole constante
   - Utilisez-la avant et après chaque commande
   - Vous dit exactement où vous en êtes

2. **git add** : Préparez vos commits
   - Sélectionnez précisément ce qui sera commité
   - Construisez des commits atomiques et propres

3. **git commit** : Enregistrez dans l'historique
   - Créez des instantanés permanents
   - Écrivez des messages clairs et descriptifs

**Workflow à retenir** :
```
Modifier → status → add → status → commit → status
```

Avec ces trois commandes maîtrisées, vous avez les fondations solides pour utiliser Git efficacement. La pratique régulière de ce workflow le rendra naturel et automatique.

---

*Dans la section suivante, nous explorerons en détail l'historique avec git log, git show et git diff pour comprendre l'évolution de votre projet.*

⏭️ [Exploration de l'historique : git log, git show, git diff](/module-02-concepts-fondamentaux/04-exploration-de-historique.md)
