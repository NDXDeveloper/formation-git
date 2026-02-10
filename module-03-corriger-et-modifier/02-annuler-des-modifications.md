🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 3 : Corriger et modifier
## 2. Annuler des modifications (git restore, git checkout)

### Introduction

Pendant votre travail, il arrive souvent de faire des modifications que vous souhaitez finalement annuler. Peut-être avez-vous testé une approche qui ne fonctionne pas, ou vous avez modifié un fichier par erreur. Git vous offre plusieurs commandes pour revenir en arrière selon la situation.

**Analogie** : Imaginez que vous êtes en train d'écrire un document. Vous pouvez utiliser Ctrl+Z pour annuler vos dernières modifications. Git propose quelque chose de similaire, mais avec plus de contrôle et de précision sur ce que vous voulez annuler.

---

### Les deux commandes principales

Git propose deux commandes pour annuler des modifications :

1. **`git restore`** (moderne, recommandée) - Introduite en 2019, plus claire et intuitive
2. **`git checkout`** (ancienne, toujours fonctionnelle) - Plus polyvalente mais parfois ambiguë

Dans ce tutoriel, nous nous concentrerons principalement sur `git restore`, la commande moderne, mais nous verrons aussi `git checkout` car vous la rencontrerez souvent dans les anciennes documentations et tutoriels.

---

### Rappel : Les trois zones de Git

Avant de comprendre comment annuler des modifications, rappelons les trois zones importantes dans Git :

```
┌─────────────────────┐
│  Working Directory  │ ← Vos fichiers actuels
│  (zone de travail)  │
└─────────────────────┘
          ↓ git add
┌─────────────────────┐
│   Staging Area      │ ← Fichiers prêts à être commités
│   (index)           │
└─────────────────────┘
          ↓ git commit
┌─────────────────────┐
│    Repository       │ ← Historique des commits
│    (.git)           │
└─────────────────────┘
```

Les commandes d'annulation agissent différemment selon la zone où se trouvent vos modifications.

---

### Cas 1 : Annuler des modifications non ajoutées (Working Directory)

Vous avez modifié un fichier mais vous n'avez pas encore fait `git add`. Vous voulez annuler ces modifications et revenir à la dernière version commitée.

#### Avec `git restore` (recommandé)

```bash
git restore nom_du_fichier.txt
```

**Exemple pratique :**

```bash
# Vous modifiez un fichier
echo "Nouvelle ligne" >> index.html

# Vous vérifiez le statut
git status
# Résultat : modified: index.html

# Vous changez d'avis et voulez annuler
git restore index.html

# Le fichier revient à son état du dernier commit
```

#### Pour restaurer tous les fichiers modifiés :

```bash
git restore .
```

Le point `.` signifie "tous les fichiers du répertoire courant et ses sous-répertoires".

#### Avec `git checkout` (ancienne méthode)

```bash
git checkout -- nom_du_fichier.txt
```

Le `--` est important : il indique à Git que ce qui suit est un nom de fichier, pas une branche.

**⚠️ Avertissement important :** Ces commandes suppriment définitivement vos modifications non commitées. Il n'y a **pas de retour en arrière possible**. Utilisez-les avec précaution !

---

### Cas 2 : Retirer un fichier de la Staging Area

Vous avez fait `git add` sur un fichier, mais vous voulez le retirer de la staging area sans perdre vos modifications.

#### Avec `git restore` (recommandé)

```bash
git restore --staged nom_du_fichier.txt
```

**Exemple pratique :**

```bash
# Vous modifiez deux fichiers
echo "Modification A" > fichierA.txt  
echo "Modification B" > fichierB.txt

# Vous ajoutez les deux à la staging area
git add fichierA.txt fichierB.txt

# Vous réalisez que fichierB.txt ne devrait pas être dans ce commit
git restore --staged fichierB.txt

# Vérification
git status
# fichierA.txt : prêt à être commité (dans la staging area)
# fichierB.txt : modifié mais pas dans la staging area
```

Les modifications de `fichierB.txt` sont toujours présentes dans votre fichier, mais elles ne sont plus marquées pour être commitées.

#### Pour retirer tous les fichiers de la staging area :

```bash
git restore --staged .
```

#### Avec `git reset` (ancienne méthode)

```bash
git reset HEAD nom_du_fichier.txt
```

Cette commande fait la même chose, mais `git restore --staged` est plus explicite et plus facile à retenir.

---

### Cas 3 : Annuler ET retirer de la staging area en même temps

Si vous voulez à la fois retirer un fichier de la staging area ET annuler les modifications, vous pouvez combiner les deux actions :

```bash
# Méthode 1 : Deux commandes séparées
git restore --staged fichier.txt    # Retire de la staging area  
git restore fichier.txt              # Annule les modifications

# Méthode 2 : Raccourci
git restore --staged --worktree fichier.txt
```

