🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 3 : Corriger et modifier
## 4. Annuler un commit publié (git revert)

### Introduction

Dans la section précédente, nous avons vu `git reset` qui permet de revenir en arrière en réécrivant l'historique. Mais que faire quand vous avez déjà partagé votre commit avec d'autres personnes ? C'est là qu'intervient `git revert`, une commande sûre qui annule un commit sans modifier l'historique.

**Analogie** : Imaginez que vous avez envoyé un email à toute l'équipe avec une erreur. Vous ne pouvez pas "rappeler" cet email (ce serait `git reset`), mais vous pouvez envoyer un nouvel email pour corriger l'erreur (c'est `git revert`).

---

### Qu'est-ce que `git revert` ?

`git revert` crée un **nouveau commit** qui annule les modifications introduites par un commit précédent. Au lieu de supprimer l'historique, il l'enrichit avec un commit d'annulation.

**Principe fondamental :**
- `git reset` : efface l'historique (dangereux si public)
- `git revert` : ajoute à l'historique (sûr, même si public)

**Syntaxe de base :**

```bash
git revert <identifiant_du_commit>
```

---

### Pourquoi utiliser `git revert` plutôt que `git reset` ?

#### Cas où `git reset` pose problème :

```bash
# Historique local
A ← B ← C (HEAD, main)

# Vous poussez sur le serveur
git push origin main

# L'équipe récupère vos commits
# Alice : A ← B ← C
# Bob : A ← B ← C
# Vous : A ← B ← C

# Vous réalisez que C est mauvais et faites un reset
git reset --hard HEAD~1
# Vous : A ← B (HEAD)

# Problème ! Alice et Bob ont toujours C
# Si vous poussez avec --force, vous créez des conflits pour tout le monde
```

#### Avec `git revert`, pas de problème :

```bash
# Même situation de départ
A ← B ← C (HEAD, main)

# Vous annulez C avec revert
git revert C
# Vous : A ← B ← C ← C' (HEAD)
# où C' annule les modifications de C

# Vous poussez normalement
git push origin main

# L'équipe peut récupérer sans problème
# Alice et Bob font : git pull
# Tout le monde a : A ← B ← C ← C'
```

---

### Comment fonctionne `git revert` ?

`git revert` examine les modifications introduites par un commit et crée un nouveau commit qui fait exactement l'inverse.

**Exemple concret :**

Si le commit à annuler a :
- Ajouté la ligne `console.log("debug");`
- Supprimé la ligne `return true;`
- Modifié `var x = 5;` en `var x = 10;`

Le commit de revert va :
- Supprimer la ligne `console.log("debug");`
- Ajouter la ligne `return true;`
- Modifier `var x = 10;` en `var x = 5;`

---

### Utilisation basique de `git revert`

#### Annuler le dernier commit

```bash
# Voir l'historique
git log --oneline
# d4e5f6g (HEAD -> main) Commit problématique
# c3d4e5f Bon commit
# b2c3d4e Autre bon commit

# Annuler le dernier commit
git revert HEAD

# Un éditeur s'ouvre avec un message par défaut :
# "Revert "Commit problématique""
#
# Vous pouvez modifier ce message ou le laisser tel quel
# Sauvegardez et fermez l'éditeur

# Vérification
git log --oneline
# a1b2c3d (HEAD -> main) Revert "Commit problématique"
# d4e5f6g Commit problématique
# c3d4e5f Bon commit
# b2c3d4e Autre bon commit
```

#### Annuler un commit spécifique

```bash
# Historique
git log --oneline
# f6g7h8i (HEAD -> main) Commit D
# d4e5f6g Commit C - celui-ci est mauvais
# c3d4e5f Commit B
# b2c3d4e Commit A

# Annuler spécifiquement le commit C
git revert d4e5f6g

# Résultat :
# x9y8z7w (HEAD -> main) Revert "Commit C"
# f6g7h8i Commit D
# d4e5f6g Commit C
# c3d4e5f Commit B
# b2c3d4e Commit A
```

---

### Options importantes de `git revert`

#### Option `-n` ou `--no-commit` : Revert sans commit automatique

Par défaut, `git revert` crée immédiatement un commit. L'option `-n` permet de faire le revert sans commiter automatiquement.

```bash
# Faire le revert mais ne pas commiter tout de suite
git revert -n d4e5f6g

# Les modifications d'annulation sont dans la staging area
git status
# Changes to be committed:
#   (modifications qui annulent le commit d4e5f6g)

# Vous pouvez :
# - Modifier encore des fichiers
# - Ajuster les changements
# - Puis commiter manuellement
git commit -m "Annulation du commit d4e5f6g avec modifications"
```

**Cas d'usage :** Utile quand vous voulez annuler plusieurs commits en un seul commit de revert.

#### Option `-m` : Spécifier le message de commit directement

```bash
# Éviter que l'éditeur s'ouvre
git revert d4e5f6g -m "Annulation : fonctionnalité causant des bugs"
```

#### Option `--no-edit` : Garder le message par défaut

```bash
# Utiliser le message par défaut sans ouvrir l'éditeur
git revert d4e5f6g --no-edit
```

---

### Annuler plusieurs commits

#### Méthode 1 : Revert individuel de chaque commit

```bash
# Annuler 3 commits un par un
git revert HEAD      # Annule le dernier
git revert HEAD~1    # Annule l'avant-dernier
git revert HEAD~2    # Annule celui d'avant

# Résultat : 3 commits de revert dans l'historique
```

#### Méthode 2 : Revert multiple en un seul commit

```bash
# Annuler les 3 derniers commits en un seul commit de revert
git revert -n HEAD~2..HEAD
# L'option -n évite les commits automatiques

# Tous les revert sont dans la staging area
git status

# Faire un seul commit pour tout
git commit -m "Annulation des 3 derniers commits"
```

#### Méthode 3 : Revert d'une plage de commits

```bash
# Annuler tous les commits entre deux références
git revert a1b2c3d..d4e5f6g

# Attention : cela crée un commit de revert pour chaque commit
```

---

### Gérer les conflits lors d'un revert

Comme pour un merge, `git revert` peut créer des conflits si les fichiers ont été modifiés depuis le commit à annuler.

#### Exemple de conflit

```bash
# Historique
# C: Modifie ligne 5 de fichier.txt
# B: Modifie ligne 10 de fichier.txt
# A: Version initiale

# Vous êtes sur C et voulez annuler B
git revert B

# Conflit ! Car C a peut-être modifié des zones proches de B
```

#### Résolution du conflit

```bash
# Git vous avertit du conflit
git revert d4e5f6g
# error: could not revert d4e5f6g... Message du commit
# hint: after resolving the conflicts, mark the corrected paths
# hint: with 'git add <paths>' or 'git rm <paths>'
# hint: and commit the result with 'git commit'

# Vérifier les fichiers en conflit
git status
# both modified: fichier.txt

# Ouvrir le fichier et résoudre le conflit
# Les conflits sont marqués avec :
# <<<<<<< HEAD
# Contenu actuel
# =======
# Contenu du revert
# >>>>>>> parent of d4e5f6g... Message

# Après résolution manuelle
git add fichier.txt
git commit
# Le message par défaut sera déjà rempli
```

#### Annuler un revert en cours

Si vous décidez d'abandonner le revert en cours :

```bash
# Abandonner le revert et revenir à l'état précédent
git revert --abort
```

---

### Cas pratiques détaillés

#### Scénario 1 : Bug introduit dans un commit récent

```bash
# Vous découvrez un bug dans le commit que vous venez de pusher
git log --oneline
# d4e5f6g (HEAD -> main, origin/main) Ajout nouvelle feature
# c3d4e5f Correction bugs
# b2c3d4e Mise à jour

# Annuler le commit problématique
git revert HEAD --no-edit

# Pousser l'annulation
git push origin main

# L'équipe peut récupérer proprement
# Historique final :
# a1b2c3d Revert "Ajout nouvelle feature"
# d4e5f6g Ajout nouvelle feature
# c3d4e5f Correction bugs
```

#### Scénario 2 : Annuler un commit au milieu de l'historique

```bash
git log --oneline
# f6g7h8i (HEAD -> main) Feature C
# d4e5f6g Feature B - celle-ci cause problème
# c3d4e5f Feature A
# b2c3d4e Base

# Annuler Feature B sans toucher aux autres
git revert d4e5f6g

# Si pas de conflit, c'est fait !
# Si conflit, résolvez-le comme expliqué ci-dessus

# Historique final :
# x9y8z7w Revert "Feature B"
# f6g7h8i Feature C
# d4e5f6g Feature B
# c3d4e5f Feature A
```

#### Scénario 3 : Annuler plusieurs commits liés

```bash
git log --oneline
# h8i9j0k Feature X - partie 3
# f6g7h8i Feature X - partie 2
# d4e5f6g Feature X - partie 1
# c3d4e5f Autres choses

# Annuler toute la feature X en un seul commit
git revert -n d4e5f6g
git revert -n f6g7h8i
git revert -n h8i9j0k

# Créer un seul commit d'annulation
git commit -m "Annulation complète de Feature X"
```

#### Scénario 4 : Revert d'un merge commit

Les merge commits ont deux parents, il faut spécifier lequel garder avec l'option `-m`.

```bash
# Vous avez mergé une branche
git log --oneline --graph
# *   m1e2r3g (HEAD -> main) Merge branch 'feature'
# |\
# | * f6g7h8i Commit sur feature
# * | d4e5f6g Commit sur main
# |/
# * c3d4e5f Base

# Annuler le merge en gardant la branche main
git revert -m 1 m1e2r3g

# -m 1 signifie "garder le premier parent" (main)
# -m 2 signifierait "garder le deuxième parent" (feature)
```

---

### Différences entre revert, reset et restore

| Commande | Action | Modifie historique ? | Sûr pour commits publics ? | Usage |
|----------|--------|---------------------|---------------------------|-------|
| `git revert` | Crée commit d'annulation | ❌ Non (ajoute) | ✅ Oui | Annuler commits publics |
| `git reset` | Déplace HEAD | ✅ Oui (supprime) | ❌ Non | Modifier commits locaux |
| `git restore` | Restaure fichiers | ❌ Non | ✅ Oui | Annuler modifications fichiers |

**Règle simple :**
- Commit déjà poussé → `git revert`
- Commit local uniquement → `git reset`
- Pas encore commité → `git restore`

---

### Visualiser l'effet d'un revert avant de l'exécuter

Avant de faire un revert, vous pouvez prévisualiser les modifications :

```bash
# Voir ce qui sera changé par le revert
git show d4e5f6g

# Voir les différences que le revert créerait (inversées)
git diff d4e5f6g^..d4e5f6g

# Simuler le revert sans le faire
git revert -n d4e5f6g
git diff --cached    # Voir les modifications
git reset --hard     # Annuler la simulation
```

---

### Revert d'un revert : revenir en arrière

Si vous avez fait un revert par erreur, vous pouvez annuler ce revert !

```bash
# Historique
git log --oneline
# a1b2c3d (HEAD -> main) Revert "Feature X"
# d4e5f6g Feature X
# c3d4e5f Base

# Vous réalisez que le revert était une erreur
# Annuler le revert (donc rétablir Feature X)
git revert a1b2c3d

# Résultat final :
# z9y8x7w Revert "Revert "Feature X""
# a1b2c3d Revert "Feature X"
# d4e5f6g Feature X
# c3d4e5f Base

# Feature X est de nouveau active !
```

---

### Options avancées

#### `--strategy` : Choisir la stratégie de merge

```bash
# Utiliser une stratégie de merge spécifique lors du revert
git revert d4e5f6g --strategy=recursive --strategy-option=theirs
```

#### `--mainline` : Pour les merge commits

```bash
# Annuler un merge en spécifiant le parent à garder
git revert -m 1 <merge_commit>
```

#### `--edit` / `--no-edit` : Contrôler le message

```bash
# Forcer l'ouverture de l'éditeur même si pas nécessaire
git revert d4e5f6g --edit

# Ne pas ouvrir l'éditeur
git revert d4e5f6g --no-edit
```

---

### Bonnes pratiques avec `git revert`

#### 1. Toujours tester après un revert

```bash
# Après un revert
git revert d4e5f6g

# Tester votre application
npm test    # ou autre commande de test
npm start   # vérifier que tout fonctionne

# Puis pousser
git push origin main
```

#### 2. Écrire des messages de commit clairs

```bash
# ❌ Message vague
git revert d4e5f6g -m "Revert"

# ✅ Message explicatif
git revert d4e5f6g -m "Revert: Annulation feature X - causait crash au démarrage"
```

#### 3. Documenter pourquoi vous faites le revert

```bash
git revert d4e5f6g -m "Revert: Feature Login

Cette fonctionnalité causait des erreurs en production :
- Crash lors du login avec Google OAuth
- Sessions utilisateurs corrompues
- Impossibilité de se déconnecter

Sera réimplémentée après investigation (ticket #1234)"
```

#### 4. Préférer plusieurs petits commits

Si vous devez annuler beaucoup de choses, préférez plusieurs petits reverts qu'un gros :

```bash
# ✅ Bon : reverts spécifiques
git revert abc123 -m "Revert: partie UI"
git revert def456 -m "Revert: partie backend"

# ❌ Moins bon : un gros revert flou
git revert -n abc123 def456 ghi789
git commit -m "Revert plein de trucs"
```

---

### Erreurs courantes et solutions

#### Erreur 1 : "error: commit X is a merge but no -m option was given"

```bash
# Vous essayez de revert un merge
git revert m1e2r3g
# error: commit m1e2r3g is a merge but no -m option was given

# Solution : spécifier le parent avec -m
git revert -m 1 m1e2r3g
```

#### Erreur 2 : Conflits pendant le revert

```bash
# Conflit lors du revert
git revert d4e5f6g
# CONFLICT (content): Merge conflict in fichier.txt

# Solutions possibles :
# 1. Résoudre le conflit manuellement
nano fichier.txt  # résoudre
git add fichier.txt
git commit

# 2. Abandonner le revert
git revert --abort
```

#### Erreur 3 : Revert du mauvais commit

```bash
# Vous avez reverté le mauvais commit
git revert xyz789

# Solution 1 : Revert du revert
git revert HEAD

# Solution 2 : Reset si pas encore pushé
git reset --hard HEAD~1
```

---

### Comparaison : Quand utiliser quoi ?

#### Utilisez `git revert` quand :

✅ Le commit a été poussé sur le serveur

✅ D'autres personnes ont déjà récupéré le commit

✅ Vous voulez garder une trace de l'annulation

✅ Vous travaillez sur une branche partagée (main, develop, etc.)

#### Utilisez `git reset` quand :

✅ Le commit est encore local (pas pushé)

✅ Vous travaillez seul sur une branche

✅ Vous voulez un historique parfaitement propre

✅ Vous êtes sûr de ce que vous faites

---

### Workflow typique avec revert

```bash
# 1. Identifier le commit problématique
git log --oneline --graph -20

# 2. Vérifier ce que le commit contient
git show d4e5f6g

# 3. Créer une branche pour tester (optionnel mais recommandé)
git checkout -b test-revert
git revert d4e5f6g

# 4. Tester que tout fonctionne
npm test
npm start

# 5. Si OK, appliquer sur la branche principale
git checkout main
git revert d4e5f6g --no-edit

# 6. Pousser
git push origin main

# 7. Supprimer la branche de test
git branch -d test-revert
```

---

### Aide-mémoire des commandes

```bash
# Revert basique
git revert <commit>                    # Annuler un commit
git revert HEAD                        # Annuler le dernier commit
git revert HEAD~2                      # Annuler l'avant-avant-dernier

# Options utiles
git revert <commit> --no-edit          # Sans ouvrir l'éditeur
git revert <commit> -n                 # Sans commit automatique
git revert <commit> -m "message"       # Avec message personnalisé

# Plusieurs commits
git revert -n commit1 commit2 commit3  # Revert multiple sans commits auto
git revert commit1..commit3            # Plage de commits (un revert par commit)

# Merge commits
git revert -m 1 <merge_commit>         # Revert un merge (garder 1er parent)

# Gestion des conflits
git revert --abort                     # Abandonner un revert en cours
git revert --continue                  # Continuer après résolution conflit

# Revert d'un revert
git revert <commit_du_revert>          # Annuler l'annulation
```

---

### Points clés à retenir

✅ `git revert` est **sûr** pour les commits publics

✅ Crée un nouveau commit au lieu de modifier l'historique

✅ Peut être utilisé sur n'importe quel commit, pas seulement le dernier

✅ Peut causer des conflits qui doivent être résolus

✅ Un revert peut lui-même être reverté

✅ Préférer `revert` à `reset --force` sur des branches partagées

❌ Ne supprime pas l'historique (le mauvais commit reste visible)

---

### Conclusion

`git revert` est votre outil de prédilection pour annuler des commits qui ont déjà été partagés avec votre équipe. Contrairement à `git reset`, il respecte l'historique et permet à tous les collaborateurs de rester synchronisés sans problème.

**Philosophie de Git revert :**
> "N'essayez pas d'effacer vos erreurs, assumez-les et corrigez-les proprement."

C'est exactement ce que fait `revert` : il reconnaît qu'une erreur a été faite (le commit reste visible) et la corrige de manière transparente (avec un nouveau commit d'annulation).

Dans la prochaine section, nous verrons comment récupérer des fichiers d'anciennes versions spécifiques, une compétence utile pour explorer l'historique de votre projet.

⏭️ [Récupérer des fichiers d'anciennes versions](/module-03-corriger-et-modifier/05-recuperer-des-fichiers-anciennes-versions.md)
