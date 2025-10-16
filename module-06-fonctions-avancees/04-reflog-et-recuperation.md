🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 4. Reflog : récupérer des commits perdus

### Introduction

Avez-vous déjà supprimé accidentellement quelque chose d'important et souhaité avoir une machine à remonter le temps ? Avec Git, cette machine existe : c'est le **reflog** !

Le **reflog** (reference log) est un journal secret que Git garde en coulisses. Il enregistre **chaque mouvement** de votre pointeur HEAD : chaque commit, chaque changement de branche, chaque reset, chaque rebase. C'est comme un historique de navigation qui conserve tout ce que vous avez fait, même ce qui a été "supprimé".

**La bonne nouvelle :** Avec le reflog, presque rien n'est vraiment perdu dans Git ! Tant que vous avez fait un commit à un moment donné, vous pouvez le récupérer.

---

### Qu'est-ce que le reflog ?

Le reflog est un **journal local** qui enregistre chaque changement de position de HEAD et des branches. C'est votre filet de sécurité personnel.

**Important à comprendre :**

- Le reflog est **local** : il n'est pas partagé avec les autres développeurs
- Le reflog garde l'historique pendant **90 jours** par défaut
- Chaque action importante est enregistrée avec un numéro de référence

**Ce que le reflog enregistre :**

- Les commits (`git commit`)
- Les changements de branche (`git checkout`, `git switch`)
- Les reset (`git reset`)
- Les merge (`git merge`)
- Les rebase (`git rebase`)
- Les cherry-pick (`git cherry-pick`)
- Les pull (`git pull`)
- Et bien plus !

---

### Afficher le reflog

#### Commande de base

```bash
git reflog
```

ou (forme plus explicite) :

```bash
git reflog show HEAD
```

**Exemple de sortie :**

```
a1b2c3d HEAD@{0}: commit: Add payment feature
e4f5g6h HEAD@{1}: commit: Add login feature
h7i8j9k HEAD@{2}: checkout: moving from main to feature/login
k0l1m2n HEAD@{3}: reset: moving to HEAD~1
n3o4p5q HEAD@{4}: commit: WIP broken code
q6r7s8t HEAD@{5}: commit: Fix bug in header
```

**Comment lire le reflog :**

- **Colonne 1** : Hash du commit
- **Colonne 2** : Référence reflog (HEAD@{0} = plus récent, HEAD@{5} = plus ancien)
- **Colonne 3** : Action effectuée et description

Le reflog se lit **de haut en bas**, du plus récent au plus ancien.

---

### Comprendre les références HEAD@{n}

La notation `HEAD@{n}` représente la position de HEAD il y a n actions.

- `HEAD@{0}` : Où est HEAD **maintenant**
- `HEAD@{1}` : Où était HEAD à l'action **précédente**
- `HEAD@{2}` : Où était HEAD il y a **2 actions**
- Et ainsi de suite...

**Exemple visuel :**

```
Action actuelle   → HEAD@{0}: commit: Add feature C
Action précédente → HEAD@{1}: commit: Add feature B
Il y a 2 actions  → HEAD@{2}: commit: Add feature A
Il y a 3 actions  → HEAD@{3}: checkout: moving from main to feature
```

---

### Cas d'usage : Récupérer des commits perdus

#### Cas 1 : Reset accidentel avec --hard

**Situation :** Vous avez fait `git reset --hard` et supprimé des commits importants.

```bash
# Vous étiez ici avec 3 commits
git log --oneline
# c3d4e5f Add important feature
# b2c3d4e Fix critical bug
# a1b2c3d Update README

# Oups ! Reset accidentel
git reset --hard HEAD~3
# Les 3 commits ont disparu !

git log --oneline
# ... plus de traces des commits récents
```

**Solution avec reflog :**

```bash
# 1. Consulter le reflog
git reflog
# c3d4e5f HEAD@{0}: reset: moving to HEAD~3
# c3d4e5f HEAD@{1}: commit: Add important feature
# b2c3d4e HEAD@{2}: commit: Fix critical bug
# a1b2c3d HEAD@{3}: commit: Update README

# 2. Récupérer l'état avant le reset
git reset --hard HEAD@{1}
# ou
git reset --hard c3d4e5f

# 3. Vérifier
git log --oneline
# c3d4e5f Add important feature
# b2c3d4e Fix critical bug
# a1b2c3d Update README
# ✅ Tout est revenu !
```

