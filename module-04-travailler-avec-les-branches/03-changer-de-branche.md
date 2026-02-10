🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 3. Changer de branche (git checkout et git switch)

### Introduction

Maintenant que vous savez créer des branches, il est temps d'apprendre à naviguer entre elles. Changer de branche est une opération que vous ferez des dizaines de fois par jour, et Git la rend extrêmement rapide et fluide.

**Analogie :** Imaginez que vous travaillez sur plusieurs puzzles en même temps. Chaque puzzle est posé sur une table différente. Changer de branche, c'est comme passer d'une table à une autre. Quand vous revenez à un puzzle, vous le retrouvez exactement dans l'état où vous l'aviez laissé.

---

### Les deux commandes pour changer de branche

Git offre deux commandes pour changer de branche :

1. **`git checkout`** (historique, polyvalente mais parfois confuse)
2. **`git switch`** (moderne, claire et spécialisée)

**Recommandation :** Si vous avez Git 2.23 ou supérieur (2019+), utilisez `git switch`. C'est plus clair et moins sujet à confusion.

---

### Méthode 1 : `git checkout` (Commande historique)

#### Syntaxe de base

```bash
git checkout nom-de-la-branche
```

#### Exemple simple

```bash
# Voir où vous êtes
git branch
# * main
#   feature-login
#   feature-payment

# Changer vers feature-login
git checkout feature-login
# Switched to branch 'feature-login'

# Vérification
git branch
#   main
# * feature-login
#   feature-payment
```

**Visualisation avant/après :**

```
Avant :  
A ← B ← C ← D
        ↑   ↑
      main  feature-login (HEAD)

Après git checkout main :  
A ← B ← C ← D
        ↑   ↑
      main (HEAD)  feature-login
```

HEAD s'est déplacé de `feature-login` vers `main`.

---

### Méthode 2 : `git switch` (Commande moderne)

#### Syntaxe de base

```bash
git switch nom-de-la-branche
```

#### Exemple simple

```bash
# Changer vers feature-payment
git switch feature-payment
# Switched to branch 'feature-payment'

# Vérification
git branch
#   main
#   feature-login
# * feature-payment
```

**Pourquoi `git switch` est meilleur que `git checkout` ?**

`git checkout` fait trop de choses différentes :
- Changer de branche : `git checkout branche`
- Restaurer un fichier : `git checkout -- fichier`
- Créer une branche : `git checkout -b nouvelle`
- Se déplacer vers un commit : `git checkout abc123`

`git switch` est dédié uniquement au changement de branche, ce qui le rend plus clair et moins sujet à erreur.

---

### Comparaison des deux commandes

| Tâche | `git checkout` | `git switch` |
|-------|---------------|--------------|
| Changer de branche | `git checkout branche` | `git switch branche` |
| Créer et changer | `git checkout -b branche` | `git switch -c branche` |
| Retour à branche précédente | `git checkout -` | `git switch -` |
| Forcer le changement | `git checkout -f branche` | `git switch -f branche` |

**Les deux font exactement la même chose** pour changer de branche. La différence est surtout dans la clarté de l'intention.

---

### Ce qui se passe quand vous changez de branche

#### Trois actions simultanées

Quand vous changez de branche, Git fait trois choses :

1. **Déplace HEAD** vers la nouvelle branche
2. **Met à jour la Staging Area** avec le contenu de la nouvelle branche
3. **Met à jour le Working Directory** avec les fichiers de la nouvelle branche

**Visualisation :**

```
┌─────────────────────────────┐
│  Working Directory          │  ← Fichiers modifiés
│  index.html, style.css      │
└─────────────────────────────┘
                ↓ git switch autre-branche
┌─────────────────────────────┐
│  Working Directory          │  ← Fichiers de autre-branche
│  index.html, style.css      │
└─────────────────────────────┘
```

#### Exemple concret

```bash
# Sur la branche main
git switch main  
cat index.html
# <h1>Version Main</h1>

# Créer et basculer vers une branche feature
git switch -c feature-new-design  
echo "<h1>Nouveau Design</h1>" > index.html  
git commit -am "Nouveau design"

# Retour à main
git switch main  
cat index.html
# <h1>Version Main</h1>  ← Le fichier a changé !

# Retour à feature
git switch feature-new-design  
cat index.html
# <h1>Nouveau Design</h1>  ← Le fichier revient !
```

**Important :** Vos fichiers physiques changent réellement quand vous changez de branche. Git remplace leur contenu par celui de la branche cible.

---

### Raccourci : Revenir à la branche précédente

Comme dans un terminal où `cd -` vous ramène au dossier précédent, Git offre un raccourci similaire.

```bash
# Aller vers feature-A
git switch feature-A

# Travailler...

# Aller vers main
git switch main

# Revenir à feature-A rapidement
git switch -
# Switched to branch 'feature-A'

# Alterner entre les deux
git switch -  # Retour à main  
git switch -  # Retour à feature-A
```

