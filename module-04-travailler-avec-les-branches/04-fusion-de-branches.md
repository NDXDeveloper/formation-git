🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 4. Fusion de branches (git merge)

### Introduction

Après avoir créé des branches et développé des fonctionnalités séparément, il arrive un moment où vous devez combiner ce travail. C'est là qu'intervient `git merge`, la commande qui fusionne deux branches ensemble.

**Analogie :** Imaginez deux auteurs qui écrivent chacun un chapitre différent d'un livre. Une fois leurs chapitres terminés, ils doivent les combiner en un seul manuscrit. C'est exactement ce que fait `git merge` : il prend le travail de deux branches et les combine en une seule.

---

### Qu'est-ce qu'un merge (fusion) ?

Un **merge** est l'opération qui intègre les modifications d'une branche dans une autre. C'est le moyen standard de Git pour combiner des lignes de développement parallèles.

**Principe de base :**

```
Avant le merge :
        ┌─ F ← G ← H
        │
A ← B ← C ← D ← E
            ↑
          main (HEAD)

Après le merge :
        ┌─ F ← G ← H ─┐
        │             │
A ← B ← C ← D ← E ← ─ M
                      ↑
                    main (HEAD)
```

Le commit `M` est un **commit de merge** qui a deux parents : `E` (de main) et `H` (de la branche feature).

---

### Syntaxe de base

Pour fusionner une branche dans votre branche actuelle :

```bash
git merge nom-de-la-branche
```

**Processus typique :**

```bash
# 1. Se positionner sur la branche qui va recevoir les modifications
git switch main

# 2. Fusionner l'autre branche
git merge feature-login

# Résultat :
# Updating a1b2c3d..d4e5f6g
# Fast-forward
#  login.html | 50 ++++++++++++++++++++++
#  auth.js    | 120 +++++++++++++++++++++++++++++++++++++++++++
#  2 files changed, 170 insertions(+)
```

**Important :** Vous devez être sur la branche de destination avant de merger.

---

### Les trois types de merge

Git utilise différentes stratégies selon la situation. Les trois principaux types sont :

#### 1. Fast-Forward (Avance rapide)

C'est le cas le plus simple. La branche cible n'a pas avancé depuis la création de la branche feature.

**Situation :**

```
        ┌─ F ← G ← H
        │
A ← B ← C ← D ← E
        ↑     ↑
      main   feature (HEAD)
```

`main` est resté sur `C`, tandis que `feature` a avancé avec les commits `D`, `E`.

**Commande :**

```bash
git switch main  
git merge feature
```

**Résultat :**

```
A ← B ← C ← D ← E
                ↑
              main, feature (HEAD)
```

Git déplace simplement le pointeur `main` vers `E`. Aucun nouveau commit n'est créé. C'est rapide et propre.

**Message Git :**

```
Updating c3d4e5f..e5f6g7h  
Fast-forward
 fichier1.txt | 10 ++++++++++
 fichier2.js  | 25 +++++++++++++++++++++++++
 2 files changed, 35 insertions(+)
```

#### 2. Merge Commit (Commit de fusion)

Quand les deux branches ont divergé, Git crée un nouveau commit qui combine les deux historiques.

**Situation :**

```
        ┌─ F ← G
        │
A ← B ← C ← D ← E
            ↑
          main (HEAD)
```

`main` a avancé avec `D` et `E`, tandis que la branche feature a les commits `F` et `G`.

**Commande :**

```bash
git switch main  
git merge feature
```

**Résultat :**

```
        ┌─ F ← G ─┐
        │         │
A ← B ← C ← D ← E ← M
                    ↑
                  main (HEAD)
```

Un nouveau commit `M` (merge commit) est créé avec deux parents : `E` et `G`.

**Message Git :**

```
Merge made by the 'ort' strategy.
 fichier3.txt | 15 +++++++++++++++
 1 file changed, 15 insertions(+)
```

Git ouvrira votre éditeur pour que vous puissiez entrer un message de commit. Le message par défaut est généralement :