#### Cas 2 : Branche supprimée accidentellement

**Situation :** Vous avez supprimé une branche qui contenait du travail important.

```bash
# Vous aviez une branche feature/important
git branch -D feature/important
# Deleted branch feature/important (was x9y8z7w)

# Oh non ! J'en avais encore besoin !
```

**Solution avec reflog :**

```bash
# 1. Consulter le reflog
git reflog
# a1b2c3d HEAD@{0}: checkout: moving from feature/important to main
# x9y8z7w HEAD@{1}: commit: Last commit on feature/important
# w8x9y0z HEAD@{2}: commit: Another commit

# 2. Recréer la branche au bon endroit
git branch feature/important x9y8z7w
# ou
git checkout -b feature/important HEAD@{1}

# ✅ La branche est de retour avec tout son contenu !
```

#### Cas 3 : Rebase qui a mal tourné

**Situation :** Vous avez fait un rebase interactif qui a créé des conflits ou supprimé des commits importants.

```bash
# Avant le rebase
git log --oneline
# e5f6g7h Commit C
# d4e5f6g Commit B
# c3d4e5f Commit A

# Rebase interactif
git rebase -i HEAD~3
# ... quelque chose se passe mal ...
# Vous abandonnez avec git rebase --abort, mais des commits semblent perdus
```

**Solution avec reflog :**

```bash
# 1. Voir ce qui s'est passé
git reflog
# a1b2c3d HEAD@{0}: rebase: aborting
# x9y8z7w HEAD@{1}: rebase: checkout HEAD~3
# e5f6g7h HEAD@{2}: commit: Commit C
# d4e5f6g HEAD@{3}: commit: Commit B
# c3d4e5f HEAD@{4}: commit: Commit A

# 2. Revenir à l'état avant le rebase
git reset --hard HEAD@{2}
# ou
git reset --hard e5f6g7h

# ✅ Retour à l'état stable d'avant le rebase
```

#### Cas 4 : Commit --amend qui a écrasé du travail

**Situation :** Vous avez fait `git commit --amend` et écrasé un commit important.

```bash
# Vous aviez un bon commit
git commit -m "Add complete feature with tests"
# Commit créé : abc123

# Plus tard, vous faites accidentellement
git commit --amend -m "Quick fix"
# Le commit abc123 est remplacé !
```

**Solution avec reflog :**

```bash
# 1. Trouver le commit original
git reflog
# def456 HEAD@{0}: commit (amend): Quick fix
# abc123 HEAD@{1}: commit: Add complete feature with tests

# 2. Voir le contenu du commit original
git show abc123

# 3. Si vous voulez le récupérer, plusieurs options :

# Option A : Reset vers l'ancien commit
git reset --hard abc123

# Option B : Cherry-pick l'ancien commit
git cherry-pick abc123

# Option C : Créer une nouvelle branche avec l'ancien commit
git branch backup-feature abc123
```

#### Cas 5 : Changement de branche avec travail non sauvegardé

**Situation :** Vous avez changé de branche et vos modifications ont "disparu".

```bash
# Sur feature/login avec des modifications
git checkout main
# Switched to branch 'main'

# Où sont mes modifications ?!
```

**Solution avec reflog :**

```bash
# 1. Consulter le reflog
git reflog
# k0l1m2n HEAD@{0}: checkout: moving from feature/login to main
# h7i8j9k HEAD@{1}: commit: Work in progress on login

# 2. Retourner sur la branche
git checkout feature/login

# 3. Si la branche a été perdue, la recréer
git checkout -b feature/login h7i8j9k
```

---

### Afficher le reflog d'une branche spécifique

Par défaut, `git reflog` montre l'historique de HEAD, mais vous pouvez voir le reflog de n'importe quelle branche :

```bash
git reflog show main
git reflog show feature/login
git reflog show origin/main
```

**Exemple :**