**Avec `git checkout` :**

```bash
git checkout -
```

---

### Créer et changer en une seule commande

Nous avons déjà vu cette syntaxe dans la section précédente, mais voici un rappel :

#### Avec `git checkout`

```bash
git checkout -b nouvelle-branche
```

#### Avec `git switch`

```bash
git switch -c nouvelle-branche
```

L'option `-c` signifie "create" (créer).

**Exemple :**

```bash
# Sur main, créer une branche feature et y aller directement
git switch -c feature-dark-mode

# Équivalent à :
git branch feature-dark-mode  
git switch feature-dark-mode
```

---

### Situations problématiques

#### Problème 1 : Modifications non commitées

Si vous avez des modifications non commitées, Git peut refuser de changer de branche pour éviter de les perdre.

**Scénario :**

```bash
# Vous êtes sur main et modifiez un fichier
echo "Modification" >> index.html  
git status
# Changes not staged for commit:
#   modified: index.html

# Essayer de changer de branche
git switch feature-login
# error: Your local changes to the following files would be overwritten by checkout:
#       index.html
# Please commit your changes or stash them before you switch branches.
# Aborting
```

**Solutions possibles :**

##### Solution 1 : Commiter les modifications

```bash
git add index.html  
git commit -m "Modifications en cours"  
git switch feature-login
```

##### Solution 2 : Mettre de côté avec stash

```bash
git stash
# Saved working directory and index state WIP on main

git switch feature-login
# Travaillez sur feature-login...

git switch main  
git stash pop
# Récupérez vos modifications
```

##### Solution 3 : Annuler les modifications

```bash
git restore index.html  
git switch feature-login
```

##### Solution 4 : Forcer le changement (⚠️ Dangereux)

```bash
git switch -f feature-login
# ou
git checkout -f feature-login
```

**Attention :** L'option `-f` (force) écrasera vos modifications non commitées. Elles seront perdues définitivement !

---

#### Problème 2 : Fichiers non trackés

Les fichiers non trackés (jamais ajoutés à Git) suivent généralement sans problème lors du changement de branche.

```bash
# Créer un nouveau fichier
echo "Test" > nouveau.txt

# Changer de branche : pas de problème
git switch feature-login
# Switched to branch 'feature-login'

# Le fichier nouveau.txt est toujours là
ls
# nouveau.txt  (il suit sur toutes les branches)
```

**Pourquoi ?** Git n'a pas de version de ce fichier, donc il ne risque pas de conflit.

**Cas problématique :**

```bash
# Sur main : créer un fichier
echo "Version Main" > config.txt

# Changer vers une branche qui a déjà un config.txt
git switch feature-payment
# error: The following untracked working tree files would be overwritten by checkout:
#       config.txt
# Please move or remove them before you switch branches.
```

**Solution :** Renommer ou supprimer le fichier non tracké, ou le commiter.

---

#### Problème 3 : Conflits potentiels

Si Git ne peut pas déterminer de manière sûre comment fusionner vos modifications locales avec la branche cible, il refuse le changement.

```bash
git switch autre-branche
# error: Your local changes to the following files would be overwritten by checkout:
#       fichier.txt
# Please commit your changes or stash them before you switch branches.
```

**Règle Git :** Il vaut mieux prévenir que de perdre du travail. Git préfère refuser le changement plutôt que d'écraser des modifications.

---

### Détacher HEAD (Detached HEAD)

Vous pouvez vous déplacer directement sur un commit sans être sur une branche. C'est appelé "detached HEAD" (HEAD détaché).

#### Comment entrer en detached HEAD

```bash
# Se déplacer vers un commit spécifique
git checkout a1b2c3d
# Note: switching to 'a1b2c3d'.
# You are in 'detached HEAD' state...
```

**Visualisation :**

```
A ← B ← C ← D
    ↑   ↑
  HEAD  main
  (detached)
```

HEAD pointe directement sur le commit B, pas sur une branche.

#### Que faire en detached HEAD ?

Vous pouvez :
- **Explorer** : regarder les fichiers tels qu'ils étaient
- **Tester** : exécuter du code ancien
- **Comparer** : voir les différences

**Mais attention :** Si vous faites des commits en detached HEAD, ils risquent d'être perdus !

```bash
# En detached HEAD, vous faites un commit
echo "Test" > fichier.txt  
git add fichier.txt  
git commit -m "Test commit"
# [detached HEAD abc123] Test commit

# Retour à une branche
git switch main
# Warning: you are leaving 1 commit behind, not connected to any branches
```

#### Sauver votre travail en detached HEAD

Si vous avez fait des commits en detached HEAD et voulez les garder :

