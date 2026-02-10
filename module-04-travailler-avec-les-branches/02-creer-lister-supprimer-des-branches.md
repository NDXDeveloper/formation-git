🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 2. Créer, lister, supprimer des branches

### Introduction

Maintenant que vous comprenez ce qu'est une branche, il est temps d'apprendre à les manipuler concrètement. Cette section couvre toutes les opérations de base pour gérer vos branches : création, consultation, et suppression.

**Analogie :** Imaginez les branches comme des onglets dans votre navigateur. Vous pouvez ouvrir de nouveaux onglets (créer des branches), voir tous vos onglets ouverts (lister les branches), et fermer ceux dont vous n'avez plus besoin (supprimer des branches).

---

### Créer une branche

Il existe plusieurs méthodes pour créer une branche dans Git. Toutes font la même chose : créer un nouveau pointeur vers un commit.

#### Méthode 1 : `git branch` (créer sans changer)

La commande `git branch` crée une nouvelle branche mais **ne vous y déplace pas**.

**Syntaxe :**

```bash
git branch nom-de-la-branche
```

**Exemple :**

```bash
# Voir où vous êtes
git branch
# * main

# Créer une nouvelle branche
git branch feature-contact

# Vérifier qu'elle existe
git branch
# * main
#   feature-contact
```

L'astérisque `*` indique que vous êtes toujours sur `main`. La branche `feature-contact` a été créée mais vous n'y êtes pas encore.

**État après création :**

```
A ← B ← C
        ↑
      main (HEAD)
      feature-contact
```

Les deux branches pointent sur le même commit `C`.

#### Méthode 2 : `git checkout -b` (créer et changer en une commande)

C'est la méthode la plus utilisée car elle combine création et déplacement.

**Syntaxe :**

```bash
git checkout -b nom-de-la-branche
```

**Exemple :**

```bash
# Créer et basculer sur la nouvelle branche
git checkout -b feature-login

# Résultat :
# Switched to a new branch 'feature-login'

# Vérification
git branch
#   main
# * feature-login
```

Vous êtes maintenant directement sur la nouvelle branche.

**État après création :**

```
A ← B ← C
        ↑
      main
      feature-login (HEAD)
```

#### Méthode 3 : `git switch -c` (commande moderne)

Git a introduit `git switch` en 2019 pour clarifier la gestion des branches.

**Syntaxe :**

```bash
git switch -c nom-de-la-branche
```

L'option `-c` signifie "create" (créer).

**Exemple :**

```bash
# Créer et basculer sur la nouvelle branche
git switch -c feature-payment

# Vérification
git branch
#   main
#   feature-login
# * feature-payment
```

**Recommandation :** Utilisez `git switch -c` si vous avez Git 2.23+, car c'est plus clair que `git checkout`.

---

### Comparaison des trois méthodes

| Méthode | Crée la branche ? | Change de branche ? | Commande |
|---------|-------------------|---------------------|----------|
| `git branch` | ✅ Oui | ❌ Non | `git branch nom` |
| `git checkout -b` | ✅ Oui | ✅ Oui | `git checkout -b nom` |
| `git switch -c` | ✅ Oui | ✅ Oui | `git switch -c nom` |

**Workflow typique :**

```bash
# Option 1 : Deux commandes
git branch feature-X  
git checkout feature-X

# Option 2 : Une commande (recommandé)
git checkout -b feature-X

# Option 3 : Commande moderne (recommandé si Git 2.23+)
git switch -c feature-X
```

---

### Créer une branche depuis un commit spécifique

Par défaut, une branche est créée à partir de votre position actuelle (HEAD). Vous pouvez aussi créer une branche à partir d'un commit précis.

#### Depuis un commit spécifique

```bash
# Créer une branche pointant sur un ancien commit
git branch branche-ancienne a1b2c3d

# Ou créer et basculer
git checkout -b branche-ancienne a1b2c3d
```

**Exemple de cas d'usage :**

```bash
# Voir l'historique
git log --oneline
# d4e5f6g (HEAD -> main) Version 3
# c3d4e5f Version 2
# b2c3d4e Version 1
# a1b2c3d Version initiale

# Créer une branche à partir de "Version 1"
git checkout -b branche-v1 b2c3d4e

# Vous êtes maintenant sur une branche qui pointe sur Version 1
```

