🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 7. Rebasage : le principe de git rebase

### Introduction

Le rebase est une alternative au merge pour intégrer les modifications d'une branche dans une autre. Contrairement au merge qui crée un commit de fusion, le rebase réécrit l'historique pour créer une ligne droite. C'est une technique puissante mais qui demande de la prudence.

**Analogie :** Imaginez que vous écrivez un chapitre d'un livre pendant que votre collègue en écrit un autre. Avec un merge, vous ajouteriez une note disant "ici, nous avons combiné nos deux chapitres". Avec un rebase, vous réécririez votre chapitre comme si vous l'aviez écrit **après** que votre collègue ait terminé le sien, créant une histoire linéaire sans mention de la combinaison.

---

### Qu'est-ce que le rebase ?

**Rebase** signifie littéralement "re-baser" ou "changer la base". C'est l'opération qui consiste à déplacer ou "rejouer" une série de commits sur une nouvelle base (un nouveau commit parent).

**En termes simples :** Au lieu de fusionner deux branches, vous prenez vos commits et les "rejouez" comme s'ils avaient été faits après les commits de l'autre branche.

---

### Merge vs Rebase : Différence visuelle

#### Situation de départ

Vous et votre collègue avez créé des commits en parallèle :

```
        ┌─ F ← G ← H
        │         ↑
A ← B ← C ← D ← E feature (vous)
            ↑
          main (collègue)
```

- Vous avez créé la branche `feature` à partir du commit `C`
- Votre collègue a ajouté les commits `D` et `E` sur `main`
- Vous avez ajouté les commits `F`, `G`, et `H` sur `feature`

#### Option 1 : Merge (fusion traditionnelle)

```bash
git switch main
git merge feature
```

**Résultat :**

```
        ┌─ F ← G ← H ─┐
        │             │
A ← B ← C ← D ← E ← ─ M
                      ↑
                    main
```

Un commit de merge `M` est créé. L'historique montre clairement qu'il y a eu deux lignes de développement parallèles.

#### Option 2 : Rebase (réécriture de l'historique)

```bash
git switch feature
git rebase main
```

**Résultat :**

```
                  ┌─ F' ← G' ← H'
                  │           ↑
A ← B ← C ← D ← E             feature
            ↑
          main
```

Les commits `F`, `G`, et `H` ont été "rejoués" sur `E`. Ils deviennent `F'`, `G'`, et `H'` (nouveaux commits avec de nouveaux identifiants, mais le même contenu).

**Ensuite, pour finir l'intégration :**

```bash
git switch main
git merge feature  # Fast-forward
```

**Résultat final :**

```
A ← B ← C ← D ← E ← F' ← G' ← H'
                            ↑
                        main, feature
```

Historique parfaitement linéaire, comme si vous aviez développé votre feature après que votre collègue ait terminé son travail sur main.

---

### Comment fonctionne le rebase ?

Le rebase se déroule en plusieurs étapes automatiques :

#### Étape 1 : Git identifie les commits à rejouer

```
Commits de feature à rejouer : F, G, H
Nouveau parent (base) : E
```

#### Étape 2 : Git "détache" temporairement vos commits

```
A ← B ← C ← D ← E
            ↑
          main

(F, G, H en attente)
```

#### Étape 3 : Git rejoue chaque commit un par un

```bash
# Rejouer F sur E
A ← B ← C ← D ← E ← F'

# Rejouer G sur F'
A ← B ← C ← D ← E ← F' ← G'

# Rejouer H sur G'
A ← B ← C ← D ← E ← F' ← G' ← H'
```

Chaque commit est réappliqué, en créant de **nouveaux commits** avec de nouveaux identifiants.

#### Étape 4 : Git déplace le pointeur de la branche

```
A ← B ← C ← D ← E ← F' ← G' ← H'
                            ↑
                          feature
```

---

### Syntaxe de base

#### Rebaser votre branche sur main

```bash
# Vous êtes sur votre branche feature
git switch feature

# Rebaser sur main
git rebase main
```

#### Rebaser main sur votre branche (moins courant)

```bash
git switch main
git rebase feature
```

**Note :** Généralement, on rebase **sa branche feature** sur **main**, pas l'inverse.

---

### Exemple pratique complet

#### Situation initiale

```bash
# État du projet
git log --oneline --graph --all
# * e5f6g7h (feature) Ajout tests
# * d4e5f6g Ajout fonctionnalité
# * c3d4e5f Début feature
# | * b2c3d4e (main) Fix bug critique
# | * a1b2c3d Update README
# |/
# * h7i8j9k Base commune
```