```bash
git reflog show feature/login
# x9y8z7w feature/login@{0}: commit: Add validation
# w8x9y0z feature/login@{1}: commit: Add form
# v7w8x9y feature/login@{2}: branch: Created from main
```

---

### Reflog avec options de formatage

#### Afficher plus de détails

```bash
git reflog show --all
```

Montre le reflog de toutes les références (HEAD, branches, tags).

#### Limiter le nombre d'entrées

```bash
git reflog -n 10
```

Affiche seulement les 10 dernières entrées.

#### Afficher avec les dates

```bash
git reflog --date=relative
```

**Exemple de sortie :**

```
a1b2c3d HEAD@{2 minutes ago}: commit: Add feature
e4f5g6h HEAD@{1 hour ago}: commit: Fix bug
h7i8j9k HEAD@{3 hours ago}: checkout: moving from main to feature
```

```bash
git reflog --date=iso
```

**Exemple de sortie :**

```
a1b2c3d HEAD@{2024-10-15 14:30:45 +0200}: commit: Add feature
```

---

### Nettoyer le reflog

Le reflog peut devenir long. Voici comment le gérer.

#### Expirer les anciennes entrées

Par défaut, Git garde le reflog pendant 90 jours. Vous pouvez changer cette durée :

```bash
# Expirer les entrées plus vieilles que 30 jours
git reflog expire --expire=30.days.ago --all

# Expirer tout ce qui est inaccessible maintenant
git reflog expire --expire=now --all
```

#### Vérifier la taille du reflog

```bash
git reflog show --all | wc -l
```

---

### Récupérer un commit vraiment perdu

Parfois, même avec le reflog, vous ne trouvez pas votre commit. Voici la commande ultime :

```bash
git fsck --lost-found
```

Cette commande recherche tous les objets Git "pendants" (non référencés). Elle affiche les commits qui ne sont plus accessibles par aucune branche ni le reflog.

**Exemple :**

```bash
git fsck --lost-found
# dangling commit a1b2c3d4e5f6...
# dangling commit e4f5g6h7i8j9...

# Voir le contenu d'un commit dangling
git show a1b2c3d4e5f6

# Si c'est le bon, le récupérer
git branch recovered-branch a1b2c3d4e5f6
```

---

### Différence entre git log et git reflog

| `git log` | `git reflog` |
|-----------|--------------|
| Montre l'historique des commits | Montre l'historique des actions |
| Suit la structure de l'arbre des commits | Suit les mouvements de HEAD |
| Ne montre que ce qui est accessible | Montre tout, même ce qui est "supprimé" |
| Partagé entre tous les développeurs | Local à votre machine |
| Ne peut pas récupérer des commits perdus | Peut récupérer des commits perdus |

**Analogie :**

- `git log` = La table des matières d'un livre (structure visible)
- `git reflog` = Votre historique de navigation dans le livre (tout ce que vous avez fait)

---

### Scénario complet de sauvetage

Voici un exemple réaliste et complet :

```bash
# Vous travaillez sur une fonctionnalité importante
git checkout -b feature/payment
git commit -m "Add payment processing"
git commit -m "Add payment validation"
git commit -m "Add payment tests"

# Vous revenez sur main
git checkout main

# Vous supprimez la branche par erreur
git branch -D feature/payment
# ❌ Oh non !

# PANIQUE ! Mais le reflog est là...

# 1. Regarder le reflog pour trouver le dernier commit de la branche
git reflog
# a1b2c3d HEAD@{0}: checkout: moving from feature/payment to main
# x9y8z7w HEAD@{1}: commit: Add payment tests
# w8x9y0z HEAD@{2}: commit: Add payment validation
# v7w8x9y HEAD@{3}: commit: Add payment processing
# u6v7w8x HEAD@{4}: checkout: moving from main to feature/payment

# 2. Le dernier commit de feature/payment était x9y8z7w
# Recréer la branche exactement comme elle était
git checkout -b feature/payment x9y8z7w

# 3. Vérifier que tout est là
git log --oneline
# x9y8z7w Add payment tests
# w8x9y0z Add payment validation
# v7w8x9y Add payment processing

# ✅ Sauvé ! Tout est revenu.
```