```
Merge branch 'feature' into main
```

#### 3. Squash Merge (Fusion écrasée)

Combine tous les commits d'une branche en un seul commit sur la branche cible.

**Situation :**

```
        ┌─ F ← G ← H
        │
A ← B ← C ← D
        ↑
      main (HEAD)
```

**Commande :**

```bash
git merge --squash feature  
git commit -m "Ajout feature complète"
```

**Résultat :**

```
        ┌─ F ← G ← H
        │
A ← B ← C ← D ← E
                ↑
              main (HEAD)
```

Le commit `E` contient toutes les modifications de `F`, `G`, et `H`, mais n'a qu'un seul parent. L'historique de feature n'apparaît pas dans main.

---

### Workflow de merge typique

#### Scénario complet

Vous développez une nouvelle fonctionnalité de login, puis la fusionnez dans main.

```bash
# 1. Créer une branche pour la fonctionnalité
git switch -c feature-login

# 2. Développer la fonctionnalité (plusieurs commits)
echo "Login form HTML" > login.html  
git add login.html  
git commit -m "Ajout formulaire login"

echo "Authentication logic" > auth.js  
git add auth.js  
git commit -m "Ajout logique authentification"

echo "Validation" >> auth.js  
git add auth.js  
git commit -m "Ajout validation formulaire"

# 3. Tester la fonctionnalité
npm test  # Tous les tests passent

# 4. Retourner sur main
git switch main

# 5. Fusionner la branche feature
git merge feature-login

# 6. Vérifier le résultat
git log --oneline --graph

# 7. Supprimer la branche feature (optionnel)
git branch -d feature-login

# 8. Pousser sur le serveur
git push origin main
```

---

### Empêcher le Fast-Forward

Parfois, vous voulez créer un commit de merge même quand un fast-forward est possible, pour garder une trace claire de l'intégration de la feature.

**Avec l'option `--no-ff` (no fast-forward) :**

```bash
git merge --no-ff feature-login
```

**Visualisation :**

```
Sans --no-ff (fast-forward) :  
A ← B ← C ← D ← E
                ↑
              main

Avec --no-ff (merge commit forcé) :
        ┌─ D ← E ─┐
        │         │
A ← B ← C ← ──── ─M
                  ↑
                main
```

**Avantage :** L'historique montre clairement qu'une feature a été intégrée. Si vous devez annuler la feature complète, vous n'avez qu'un commit à revert.

**Convention :** Beaucoup d'équipes utilisent systématiquement `--no-ff` pour les branches de feature.

---

### Conflits de merge

Un **conflit** se produit quand Git ne peut pas fusionner automatiquement parce que les mêmes lignes ont été modifiées différemment dans les deux branches.

#### Quand surviennent les conflits ?

```
Branche main :        Branche feature :  
ligne 1               ligne 1  
ligne 2 modifiée A    ligne 2 modifiée B  ← CONFLIT !  
ligne 3               ligne 3
```

Les deux branches ont modifié la ligne 2 différemment. Git ne peut pas décider quelle version garder.

#### Détecter un conflit