**Visualisation :**

```
                    ┌─ branche-v1 (HEAD)
                    │
A ← B ← C ← D
            ↑
          main
```

#### Depuis une autre branche

```bash
# Créer une branche à partir de develop (sans y aller)
git branch feature-X develop

# Créer et basculer
git checkout -b feature-X develop
```

#### Depuis un tag

```bash
# Créer une branche à partir d'un tag de version
git checkout -b hotfix-v1 v1.0.0
```

---

### Lister les branches

Il existe plusieurs façons de voir vos branches selon ce que vous voulez afficher.

#### Commande de base : `git branch`

```bash
git branch
```

**Résultat :**

```
  develop
  feature-login
* main
  bugfix-header
```

- L'astérisque `*` indique la branche actuelle
- Seules les **branches locales** sont affichées

#### Voir toutes les branches (locales + distantes)

```bash
git branch -a
```

**Résultat :**

```
  develop
  feature-login
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature-payment
```

- Branches locales en blanc
- Branches distantes préfixées par `remotes/origin/`

#### Voir uniquement les branches distantes

```bash
git branch -r
```

**Résultat :**

```
  origin/HEAD -> origin/main
  origin/main
  origin/develop
  origin/feature-payment
```

#### Voir les branches avec informations supplémentaires

```bash
git branch -v
```

**Résultat :**

```
  develop       c3d4e5f Work in progress
  feature-login a1b2c3d Ajout formulaire
* main          d4e5f6g Merge feature-X
  bugfix-header b2c3d4e Fix responsive
```

Affiche le dernier commit de chaque branche (hash et message).

#### Voir les branches avec état de tracking

```bash
git branch -vv
```

**Résultat :**

```
  develop       c3d4e5f [origin/develop] Work in progress
  feature-login a1b2c3d Ajout formulaire
* main          d4e5f6g [origin/main: ahead 2] Merge feature-X
  bugfix-header b2c3d4e Fix responsive
```

- `[origin/main]` : branche trackée
- `ahead 2` : 2 commits en avance sur le serveur
- `behind 3` : 3 commits en retard sur le serveur

---

### Tableau récapitulatif des options de listage

| Commande | Affichage |
|----------|-----------|
| `git branch` | Branches locales uniquement |
| `git branch -a` | Toutes les branches (locales + distantes) |
| `git branch -r` | Branches distantes uniquement |
| `git branch -v` | Avec dernier commit |
| `git branch -vv` | Avec statut de tracking |
| `git branch --merged` | Branches déjà fusionnées |
| `git branch --no-merged` | Branches non fusionnées |

---

### Branches fusionnées vs non fusionnées

#### Voir les branches déjà fusionnées

```bash
git branch --merged
```

**Résultat :**

```
  feature-login
  feature-payment
* main
```

Ces branches ont été fusionnées dans la branche actuelle. Elles peuvent généralement être supprimées sans risque.

#### Voir les branches non fusionnées

```bash
git branch --no-merged
```

**Résultat :**

```
  feature-newsletter
  bugfix-header
```

Ces branches contiennent du travail qui n'a pas encore été fusionné. Soyez prudent avant de les supprimer.

**Cas pratique :**

```bash
# Vous êtes sur main et voulez nettoyer les branches inutiles
git checkout main

# Voir ce qui a été fusionné
git branch --merged
#   feature-A  (peut être supprimée)
#   feature-B  (peut être supprimée)

# Voir ce qui n'a pas été fusionné
git branch --no-merged
#   feature-C  (travail en cours, à garder)
```

---

### Supprimer une branche

Une fois qu'une branche n'est plus nécessaire (fonctionnalité fusionnée, expérimentation abandonnée), vous pouvez la supprimer.

#### Suppression normale : `git branch -d`

L'option `-d` (delete) supprime une branche de manière sécurisée.

**Syntaxe :**

```bash
git branch -d nom-de-la-branche
```

**Exemple :**

```bash
# Fusionner une branche
git checkout main  
git merge feature-login

# Supprimer la branche fusionnée
git branch -d feature-login
# Deleted branch feature-login (was a1b2c3d)
```

**Protection intégrée :** Git refuse de supprimer une branche qui contient du travail non fusionné.

