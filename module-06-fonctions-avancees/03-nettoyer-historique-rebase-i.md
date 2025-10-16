🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 3. Nettoyer l'historique avant de partager (rebase interactif)

### Introduction

Imaginez que vous écrivez un brouillon d'article avec plein de ratures, de corrections, de notes temporaires. Avant de le publier, vous le relisez et le nettoyez pour qu'il soit clair et professionnel. C'est exactement ce que fait le **rebase interactif** avec votre historique Git !

Le **rebase interactif** (`git rebase -i`) est un outil puissant qui vous permet de réécrire, réorganiser, fusionner et nettoyer vos commits **avant** de les partager avec votre équipe. C'est comme avoir une machine à remonter le temps pour perfectionner votre historique.

**Important :** Le rebase interactif modifie l'historique Git. Il ne doit être utilisé que sur des commits **non partagés** (non pushés sur un dépôt distant).

---

### Pourquoi nettoyer son historique ?

Un historique Git propre présente de nombreux avantages :

- **Facilite la revue de code** : Les reviewers comprennent mieux votre travail
- **Simplifie le débogage** : Chaque commit a un objectif clair
- **Améliore la documentation** : L'historique raconte une histoire cohérente
- **Facilite le cherry-pick** : Les commits isolés sont plus faciles à réutiliser
- **Professionnalisme** : Un historique soigné reflète un travail de qualité

**Exemple d'historique "sale" :**

```
a1b2c3d WIP
e4f5g6h Fix typo
h7i8j9k Ajout feature login
k0l1m2n Oops forgot file
n3o4p5q Fix another typo
q6r7s8t Actually fix the bug
```

**Même historique "propre" après rebase interactif :**

```
x9y8z7w Add login feature with authentication
```

---

### La commande de base

#### Syntaxe

```bash
git rebase -i <commit-de-base>
```

Le `<commit-de-base>` est le commit **parent** du premier commit que vous voulez modifier. Par exemple, pour modifier les 3 derniers commits :

```bash
git rebase -i HEAD~3
```

**Raccourcis utiles :**

- `HEAD~3` : Les 3 derniers commits
- `HEAD~5` : Les 5 derniers commits
- `abc123` : Tous les commits depuis le commit abc123 (non inclus)

---

### Comment fonctionne le rebase interactif

Quand vous lancez `git rebase -i HEAD~3`, Git ouvre votre éditeur de texte avec une liste de vos commits :

```
pick a1b2c3d Add login form
pick e4f5g6h Fix typo in login
pick h7i8j9k Add validation

# Rebase k0l1m2n..h7i8j9k onto k0l1m2n (3 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# d, drop = remove commit
```

Vous modifiez ce fichier pour indiquer ce que vous voulez faire avec chaque commit, puis vous sauvegardez et fermez l'éditeur. Git exécute ensuite vos instructions.

---

### Les commandes disponibles

#### 1. pick (p) - Garder le commit tel quel

```
pick a1b2c3d Add login form
```

Le commit est conservé sans modification. C'est l'action par défaut.

#### 2. reword (r) - Modifier le message du commit

```
reword a1b2c3d Add login form
```

Git garde le commit mais vous permet de modifier son message. Un éditeur s'ouvrira pour que vous puissiez réécrire le message.

**Exemple :**

```
# Avant
reword a1b2c3d Add login form

# Git ouvre l'éditeur, vous changez :
"Add login form"
# en
"feat: Add user login form with email validation"
```

#### 3. edit (e) - Modifier le commit

```
edit a1b2c3d Add login form
```

Git s'arrête sur ce commit, vous permettant de :
- Modifier des fichiers
- Ajouter ou supprimer des fichiers
- Diviser le commit en plusieurs commits
- Modifier le message

Une fois vos modifications faites :

```bash
git add fichiers-modifies
git commit --amend  # Pour modifier le commit actuel
git rebase --continue  # Pour continuer le rebase
```

#### 4. squash (s) - Fusionner avec le commit précédent