#### Rebase étape par étape

```bash
# 1. Aller sur la branche à rebaser
git switch feature

# 2. Lancer le rebase
git rebase main
# Successfully rebased and updated refs/heads/feature.

# 3. Vérifier le nouvel historique
git log --oneline --graph --all
# * z9y8x7w (feature) Ajout tests
# * y8x7w6v Ajout fonctionnalité
# * x7w6v5u Début feature
# * b2c3d4e (main) Fix bug critique
# * a1b2c3d Update README
# * h7i8j9k Base commune
```

Maintenant, `feature` est basée sur le dernier commit de `main` !

#### Finaliser l'intégration (fast-forward)

```bash
# 4. Retourner sur main
git switch main

# 5. Merger (sera un fast-forward)
git merge feature
# Updating b2c3d4e..z9y8x7w
# Fast-forward

# 6. Vérifier l'historique final
git log --oneline --graph
# * z9y8x7w (HEAD -> main, feature) Ajout tests
# * y8x7w6v Ajout fonctionnalité
# * x7w6v5u Début feature
# * b2c3d4e Fix bug critique
# * a1b2c3d Update README
# * h7i8j9k Base commune
```

Historique parfaitement linéaire !

---

### Avantages du rebase

#### 1. Historique linéaire et propre

```bash
# Avec merge
git log --oneline --graph
# *   Merge branch 'feature'
# |\
# | * Feature commit 3
# | * Feature commit 2
# | * Feature commit 1
# * | Main commit 2
# * | Main commit 1
# |/
# * Base

# Avec rebase
git log --oneline --graph
# * Feature commit 3
# * Feature commit 2
# * Feature commit 1
# * Main commit 2
# * Main commit 1
# * Base
```

Plus facile à lire et à comprendre.

#### 2. Pas de commits de merge

Pas de pollution avec des commits de merge inutiles pour les petites features.

#### 3. Historique "comme si"

L'historique raconte l'histoire **logique** du projet, pas nécessairement l'histoire **chronologique**.

#### 4. Facilite git bisect

Avec un historique linéaire, `git bisect` (pour trouver un bug) est plus simple.

#### 5. Intégration continue plus propre

Chaque commit de la branche est testable individuellement après rebase.

---

### Inconvénients et dangers du rebase

#### Danger 1 : Réécrit l'historique

⚠️ **RÈGLE D'OR : Ne jamais rebaser des commits qui ont été poussés et partagés avec d'autres !**

```bash
# ❌ DANGEREUX : Rebaser une branche publique
git switch main
git rebase feature  # main est publique !
git push --force    # Catastrophe pour l'équipe !

# ✅ SÛR : Rebaser une branche locale
git switch feature  # feature est locale
git rebase main     # OK !
```

**Pourquoi c'est dangereux ?**

Si d'autres personnes ont basé leur travail sur vos anciens commits, et que vous les réécrivez, cela crée des conflits massifs et de la confusion.

#### Danger 2 : Perte d'information temporelle

Vous perdez la trace de **quand** les choses se sont vraiment passées.

```bash
# On ne voit plus que feature a été développée en parallèle
# On perd l'information de synchronisation
```

#### Danger 3 : Conflits potentiellement multiples

Avec un merge, vous résolvez les conflits une seule fois. Avec un rebase, vous pourriez devoir résoudre le même conflit pour **chaque commit** rebasé.

```bash
# Si vous rebasez 10 commits et qu'il y a des conflits
# Vous pourriez avoir 10 résolutions de conflits à faire
```

#### Danger 4 : Plus complexe pour les débutants

Le rebase est conceptuellement plus difficile à comprendre que le merge.

---

### Quand utiliser rebase ?

#### ✅ Utilisez rebase pour :

**1. Nettoyer votre historique local avant de partager**

```bash
# Vous avez beaucoup de commits "WIP" locaux
git rebase -i main  # Rebase interactif pour nettoyer
git push origin feature
```

**2. Mettre à jour votre branche feature avec les derniers changements de main**

```bash
# Au lieu de merger main dans feature
git switch feature
git rebase main  # Garde feature à jour
```

**3. Maintenir un historique linéaire sur vos branches personnelles**

```bash
# Pour vos expérimentations personnelles
git rebase main
```

**4. Projets avec workflow "rebase-only"**

Certaines équipes adoptent une politique "rebase uniquement" pour un historique ultra-propre.

#### ❌ N'utilisez PAS rebase pour :

