🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 2. Cherry-pick : appliquer des commits spécifiques

### Introduction

Imaginez que vous avez un panier rempli de différents fruits (vos commits), et que vous voulez sélectionner uniquement une cerise spécifique pour la mettre dans un autre panier. C'est exactement ce que fait **git cherry-pick** !

**Git cherry-pick** vous permet de copier un ou plusieurs commits spécifiques d'une branche et de les appliquer sur une autre branche, sans avoir à fusionner toute la branche. C'est comme dire : "Je veux uniquement ce commit précis, pas tout le reste".

---

### Pourquoi utiliser cherry-pick ?

Cherry-pick est particulièrement utile dans ces situations :

- **Correction de bug** : Vous avez corrigé un bug sur la branche `develop`, mais vous devez appliquer ce même correctif sur `main` sans merger toute la branche
- **Récupération de commit** : Vous avez fait un commit sur la mauvaise branche et voulez le déplacer
- **Fonctionnalité isolée** : Vous voulez intégrer une fonctionnalité spécifique d'une branche sans prendre le reste
- **Backporting** : Appliquer un correctif d'une version récente vers une version plus ancienne

---

### La commande de base

#### Syntaxe

```bash
git cherry-pick <hash-du-commit>
```

Le hash du commit est l'identifiant unique que vous trouvez avec `git log`.

#### Exemple simple

```bash
# Vous êtes sur la branche main
git log --oneline
# a1b2c3d Fix typo in header
# e4f5g6h Add footer
# i7j8k9l Initial commit

# Vous voulez récupérer le commit de la branche feature
git log feature --oneline
# x9y8z7w Add new analytics feature  ← Vous voulez ce commit
# m6n5o4p Update README
# a1b2c3d Fix typo in header

# Appliquer le commit x9y8z7w sur votre branche actuelle (main)
git cherry-pick x9y8z7w
```

