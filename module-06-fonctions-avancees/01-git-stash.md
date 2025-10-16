🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 1. Mettre de côté temporairement (git stash)

### Introduction

Imaginez que vous êtes en train de travailler sur une nouvelle fonctionnalité dans votre projet. Vous avez modifié plusieurs fichiers, mais soudain, votre collègue vous demande de corriger un bug urgent sur la branche principale. Problème : vos modifications ne sont pas prêtes à être commitées, et vous ne voulez pas les perdre.

C'est exactement dans ce genre de situation que **git stash** devient votre meilleur ami !

**Git stash** vous permet de mettre de côté temporairement vos modifications non commitées (dans le Working Directory et la Staging Area) pour revenir à un état propre de votre branche. Vous pouvez ensuite changer de branche, faire votre correction urgente, et récupérer vos modifications exactement là où vous les aviez laissées.

---

### Pourquoi utiliser git stash ?

- **Changer de branche rapidement** sans avoir à commiter un travail incomplet
- **Tester quelque chose** sans perdre vos modifications en cours
- **Éviter les conflits** lors d'un `git pull`
- **Garder un historique propre** en évitant les commits temporaires du type "WIP" ou "sauvegarde"

---

### La commande de base : git stash

#### Mettre de côté vos modifications

```bash
git stash
```

ou (forme plus explicite) :

```bash
git stash push
```

Cette commande prend toutes vos modifications **suivies** (fichiers déjà trackés par Git) et les met de côté dans une zone temporaire. Votre répertoire de travail redevient propre, comme après le dernier commit.

**Exemple concret :**

```bash
# Vous êtes sur la branche feature/nouveau-design
# Vous avez modifié style.css et index.html

git status
# modified:   style.css
# modified:   index.html

git stash
# Saved working directory and index state WIP on feature/nouveau-design

git status
# nothing to commit, working tree clean
```

---

### Récupérer vos modifications

#### git stash pop

```bash
git stash pop
```

Cette commande réapplique la dernière modification mise de côté ET la supprime de la liste des stash.

**Scénario typique :**

```bash
# Vous avez mis vos modifications de côté
git stash

# Vous changez de branche pour corriger un bug
git checkout main
# ... corrections et commit ...

# Vous revenez sur votre branche
git checkout feature/nouveau-design

# Vous récupérez vos modifications
git stash pop
# Les fichiers style.css et index.html sont de nouveau modifiés
```

#### git stash apply

```bash
git stash apply
```

Identique à `git stash pop`, MAIS garde la copie dans la liste des stash. Utile si vous voulez appliquer les mêmes modifications sur plusieurs branches.

---

### Gérer plusieurs stash

Vous pouvez créer plusieurs stash successifs. Ils sont empilés comme une pile d'assiettes.

#### Lister vos stash

```bash
git stash list
```

**Exemple de sortie :**

```
stash@{0}: WIP on feature/login: a3f5d8e Ajout formulaire
stash@{1}: WIP on feature/design: 2b8c4fa Modification CSS
stash@{2}: WIP on main: 9e1f3bc Correctif urgent
```

Le stash le plus récent est `stash@{0}`.

#### Appliquer un stash spécifique

```bash
git stash apply stash@{1}
```

ou

```bash
git stash pop stash@{1}
```

---

### Ajouter un message descriptif

Pour vous y retrouver plus facilement, donnez un nom à votre stash :

```bash
git stash push -m "Travail en cours sur le menu responsive"
```

**Résultat dans git stash list :**

```
stash@{0}: On feature/menu: Travail en cours sur le menu responsive
```

Beaucoup plus clair que "WIP" !

---

### Inclure les fichiers non suivis

Par défaut, `git stash` ne sauvegarde que les fichiers déjà trackés par Git. Pour inclure également les fichiers non suivis (nouveaux fichiers) :

```bash
git stash -u
```

ou

```bash
git stash --include-untracked
```

**Exemple :**

```bash
# Vous avez créé un nouveau fichier nouveau.js (non tracké)
git status
# Untracked files:
#   nouveau.js

git stash -u
# Saved working directory and index state WIP on feature/test

# Le fichier nouveau.js a également été mis de côté
```

---

### Visualiser le contenu d'un stash

Pour voir ce qui a été sauvegardé dans un stash sans l'appliquer :

```bash
git stash show
```

Pour voir les modifications détaillées (comme un diff) :

```bash
git stash show -p
```

ou

```bash
git stash show -p stash@{1}
```

---

### Supprimer un stash