**1. Branches publiques partagées**

```bash
# ❌ Ne jamais rebaser main si d'autres travaillent dessus
git switch main
git rebase feature  # DANGER !
```

**2. Quand la chronologie est importante**

Pour des audits, conformité légale, debugging temporel.

**3. Quand vous travaillez en équipe sur la même branche**

Si plusieurs personnes travaillent sur `feature-X`, pas de rebase !

**4. Si vous n'êtes pas à l'aise**

En cas de doute, utilisez merge. C'est plus sûr.

---

### Conflits pendant un rebase

Comme avec merge, des conflits peuvent survenir pendant un rebase.

#### Quand survient un conflit

```bash
git rebase main
# Auto-merging fichier.js
# CONFLICT (content): Merge conflict in fichier.js
# error: could not apply a1b2c3d... Mon commit
# hint: Resolve all conflicts manually, mark them as resolved with
# hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
```

Git s'arrête à chaque commit qui cause un conflit.

#### Résoudre un conflit pendant rebase

**Processus :**

```bash
# 1. Git s'arrête sur un conflit
git rebase main
# CONFLICT in fichier.js

# 2. Vérifier l'état
git status
# rebase in progress
# Unmerged paths:
#   both modified: fichier.js

# 3. Ouvrir et résoudre le conflit
code fichier.js
# Éditer, résoudre, supprimer les marqueurs

# 4. Marquer comme résolu
git add fichier.js

# 5. Continuer le rebase
git rebase --continue

# 6. Si d'autres conflits, répéter les étapes 2-5
# Sinon, le rebase est terminé
```

#### Options pendant un conflit de rebase

**Continuer après résolution :**

```bash
git rebase --continue
```

**Sauter le commit actuel :**

```bash
git rebase --skip
# Attention : le commit est perdu !
```

**Abandonner complètement le rebase :**

```bash
git rebase --abort
# Retour à l'état avant le rebase
```

#### Exemple complet de résolution

```bash
# Démarrer le rebase
git rebase main
# CONFLICT in app.js on commit "Ajout feature X"

# Résoudre le conflit
code app.js
# ... éditer ...

# Marquer comme résolu
git add app.js

# Continuer
git rebase --continue
# CONFLICT in utils.js on commit "Ajout helper"

# Résoudre ce deuxième conflit
code utils.js
git add utils.js

# Continuer
git rebase --continue
# Successfully rebased and updated refs/heads/feature.

# Terminé !
```

---

### Rebase interactif : Nettoyer l'historique

Le rebase interactif permet de modifier, réorganiser, ou combiner des commits avant de les partager.

#### Syntaxe

```bash
# Rebaser les 5 derniers commits de manière interactive
git rebase -i HEAD~5

# Ou rebaser depuis un commit spécifique
git rebase -i main
```

#### Interface du rebase interactif

Git ouvre un éditeur avec vos commits :

```
pick a1b2c3d Premier commit
pick c3d4e5f Deuxième commit
pick e5f6g7h Troisième commit
pick g7h8i9j Quatrième commit

# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# d, drop = remove commit
```

#### Commandes disponibles

**pick (p)** : Garder le commit tel quel

```
pick a1b2c3d Mon commit
```

**reword (r)** : Modifier le message du commit

```
reword a1b2c3d Mon commit
# Git ouvrira un éditeur pour changer le message
```

**edit (e)** : Arrêter pour modifier le commit

```
edit a1b2c3d Mon commit
# Git s'arrête, vous pouvez modifier des fichiers
# Puis : git commit --amend
# Puis : git rebase --continue
```

**squash (s)** : Fusionner avec le commit précédent

```
pick a1b2c3d Premier commit
squash c3d4e5f Deuxième commit
# Les deux commits seront combinés
```

**fixup (f)** : Comme squash, mais ignore le message

```
pick a1b2c3d Mon commit
fixup c3d4e5f Fix typo
# Le commit "Fix typo" est absorbé silencieusement
```

**drop (d)** : Supprimer le commit

```
drop a1b2c3d Mauvais commit
# Le commit disparaît de l'historique
```

#### Exemple : Nettoyer un historique chaotique

**Avant :**

```bash
git log --oneline
# g7h8i9j (HEAD -> feature) Fix final typo
# e5f6g7h Oops forgot file
# c3d4e5f Add tests
# a1b2c3d WIP
# h7i8j9k (main) Base
```

**Rebase interactif :**

```bash
git rebase -i main
```

**Dans l'éditeur :**

