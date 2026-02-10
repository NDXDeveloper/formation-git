🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 8 : Dépannage et résolution de problèmes

## 2. Annuler un merge ou un rebase

### Introduction

Il arrive parfois qu'un merge ou un rebase se passe mal, ou que vous réalisiez après coup que ce n'était pas la bonne décision. Bonne nouvelle : Git permet d'annuler ces opérations ! Dans cette section, nous allons voir comment revenir en arrière en toute sécurité.

### Partie 1 : Annuler un merge

#### Scénario 1 : Annuler un merge en cours (avant le commit final)

Si vous êtes **en plein milieu d'un merge** qui pose des problèmes (conflits compliqués, erreur de manipulation, etc.), vous pouvez l'abandonner complètement :

```bash
git merge --abort
```

Cette commande :
- Annule le merge en cours
- Restaure votre dépôt à l'état exact d'avant le merge
- Ne laisse aucune trace du merge tenté

**Quand l'utiliser ?**
- Vous êtes confronté à des conflits trop complexes
- Vous réalisez que vous avez mergé la mauvaise branche
- Vous voulez simplement recommencer le merge plus tard

**Exemple concret :**
```bash
# Vous commencez un merge
git merge feature-nouvelle-fonction

# Git vous signale des conflits
# Auto-merging fichier.js
# CONFLICT (content): Merge conflict in fichier.js

# Vous décidez d'abandonner
git merge --abort

# ✅ Vous êtes revenu à l'état d'avant le merge
```

#### Scénario 2 : Annuler un merge déjà terminé (non partagé)

Si vous avez **déjà terminé le merge** (le commit de merge existe) mais que vous ne l'avez **pas encore partagé** avec d'autres (pas fait de `git push`), vous pouvez utiliser `git reset` :

```bash
# Revenir au commit juste avant le merge
git reset --hard HEAD~1
```

**Visualisation :**

Avant l'annulation :
```
A---B---C---D (main)
     \     /
      E---F (feature)

D = commit de merge
```

Après `git reset --hard HEAD~1` :
```
A---B---C (main)
     \
      E---F (feature)

Le commit D a disparu, main est revenu à C
```

**⚠️ Attention :** `--hard` supprime aussi vos modifications non commitées. Si vous voulez les garder, utilisez :
```bash
git reset --soft HEAD~1  # Annule le merge mais garde les modifications
```

#### Scénario 3 : Annuler un merge déjà partagé (avec push)

Si vous avez **déjà partagé le merge** avec d'autres personnes (fait un `git push`), vous ne devez **pas** utiliser `git reset` car cela réécrirait l'historique public.

À la place, utilisez `git revert` pour créer un nouveau commit qui annule les effets du merge :

```bash
# Créer un commit qui annule le merge
git revert -m 1 HEAD
```

**Explication du `-m 1` :**
Un commit de merge a deux "parents" (les deux branches mergées). Le `-m 1` indique à Git que vous voulez revenir au premier parent (généralement votre branche principale).

**Visualisation :**

Avant le revert :
```
A---B---C---M (main)
     \     /
      D---E (feature)

M = commit de merge
```

Après `git revert -m 1 HEAD` :
```
A---B---C---M---R (main)
     \     /
      D---E (feature)

R = commit de revert (annule M)
```

**Important :** L'historique n'est pas modifié, on ajoute simplement un nouveau commit qui défait les changements du merge.

#### Annuler un merge ancien

Si le merge que vous voulez annuler n'est pas le tout dernier commit, utilisez le hash du commit de merge :

```bash
# Trouver le hash du commit de merge
git log --oneline --graph

# Annuler ce merge spécifique
git revert -m 1 abc1234
```

### Partie 2 : Annuler un rebase

#### Scénario 1 : Annuler un rebase en cours

Si vous êtes **en plein rebase** et que vous voulez l'abandonner :

```bash
git rebase --abort
```

Cette commande :
- Arrête le rebase en cours
- Restaure votre branche à son état d'avant le rebase
- Annule tous les commits déjà appliqués pendant le rebase

**Exemple :**
```bash
# Vous commencez un rebase
git rebase main

# Des conflits apparaissent
# CONFLICT (content): Merge conflict in app.js

# Vous décidez d'abandonner
git rebase --abort

# ✅ Vous êtes revenu à l'état d'avant le rebase
```

#### Scénario 2 : Annuler un rebase déjà terminé

Si le rebase est **déjà terminé** mais que vous réalisez que c'était une erreur, vous pouvez utiliser `reflog` pour retrouver l'état d'avant le rebase :

**Étape 1 : Consulter le reflog**
```bash
git reflog
```