```bash
git merge feature-payment
# Auto-merging fichier.txt
# CONFLICT (content): Merge conflict in fichier.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

Git vous avertit qu'un conflit existe et indique quels fichiers sont concernés.

#### Vérifier l'état

```bash
git status
# On branch main
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#   (use "git merge --abort" to abort the merge)
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#       both modified:   fichier.txt
```

---

### Résoudre un conflit

#### Étape 1 : Ouvrir le fichier en conflit

Git marque les conflits avec des marqueurs spéciaux :

```javascript
function calculate() {
<<<<<<< HEAD
  // Version de main
  return price * 1.20;
=======
  // Version de feature-payment
  return price * 1.15;
>>>>>>> feature-payment
}
```

**Explication des marqueurs :**

- `<<<<<<< HEAD` : Début de votre version (branche actuelle)
- `=======` : Séparation entre les deux versions
- `>>>>>>> feature-payment` : Fin de la version de l'autre branche

#### Étape 2 : Choisir la bonne version

Vous avez trois options :

**Option A : Garder la version de HEAD (main)**

```javascript
function calculate() {
  // Version de main
  return price * 1.20;
}
```

**Option B : Garder la version de feature-payment**

```javascript
function calculate() {
  // Version de feature-payment
  return price * 1.15;
}
```

**Option C : Combiner les deux ou créer une nouvelle version**

```javascript
function calculate() {
  // Version combinée après réflexion
  const taxRate = isEuropeanCustomer() ? 1.20 : 1.15;
  return price * taxRate;
}
```

**Important :** Supprimez les marqueurs de conflit (`<<<<<<<`, `=======`, `>>>>>>>`) !

#### Étape 3 : Marquer le conflit comme résolu

```bash
# Après avoir édité le fichier
git add fichier.txt
```

En ajoutant le fichier, vous indiquez à Git que le conflit est résolu.

#### Étape 4 : Finaliser le merge

```bash
git commit
```

Git ouvrira votre éditeur avec un message pré-rempli :

```
Merge branch 'feature-payment'

# Conflicts:
#       fichier.txt
```

Vous pouvez modifier ce message pour expliquer comment vous avez résolu le conflit, puis sauvegarder et fermer l'éditeur.

---

### Workflow complet de résolution de conflit

```bash
# 1. Tenter le merge
git merge feature-payment
# CONFLICT (content): Merge conflict in payment.js

# 2. Voir quels fichiers sont en conflit
git status

# 3. Ouvrir et éditer les fichiers en conflit
code payment.js  # ou vim, nano, etc.

# 4. Rechercher les marqueurs <<<<<<<
# 5. Résoudre chaque conflit
# 6. Supprimer les marqueurs

# 7. Tester que tout fonctionne
npm test

# 8. Marquer comme résolu
git add payment.js

# 9. Vérifier que tous les conflits sont résolus
git status
# All conflicts fixed

# 10. Finaliser le merge
git commit

# 11. Vérifier le résultat
git log --oneline --graph
```

---

### Annuler un merge

#### Annuler un merge avant le commit

Si vous êtes au milieu d'un merge avec des conflits et voulez tout annuler :

```bash
git merge --abort
```

Cela ramène votre branche à l'état d'avant le merge.

**Exemple :**

```bash
git merge feature-complex
# CONFLICT (content): Merge conflict in 10 files
# C'est trop compliqué, j'abandonne

git merge --abort
# Retour à l'état initial, comme si rien ne s'était passé
```

#### Annuler un merge après le commit

Si vous avez déjà commité le merge et voulez revenir en arrière :

**Méthode 1 : Reset (si pas encore pushé)**

```bash
git reset --hard HEAD~1
```

Cela supprime le commit de merge et revient au commit précédent.

**Méthode 2 : Revert (si déjà pushé)**

```bash
git revert -m 1 HEAD
```

Cela crée un nouveau commit qui annule le merge. L'option `-m 1` indique de garder le premier parent (généralement la branche main).

---

### Stratégies de merge

Git utilise différentes stratégies selon la situation. Vous pouvez les spécifier avec l'option `-s`.

#### Stratégie `ort` (défaut depuis Git 2.34)

Stratégie moderne et rapide, utilisée par défaut.

```bash
git merge feature  # Utilise ort automatiquement
```

#### Stratégie `recursive` (ancien défaut)

Ancienne stratégie par défaut, toujours disponible.

```bash
git merge -s recursive feature
```

#### Stratégie `ours`

Garde uniquement la version de la branche actuelle en cas de conflit.

```bash
git merge -s ours feature
```

**Attention :** Cela ignore complètement les modifications de l'autre branche !

#### Stratégie `theirs` (via option)

Pour privilégier la version de l'autre branche :

```bash
git merge -X theirs feature
```

**Différence :**
- `-s ours` : stratégie qui ignore tout de l'autre branche
- `-X theirs` : option qui favorise l'autre branche en cas de conflit, mais garde les modifications non conflictuelles

---

### Options utiles de git merge

#### `--no-commit`

Effectue le merge mais ne crée pas automatiquement le commit.

```bash
git merge --no-commit feature-login
```

Utile pour :
- Vérifier les modifications avant de commiter
- Ajouter des changements supplémentaires au merge
- Modifier le message de commit

```bash
git merge --no-commit feature-login
# Vérifier les modifications
git diff --staged
# Ajouter des modifications si nécessaire
git add autre-fichier.txt
# Commiter manuellement
git commit -m "Merge feature-login avec modifications supplémentaires"
```

#### `--squash`

Combine tous les commits de la branche en un seul.

```bash
git merge --squash feature-multiple-commits  
git commit -m "Ajout feature complète"
```

**Avantage :** Historique plus propre sur main.

**Inconvénient :** Perte de l'historique détaillé de la branche feature.

#### `--log`

Inclut les messages des commits fusionnés dans le message de merge.

```bash
git merge --log feature-login
```

Le message de merge contiendra la liste des commits de la branche feature.

#### `--edit` / `--no-edit`

Contrôle si l'éditeur s'ouvre pour le message de merge.

```bash
# Forcer l'ouverture de l'éditeur
git merge --edit feature-login