#### Supprimer le dernier stash

```bash
git stash drop
```

#### Supprimer un stash spécifique

```bash
git stash drop stash@{1}
```

#### Supprimer tous les stash

```bash
git stash clear
```

⚠️ **Attention :** Cette action est irréversible !

---

### Créer une branche à partir d'un stash

Parfois, vous voulez transformer votre stash en une nouvelle branche pour mieux organiser votre travail :

```bash
git stash branch nom-de-la-branche
```

Cette commande :
1. Crée une nouvelle branche
2. Change vers cette branche
3. Applique le stash
4. Supprime le stash

**Exemple :**

```bash
git stash branch feature/correctif-menu
# Switched to a new branch 'feature/correctif-menu'
# On branch feature/correctif-menu
# Changes not staged for commit:
#   modified:   menu.js
```

---

### Cas d'usage pratiques

#### Cas 1 : Correction urgente

```bash
# Vous travaillez sur feature/nouvelle-page
git add .
git stash -m "Page de contact en cours"

# Correction urgente sur main
git checkout main
# ... correctifs ...
git add .
git commit -m "Fix: correction bug critique"
git push

# Retour au travail
git checkout feature/nouvelle-page
git stash pop
```

#### Cas 2 : Mettre à jour votre branche

```bash
# Vous avez des modifications locales
git stash

# Récupérer les dernières modifications
git pull origin main

# Réappliquer vos modifications
git stash pop
```

#### Cas 3 : Tester quelque chose rapidement

```bash
# Vous voulez tester une idée sans perdre votre travail actuel
git stash -m "Travail actuel avant test"

# ... faites vos tests ...

# Revenez en arrière
git stash pop
```

---

### Résoudre les conflits après un stash pop

Parfois, quand vous faites `git stash pop`, Git peut rencontrer des conflits (si les fichiers ont été modifiés entre temps). Dans ce cas :

1. Git vous indique les fichiers en conflit
2. Résolvez les conflits manuellement (comme pour un merge)
3. Ajoutez les fichiers résolus avec `git add`
4. Le stash reste dans la liste (il ne se supprime pas automatiquement en cas de conflit)
5. Une fois résolu, supprimez-le manuellement avec `git stash drop`

**Exemple :**

```bash
git stash pop
# Auto-merging index.html
# CONFLICT (content): Merge conflict in index.html

# Résoudre le conflit dans index.html
# Ensuite :
git add index.html
git stash drop
```

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git stash` | Met de côté les modifications |
| `git stash push -m "message"` | Met de côté avec un message descriptif |
| `git stash -u` | Met de côté (y compris les fichiers non suivis) |
| `git stash list` | Liste tous les stash |
| `git stash show` | Affiche le contenu du dernier stash |
| `git stash show -p` | Affiche les différences détaillées |
| `git stash pop` | Applique et supprime le dernier stash |
| `git stash apply` | Applique sans supprimer le stash |
| `git stash drop` | Supprime le dernier stash |
| `git stash clear` | Supprime tous les stash |
| `git stash branch nom` | Crée une branche avec le stash |

---

### Conseils et bonnes pratiques

✅ **Donnez toujours un message descriptif** avec `-m` pour vous y retrouver facilement

✅ **N'oubliez pas d'appliquer vos stash** : ils ne sont pas éternels et peuvent être confus si vous en accumulez trop

✅ **Utilisez `git stash pop`** plutôt que `apply` pour garder votre liste de stash propre

✅ **Vérifiez avec `git stash list`** régulièrement pour ne pas laisser traîner des modifications oubliées

❌ **N'utilisez pas git stash comme système de sauvegarde** : préférez les commits sur des branches dédiées pour du travail important

❌ **Évitez d'accumuler trop de stash** : au-delà de 3-4, il devient difficile de s'y retrouver

---

### Conclusion

**Git stash** est un outil puissant et pratique pour gérer vos modifications temporaires sans polluer votre historique de commits. C'est particulièrement utile dans le travail quotidien où les interruptions et les changements de contexte sont fréquents.

Retenez l'essentiel :
- `git stash` pour mettre de côté
- `git stash pop` pour récupérer
- `git stash list` pour voir ce qui est stocké

Avec ces trois commandes, vous couvrez déjà 90% des cas d'usage !

---

**Prochaine étape :** Module 6.2 - Cherry-pick : appliquer des commits spécifiques

⏭️ [Cherry-pick : appliquer des commits spécifiques](/module-06-fonctions-avancees/02-cherry-pick.md)