```bash
# Essayer de supprimer une branche non fusionnée
git branch -d feature-inprogress
# error: The branch 'feature-inprogress' is not fully merged.
```

#### Suppression forcée : `git branch -D`

L'option `-D` (majuscule) force la suppression même si la branche n'est pas fusionnée.

**Syntaxe :**

```bash
git branch -D nom-de-la-branche
```

**Exemple :**

```bash
# Supprimer une branche expérimentale ratée
git branch -D feature-experiment
# Deleted branch feature-experiment (was c3d4e5f)
```

**⚠️ Attention :** Vous perdrez tout le travail non fusionné sur cette branche !

#### Quand utiliser -d vs -D ?

| Option | Usage | Sécurité |
|--------|-------|----------|
| `-d` | Branche fusionnée | ✅ Sûre, refuse si non fusionnée |
| `-D` | Force la suppression | ⚠️ Dangereux, supprime même si non fusionnée |

**Règle simple :**
- Utilisez **`-d`** par défaut
- Utilisez **`-D`** uniquement si vous êtes certain de vouloir perdre le travail

---

### Supprimer plusieurs branches

#### Supprimer plusieurs branches spécifiques

```bash
# Supprimer plusieurs branches d'un coup
git branch -d feature-A feature-B feature-C
```

#### Supprimer toutes les branches fusionnées

```bash
# Lister les branches fusionnées (sauf main)
git branch --merged | grep -v "main" | xargs git branch -d
```

Cette commande :
1. Liste les branches fusionnées
2. Exclut `main` du résultat
3. Supprime toutes les autres

**Version Windows (PowerShell) :**

```powershell
git branch --merged | Select-String -NotMatch "main" | ForEach-Object { git branch -d $_.ToString().Trim() }
```

---

### Supprimer une branche distante

Supprimer une branche locale ne supprime pas automatiquement la branche sur le serveur.

#### Supprimer sur le serveur

```bash
# Méthode 1 (recommandée)
git push origin --delete nom-de-la-branche

# Méthode 2 (syntaxe alternative)
git push origin :nom-de-la-branche
```

**Exemple complet :**

```bash
# Supprimer localement
git branch -d feature-login

# Supprimer sur le serveur
git push origin --delete feature-login
# To https://github.com/user/repo.git
#  - [deleted]         feature-login
```

---

### Renommer une branche

#### Renommer la branche actuelle

```bash
# Vous êtes sur la branche à renommer
git branch -m nouveau-nom
```

L'option `-m` signifie "move" (déplacer/renommer).

**Exemple :**

```bash
# Sur la branche avec le mauvais nom
git branch
# * feture-login  (faute de frappe)

# Renommer
git branch -m feature-login

# Vérification
git branch
# * feature-login  (corrigé)
```

#### Renommer une autre branche

```bash
# Renommer sans être dessus
git branch -m ancien-nom nouveau-nom
```

**Exemple :**

```bash
git branch -m old-feature new-feature
```

#### Renommer et mettre à jour le serveur

```bash
# 1. Renommer localement
git branch -m ancien-nom nouveau-nom

# 2. Supprimer l'ancienne branche sur le serveur
git push origin --delete ancien-nom

# 3. Pousser la nouvelle branche
git push origin nouveau-nom

# 4. Configurer le tracking
git push --set-upstream origin nouveau-nom
```

---

### Cas pratiques détaillés

#### Scénario 1 : Workflow de développement typique

```bash
# 1. Créer une branche pour une nouvelle fonctionnalité
git checkout -b feature-dark-mode

# 2. Développer (plusieurs commits)
git add style.css  
git commit -m "Ajout variables CSS pour dark mode"  
git add script.js  
git commit -m "Toggle dark/light mode"

# 3. Fusionner dans main
git checkout main  
git merge feature-dark-mode

# 4. Supprimer la branche
git branch -d feature-dark-mode

# 5. Pousser sur le serveur
git push origin main
```

#### Scénario 2 : Expérimentation ratée

```bash
# Créer une branche expérimentale
git checkout -b experiment-new-design

# Travailler dessus
# ... plusieurs commits ...

# Ça ne fonctionne pas bien, on abandonne
git checkout main

# Supprimer la branche (force car non fusionnée)
git branch -D experiment-new-design
```