```
pick a1b2c3d Add login form
squash e4f5g6h Fix typo in login
```

Le commit `e4f5g6h` sera fusionné avec `a1b2c3d`. Git vous demandera de composer un nouveau message qui combine les deux.

**Résultat :**

```
# Les deux commits deviennent un seul :
a9b8c7d Add login form

Fix typo in login
```

#### 5. fixup (f) - Fusionner et ignorer le message

```
pick a1b2c3d Add login form
fixup e4f5g6h Fix typo in login
```

Comme `squash`, mais le message du second commit est complètement ignoré. Seul le message du premier commit est conservé.

**Résultat :**

```
# Un seul commit avec seulement le premier message
a9b8c7d Add login form
```

#### 6. drop (d) - Supprimer le commit

```
drop a1b2c3d Add login form
```

Le commit est supprimé de l'historique comme s'il n'avait jamais existé.

**Astuce :** Vous pouvez aussi simplement supprimer la ligne du commit dans l'éditeur.

---

### Cas d'usage pratiques

#### Cas 1 : Fusionner les commits "WIP" et "Fix"

**Situation :** Vous avez fait plusieurs commits temporaires pendant le développement.

```bash
git log --oneline
# h7i8j9k Add tests
# e4f5g6h Fix bug
# a1b2c3d WIP
# k0l1m2n Add feature
# n3o4p5q Previous commit
```

**Objectif :** Fusionner les 3 premiers commits en un seul commit propre.

```bash
git rebase -i HEAD~4
```

**Dans l'éditeur :**

```
pick k0l1m2n Add feature
fixup a1b2c3d WIP
fixup e4f5g6h Fix bug
pick h7i8j9k Add tests
```

**Résultat :**

```bash
git log --oneline
# h7i8j9k Add tests
# x9y8z7w Add feature
# n3o4p5q Previous commit
```

#### Cas 2 : Corriger des messages de commit

**Situation :** Vos messages de commit sont peu clairs.

```bash
git log --oneline
# h7i8j9k stuff
# e4f5g6h Fix
# a1b2c3d Update
```

```bash
git rebase -i HEAD~3
```

**Dans l'éditeur :**

```
reword a1b2c3d Update
reword e4f5g6h Fix
reword h7i8j9k stuff
```

Git ouvrira l'éditeur 3 fois pour que vous corrigiez chaque message :

```
"Update" → "feat: Add user authentication system"
"Fix" → "fix: Correct password validation logic"
"stuff" → "docs: Update README with auth instructions"
```

#### Cas 3 : Réorganiser des commits

**Situation :** Vous avez fait des commits dans le désordre.

```bash
git log --oneline
# c3d4e5f Add tests
# a1b2c3d Add documentation
# e4f5g6h Add feature
```

**Objectif :** Mettre la feature en premier, puis les tests, puis la doc.

```bash
git rebase -i HEAD~3
```

**Dans l'éditeur, réorganisez les lignes :**

```
pick e4f5g6h Add feature
pick c3d4e5f Add tests
pick a1b2c3d Add documentation
```

**Résultat :** L'ordre des commits est maintenant logique !

#### Cas 4 : Diviser un commit trop gros

**Situation :** Un commit contient trop de modifications différentes.

```bash
git log --oneline
# a1b2c3d Add login and payment features
```

```bash
git rebase -i HEAD~1
```

**Dans l'éditeur :**

```
edit a1b2c3d Add login and payment features
```

**Ensuite :**

```bash
# Git s'arrête sur le commit
git reset HEAD~
# Les modifications sont maintenant dans le Working Directory

# Créer le premier commit
git add login.js
git commit -m "feat: Add login feature"

# Créer le second commit
git add payment.js
git commit -m "feat: Add payment feature"

# Continuer le rebase
git rebase --continue
```

**Résultat :** Un commit est devenu deux commits distincts et clairs.

#### Cas 5 : Supprimer des commits indésirables

**Situation :** Vous avez un commit qui ne devrait pas être dans l'historique.