```bash
# Créer une branche à partir d'ici
git switch -c nouvelle-branche

# Ou avec checkout
git checkout -b nouvelle-branche
```

#### Sortir de detached HEAD

```bash
# Revenir à une branche normale
git switch main
# ou
git checkout main
```

---

### Vérifier sur quelle branche vous êtes

#### Commande 1 : `git branch`

```bash
git branch
#   develop
#   feature-login
# * main
```

L'astérisque `*` indique la branche actuelle.

#### Commande 2 : `git status`

```bash
git status
# On branch main
# nothing to commit, working tree clean
```

La première ligne indique toujours la branche.

#### Commande 3 : Afficher dans le prompt

Vous pouvez configurer votre terminal pour afficher la branche actuelle dans le prompt.

**Bash/Zsh (Linux/Mac) :**

```bash
# Exemple de résultat :
user@computer ~/project (main) $  
user@computer ~/project (feature-login) $
```

**Configuration automatique avec Oh My Zsh ou Git Bash.**

---

### Workflow typique de changement de branche

#### Scénario : Interruption pour une urgence

```bash
# Vous travaillez sur une feature
git switch feature-newsletter
# ... modifications en cours ...

# URGENCE : bug critique sur main !
# Sauvegarder votre travail en cours
git stash save "WIP: newsletter form"

# Aller sur main
git switch main

# Créer une branche pour le hotfix
git switch -c hotfix-critical-bug

# Corriger le bug
# ... corrections ...
git add .  
git commit -m "Fix critical security bug"

# Fusionner dans main
git switch main  
git merge hotfix-critical-bug

# Pousser
git push origin main

# Nettoyer
git branch -d hotfix-critical-bug

# Retourner à votre travail
git switch feature-newsletter  
git stash pop
# Continuer le développement
```

---

### Cas pratiques détaillés

#### Cas 1 : Développement sur plusieurs features en parallèle

```bash
# Feature A : système de login
git switch -c feature-login
# ... développer ...
git commit -am "WIP: login form"

# Changer vers Feature B : système de paiement
git switch -c feature-payment
# ... développer ...
git commit -am "WIP: payment integration"

# Revenir à Feature A
git switch feature-login
# Continuer le login
git commit -am "Complete login feature"

# Finir Feature A
git switch main  
git merge feature-login

# Revenir à Feature B
git switch feature-payment
# Continuer le paiement...
```

#### Cas 2 : Tester une ancienne version

```bash
# Voir l'historique
git log --oneline
# d4e5f6g (HEAD -> main) Version actuelle avec bug
# c3d4e5f Ajout feature X
# b2c3d4e Dernière version stable
# a1b2c3d Version initiale

# Se déplacer vers la version stable pour tester
git checkout b2c3d4e
# En detached HEAD

# Tester...
npm test  
npm start

# Tout fonctionne, retour à main
git switch main

# Créer une branche pour corriger basée sur la version stable
git switch -c fix-from-stable b2c3d4e
```

#### Cas 3 : Basculer rapidement entre branches

```bash
# Travailler sur feature-A
git switch feature-A
# ... code ...

# Vérifier quelque chose sur main
git switch main
# ... vérification ...

# Retour rapide à feature-A
git switch -

# Aller sur feature-B
git switch feature-B
# ... code ...

# Retour rapide à feature-A
git switch -
```

---

### Options avancées

#### Option `--discard-changes` (Git 2.23+)

Force le changement même avec modifications locales, en les écrasant.

```bash
# Avec git switch
git switch --discard-changes feature-autre

# Équivalent à git checkout -f
git checkout -f feature-autre
```

#### Option `--merge`

Tente de fusionner les modifications locales avec la branche cible.

```bash
git switch --merge feature-autre
```

Si ça réussit, vos modifications locales sont préservées. Si ça échoue, vous obtenez des marqueurs de conflit à résoudre.

#### Option `--no-guess`

Par défaut, Git devine automatiquement les branches distantes.

```bash
# Si origin/feature-X existe, ceci créera une branche locale qui la track
git switch feature-X

# Désactiver ce comportement
git switch --no-guess feature-X
```

---

### Différences entre `git checkout` et `git switch`

| Aspect | `git checkout` | `git switch` |
|--------|---------------|--------------|
| **Année d'introduction** | 2005 (origine de Git) | 2019 (Git 2.23) |
| **Objectif** | Polyvalent (trop) | Spécialisé pour les branches |
| **Clarté** | Ambigu (fait trop de choses) | Clair (une seule responsabilité) |
| **Changer de branche** | ✅ `checkout branche` | ✅ `switch branche` |
| **Restaurer fichier** | ✅ `checkout -- fichier` | ❌ Utiliser `git restore` |
| **Compatibilité** | Tous les Git | Git 2.23+ (2019+) |