#### Scénario 3 : Nettoyage des branches obsolètes

```bash
# Voir toutes les branches
git branch -a
# Beaucoup de branches anciennes...

# Voir celles qui sont fusionnées
git branch --merged
#   feature-login     (fusionnée il y a 2 mois)
#   feature-payment   (fusionnée il y a 1 mois)
#   bugfix-header     (fusionnée la semaine dernière)

# Supprimer toutes les branches fusionnées sauf main
git branch --merged | grep -v "main" | xargs git branch -d

# Nettoyer aussi sur le serveur
git push origin --delete feature-login  
git push origin --delete feature-payment  
git push origin --delete bugfix-header
```

#### Scénario 4 : Créer une branche depuis un commit ancien

```bash
# Un bug a été introduit quelque part
# Trouver le dernier commit fonctionnel
git log --oneline
# d4e5f6g (HEAD -> main) Feature C - bug introduit ici
# c3d4e5f Feature B - bug introduit ici
# b2c3d4e Feature A - ça marchait encore
# a1b2c3d Version initiale

# Créer une branche depuis le dernier bon commit
git checkout -b hotfix-bug b2c3d4e

# Corriger le bug sur cette branche
# ... corrections ...
git commit -m "Fix du bug critique"

# Fusionner le fix dans main
git checkout main  
git merge hotfix-bug

# Supprimer la branche de fix
git branch -d hotfix-bug
```

---

### Erreurs courantes et solutions

#### Erreur 1 : Impossible de supprimer la branche actuelle

```bash
git branch -d main
# error: Cannot delete branch 'main' checked out at '/path/to/repo'
```

**Solution :** Changez de branche d'abord

```bash
git checkout autre-branche  
git branch -d main  # Maintenant ça marche (mais c'est rarement une bonne idée !)
```

#### Erreur 2 : Branche non fusionnée

```bash
git branch -d feature-X
# error: The branch 'feature-X' is not fully merged.
```

**Solutions possibles :**

```bash
# Option 1 : Fusionner d'abord
git merge feature-X  
git branch -d feature-X

# Option 2 : Forcer la suppression (si vous êtes sûr)
git branch -D feature-X
```

#### Erreur 3 : Nom de branche déjà existant

```bash
git branch feature-login
# fatal: A branch named 'feature-login' already exists.
```

**Solutions :**

```bash
# Option 1 : Choisir un autre nom
git branch feature-login-v2

# Option 2 : Supprimer l'ancienne branche d'abord
git branch -d feature-login  
git branch feature-login

# Option 3 : Utiliser le nom différemment
git checkout feature-login  # Basculer sur l'existante
```

#### Erreur 4 : Modifications non commitées

```bash
git checkout autre-branche
# error: Your local changes to the following files would be overwritten:
#   fichier.txt
# Please commit your changes or stash them before you switch branches.
```

**Solutions :**

```bash
# Option 1 : Commiter les modifications
git add fichier.txt  
git commit -m "WIP"  
git checkout autre-branche

# Option 2 : Mettre de côté avec stash
git stash  
git checkout autre-branche
# Plus tard : git stash pop

# Option 3 : Annuler les modifications
git restore fichier.txt  
git checkout autre-branche
```

---

### Conventions de nommage des branches

Suivre des conventions aide à organiser votre projet et à collaborer efficacement.

#### Format recommandé

```
<type>/<courte-description>
```

**Types courants :**

- `feature/` : Nouvelle fonctionnalité
- `bugfix/` ou `fix/` : Correction de bug
- `hotfix/` : Correction urgente en production
- `refactor/` : Refactoring du code
- `test/` : Ajout ou modification de tests
- `docs/` : Documentation
- `chore/` : Tâches de maintenance

**Exemples :**

```bash
git checkout -b feature/user-authentication  
git checkout -b bugfix/responsive-menu  
git checkout -b hotfix/security-vulnerability  
git checkout -b refactor/database-queries  
git checkout -b test/payment-integration  
git checkout -b docs/api-documentation  
git checkout -b chore/update-dependencies
```

#### Bonnes pratiques de nommage

✅ **Utiliser des tirets** : `feature-login` plutôt que `feature_login`

✅ **Être descriptif** : `fix-header-alignment` plutôt que `fix`