L'option `--worktree` indique que vous voulez aussi restaurer le fichier dans votre répertoire de travail (Working Directory).

---

### Cas 4 : Restaurer un fichier depuis un commit spécifique

Parfois, vous voulez récupérer une version d'un fichier qui se trouve dans un ancien commit, sans toucher aux autres fichiers.

#### Avec `git restore`

```bash
git restore --source=identifiant_commit nom_du_fichier.txt
```

**Exemple pratique :**

```bash
# Afficher l'historique pour trouver le commit voulu
git log --oneline
# a1b2c3d Ajout fonctionnalité X
# e4f5g6h Correction bug Y
# h7i8j9k Version initiale

# Restaurer index.html depuis le commit h7i8j9k
git restore --source=h7i8j9k index.html

# Le fichier index.html revient à sa version du commit h7i8j9k
```

Vous pouvez aussi utiliser des références relatives :

```bash
# Restaurer depuis le commit précédent
git restore --source=HEAD~1 fichier.txt

# Restaurer depuis 3 commits en arrière
git restore --source=HEAD~3 fichier.txt
```

#### Avec `git checkout` (ancienne méthode)

```bash
git checkout h7i8j9k -- fichier.txt
```

---

### Cas 5 : Annuler la suppression d'un fichier

Vous avez supprimé un fichier par erreur et vous voulez le récupérer.

```bash
# Vous supprimez accidentellement un fichier
rm important.txt

# Vérification
git status
# deleted: important.txt

# Pour le récupérer
git restore important.txt

# Le fichier est restauré !
```

Si vous aviez déjà fait `git add` de la suppression :

```bash
# La suppression est dans la staging area
git status
# deleted: important.txt (staged)

# Retirer de la staging area ET restaurer le fichier
git restore --staged --worktree important.txt
```

---

### Tableau récapitulatif des commandes

| Situation | Commande moderne | Ancienne commande |
|-----------|-----------------|-------------------|
| Annuler modifications non ajoutées | `git restore fichier.txt` | `git checkout -- fichier.txt` |
| Annuler tous les fichiers modifiés | `git restore .` | `git checkout -- .` |
| Retirer de la staging area | `git restore --staged fichier.txt` | `git reset HEAD fichier.txt` |
| Retirer de staging + annuler | `git restore --staged --worktree fichier.txt` | `git reset HEAD fichier.txt` puis `git checkout -- fichier.txt` |
| Restaurer depuis un commit ancien | `git restore --source=abc123 fichier.txt` | `git checkout abc123 -- fichier.txt` |
| Récupérer un fichier supprimé | `git restore fichier.txt` | `git checkout -- fichier.txt` |

---

### Comprendre `git checkout` : une commande polyvalente

La commande `git checkout` est historiquement utilisée pour plusieurs tâches différentes, ce qui peut être source de confusion :

1. **Changer de branche** : `git checkout nom_branche`
2. **Annuler des modifications** : `git checkout -- fichier.txt`
3. **Créer une nouvelle branche** : `git checkout -b nouvelle_branche`

C'est pourquoi Git a introduit des commandes plus spécialisées :
- `git switch` pour changer de branche
- `git restore` pour restaurer des fichiers

**Note :** Dans les versions récentes de Git, `git checkout` fonctionne toujours mais il est recommandé d'utiliser les nouvelles commandes pour plus de clarté.

---

### Vérifier avant d'annuler

Avant d'annuler des modifications, il est toujours bon de vérifier ce que vous allez perdre.

#### Voir les modifications non ajoutées :

```bash
git diff fichier.txt
```

Cette commande affiche les différences entre votre fichier actuel et la dernière version commitée.

#### Voir les modifications dans la staging area :

```bash
git diff --staged fichier.txt
```

#### Exemple de lecture d'un diff :

```diff
diff --git a/index.html b/index.html  
index 1234567..abcdefg 100644
--- a/index.html
+++ b/index.html
@@ -1,3 +1,4 @@
 <html>
   <body>
-    <h1>Ancien titre</h1>
+    <h1>Nouveau titre</h1>
+    <p>Nouveau paragraphe</p>
   </body>
 </html>
```

- Les lignes commençant par `-` (en rouge) sont supprimées
- Les lignes commençant par `+` (en vert) sont ajoutées

---

### ⚠️ Précautions importantes

#### 1. Les modifications annulées sont perdues définitivement

Quand vous utilisez `git restore` pour annuler des modifications dans votre Working Directory, ces modifications sont **définitivement perdues**. Git ne peut pas les récupérer car elles n'ont jamais été commitées.

```bash
# ❌ DANGER : Cette commande supprime vos modifications sans possibilité de retour
git restore fichier.txt
```

**Conseil :** Avant d'annuler, vérifiez toujours avec `git diff` ce que vous allez perdre.

#### 2. Différence entre retirer de staging et annuler