# Ne pas ouvrir l'éditeur (utiliser le message par défaut)
git merge --no-edit feature-login
```

---

### Cas pratiques détaillés

#### Cas 1 : Merge simple sans conflit

```bash
# État initial
git log --oneline --graph
# * c3d4e5f (HEAD -> main) Update README
# * b2c3d4e Initial commit

# Créer et développer une feature
git switch -c feature-navbar  
echo "Nav HTML" > navbar.html  
git add navbar.html  
git commit -m "Ajout navbar"

echo "Nav CSS" > navbar.css  
git add navbar.css  
git commit -m "Style navbar"

# Retour sur main
git switch main

# Merge de la feature
git merge feature-navbar
# Updating c3d4e5f..e5f6g7h
# Fast-forward
#  navbar.html | 1 +
#  navbar.css  | 1 +
#  2 files changed, 2 insertions(+)

# Vérifier l'historique
git log --oneline --graph
# * e5f6g7h (HEAD -> main, feature-navbar) Style navbar
# * d4e5f6g Ajout navbar
# * c3d4e5f Update README
# * b2c3d4e Initial commit

# Nettoyer
git branch -d feature-navbar
```

#### Cas 2 : Merge avec conflit

```bash
# Sur main
git switch main  
echo "Version Main" > config.txt  
git add config.txt  
git commit -m "Config main"

# Sur feature
git switch -c feature-new-config  
echo "Version Feature" > config.txt  
git add config.txt  
git commit -m "Config feature"

# Retour sur main pour merger
git switch main  
git merge feature-new-config
# CONFLICT (content): Merge conflict in config.txt

# Ouvrir config.txt
# <<<<<<< HEAD
# Version Main
# =======
# Version Feature
# >>>>>>> feature-new-config

# Éditer pour résoudre
echo "Version Combinée" > config.txt

# Marquer comme résolu
git add config.txt

# Finaliser
git commit
# Message : "Merge branch 'feature-new-config'"

# Nettoyer
git branch -d feature-new-config
```

#### Cas 3 : Merge de plusieurs branches

```bash
# Développement parallèle de 3 features
git switch -c feature-A
# ... développer feature A ...
git commit -m "Feature A ready"

git switch main  
git switch -c feature-B
# ... développer feature B ...
git commit -m "Feature B ready"

git switch main  
git switch -c feature-C
# ... développer feature C ...
git commit -m "Feature C ready"

# Fusionner toutes les features dans main
git switch main  
git merge feature-A  
git merge feature-B  
git merge feature-C