```
pick a1b2c3d WIP
squash c3d4e5f Add tests
squash e5f6g7h Oops forgot file
fixup g7h8i9j Fix final typo

# Devient : un seul commit propre
```

**Après :**

```bash
git log --oneline
# a1b2c3d (HEAD -> feature) Implémentation complète de la feature X
# h7i8j9k (main) Base
```

Un seul commit propre au lieu de 4 commits chaotiques !

---

### Rebase vs Merge : Tableau comparatif

| Aspect | Merge | Rebase |
|--------|-------|--------|
| **Historique** | Préserve l'historique complet | Réécrit pour linéariser |
| **Commits de merge** | Crée des commits de merge | Aucun commit de merge |
| **Traçabilité temporelle** | Préservée | Perdue |
| **Lisibilité** | Graphe complexe | Ligne droite |
| **Sûreté** | Sûr pour branches publiques | Dangereux pour branches publiques |
| **Conflits** | Une fois maximum | Potentiellement par commit |
| **Complexité** | Simple | Plus complexe |
| **Quand utiliser** | Par défaut, branches publiques | Nettoyage local, mise à jour de feature |

---

### Workflow recommandé avec rebase

#### Workflow 1 : Feature branch avec rebase

```bash
# 1. Créer une branche feature
git switch -c feature-login

# 2. Développer (plusieurs commits)
git commit -m "WIP: start login"
git commit -m "Add form"
git commit -m "Fix bug"
git commit -m "Add validation"

# 3. Mettre à jour avec main régulièrement
git fetch origin
git rebase origin/main

# 4. Nettoyer l'historique avant de partager
git rebase -i origin/main
# Squash ou fixup les commits "WIP"

# 5. Pousser la branche propre
git push origin feature-login

# 6. Créer une Pull Request
# (Sur GitHub/GitLab)

# 7. Après validation, merger dans main
git switch main
git merge feature-login  # Fast-forward
```

#### Workflow 2 : Synchronisation quotidienne

```bash
# Chaque matin, avant de commencer à travailler
git switch feature-my-work
git fetch origin
git rebase origin/main

# Si conflits, les résoudre
# Continuer le travail avec une base à jour
```

#### Workflow 3 : Collaboration sur une feature partagée

```bash
# ⚠️ Si plusieurs personnes travaillent sur la même branche
# NE PAS rebaser !

# Utiliser merge à la place
git switch feature-shared
git merge main

# Le rebase casserait le travail des autres
```

---

### Configuration Git pour le rebase

#### Configurer pull avec rebase par défaut

```bash
# Au lieu de merger lors d'un git pull, rebaser
git config --global pull.rebase true
```

Maintenant, `git pull` fera :

```bash
git fetch
git rebase  # Au lieu de git merge
```

#### Configurer pour une seule branche

```bash
git config branch.feature.rebase true
```

#### Activer autostash

```bash
# Stasher automatiquement lors d'un rebase
git config --global rebase.autoStash true
```

Si vous avez des modifications non commitées, Git les mettra de côté automatiquement et les restaurera après le rebase.

---

### Cas pratiques détaillés

#### Cas 1 : Mise à jour simple de feature branch

```bash
# Situation : main a avancé pendant que vous travailliez
git switch feature-payment
git log --oneline --graph --all
# * e5f6g7h (feature-payment) Add payment form
# * d4e5f6g Add payment logic
# | * c3d4e5f (main) Security update
# | * b2c3d4e Bug fix
# |/
# * a1b2c3d Base

# Rebaser sur main
git rebase main
# Successfully rebased

git log --oneline --graph --all
# * z9y8x7w (feature-payment) Add payment form
# * y8x7w6v Add payment logic
# * c3d4e5f (main) Security update
# * b2c3d4e Bug fix
# * a1b2c3d Base

# Maintenant feature-payment est à jour avec main
```

#### Cas 2 : Nettoyer avant Pull Request

```bash
# Historique local chaotique
git log --oneline
# h7i8j9k Final fix
# g7h8i9j More changes
# f6g7h8i Oops
# e5f6g7h WIP
# d4e5f6g Start feature

# Nettoyer avec rebase interactif
git rebase -i HEAD~5

# Dans l'éditeur
# pick d4e5f6g Start feature
# fixup e5f6g7h WIP
# fixup f6g7h8i Oops
# squash g7h8i9j More changes
# fixup h7i8j9k Final fix

# Résultat : 2 commits propres
git log --oneline
# a1b2c3d Implementation complete
# c3d4e5f Start feature

# Pousser proprement
git push origin feature-clean
```

