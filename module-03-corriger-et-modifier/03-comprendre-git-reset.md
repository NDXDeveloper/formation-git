🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 3 : Corriger et modifier
## 3. Comprendre git reset (--soft, --mixed, --hard)

### Introduction

La commande `git reset` est l'une des plus puissantes de Git, mais aussi l'une des plus délicates à maîtriser. Elle permet de "remonter dans le temps" en déplaçant la branche courante vers un commit antérieur. Selon l'option utilisée, elle agit différemment sur vos fichiers et votre staging area.

**Analogie** : Imaginez un livre avec des marque-pages. `git reset` vous permet de déplacer votre marque-page principal (HEAD) vers une page précédente. Selon l'option choisie, vous décidez également ce qui arrive aux notes que vous avez prises depuis (vos modifications).

---

### Rappel : Les trois zones de Git

Avant de comprendre `git reset`, il est essentiel de bien visualiser les trois zones de Git :

```
┌─────────────────────────────┐
│    Working Directory        │  ← Vos fichiers tels que vous les voyez
│    (Répertoire de travail)  │
└─────────────────────────────┘
              ↕
┌─────────────────────────────┐
│     Staging Area            │  ← Fichiers préparés pour le prochain commit
│     (Index)                 │
└─────────────────────────────┘
              ↕
┌─────────────────────────────┐
│     Repository              │  ← Historique des commits
│     HEAD → commit actuel    │
└─────────────────────────────┘
```

`git reset` agit sur ces trois zones selon l'option utilisée.

---

### Qu'est-ce que `git reset` ?

`git reset` déplace la référence HEAD (et la branche courante) vers un commit spécifique. C'est comme "revenir en arrière" dans l'historique de votre projet.

**Syntaxe générale :**

```bash
git reset [--soft | --mixed | --hard] <commit>
```

Les trois options (`--soft`, `--mixed`, `--hard`) déterminent ce qui arrive aux zones Working Directory et Staging Area.

---

### Visualiser l'historique avant de reset

Avant d'utiliser `git reset`, visualisez toujours votre historique :

```bash
# Afficher l'historique simplifié
git log --oneline

# Exemple de résultat :
# d4e5f6g (HEAD -> main) Commit C - Ajout fonctionnalité Z
# a1b2c3d Commit B - Correction bug Y
# h7i8j9k Commit A - Version initiale
```

Dans cet exemple :
- HEAD pointe sur le commit `d4e5f6g` (Commit C)
- Nous pouvons reset vers le commit B ou A

---

### Option 1 : `git reset --soft` (Le plus doux)

#### Que fait --soft ?

`git reset --soft` **déplace seulement HEAD** vers un commit antérieur. Vos modifications restent intactes :
- **Working Directory** : inchangé (vos fichiers gardent toutes les modifications)
- **Staging Area** : conserve toutes les modifications comme si vous aviez fait `git add`
- **Repository** : HEAD se déplace vers le commit spécifié

**Analogie** : Vous revenez à une page précédente du livre, mais vous gardez toutes vos notes prêtes à être réécrites.

#### Cas d'usage typique

C'est utile quand vous voulez **refaire un ou plusieurs commits** en les combinant ou en modifiant leur message.

#### Exemple pratique :

```bash
# État initial
git log --oneline
# d4e5f6g (HEAD -> main) Ajout feature X
# a1b2c3d Ajout feature Y
# h7i8j9k Ajout feature Z

# Vous réalisez que les 3 derniers commits devraient être un seul
# Revenir 3 commits en arrière avec --soft
git reset --soft HEAD~3

# Maintenant HEAD pointe sur le commit avant h7i8j9k
# MAIS : toutes les modifications des commits h7i8j9k, a1b2c3d et d4e5f6g
# sont dans la staging area, prêtes à être commitées

git status
# Changes to be committed:
#   (tous les fichiers des 3 commits)

# Vous pouvez maintenant faire un seul commit propre
git commit -m "Ajout des features X, Y et Z"
```

#### Schéma visuel :

```
Avant git reset --soft HEAD~1 :
Repository:    Commit A ← Commit B ← Commit C (HEAD)
Staging:       [vide]
Working:       [fichiers = Commit C]

Après git reset --soft HEAD~1 :
Repository:    Commit A ← Commit B (HEAD)
Staging:       [modifications de Commit C]
Working:       [fichiers = Commit C]
```

---

### Option 2 : `git reset --mixed` (Mode par défaut)

#### Que fait --mixed ?