```bash
git log --oneline
# h7i8j9k Add feature
# e4f5g6h Debug console.log everywhere
# a1b2c3d Add tests
```

```bash
git rebase -i HEAD~3
```

**Dans l'éditeur :**

```
pick a1b2c3d Add tests
drop e4f5g6h Debug console.log everywhere
pick h7i8j9k Add feature
```

**Résultat :** Le commit de debug a disparu de l'historique.

---

### Workflow complet : Nettoyer avant de push

Voici un workflow typique avant de partager votre travail :

```bash
# 1. Vous avez travaillé sur une branche feature
git log --oneline
# g9h0i1j Update readme
# f8g9h0i Fix typo
# e7f8g9h WIP tests
# d6e7f8g Add tests
# c5d6e7f WIP
# b4c5d6e Add validation
# a3b4c5d Add login feature

# 2. Avant de créer une Pull Request, nettoyez l'historique
git rebase -i HEAD~7

# 3. Dans l'éditeur, organisez intelligemment
pick a3b4c5d Add login feature
fixup b4c5d6e Add validation
fixup c5d6e7f WIP
pick d6e7f8g Add tests
fixup e7f8g9h WIP tests
reword f8g9h0i Fix typo
squash g9h0i1j Update readme

# 4. Résultat : historique propre en 2 commits
# x1y2z3a feat: Add login feature with validation
# w4v5u6t docs: Fix typo and update readme

# 5. Maintenant vous pouvez push
git push origin feature/login
```

---

### Gérer les conflits pendant un rebase

Comme pour un merge, des conflits peuvent survenir pendant un rebase.

#### Quand un conflit apparaît

```bash
git rebase -i HEAD~3
# ...
# CONFLICT (content): Merge conflict in fichier.js
# error: could not apply a1b2c3d...
```

#### Résoudre et continuer

```bash
# 1. Ouvrir le fichier en conflit et corriger
# Supprimer les marqueurs de conflit <<<<<<, =======, >>>>>>>

# 2. Ajouter le fichier résolu
git add fichier.js

# 3. Continuer le rebase
git rebase --continue
```

#### Annuler le rebase

Si ça devient trop compliqué :

```bash
git rebase --abort
```

Tout revient à l'état d'avant le rebase.

---

### Règle d'or : Ne jamais rebase des commits publics

⚠️ **TRÈS IMPORTANT** ⚠️

**Ne faites JAMAIS de rebase interactif sur des commits qui ont été pushés et partagés avec d'autres personnes.**

**Pourquoi ?**

Le rebase réécrit l'historique en créant de nouveaux commits avec de nouveaux hashs. Si d'autres personnes ont basé leur travail sur vos anciens commits, cela créera un énorme désordre.

**Règle simple :**

✅ **Rebase avant le push** : OK, c'est votre historique personnel

❌ **Rebase après le push** : Dangereux, n'affecte pas seulement vous