Vous verrez quelque chose comme :
```
a1b2c3d HEAD@{0}: rebase finished: returning to refs/heads/feature  
a1b2c3d HEAD@{1}: rebase: commit 3  
e4f5g6h HEAD@{2}: rebase: commit 2  
i7j8k9l HEAD@{3}: rebase: commit 1  
m0n1o2p HEAD@{4}: rebase: checkout main  
q3r4s5t HEAD@{5}: commit: Mon dernier commit avant rebase
```

**Étape 2 : Identifier le commit d'avant le rebase**

Cherchez la ligne qui indique `rebase: checkout` ou le dernier commit avant le début du rebase (ici `q3r4s5t HEAD@{5}`).

**Étape 3 : Revenir à cet état**
```bash
git reset --hard q3r4s5t
```

Ou en utilisant directement la référence du reflog :
```bash
git reset --hard HEAD@{5}
```

**Visualisation :**

Avant le rebase :
```
A---B---C (main)
     \
      D---E---F (feature)
```

Après le rebase :
```
A---B---C (main)
         \
          D'---E'---F' (feature)
```

Après l'annulation avec reflog :
```
A---B---C (main)
     \
      D---E---F (feature)

Vous êtes revenu à l'état d'origine !
```

#### Scénario 3 : Annuler un rebase interactif

Si vous avez fait un `git rebase -i` (rebase interactif) et que vous voulez l'annuler, la procédure est la même :

1. Si le rebase est en cours : `git rebase --abort`
2. Si le rebase est terminé : utiliser `reflog` pour revenir en arrière

### Partie 3 : Cas particuliers et astuces

#### Rebase sur une branche distante

Si vous avez fait un rebase et déjà push avec `--force`, il est très difficile de revenir en arrière pour les autres collaborateurs. Dans ce cas :

1. **Prévenez votre équipe immédiatement**
2. **Utilisez le reflog pour retrouver l'ancien état**
3. **Restaurez l'ancienne version** :
   ```bash
   git reset --hard HEAD@{X}
   git push --force
   ```

#### Vérifier avant d'annuler

Avant d'annuler un merge ou un rebase, vérifiez bien ce que vous allez perdre :

```bash
# Voir les différences entre maintenant et l'état précédent
git diff HEAD HEAD@{1}

# Voir les commits qui seront affectés
git log HEAD@{1}..HEAD
```

#### Créer une branche de sauvegarde

Avant d'annuler quelque chose, créez une branche de sauvegarde au cas où :

```bash
# Créer une branche à partir de l'état actuel
git branch sauvegarde-avant-annulation

# Puis procéder à l'annulation
git reset --hard HEAD@{X}
```

Si vous changez d'avis, vous pourrez toujours revenir à la sauvegarde :
```bash
git checkout sauvegarde-avant-annulation
```

### Tableau récapitulatif

| Situation | Commande | Réécrit l'historique ? |
|-----------|----------|----------------------|
| Merge en cours | `git merge --abort` | Non (annule simplement) |
| Rebase en cours | `git rebase --abort` | Non (annule simplement) |
| Merge terminé (non partagé) | `git reset --hard HEAD~1` | Oui ⚠️ |
| Merge terminé (partagé) | `git revert -m 1 HEAD` | Non |
| Rebase terminé | `git reset --hard HEAD@{X}` | Oui ⚠️ |

### Règle d'or

**Si vous avez déjà partagé vos commits avec d'autres (après un `git push`), utilisez toujours `git revert` au lieu de `git reset`.**

Pourquoi ? Parce que `git reset` réécrit l'historique, ce qui créera des problèmes pour tous ceux qui ont déjà récupéré vos commits.

### Points clés à retenir

1. **Pendant l'opération :** Utilisez `--abort` pour annuler un merge ou rebase en cours
2. **Après l'opération (non partagée) :** Utilisez `git reset --hard` pour revenir en arrière
3. **Après l'opération (partagée) :** Utilisez `git revert` pour créer un commit d'annulation
4. **Le reflog est votre ami :** Il garde l'historique de tous vos mouvements pendant 30-90 jours
5. **Sauvegardez avant d'annuler :** Créez une branche de sauvegarde pour plus de sécurité

### En résumé

Annuler un merge ou un rebase n'est pas compliqué si vous connaissez les bonnes commandes. Le plus important est de :
- Savoir où vous en êtes (en cours ou terminé ?)
- Savoir si c'est partagé ou non
- Utiliser l'outil approprié (`--abort`, `reset` ou `revert`)

Et n'oubliez pas : le reflog est votre filet de sécurité ultime. Tant que vous n'avez pas fait de nettoyage de dépôt, vos commits sont récupérables !

⏭️ [Récupérer du travail perdu (avec reflog et reset)](/module-08-depannage/03-recuperer-travail-perdu.md)