Il est important de comprendre la différence :

- `git restore --staged fichier.txt` : Retire le fichier de la staging area mais **conserve vos modifications** dans le fichier
- `git restore fichier.txt` : Annule vos modifications et restaure la version du dernier commit

#### 3. Utiliser `git stash` comme alternative

Si vous n'êtes pas sûr de vouloir annuler définitivement vos modifications, utilisez plutôt `git stash` pour les mettre de côté temporairement :

```bash
# Mettre de côté vos modifications
git stash

# Plus tard, si vous voulez les récupérer
git stash pop

# Ou les supprimer définitivement
git stash drop
```

Nous verrons `git stash` en détail dans le Module 6.

---

### Cas pratiques courants

#### Scénario 1 : Expérimentation ratée

```bash
# Vous expérimentez une nouvelle fonctionnalité
# ... plusieurs modifications ...

# Ça ne fonctionne pas, vous voulez tout annuler
git restore .

# Tous les fichiers modifiés reviennent à leur état d'origine
```

#### Scénario 2 : Mauvais fichiers ajoutés

```bash
# Vous ajoutez plusieurs fichiers
git add *.txt

# Oups, vous avez aussi ajouté config.txt par erreur
git restore --staged config.txt

# config.txt n'est plus dans la staging area
# mais les autres fichiers .txt y sont toujours
```

#### Scénario 3 : Récupération après suppression

```bash
# Vous nettoyez votre projet et supprimez trop de fichiers
rm ancien_code.js

# En fait, vous en avez encore besoin
git restore ancien_code.js

# Le fichier est restauré depuis le dernier commit
```

#### Scénario 4 : Version spécifique d'un fichier

```bash
# Vous voulez comparer deux versions d'un fichier
# Restaurer la version d'il y a 5 commits
git restore --source=HEAD~5 analyse.py

# Comparer avec la version actuelle
# ... analyse ...

# Revenir à la version actuelle
git restore analyse.py
```

---

### Erreurs courantes et solutions

#### Erreur 1 : "pathspec 'fichier.txt' did not match any files"

```bash
git restore fichier_qui_nexiste_pas.txt
# error: pathspec 'fichier_qui_nexiste_pas.txt' did not match any file(s) known to git
```

**Solution :** Vérifiez le nom du fichier avec `git status` ou `ls`.

#### Erreur 2 : Oublier l'option --staged

```bash
# Vous voulez retirer de la staging area
git restore fichier.txt  # ❌ Annule les modifications au lieu !

# Correct
git restore --staged fichier.txt  # ✅ Retire de la staging area
```

#### Erreur 3 : Confondre restore et reset

- `git restore` : pour les fichiers individuels
- `git reset` : pour déplacer la branche entière (nous verrons ça dans la prochaine section)

---

### Aide-mémoire visuel

```
État initial (Working Directory)
    fichier.txt [modifié]
           ↓
    git add fichier.txt
           ↓
État après add (Staging Area)
    fichier.txt [prêt à commiter]
           ↓
   git restore --staged fichier.txt  ←── Retour à Working Directory
           ↓
    fichier.txt [modifié]
           ↓
    git restore fichier.txt  ←── Annulation définitive
           ↓
    fichier.txt [version du dernier commit]
```

---

### Résumé des options de `git restore`

| Option | Description |
|--------|-------------|
| (sans option) | Restaure dans le Working Directory |
| `--staged` | Retire de la Staging Area |
| `--worktree` | Restaure dans le Working Directory (équivalent à sans option) |
| `--staged --worktree` | Les deux en même temps |
| `--source=<commit>` | Restaure depuis un commit spécifique |

---

### Points clés à retenir

✅ `git restore` est la commande moderne pour annuler des modifications

✅ Sans option, elle annule les modifications dans le Working Directory

✅ Avec `--staged`, elle retire de la Staging Area sans toucher au fichier

✅ Les modifications annulées sont **perdues définitivement**

✅ Toujours vérifier avec `git diff` avant d'annuler

✅ `git checkout` fonctionne toujours mais est moins clair

✅ En cas de doute, utilisez `git stash` pour mettre de côté temporairement

---

### Conclusion

Les commandes `git restore` et `git checkout` vous donnent un contrôle précis sur l'annulation de modifications à différents niveaux. `git restore` est plus intuitive et explicite, ce qui en fait le choix recommandé pour les débutants et les utilisateurs expérimentés.

La règle d'or : **toujours vérifier ce que vous allez perdre avant d'utiliser ces commandes**, car les modifications non commitées ne peuvent pas être récupérées une fois annulées.

Dans la prochaine section, nous verrons `git reset`, une commande plus puissante qui permet de déplacer des commits entiers dans l'historique.

⏭️ [Comprendre git reset (--soft, --mixed, --hard)](/module-03-corriger-et-modifier/03-comprendre-git-reset.md)