`git reset --mixed` est le mode par défaut si vous ne spécifiez pas d'option. Il :
- **Working Directory** : inchangé (vos fichiers gardent toutes les modifications)
- **Staging Area** : **vidée** (comme si vous n'aviez jamais fait `git add`)
- **Repository** : HEAD se déplace vers le commit spécifié

**Analogie** : Vous revenez à une page précédente et vos notes sont déplacées sur un brouillon séparé, pas encore prêtes à être réécrites.

#### Cas d'usage typique

C'est utile quand vous voulez **décommiter** pour refaire vos commits différemment, en choisissant manuellement ce qui va dans chaque commit.

#### Exemple pratique :

```bash
# État initial
git log --oneline
# d4e5f6g (HEAD -> main) Modifications de A, B et C ensemble
# a1b2c3d Version précédente

# Vous réalisez que A, B et C devraient être dans 3 commits séparés
git reset HEAD~1
# ou : git reset --mixed HEAD~1 (équivalent)

# Maintenant les modifications sont dans le Working Directory
git status
# Changes not staged for commit:
#   modified: fichierA.txt
#   modified: fichierB.txt
#   modified: fichierC.txt

# Vous pouvez reconstruire des commits propres
git add fichierA.txt
git commit -m "Modification de A"

git add fichierB.txt
git commit -m "Modification de B"

git add fichierC.txt
git commit -m "Modification de C"
```

#### Schéma visuel :

```
Avant git reset --mixed HEAD~1 :
Repository:    Commit A ← Commit B (HEAD)
Staging:       [vide]
Working:       [fichiers = Commit B]

Après git reset --mixed HEAD~1 :
Repository:    Commit A (HEAD)
Staging:       [vide]
Working:       [fichiers = Commit B] (modifications toujours là)
```

---

### Option 3 : `git reset --hard` (Le plus radical)

#### Que fait --hard ?

`git reset --hard` est l'option la plus agressive. Elle :
- **Working Directory** : **réinitialisé** complètement (modifications perdues !)
- **Staging Area** : **vidée** complètement
- **Repository** : HEAD se déplace vers le commit spécifié

**Analogie** : Vous revenez à une page précédente et vous déchirez toutes les notes que vous aviez prises depuis. **C'est irréversible !**

#### ⚠️ DANGER : Perte de données

`git reset --hard` **supprime définitivement** toutes vos modifications non commitées. Utilisez cette commande avec une extrême prudence !

#### Cas d'usage typique

Utilisez `--hard` uniquement quand vous êtes **absolument certain** de vouloir abandonner tout votre travail et revenir à un état antérieur propre.

#### Exemple pratique :

```bash
# Vous avez fait beaucoup de modifications expérimentales
# qui ne fonctionnent pas du tout

git status
# Beaucoup de fichiers modifiés...

# Vous voulez tout abandonner et revenir au dernier commit
git reset --hard HEAD

# Ou revenir 2 commits en arrière
git reset --hard HEAD~2

# TOUT est perdu : modifications dans Working Directory ET Staging Area
```

#### Schéma visuel :

```
Avant git reset --hard HEAD~1 :
Repository:    Commit A ← Commit B (HEAD)
Staging:       [quelques modifications]
Working:       [beaucoup de modifications]

Après git reset --hard HEAD~1 :
Repository:    Commit A (HEAD)
Staging:       [vide]
Working:       [fichiers = Commit A] (tout le reste est PERDU)
```

---

### Tableau comparatif des trois options

| Option | Repository (HEAD) | Staging Area | Working Directory | Modifications perdues ? |
|--------|-------------------|--------------|-------------------|------------------------|
| `--soft` | ✅ Déplacé | ✅ Conservée | ✅ Conservée | ❌ Non |
| `--mixed` | ✅ Déplacé | ❌ Vidée | ✅ Conservée | ❌ Non |
| `--hard` | ✅ Déplacé | ❌ Vidée | ❌ Réinitialisée | ⚠️ **OUI** |

---

### Références de commits

Vous pouvez utiliser différentes façons de référencer un commit avec `git reset` :

#### Par identifiant (hash) :

```bash
git reset --soft a1b2c3d
```

#### Relatif à HEAD :

```bash
# Revenir 1 commit en arrière
git reset --soft HEAD~1

# Revenir 3 commits en arrière
git reset --soft HEAD~3

# Équivalent avec ^
git reset --soft HEAD^  # 1 commit en arrière
git reset --soft HEAD^^ # 2 commits en arrière
```

#### Par nom de branche ou tag :

```bash
git reset --mixed origin/main
git reset --hard v1.0.0
```

---

### Cas pratiques détaillés

#### Scénario 1 : Refaire le dernier commit avec --soft

```bash
# Vous venez de commiter
git commit -m "Ajout fonctionnalité"

# Vous réalisez que vous vouliez aussi modifier autre chose
# et inclure tout dans le même commit

# Annuler le commit mais garder les modifications
git reset --soft HEAD~1

# Les fichiers sont toujours dans la staging area
# Ajoutez vos modifications supplémentaires
git add autre_fichier.txt

# Refaites un commit complet
git commit -m "Ajout fonctionnalité complète"
```

#### Scénario 2 : Séparer un gros commit avec --mixed

```bash
# Vous avez fait un commit trop gros
git log --oneline
# d4e5f6g Modifications multiples

# Décommiter pour refaire proprement
git reset HEAD~1  # --mixed par défaut

# Les modifications sont dans le Working Directory
# Vous pouvez maintenant les commiter de façon organisée
git add fichier1.txt
git commit -m "Modification partie 1"

git add fichier2.txt fichier3.txt
git commit -m "Modification partie 2"
```

#### Scénario 3 : Abandonner tout avec --hard

```bash
# Vous avez expérimenté mais rien ne fonctionne
# Beaucoup de fichiers modifiés partout

# Tout abandonner et revenir à un état propre
git reset --hard HEAD

# Ou revenir au dernier commit de la branche distante
git reset --hard origin/main
```

#### Scénario 4 : Revenir plusieurs commits en arrière

```bash
git log --oneline
# d4e5f6g (HEAD -> main) Mauvais commit 3
# c3d4e5f Mauvais commit 2
# b2c3d4e Mauvais commit 1
# a1b2c3d Dernier bon commit

# Revenir au dernier bon commit en gardant les modifications
git reset --mixed a1b2c3d

# Ou tout abandonner
git reset --hard a1b2c3d
```

---

### ⚠️ Règle d'or : Ne jamais reset des commits publics

**IMPORTANT** : Ne faites **JAMAIS** `git reset` sur des commits qui ont déjà été poussés (push) sur un dépôt distant partagé avec d'autres personnes.

#### Pourquoi ?

Quand vous faites un reset, vous réécrivez l'historique. Si d'autres personnes ont déjà récupéré ces commits, cela créera des conflits et des problèmes de synchronisation.

```bash
# ❌ MAUVAIS : Reset après avoir pushé
git push origin main
git reset --hard HEAD~1  # Problème ! D'autres ont peut-être déjà pull

# ✅ BON : Reset avant de push
git reset --hard HEAD~1
git push origin main      # OK, l'historique n'était pas public
```

#### Que faire si vous avez déjà pushé ?

Utilisez plutôt `git revert` (que nous verrons dans la section suivante). Cette commande crée un nouveau commit qui annule les changements, sans réécrire l'historique.

---

### Récupérer après un reset accidentel

Si vous avez fait un `git reset --hard` par erreur, il existe une solution de secours : **`git reflog`**

#### Le reflog : votre filet de sécurité

Git garde une trace de tous les déplacements de HEAD dans le reflog. Vous pouvez voir l'historique et récupérer un commit "perdu".

```bash
# Vous avez fait un reset --hard par erreur
git reset --hard HEAD~3

# Oups ! Vous voulez récupérer votre travail
# Consultez le reflog
git reflog

# Résultat :
# a1b2c3d HEAD@{0}: reset: moving to HEAD~3
# d4e5f6g HEAD@{1}: commit: Commit que je veux récupérer
# c3d4e5f HEAD@{2}: commit: Autre commit

# Récupérer le commit perdu
git reset --hard HEAD@{1}
# ou
git reset --hard d4e5f6g

# Vous avez récupéré votre travail !
```

**Note** : Le reflog conserve l'historique pendant environ 90 jours par défaut. Au-delà, les commits non référencés peuvent être définitivement supprimés par le garbage collector de Git.

---

### Différences entre reset, restore et revert

Il est facile de confondre ces trois commandes. Voici un tableau pour clarifier :

| Commande | Action | Modifie l'historique ? | Usage principal |
|----------|--------|----------------------|-----------------|
| `git restore` | Restaure des fichiers individuels | ❌ Non | Annuler modifications de fichiers |
| `git reset` | Déplace HEAD et la branche | ✅ Oui (localement) | Modifier commits non publiés |
| `git revert` | Crée un nouveau commit d'annulation | ❌ Non | Annuler commits publiés |

**Règle simple** :
- Fichiers uniquement → `git restore`
- Commits locaux → `git reset`
- Commits publiés → `git revert`

---

### Commandes pour vérifier l'état après reset

Après avoir fait un reset, vérifiez toujours l'état de votre dépôt :

```bash
# Voir où pointe HEAD
git log --oneline -5

# Voir l'état des fichiers
git status

# Voir les différences
git diff

# Voir l'historique des déplacements de HEAD
git reflog
```

---

### Exemples de cas d'erreur et solutions

#### Erreur 1 : Reset --hard par accident

```bash
# Vous faites un reset --hard et regrettez immédiatement
git reset --hard HEAD~2

# Solution : Utiliser reflog
git reflog
git reset --hard HEAD@{1}
```

#### Erreur 2 : Vouloir --soft mais faire --hard

```bash
# Vous vouliez --soft mais tapez --hard
git reset --hard HEAD~1  # Oups !

# Solution immédiate avec reflog
git reflog
git reset --hard HEAD@{1}
```

#### Erreur 3 : Reset avec des modifications non commitées

```bash
# Vous avez des modifications en cours
# Vous faites un reset --hard
git reset --hard HEAD~1

# ⚠️ Vos modifications en cours sont PERDUES
# Pas de solution si elles n'étaient pas commitées

# PRÉVENTION : Toujours commit ou stash avant reset --hard
git stash  # Met de côté vos modifications
git reset --hard HEAD~1
git stash pop  # Récupère vos modifications
```

---

### Bonnes pratiques

#### 1. Toujours vérifier l'état avant

```bash
# Avant un reset, vérifiez où vous êtes
git log --oneline -10
git status
```

#### 2. Commencer par --soft

Si vous n'êtes pas sûr, commencez par `--soft`. Vous pouvez toujours faire ensuite :

```bash
git reset --soft HEAD~1    # Mode doux
# Vérifier que c'est bien ce que vous voulez
# Si oui et vous voulez --hard :
git reset --hard HEAD      # Supprime les modifications
```

#### 3. Utiliser --hard uniquement en connaissance de cause

Avant un `--hard`, posez-vous la question :
- Ai-je des modifications non commitées importantes ?
- Suis-je sûr de vouloir tout perdre ?
- Ai-je un backup ou puis-je récupérer avec reflog ?

#### 4. Faire un commit de sauvegarde

Avant un reset risqué, créez un commit ou une branche de sauvegarde :

```bash
# Créer une branche de sauvegarde
git branch backup-avant-reset

# Faire votre reset
git reset --hard HEAD~3

# Si problème, revenir à la sauvegarde
git reset --hard backup-avant-reset
```

---

### Aide-mémoire visuel

```
git reset --soft HEAD~1
═══════════════════════
Commit A ← Commit B ← [Commit C supprimé]
              ↑
            HEAD
Staging Area: [contenu de Commit C]
Working Dir:  [contenu de Commit C]


git reset --mixed HEAD~1 (ou git reset HEAD~1)
══════════════════════════════════════════════
Commit A ← Commit B ← [Commit C supprimé]
              ↑
            HEAD
Staging Area: [vide]
Working Dir:  [contenu de Commit C]


git reset --hard HEAD~1
═══════════════════════
Commit A ← Commit B ← [Commit C supprimé]
              ↑
            HEAD
Staging Area: [vide]
Working Dir:  [contenu de Commit B] ⚠️ perte de données
```

---

### Résumé des commandes

```bash
# Reset --soft (garde tout)
git reset --soft HEAD~1
git reset --soft a1b2c3d

# Reset --mixed (vide staging)
git reset HEAD~1         # mode par défaut
git reset --mixed HEAD~1
git reset a1b2c3d

# Reset --hard (supprime tout)
git reset --hard HEAD~1
git reset --hard a1b2c3d

# Récupération d'urgence
git reflog
git reset --hard HEAD@{1}
```

---

### Points clés à retenir

✅ `git reset` déplace HEAD vers un commit antérieur

✅ `--soft` garde tout (staging + working directory)

✅ `--mixed` garde les modifications dans le working directory

✅ `--hard` supprime TOUT (⚠️ dangereux !)

❌ Ne jamais reset des commits déjà poussés publiquement

✅ `git reflog` peut sauver en cas d'erreur

✅ Toujours vérifier avec `git status` et `git log` avant un reset

✅ En cas de doute, utiliser `--soft` d'abord

---

### Conclusion

`git reset` est un outil puissant qui vous donne un contrôle précis sur l'historique de vos commits. Les trois options (`--soft`, `--mixed`, `--hard`) offrent différents niveaux de "retour en arrière" selon vos besoins.

**Retenez surtout :**
- `--soft` : le plus sûr, garde tout
- `--mixed` : vide la staging area, garde les fichiers
- `--hard` : supprime tout, à utiliser avec précaution

Avec la pratique, vous saurez instinctivement quelle option utiliser selon la situation. Et rappelez-vous : `git reflog` est votre filet de sécurité en cas d'erreur !

Dans la section suivante, nous verrons `git revert`, qui permet d'annuler des commits de manière sûre, même s'ils sont déjà publics.

⏭️ [Annuler un commit publié (git revert)](/module-03-corriger-et-modifier/04-annuler-un-commit-publie.md)