# Si conflits, les résoudre un par un
```

#### Cas 4 : Merge avec --no-ff pour garder l'historique

```bash
# Feature avec plusieurs commits
git switch -c feature-multi-step  
git commit -m "Step 1"  
git commit -m "Step 2"  
git commit -m "Step 3"

# Merge avec commit de fusion explicite
git switch main  
git merge --no-ff feature-multi-step -m "Merge feature-multi-step : ajout fonctionnalité complète"

# L'historique montre clairement la feature
git log --oneline --graph
# *   a1b2c3d (HEAD -> main) Merge feature-multi-step
# |\
# | * e5f6g7h Step 3
# | * d4e5f6g Step 2
# | * c3d4e5f Step 1
# |/
# * b2c3d4e Base
```

---

### Outils pour faciliter la résolution de conflits

#### Outils en ligne de commande

**git mergetool**

Lance un outil graphique pour résoudre les conflits.

```bash
git mergetool
```

Outils supportés : vimdiff, meld, kdiff3, opendiff, etc.

Configuration :

```bash
# Configurer meld comme outil de merge
git config --global merge.tool meld
```

#### Éditeurs modernes

**VS Code**

VS Code détecte automatiquement les conflits et offre des boutons interactifs :
- Accept Current Change
- Accept Incoming Change
- Accept Both Changes
- Compare Changes

**IntelliJ / WebStorm**

Offre une interface trois volets pour comparer et fusionner.

#### Visualiser les différences

```bash
# Voir ce qui serait fusionné
git diff main..feature-branch

# Voir les conflits actuels
git diff --name-only --diff-filter=U

# Voir les détails des conflits
git diff
```

---

### Bonnes pratiques de merge

#### 1. Toujours merger depuis la branche de destination

```bash
# ✅ Correct
git switch main  
git merge feature-login

# ❌ Incorrect (source de confusion)
git switch feature-login  
git merge main  # Vous mergez main dans feature, pas l'inverse !
```

#### 2. Mettre à jour main avant le merge

```bash
# S'assurer que main est à jour
git switch main  
git pull origin main

# Puis merger
git merge feature-login
```

#### 3. Tester avant de merger

```bash
# Tester la feature sur sa branche
git switch feature-login  
npm test  
npm run build

# Si OK, merger
git switch main  
git merge feature-login
```

#### 4. Résoudre les conflits localement sur la feature

Plutôt que de résoudre les conflits lors du merge final, mettez à jour votre feature avec main régulièrement :

```bash
# Sur votre branche feature
git switch feature-login

# Merger main dans feature pour résoudre les conflits tôt
git merge main
# Résoudre les conflits ici

# Plus tard, le merge inverse sera plus simple
git switch main  
git merge feature-login  # Pas de conflit !
```

#### 5. Commiter régulièrement avant de merger

```bash
# ❌ Ne pas merger avec des modifications non commitées
git status
# Changes not staged for commit...

# ✅ Commiter ou stasher d'abord
git commit -am "WIP"  
git merge feature-login
```

#### 6. Utiliser des messages de merge descriptifs

```bash
# ❌ Message vague
git merge feature-login -m "Merge"

# ✅ Message descriptif
git merge feature-login -m "Merge feature-login: Ajout système authentification OAuth"
```

---

### Erreurs courantes et solutions

#### Erreur 1 : "Already up to date"

```bash
git merge feature-login
# Already up to date.
```

**Cause :** La branche feature ne contient rien que main n'ait déjà.

**Solution :** Vérifier que vous êtes sur la bonne branche et que la feature a bien des commits supplémentaires.

#### Erreur 2 : Merger dans le mauvais sens

```bash
# Vous vouliez merger feature dans main
# Mais vous avez fait l'inverse
git switch feature-login  
git merge main  # ❌ Main est maintenant dans feature !
```

**Solution :** Annuler avec reset

```bash
git reset --hard HEAD~1
```

#### Erreur 3 : Conflits non résolus

```bash
git commit
# error: Committing is not possible because you have unmerged files.
```

**Cause :** Il reste des fichiers avec des conflits non résolus.

**Solution :**

```bash
# Voir quels fichiers sont encore en conflit
git status