#### Cas 3 : Résolution de conflit pendant rebase

```bash
git rebase main
# CONFLICT in config.js

# Résoudre
code config.js
# ... éditer ...

git add config.js
git rebase --continue

# Nouveau conflit dans utils.js
# CONFLICT in utils.js

code utils.js
git add utils.js
git rebase --continue

# Terminé
# Successfully rebased
```

#### Cas 4 : Abandon de rebase problématique

```bash
git rebase main
# CONFLICT in 5 files...
# Trop compliqué !

git rebase --abort
# HEAD is now at d4e5f6g

# Utiliser merge à la place
git merge main
# Plus simple à gérer
```

---

### Erreurs courantes et solutions

#### Erreur 1 : Rebaser une branche publique

```bash
# ❌ Erreur
git switch main
git rebase feature
git push --force origin main
# Toute l'équipe a des problèmes !

# ✅ Solution : Ne jamais faire ça
# Utiliser merge pour les branches publiques
```

#### Erreur 2 : Oublier de continuer après résolution

```bash
git rebase main
# CONFLICT...

# Résoudre le conflit
git add fichier.js

# ❌ Oublier de continuer
# Le rebase reste en suspens

# ✅ Toujours continuer
git rebase --continue
```

#### Erreur 3 : Perdre des commits

```bash
# Pendant un rebase interactif, vous supprimez un commit important
# par erreur avec 'drop'

# ✅ Récupération avec reflog
git reflog
# Trouver le commit perdu
git cherry-pick a1b2c3d
```

#### Erreur 4 : Rebase avec modifications non commitées

```bash
git rebase main
# error: cannot rebase: You have unstaged changes.

# ✅ Solution 1 : Commiter
git commit -am "WIP"
git rebase main

# ✅ Solution 2 : Stasher
git stash
git rebase main
git stash pop

# ✅ Solution 3 : Activer autostash
git config rebase.autoStash true
git rebase main  # Stash automatique
```

---

### Commandes essentielles

```bash
# REBASE DE BASE
git rebase branche              # Rebaser sur une branche
git rebase main                 # Rebaser sur main

# REBASE INTERACTIF
git rebase -i HEAD~5            # 5 derniers commits
git rebase -i main              # Tous les commits depuis main

# PENDANT UN REBASE
git rebase --continue           # Continuer après résolution
git rebase --skip               # Sauter le commit actuel
git rebase --abort              # Annuler le rebase

# CONFIGURATION
git config pull.rebase true     # Pull avec rebase par défaut
git config rebase.autoStash true # Autostash pendant rebase

# VÉRIFICATION
git log --oneline --graph       # Voir l'historique
git reflog                      # Voir les anciens états
```

---

### Points clés à retenir

✅ **Rebase réécrit l'historique** pour créer une ligne droite

✅ **Ne JAMAIS rebaser des commits publics** partagés avec l'équipe

✅ **Rebase = rejouer des commits** sur une nouvelle base

✅ **Merge préserve, rebase réécrit** l'historique

✅ **Rebase interactif** nettoie l'historique avant partage

✅ **Conflits pendant rebase** : résoudre puis `--continue`

✅ **`git rebase --abort`** annule tout en cas de problème

✅ **Rebase pour branches locales**, merge pour branches publiques

---

### Conclusion

Le rebase est un outil puissant qui permet de maintenir un historique Git propre et linéaire. Utilisé correctement, il améliore grandement la lisibilité de l'historique. Cependant, il nécessite de la prudence et de la discipline.

**Règles d'or du rebase :**

1. **Jamais sur des commits publics** - Réservé aux commits locaux non partagés
2. **Toujours tester après** - Un rebase peut introduire des bugs subtils
3. **Préférer merge en cas de doute** - La sûreté avant tout
4. **Communiquer avec l'équipe** - Si vous rebasez, informez les autres

**Quand utiliser quoi :**
- **Merge** : Par défaut, toujours sûr
- **Rebase** : Pour nettoyer l'historique local, mettre à jour une feature branch

Avec la pratique, vous développerez une intuition pour savoir quand utiliser merge ou rebase. Et rappelez-vous : en cas de doute, le merge est toujours le choix le plus sûr !

Dans la section suivante, nous comparerons en détail **Rebase vs Merge** pour vous aider à choisir la meilleure approche selon les situations.

⏭️ [Rebase vs Merge : la règle d'or et quand choisir](/module-04-travailler-avec-les-branches/08-rebase-vs-merge.md)