Après cette commande, le commit `x9y8z7w` est copié et appliqué sur votre branche `main` avec un **nouveau hash** (car c'est un nouveau commit).

---

### Comprendre ce qui se passe

Quand vous faites un cherry-pick :

1. Git prend les modifications introduites par le commit spécifié
2. Il tente de les appliquer sur votre branche actuelle
3. Un nouveau commit est créé avec les mêmes modifications mais un hash différent
4. Le commit original reste inchangé sur sa branche d'origine

**Schéma conceptuel :**

```
Avant cherry-pick :

main:        A---B---C
                      ↑ vous êtes ici

feature:     A---B---D---E
                         ↑ vous voulez E

Après git cherry-pick E :

main:        A---B---C---E'
                         ↑ copie de E avec nouveau hash

feature:     A---B---D---E
                         ↑ E original inchangé
```

---

### Cherry-pick de plusieurs commits

#### Plusieurs commits consécutifs

```bash
git cherry-pick <premier-commit>..<dernier-commit>
```

**Attention :** Cette syntaxe n'inclut **pas** le premier commit, seulement ceux qui suivent jusqu'au dernier.

Pour inclure le premier commit :

```bash
git cherry-pick <premier-commit>^..<dernier-commit>
```

**Exemple :**

```bash
# Appliquer les commits de a1b2c3d à x9y8z7w (a1b2c3d non inclus)
git cherry-pick a1b2c3d..x9y8z7w

# Appliquer les commits de a1b2c3d à x9y8z7w (a1b2c3d inclus)
git cherry-pick a1b2c3d^..x9y8z7w
```

#### Plusieurs commits non consécutifs

```bash
git cherry-pick <commit1> <commit2> <commit3>
```

**Exemple :**

```bash
git cherry-pick a1b2c3d x9y8z7w m6n5o4p
```

---

### Options utiles

#### -n ou --no-commit : Appliquer sans commiter

```bash
git cherry-pick -n <hash-du-commit>
```

Les modifications sont appliquées dans votre Working Directory et Staging Area, mais aucun commit n'est créé automatiquement. Utile si vous voulez modifier quelque chose avant de commiter.

**Exemple :**

```bash
git cherry-pick -n x9y8z7w
# Les modifications sont appliquées
git status
# Changes to be committed...

# Vous pouvez modifier des fichiers si nécessaire
# Puis commiter manuellement
git commit -m "Ajout analytics avec modifications"
```

#### -e ou --edit : Modifier le message de commit

```bash
git cherry-pick -e <hash-du-commit>
```

Ouvre votre éditeur pour modifier le message du commit avant de l'appliquer.

#### -x : Ajouter une référence au commit original

```bash
git cherry-pick -x <hash-du-commit>
```

Ajoute automatiquement une ligne dans le message de commit indiquant la provenance :

```
(cherry picked from commit a1b2c3d4e5f6...)
```

Très utile pour la traçabilité !

---

### Cas d'usage pratiques

#### Cas 1 : Hotfix urgent sur production

```bash
# Vous êtes sur develop et corrigez un bug
git checkout develop
# ... corrections dans fichier.js ...
git add fichier.js
git commit -m "Fix: correction calcul prix"
# Commit créé : a1b2c3d

# Le bug existe aussi en production (branche main)
git checkout main
git cherry-pick a1b2c3d
# Le correctif est maintenant aussi sur main !

git push origin main
```

#### Cas 2 : Commit sur la mauvaise branche

```bash
# Oups, vous avez commité sur main au lieu de feature
git log --oneline
# e5f6g7h Ajout nouvelle fonctionnalité  ← mauvaise branche !
# a1b2c3d Commit précédent

# Créer ou aller sur la bonne branche
git checkout -b feature/nouvelle-fonctionnalite

# Récupérer le commit
git cherry-pick e5f6g7h

# Retourner sur main et annuler le commit
git checkout main
git reset --hard HEAD~1
# Le commit est maintenant seulement sur la branche feature
```

#### Cas 3 : Backport vers une ancienne version

```bash
# Vous avez corrigé un bug sur la version 2.0 (branche v2.0)
git checkout v2.0
# Commit de correction : x9y8z7w

# Le bug existe aussi sur la version 1.5 encore maintenue
git checkout v1.5
git cherry-pick x9y8z7w

# La correction est maintenant aussi sur v1.5
```

#### Cas 4 : Sélectionner des features d'une branche

```bash
# La branche feature/dashboard a plusieurs commits
git log feature/dashboard --oneline
# z9y8x7w Ajout graphique performance  ← je veux celui-ci
# w6v5u4t Ajout graphique utilisateurs  ← je veux celui-ci
# t3s2r1q Refonte complète CSS        ← je ne veux pas celui-ci
# q9p8o7n Initial dashboard

# Sur main, récupérer seulement les commits voulus
git checkout main
git cherry-pick z9y8x7w w6v5u4t
```

---

### Gérer les conflits

Comme pour un merge, un cherry-pick peut créer des conflits si les mêmes lignes ont été modifiées différemment.

#### Quand un conflit survient

```bash
git cherry-pick a1b2c3d
# error: could not apply a1b2c3d... Ajout feature
# hint: after resolving the conflicts, mark them with
# hint: "git add/rm <files>", then run "git cherry-pick --continue"
```

#### Résoudre le conflit

```bash
# 1. Ouvrir les fichiers en conflit et les corriger manuellement
# Les marqueurs de conflit ressemblent à ceci :
# <<<<<<< HEAD
# Code de votre branche actuelle
# =======
# Code du commit cherry-pické
# >>>>>>> a1b2c3d (Ajout feature)

# 2. Une fois les conflits résolus
git add fichier-corrige.js

# 3. Continuer le cherry-pick
git cherry-pick --continue
```

#### Annuler le cherry-pick

Si vous changez d'avis pendant la résolution :

```bash
git cherry-pick --abort
```

Tout revient à l'état d'avant le cherry-pick.

---

### Cherry-pick vs Merge : Quelle différence ?

| **Cherry-pick** | **Merge** |
|-----------------|-----------|
| Copie des commits **spécifiques** | Fusionne **toute** la branche |
| Crée de **nouveaux commits** | Préserve l'historique original |
| Sélectif et précis | Global et complet |
| Bon pour corrections ponctuelles | Bon pour intégrer des fonctionnalités complètes |
| Peut dupliquer l'historique | Garde un historique propre |

**Exemple visuel :**

```
Cherry-pick :
feature:  A---B---C---D
main:     A---E---F---C'  (seulement C est copié)

Merge :
feature:  A---B---C---D
               \       \
main:     A---E---F-----M  (tout B, C, D est intégré)
```

---

### Précautions et bonnes pratiques

✅ **Utilisez cherry-pick pour des corrections** : bugs, typos, petits correctifs

✅ **Ajoutez -x pour la traçabilité** : `git cherry-pick -x` pour garder la référence au commit original

✅ **Privilégiez le merge pour des features complètes** : ne cherry-pickez pas une série de 20 commits, faites plutôt un merge

✅ **Documentez vos cherry-picks** : expliquez pourquoi vous avez cherry-pické dans le message de commit

❌ **Évitez de cherry-pick sur des branches publiques partagées** : cela peut créer de la confusion et des commits dupliqués

❌ **N'abusez pas du cherry-pick** : trop de cherry-picks peuvent rendre l'historique difficile à suivre

❌ **Ne cherry-pickez pas de gros refactorings** : ils dépendent souvent de commits précédents

---

### Cherry-pick avec rebase

Parfois, cherry-pick est utilisé automatiquement lors d'un rebase. En fait, `git rebase` utilise en interne un mécanisme similaire à cherry-pick pour réappliquer vos commits.

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git cherry-pick <hash>` | Applique un commit spécifique |
| `git cherry-pick <hash1> <hash2>` | Applique plusieurs commits non consécutifs |
| `git cherry-pick <hash1>..<hash2>` | Applique une série de commits (sans le premier) |
| `git cherry-pick <hash1>^..<hash2>` | Applique une série de commits (avec le premier) |
| `git cherry-pick -n <hash>` | Applique sans commiter automatiquement |
| `git cherry-pick -e <hash>` | Permet de modifier le message du commit |
| `git cherry-pick -x <hash>` | Ajoute une référence au commit original |
| `git cherry-pick --continue` | Continue après résolution de conflits |
| `git cherry-pick --abort` | Annule le cherry-pick en cours |
| `git cherry-pick --skip` | Saute le commit en conflit et continue |

---

### Vérifier le résultat

Après un cherry-pick, vérifiez que tout s'est bien passé :

```bash
# Voir le dernier commit créé
git log -1

# Vérifier les modifications
git show

# Comparer avec le commit original
git show <hash-original>
```

---

### Exemple complet pas à pas

Voici un scénario complet pour bien comprendre :

```bash
# 1. Créer une situation de départ
git checkout -b feature/new-button
echo "Button component" > button.js
git add button.js
git commit -m "Add button component"
# Commit créé : abc123

echo "Button styles" > button.css
git add button.css
git commit -m "Add button styles"
# Commit créé : def456

# 2. Retourner sur main
git checkout main

# 3. Cherry-picker seulement le premier commit
git cherry-pick abc123

# 4. Vérifier
git log --oneline
# xyz789 Add button component  (nouveau hash !)
# ... autres commits de main

# 5. Vérifier le fichier
ls
# button.js existe maintenant sur main

# 6. Vérifier que button.css n'existe pas (non cherry-pické)
ls button.css
# No such file or directory
```

---

### Quand NE PAS utiliser cherry-pick

- Quand vous voulez intégrer une fonctionnalité complète → Utilisez `git merge`
- Quand les commits dépendent les uns des autres → Mergez toute la branche
- Quand vous travaillez en équipe sur une branche partagée → Préférez le merge pour éviter la duplication
- Quand l'historique devient complexe → Simplifiez votre workflow plutôt que de multiplier les cherry-picks

---

### Conclusion

**Git cherry-pick** est un outil puissant qui vous donne un contrôle précis sur quels commits appliquer et où. C'est particulièrement utile pour :

- Les correctifs urgents à appliquer sur plusieurs branches
- La récupération de commits mal placés
- La sélection de fonctionnalités spécifiques

Retenez l'essentiel :
- `git cherry-pick <hash>` pour copier un commit
- Utilisez `-x` pour garder une trace du commit original
- Privilégiez le merge pour des intégrations complètes
- Cherry-pick crée un **nouveau** commit avec un nouveau hash

Avec la pratique, vous saurez instinctivement quand utiliser cherry-pick plutôt que merge !

---

**Prochaine étape :** Module 6.3 - Nettoyer l'historique avant de partager (rebase interactif)

⏭️ [Nettoyer l'historique avant de partager (rebase interactif)](/module-06-fonctions-avancees/03-nettoyer-historique-rebase-i.md)