# Les éditer et résoudre les conflits
# Puis les ajouter
git add fichier-en-conflit.txt

# Vérifier que tout est résolu
git status
# All conflicts fixed but you are still merging

# Commiter
git commit
```

#### Erreur 4 : Marqueurs de conflit oubliés

Vous avez oublié de supprimer les `<<<<<<<`, `=======`, `>>>>>>>` du fichier.

**Prévention :** Tester le code après résolution

```bash
npm test  # Les tests échoueront si les marqueurs sont toujours là
```

---

### Merge vs Rebase : Quelle différence ?

| Aspect | Merge | Rebase |
|--------|-------|--------|
| **Historique** | Conserve l'historique complet | Réécrit l'historique linéairement |
| **Commits de merge** | Crée des commits de merge | Pas de commit de merge |
| **Complexité visuelle** | Graphe avec branches | Historique linéaire |
| **Sécurité** | Sûr pour branches publiques | Dangereux pour branches publiques |
| **Traçabilité** | Montre quand les features ont été intégrées | Perd l'information temporelle |

**Recommandation :**
- Utilisez **merge** pour intégrer des features dans main (branches publiques)
- Utilisez **rebase** pour nettoyer votre historique local avant de pousser

Nous verrons rebase en détail dans la section suivante.

---

### Aide-mémoire des commandes

```bash
# MERGE DE BASE
git merge branche                 # Fusionner branche dans la branche actuelle  
git merge --no-ff branche         # Forcer un commit de merge  
git merge --squash branche        # Combiner tous les commits en un seul

# GESTION DES CONFLITS
git merge --abort                 # Annuler un merge en cours  
git mergetool                     # Ouvrir l'outil de résolution  
git diff --name-only --diff-filter=U  # Lister les fichiers en conflit

# OPTIONS AVANCÉES
git merge --no-commit branche     # Merger sans commiter  
git merge --edit branche          # Forcer l'édition du message  
git merge -s strategy branche     # Spécifier une stratégie  
git merge -X theirs branche       # Favoriser l'autre branche en conflit

# ANNULATION
git reset --hard HEAD~1           # Annuler dernier merge (non pushé)  
git revert -m 1 HEAD              # Annuler dernier merge (pushé)

# VÉRIFICATION
git log --oneline --graph         # Voir l'historique avec merges  
git show HEAD                     # Voir le dernier commit (merge)
```

---

### Points clés à retenir

✅ **`git merge`** combine deux branches en une

✅ **Fast-forward** : simple déplacement de pointeur si pas de divergence

✅ **Merge commit** : créé quand les branches ont divergé

✅ **Conflits** : surviennent quand les mêmes lignes sont modifiées différemment

✅ **Marqueurs de conflit** : `<<<<<<<`, `=======`, `>>>>>>>`  à supprimer

✅ **`git merge --abort`** annule un merge en cours

✅ **`--no-ff`** force un commit de merge pour traçabilité

✅ **Toujours tester** après un merge et résolution de conflits

✅ **Merger depuis la branche de destination** (main), pas l'inverse

---

### Conclusion

Le merge est l'opération fondamentale pour combiner le travail de plusieurs branches. Bien que les conflits puissent sembler intimidants au début, ils font partie du processus normal de développement collaboratif. Avec la pratique, vous apprendrez à les anticiper et à les résoudre efficacement.

**Workflow recommandé :**
1. Développez sur une branche feature
2. Testez abondamment
3. Revenez sur main et mettez-le à jour
4. Mergez votre feature
5. Résolvez les conflits si nécessaire
6. Testez à nouveau
7. Poussez sur le serveur

Dans la prochaine section, nous approfondirons les **stratégies de merge** et verrons quand utiliser fast-forward, merge commit ou squash selon les situations.

⏭️ [Stratégies de merge (fast-forward, merge commit, squash)](/module-04-travailler-avec-les-branches/05-strategies-de-merge.md)