**Exception :** Si vous travaillez seul sur une branche et êtes sûr que personne d'autre ne l'utilise, vous pouvez rebase après push avec `git push --force-with-lease` (mais c'est risqué).

---

### Conseils et bonnes pratiques

✅ **Faites des sauvegardes** : Créez une branche de backup avant un gros rebase

```bash
git branch backup-avant-rebase
git rebase -i HEAD~5
```

✅ **Commencez petit** : Pratiquez avec 2-3 commits avant de nettoyer 20 commits

✅ **Un commit = une idée** : Après rebase, chaque commit devrait représenter une modification logique et complète

✅ **Messages clairs** : Profitez du rebase pour écrire de meilleurs messages

✅ **Testez après rebase** : Vérifiez que tout fonctionne toujours

```bash
git rebase -i HEAD~5
# Après le rebase
npm test  # ou votre commande de test
```

✅ **Utilisez fixup régulièrement** : Créez des commits "fixup!" pendant le dev

```bash
git commit --fixup=a1b2c3d
# Plus tard
git rebase -i --autosquash HEAD~5
```

❌ **Ne rebasez pas main ou master** : Ces branches sont partagées

❌ **Ne paniquez pas** : `git rebase --abort` annule tout

❌ **Ne rebasez pas trop souvent** : Gardez de la flexibilité pendant le développement

---

### Commandes utiles avec rebase interactif

#### Rebase avec autosquash

Si vous créez des commits de fix avec `--fixup` :

```bash
git commit --fixup=a1b2c3d
git rebase -i --autosquash HEAD~5
```

Git organise automatiquement les commits fixup sous le commit original.

#### Voir le reflog

Si vous avez fait une erreur pendant le rebase :

```bash
git reflog
# Trouver l'état avant le rebase
git reset --hard HEAD@{2}
```

---

### Exemple complet pas à pas

```bash
# Situation de départ : historique désordonné
git log --oneline
# e5f6g7h Typo in readme
# d4e5f6g Add unit tests
# c3d4e5f WIP
# b2c3d4e Add feature
# a1b2c3d Previous work

# Objectif : Nettoyer en 2 commits propres
git rebase -i HEAD~4

# Dans l'éditeur :
pick b2c3d4e Add feature
fixup c3d4e5f WIP
pick d4e5f6g Add unit tests
squash e5f6g7h Typo in readme

# Git ouvre l'éditeur pour le squash :
# Add unit tests
#
# Typo in readme

# Vous modifiez en :
# test: Add comprehensive unit tests and update readme

# Résultat final
git log --oneline
# x9y8z7w test: Add comprehensive unit tests and update readme
# w8x9y0z feat: Add new authentication feature
# a1b2c3d Previous work

# Vérifier que tout va bien
git diff a1b2c3d  # Compare avec l'état d'avant

# Push
git push origin feature/auth
```

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git rebase -i HEAD~n` | Rebase interactif des n derniers commits |
| `git rebase -i <hash>` | Rebase depuis un commit spécifique |
| `pick` | Garder le commit tel quel |
| `reword` | Modifier le message du commit |
| `edit` | Modifier le contenu du commit |
| `squash` | Fusionner avec le commit précédent (garder les messages) |
| `fixup` | Fusionner avec le commit précédent (ignorer le message) |
| `drop` | Supprimer le commit |
| `git rebase --continue` | Continuer après résolution de conflit |
| `git rebase --abort` | Annuler le rebase en cours |
| `git rebase --skip` | Sauter le commit en conflit |
| `git commit --fixup=<hash>` | Créer un commit de fixup |
| `git rebase -i --autosquash` | Rebase avec fusion automatique des fixup |

---

### Différence entre rebase interactif et autres commandes

| Action | Commande | Usage |
|--------|----------|-------|
| Modifier le dernier commit | `git commit --amend` | Rapide pour le dernier commit uniquement |
| Nettoyer plusieurs commits | `git rebase -i` | Pour réorganiser l'historique |
| Fusionner des branches | `git merge` | Pour intégrer des branches complètes |
| Annuler un commit | `git revert` | Pour annuler sans réécrire l'historique |

---

### Conclusion

Le **rebase interactif** est un outil puissant qui transforme un historique brouillon en une histoire claire et professionnelle. C'est comme éditer un livre avant publication : vous organisez, clarifiez et améliorez le contenu.

**Points clés à retenir :**

- Utilisez `git rebase -i HEAD~n` pour nettoyer vos n derniers commits
- Les commandes principales sont `pick`, `reword`, `squash`, `fixup`, `drop`
- Ne rebasez JAMAIS des commits déjà partagés
- Faites des sauvegardes avant de gros rebase
- Un historique propre facilite la vie de toute l'équipe

Avec la pratique, nettoyer son historique avant de partager deviendra une habitude naturelle qui améliorera la qualité de vos projets !

---

**Prochaine étape :** Module 6.4 - Reflog : récupérer des commits perdus

⏭️ [Reflog : récupérer des commits perdus](/module-06-fonctions-avancees/04-reflog-et-recuperation.md)