---

### Conseils et bonnes pratiques

✅ **Consultez le reflog dès qu'un problème survient** : C'est votre premier réflexe de sauvetage

✅ **Utilisez les dates pour vous repérer** : `git reflog --date=relative` aide à trouver rapidement

✅ **Créez des branches de backup avant des opérations risquées** :

```bash
git branch backup-$(date +%Y%m%d)
git rebase -i HEAD~10
```

✅ **Le reflog est local** : N'oubliez pas que vos collègues n'ont pas accès à votre reflog

✅ **90 jours de sécurité** : Vous avez largement le temps de récupérer des erreurs

❌ **Ne comptez pas sur le reflog éternellement** : Après 90 jours, les entrées disparaissent

❌ **Le reflog ne sauve pas ce qui n'a jamais été commité** : Faites des commits régulièrement

❌ **Attention au git gc** : Cette commande peut nettoyer des objets inaccessibles

---

### Commandes utiles combinées avec reflog

#### Créer une branche de sauvegarde depuis le reflog

```bash
git branch backup HEAD@{5}
```

#### Comparer l'état actuel avec un ancien état

```bash
git diff HEAD@{2}
```

#### Voir les fichiers modifiés à un moment donné

```bash
git show HEAD@{3} --name-only
```

#### Reset à un état précis

```bash
git reset --hard HEAD@{1}
```

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git reflog` | Affiche l'historique de HEAD |
| `git reflog show <branche>` | Affiche l'historique d'une branche |
| `git reflog --date=relative` | Affiche avec dates relatives |
| `git reflog -n 20` | Affiche les 20 dernières entrées |
| `git reset --hard HEAD@{n}` | Revenir à un état précédent |
| `git branch <nom> HEAD@{n}` | Créer une branche depuis le reflog |
| `git show HEAD@{n}` | Voir le contenu d'un état |
| `git diff HEAD@{n}` | Comparer avec un état ancien |
| `git fsck --lost-found` | Trouver les commits vraiment perdus |
| `git reflog expire --expire=now --all` | Nettoyer le reflog |

---

### Quand le reflog ne peut pas vous sauver

Le reflog est puissant, mais il a des limites :

❌ **Modifications non commitées** : Si vous n'avez jamais fait `git add` et `git commit`, le reflog ne peut rien faire

❌ **Après 90 jours** : Les anciennes entrées sont automatiquement supprimées

❌ **Après git gc agressif** : Si le garbage collector a nettoyé les objets

❌ **Sur une autre machine** : Le reflog est local, pas sur le serveur distant

**Solution :** Commitez régulièrement, même avec des messages "WIP" !

---

### Le reflog comme outil de débogage

Le reflog n'est pas seulement pour récupérer des erreurs. C'est aussi un excellent outil pour comprendre ce qui s'est passé :

```bash
# Quelqu'un a modifié la branche main, mais qui et quand ?
git reflog show main --date=iso

# Qu'ai-je fait dans les 2 dernières heures ?
git reflog --date=relative | head -20

# Quand ai-je fait mon dernier commit ?
git reflog | grep "commit:" | head -1
```

---

### Conclusion

Le **reflog** est votre filet de sécurité ultime avec Git. C'est la preuve que "rien n'est vraiment perdu" tant que vous avez fait un commit.

**Points clés à retenir :**

- Le reflog enregistre **toutes** vos actions pendant 90 jours
- Utilisez `git reflog` pour voir votre historique d'actions
- Utilisez `HEAD@{n}` pour référencer des états passés
- `git reset --hard HEAD@{n}` pour revenir en arrière
- Le reflog est **local** et ne se partage pas
- Presque tout est récupérable tant que ça a été commité

**La règle d'or :** En cas de doute ou de panique avec Git, votre premier réflexe doit être :

```bash
git reflog
```

Avec le reflog, vous avez le pouvoir de remonter le temps et de corriger presque toutes les erreurs. C'est votre superpouvoir Git !

---

**Prochaine étape :** Module 6.5 - Déboguer avec git bisect

⏭️ [Déboguer avec git bisect](/module-06-fonctions-avancees/05-deboger-avec-git-bisect.md)