✅ **Utiliser des minuscules** : `feature-payment` plutôt que `Feature-Payment`

✅ **Inclure un numéro de ticket** : `feature/JIRA-123-user-profile`

❌ **Éviter les espaces** : `feature-login` pas `feature login`

❌ **Éviter les caractères spéciaux** : `feature-login` pas `feature/login!`

❌ **Éviter les noms trop longs** : `feat-login` plutôt que `feature-complete-user-authentication-with-oauth`

---

### Voir l'historique d'une branche supprimée

Si vous supprimez une branche par erreur, vous pouvez la récupérer avec le reflog.

```bash
# Vous avez supprimé une branche
git branch -D feature-importante

# Voir le reflog pour trouver le commit
git reflog
# a1b2c3d HEAD@{0}: checkout: moving from feature-importante to main
# c3d4e5f HEAD@{1}: commit: Dernier commit de feature-importante

# Recréer la branche au bon endroit
git branch feature-importante c3d4e5f
```

---

### Commandes avancées

#### Copier une branche

```bash
# Créer une copie d'une branche
git checkout -b branche-copie branche-originale
```

#### Voir les branches contenant un commit spécifique

```bash
# Quelles branches contiennent ce commit ?
git branch --contains a1b2c3d
```

#### Voir les branches avec pattern

```bash
# Lister seulement les branches feature
git branch --list "feature/*"
```

#### Trier les branches par date

```bash
# Branches triées par date de dernier commit
git branch --sort=-committerdate

# Avec détails
git for-each-ref --sort=-committerdate refs/heads/ --format='%(committerdate:short) %(refname:short)'
```

---

### Aide-mémoire des commandes

```bash
# CRÉER
git branch nom                    # Créer sans changer  
git checkout -b nom               # Créer et changer (méthode classique)  
git switch -c nom                 # Créer et changer (méthode moderne)  
git branch nom commit             # Créer depuis un commit spécifique

# LISTER
git branch                        # Branches locales  
git branch -a                     # Toutes (locales + distantes)  
git branch -r                     # Distantes uniquement  
git branch -v                     # Avec dernier commit  
git branch -vv                    # Avec statut tracking  
git branch --merged               # Branches fusionnées  
git branch --no-merged            # Branches non fusionnées

# SUPPRIMER
git branch -d nom                 # Suppression sûre  
git branch -D nom                 # Suppression forcée  
git push origin --delete nom      # Supprimer sur le serveur

# RENOMMER
git branch -m nouveau-nom         # Renommer la branche actuelle  
git branch -m ancien nouveau      # Renommer une autre branche

# UTILITAIRES
git branch --contains commit      # Branches contenant un commit  
git branch --list "pattern"       # Filtrer par pattern  
git branch --sort=-committerdate  # Trier par date
```

---

### Points clés à retenir

✅ **Trois façons de créer** : `git branch`, `git checkout -b`, `git switch -c`

✅ **`git branch`** liste les branches locales

✅ **`-d`** pour suppression sûre, **`-D`** pour forcer

✅ **Supprimer localement ≠ supprimer sur le serveur**

✅ **`--merged`** aide à identifier les branches nettoyables

✅ **Renommer avec `-m`** (move)

✅ **Suivre des conventions de nommage** claires

✅ **Le reflog permet de récupérer** une branche supprimée

---

### Conclusion

La gestion des branches est une compétence fondamentale dans Git. Créer des branches est simple et rapide, ce qui encourage à en créer librement pour chaque tâche. Lister les branches vous aide à naviguer dans votre projet, et savoir les supprimer proprement maintient votre dépôt organisé.

**Workflow recommandé :**
1. Créez des branches souvent et pour tout
2. Nommez-les clairement
3. Fusionnez-les quand le travail est terminé
4. Supprimez-les après fusion pour garder un dépôt propre

N'ayez pas peur de créer des branches ! Elles sont légères, rapides, et c'est exactement pour ça que Git a été conçu.

Dans la prochaine section, nous verrons comment **changer de branche** avec `git checkout` et `git switch`, et comment Git gère vos fichiers lors du basculement entre branches.

⏭️ [Changer de branche (git checkout et git switch)](/module-04-travailler-avec-les-branches/03-changer-de-branche.md)