**Recommandation moderne :**
- `git switch` pour changer de branche
- `git restore` pour restaurer des fichiers
- Oublier `git checkout` (sauf pour compatibilité avec vieux tutoriels)

---

### Erreurs courantes et solutions

#### Erreur 1 : Branche inexistante

```bash
git switch feature-inexistante
# error: pathspec 'feature-inexistante' did not match any file(s) known to git
```

**Solutions :**

```bash
# Vérifier les branches disponibles
git branch -a

# Créer la branche au lieu de basculer dessus
git switch -c feature-inexistante
```

#### Erreur 2 : Modifications non commitées bloquent le changement

```bash
git switch autre-branche
# error: Your local changes would be overwritten
```

**Solutions :**

```bash
# 1. Commiter
git commit -am "WIP"

# 2. Stasher
git stash

# 3. Annuler
git restore .

# 4. Forcer (⚠️ perte de données)
git switch -f autre-branche
```

#### Erreur 3 : Nom ambigu entre branche et fichier

```bash
# Si vous avez un fichier et une branche avec le même nom
git switch test
# Ambiguïté possible
```

**Solution :**

```bash
# Être explicite
git switch test          # Pour la branche  
git restore -- test      # Pour le fichier
```

---

### Bonnes pratiques

#### 1. Toujours vérifier où vous êtes

```bash
# Avant de faire des modifications importantes
git branch
# ou
git status
```

#### 2. Commiter ou stasher avant de changer

```bash
# Au lieu de forcer
git switch -f autre-branche  # ❌ Mauvais

# Faire proprement
git stash                    # ✅ Bon  
git switch autre-branche
```

#### 3. Utiliser des noms de branche clairs

```bash
# ✅ Bon
git switch feature-user-authentication

# ❌ Éviter
git switch test  
git switch tmp  
git switch nouvelle
```

#### 4. Nettoyer les branches après fusion

```bash
git switch main  
git merge feature-completed  
git branch -d feature-completed  # Supprimer après fusion
```

#### 5. Configurer l'autocomplétion

Activez l'autocomplétion dans votre terminal pour éviter les erreurs de frappe.

```bash
# Avec autocomplétion, tapez :
git switch feat<TAB>
# Se complète en :
git switch feature-login
```

---

### Aide-mémoire des commandes

```bash
# CHANGER DE BRANCHE
git switch branche              # Méthode moderne  
git checkout branche            # Méthode classique

# CRÉER ET CHANGER
git switch -c nouvelle          # Méthode moderne  
git checkout -b nouvelle        # Méthode classique

# RACCOURCIS
git switch -                    # Retour à la branche précédente  
git checkout -                  # Retour à la branche précédente (classique)

# FORCER LE CHANGEMENT (⚠️ Dangereux)
git switch -f branche           # Écrase modifications locales  
git checkout -f branche         # Écrase modifications locales

# AVEC MERGE DES MODIFICATIONS
git switch --merge branche      # Tente de fusionner modifications

# DETACHED HEAD
git checkout <commit>           # Se déplacer vers un commit  
git switch -c nouvelle-branche  # Créer branche depuis detached HEAD

# VÉRIFIER LA BRANCHE ACTUELLE
git branch                      # Voir toutes les branches (avec *)  
git status                      # Première ligne indique la branche
```

---

### Points clés à retenir

✅ **`git switch`** est la commande moderne recommandée

✅ **Changer de branche modifie vos fichiers** physiquement

✅ **Git refuse de changer** si ça écrase des modifications non commitées

✅ **`git switch -`** pour revenir rapidement à la branche précédente

✅ **Commits en detached HEAD** risquent d'être perdus

✅ **Toujours vérifier** sur quelle branche vous êtes avant de travailler

✅ **Stash ou commit** avant de changer de branche

✅ **`-f` écrase les modifications** → à utiliser avec précaution

---

### Conclusion

Changer de branche est l'une des opérations les plus fréquentes dans Git, et une fois que vous y êtes habitué, cela devient naturel et instantané. La commande `git switch` rend cette opération plus intuitive et moins sujette à erreur que l'ancien `git checkout`.

**Métaphore finale :** Pensez aux branches comme à des espaces de travail parallèles. Changer de branche, c'est comme changer de bureau : quand vous revenez, tout est exactement comme vous l'aviez laissé.

**Workflow recommandé :**
1. Vérifiez toujours où vous êtes (`git branch`)
2. Sauvegardez votre travail en cours (commit ou stash)
3. Changez de branche (`git switch`)
4. Travaillez
5. Revenez quand vous voulez

Dans la prochaine section, nous verrons **la fusion de branches** avec `git merge`, qui permet de combiner le travail de plusieurs branches en une seule.

⏭️ [Fusion de branches (git merge)](/module-04-travailler-avec-les-branches/04-fusion-de-branches.md)
