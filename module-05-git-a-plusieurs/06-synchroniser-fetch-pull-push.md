🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 6. Synchroniser : git fetch, git pull, git push

### Introduction

Maintenant que votre projet est connecté à un dépôt distant, vous devez apprendre à **synchroniser** votre travail. La synchronisation, c'est l'échange de commits entre votre dépôt local et le dépôt distant.

Il existe trois commandes essentielles pour synchroniser :
- **`git fetch`** : récupère les nouveautés sans les fusionner
- **`git pull`** : récupère et fusionne automatiquement
- **`git push`** : envoie vos commits vers le distant

Comprendre ces trois commandes et leurs différences est fondamental pour bien collaborer avec Git.

### Le cycle de synchronisation typique

Voici comment fonctionne la collaboration quotidienne avec Git :

```
1. Vous récupérez les nouveautés des autres (fetch ou pull)
2. Vous travaillez en local (modifications, commits)
3. Vous envoyez votre travail vers le distant (push)
4. Vos collègues répètent le cycle
```

C'est un flux continu d'échanges entre votre machine et le serveur.

### git fetch : récupérer sans fusionner

#### Qu'est-ce que fetch ?

`git fetch` télécharge les nouveaux commits depuis le dépôt distant, mais ne les intègre **pas** dans votre branche actuelle. C'est comme récupérer le courrier sans encore l'ouvrir.

**Syntaxe de base :**
```bash
git fetch
```

ou pour un distant spécifique :
```bash
git fetch origin
```

#### Que fait fetch exactement ?

Quand vous exécutez `git fetch`, Git :

1. Se connecte au dépôt distant
2. Télécharge tous les nouveaux commits
3. Télécharge les nouvelles branches distantes
4. Met à jour les références distantes (comme `origin/main`)
5. **N'affecte PAS** votre branche de travail locale

#### Visualiser ce que fetch a récupéré

Après un fetch, vos branches distantes sont à jour, mais pas vos branches locales :

```bash
# Avant fetch
Branche locale : main (commit A)
Branche distante : origin/main (commit A)

# Après fetch (si quelqu'un a poussé le commit B)
Branche locale : main (commit A) <- vous êtes ici
Branche distante : origin/main (commit B) <- nouveauté téléchargée
```

Pour voir les différences :
```bash
# Voir l'historique de la branche distante
git log origin/main

# Comparer avec votre branche locale
git log main..origin/main

# Voir les différences de fichiers
git diff main origin/main
```

#### Quand utiliser fetch ?

**Vérifier avant de fusionner**
Vous voulez voir ce que les autres ont fait avant d'intégrer leurs changements :
```bash
git fetch
git log origin/main  # Examiner les nouveaux commits
git diff main origin/main  # Voir les changements
# Si tout va bien, alors fusionner
git merge origin/main
```

**Récupérer des informations sur les branches**
Quelqu'un a créé une nouvelle branche distante :
```bash
git fetch
git branch -r  # Voir toutes les branches distantes
```

**Travailler avec plusieurs distants**
Vous voulez récupérer depuis plusieurs sources :
```bash
git fetch origin
git fetch upstream
```

**Éviter les surprises**
Vous préférez un contrôle total sur ce qui entre dans votre code.

#### Avantages de fetch

- **Sécurité** : aucun risque de casser votre code en cours
- **Contrôle** : vous décidez quand et comment intégrer les changements
- **Inspection** : vous pouvez examiner les nouveautés avant de les fusionner
- **Flexibilité** : vous pouvez choisir de fusionner, rebaser, ou ne rien faire

### git pull : récupérer et fusionner

#### Qu'est-ce que pull ?

`git pull` est une commande combinée qui fait deux choses d'un coup :

1. **Fetch** : télécharge les nouveautés
2. **Merge** : fusionne automatiquement dans votre branche actuelle

C'est un raccourci pratique, mais moins contrôlé que fetch.

**Syntaxe de base :**
```bash
git pull
```

ou de manière plus explicite :
```bash
git pull origin main
```

#### Que fait pull exactement ?

```bash
git pull
# équivaut à
git fetch
git merge origin/main
```

Git récupère les commits distants et les fusionne immédiatement dans votre branche locale.

#### Exemple visuel

```
Avant pull :
Local:    A---B---C (main)
Distant:  A---B---C---D---E (origin/main)

Après pull :
Local:    A---B---C-------M (main)
                   \     /
Distant:            D---E (origin/main)
```

Si vous aviez des commits locaux, Git créera un commit de fusion (M).

#### Quand utiliser pull ?

**Workflow quotidien simple**
Vous commencez votre journée de travail :
```bash
git pull  # Récupérer le travail des autres
# Commencer à travailler
```

**Branche à jour avec le distant**
Vous n'avez pas de modifications locales et voulez juste mettre à jour :
```bash
git pull
```

**Confiance dans l'équipe**
Vous savez que le code distant est bon et voulez l'intégrer rapidement.

**Projet personnel**
Vous synchronisez entre deux ordinateurs (travail/maison).

#### Pull avec rebase

Par défaut, `git pull` utilise merge. Vous pouvez utiliser rebase à la place :

```bash
git pull --rebase
```

Cela équivaut à :
```bash
git fetch
git rebase origin/main
```

Le résultat est un historique linéaire sans commit de fusion :

```
Avant pull --rebase :
Local:    A---B---C---F (main, vos commits)
Distant:  A---B---C---D---E (origin/main)

Après pull --rebase :
Local:    A---B---C---D---E---F' (main)
```

Vos commits (F) sont rejoués après les commits distants, donnant un historique plus propre.

#### Configurer pull pour utiliser rebase par défaut

```bash
git config pull.rebase true  # Pour ce dépôt
git config --global pull.rebase true  # Pour tous vos dépôts
```

### git push : envoyer vos commits

#### Qu'est-ce que push ?

`git push` envoie vos commits locaux vers le dépôt distant. C'est le contraire de pull : au lieu de recevoir, vous donnez.

**Syntaxe de base :**
```bash
git push
```

ou de manière plus explicite :
```bash
git push origin main
```

#### Que fait push exactement ?

Quand vous exécutez `git push`, Git :

1. Vérifie que votre branche locale est à jour
2. Se connecte au dépôt distant
3. Envoie vos nouveaux commits
4. Met à jour la branche distante

```
Avant push :
Local:    A---B---C---D (main)
Distant:  A---B---C (origin/main)

Après push :
Local:    A---B---C---D (main)
Distant:  A---B---C---D (origin/main)
```

#### Quand utiliser push ?

**Après chaque série de commits**
Vous avez terminé une fonctionnalité :
```bash
git add .
git commit -m "Ajout de la fonctionnalité X"
git push
```

**En fin de journée**
Sauvegarde de votre travail :
```bash
git push  # Tout est maintenant sauvegardé en ligne
```

**Avant de changer d'ordinateur**
Vous voulez continuer sur une autre machine :
```bash
git push  # Sur le premier ordinateur
# Sur le second ordinateur
git pull
```

**Pour partager avec l'équipe**
Vos collègues ont besoin de votre code :
```bash
git push  # Ils peuvent maintenant faire git pull
```

#### Push une nouvelle branche

Quand vous créez une branche locale et voulez la publier :

```bash
git checkout -b nouvelle-fonctionnalite
# Travail et commits
git push -u origin nouvelle-fonctionnalite
```

L'option `-u` (ou `--set-upstream`) crée le lien de suivi pour les pushs futurs.

#### Push forcé (à utiliser avec précaution !)

**Push normal :**
```bash
git push
```

**Push forcé :**
```bash
git push --force
# ou
git push -f
```

**⚠️ ATTENTION :** Le push forcé écrase l'historique distant. À utiliser seulement si :
- Vous êtes seul sur la branche
- Vous avez fait un rebase et devez mettre à jour le distant
- Vous savez exactement ce que vous faites

**Jamais** sur une branche partagée comme `main` ou `develop` !

**Alternative plus sûre :**
```bash
git push --force-with-lease
```

Cette option refuse le push si quelqu'un d'autre a poussé entre temps, évitant d'écraser le travail des autres.

### Différences fetch vs pull

| Aspect | git fetch | git pull |
|--------|-----------|----------|
| **Téléchargement** | Oui | Oui |
| **Fusion automatique** | Non | Oui |
| **Sécurité** | Très sûr | Peut créer des conflits |
| **Contrôle** | Total | Automatique |
| **Inspection possible** | Oui, avant fusion | Non, déjà fusionné |
| **Usage** | Vérification prudente | Mise à jour rapide |
| **Branche locale affectée** | Non | Oui |

### Workflow recommandé pour débutants

**Approche sûre avec fetch :**

```bash
# 1. Récupérer les nouveautés
git fetch origin

# 2. Voir ce qui a changé
git log origin/main

# 3. Examiner les différences
git diff main origin/main

# 4. Si tout va bien, fusionner
git merge origin/main

# 5. Travailler et commiter
git add .
git commit -m "Votre message"

# 6. Envoyer vos commits
git push origin main
```

**Approche rapide avec pull :**

```bash
# 1. Mettre à jour
git pull

# 2. Travailler et commiter
git add .
git commit -m "Votre message"

# 3. Envoyer
git push
```

### Gestion des conflits lors du pull

Parfois, `git pull` rencontre des **conflits** : vous et quelqu'un d'autre avez modifié les mêmes lignes de code.

**Scénario :**
```bash
git pull
# Auto-merging fichier.txt
# CONFLICT (content): Merge conflict in fichier.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

**Résolution :**

1. **Voir les fichiers en conflit :**
```bash
git status
```

2. **Ouvrir les fichiers** et chercher les marqueurs de conflit :
```
<<<<<<< HEAD
Votre code local
=======
Code du distant
>>>>>>> origin/main
```

3. **Choisir la bonne version** et supprimer les marqueurs

4. **Marquer comme résolu :**
```bash
git add fichier.txt
```

5. **Terminer la fusion :**
```bash
git commit
```

Nous verrons la résolution de conflits en détail dans le Module 4 sur les branches.

### Erreurs courantes et solutions

#### "Your branch is behind 'origin/main'"

**Signification :** Le distant a des commits que vous n'avez pas.

**Solution :**
```bash
git pull
```

#### "Your branch is ahead of 'origin/main'"

**Signification :** Vous avez des commits locaux non poussés.

**Solution :**
```bash
git push
```

#### "Your branch and 'origin/main' have diverged"

**Signification :** Vous et le distant avez des commits différents.

**Solution :**
```bash
git pull  # Créera un commit de fusion
# ou
git pull --rebase  # Rebase vos commits
```

#### "error: failed to push some refs"

**Cause :** Le distant contient du travail que vous n'avez pas.

**Solution :**
```bash
git pull  # D'abord récupérer
git push  # Puis pousser
```

#### "fatal: refusing to merge unrelated histories"

**Cause :** Les historiques local et distant sont complètement différents.

**Solution :**
```bash
git pull --allow-unrelated-histories
```

Cette situation arrive si vous avez initialisé le dépôt local et créé le distant séparément avec un README.

#### "You have unstaged changes"

**Cause :** Vous avez des modifications non commitées.

**Solutions :**

Commiter d'abord :
```bash
git add .
git commit -m "WIP: travail en cours"
git pull
```

Ou mettre de côté temporairement :
```bash
git stash
git pull
git stash pop
```

### Vérifier l'état de synchronisation

**Voir où vous en êtes :**
```bash
git status
```

Messages possibles :
```
Your branch is up to date with 'origin/main'.
→ Parfaitement synchronisé

Your branch is ahead of 'origin/main' by 2 commits.
→ Vous avez 2 commits à pousser

Your branch is behind 'origin/main' by 3 commits.
→ Il y a 3 commits à récupérer

Your branch and 'origin/main' have diverged.
→ Historiques différents, besoin de fusion
```

**Comparer les branches :**
```bash
# Commits dans le distant mais pas en local
git log main..origin/main

# Commits en local mais pas dans le distant
git log origin/main..main

# Vue graphique de la divergence
git log --all --graph --oneline
```

### Bonnes pratiques de synchronisation

**1. Pullez avant de commencer à travailler**

Chaque matin ou session de travail :
```bash
git pull
```

Cela évite de travailler sur du code obsolète.

**2. Pushez régulièrement**

Ne laissez pas s'accumuler des dizaines de commits :
```bash
# Après chaque fonctionnalité terminée
git push
```

**3. Committez avant de puller**

Ne pullez jamais avec des modifications non commitées :
```bash
git add .
git commit -m "Message"
git pull
```

**4. Communiquez avec l'équipe**

Prévenez avant de pousser des changements majeurs.

**5. Utilisez des branches**

Travaillez sur des branches séparées pour éviter les conflits sur `main` :
```bash
git checkout -b ma-fonctionnalite
# Travail
git push -u origin ma-fonctionnalite
```

**6. Fetch pour vérifier avant pull**

Quand vous n'êtes pas sûr :
```bash
git fetch
git log origin/main  # Vérifier
git pull  # Si tout va bien
```

**7. Résolvez les conflits rapidement**

Ne laissez pas traîner les conflits, résolvez-les dès qu'ils apparaissent.

### Synchronisation avec plusieurs distants

Si vous avez plusieurs distants (origin, upstream) :

```bash
# Récupérer depuis tous les distants
git fetch --all

# Récupérer depuis upstream (projet original)
git fetch upstream

# Fusionner upstream dans votre branche
git merge upstream/main

# Pousser vers votre fork
git push origin main
```

### Automatiser avec des alias

Créez des raccourcis pour vos commandes fréquentes :

```bash
# Alias pour pull avec rebase
git config --global alias.pr 'pull --rebase'

# Alias pour push forcé sécurisé
git config --global alias.pf 'push --force-with-lease'

# Alias pour synchroniser complètement
git config --global alias.sync '!git fetch && git pull && git push'

# Utilisation
git pr
git pf
git sync
```

### Résumé des commandes

```bash
# Récupérer sans fusionner
git fetch

# Récupérer depuis un distant spécifique
git fetch origin

# Récupérer et fusionner
git pull

# Récupérer et rebaser
git pull --rebase

# Envoyer vers le distant
git push

# Envoyer une nouvelle branche
git push -u origin nom-branche

# Push forcé (danger !)
git push --force

# Push forcé sécurisé
git push --force-with-lease

# Voir l'état de synchronisation
git status

# Comparer avec le distant
git diff main origin/main
```

### Ce qu'il faut retenir

La synchronisation repose sur trois commandes complémentaires :

- **`git fetch`** : télécharge les nouveautés, vous garde le contrôle
- **`git pull`** : télécharge et fusionne automatiquement, pratique mais moins sûr
- **`git push`** : envoie vos commits vers le distant

Le workflow de base est simple :
1. Pull ou fetch pour récupérer
2. Travail et commits en local
3. Push pour partager

Pour débuter, `git pull` et `git push` suffiront. Avec l'expérience, vous apprécierez la prudence de `git fetch` pour les situations complexes.

L'essentiel est de synchroniser régulièrement : pullez souvent pour rester à jour, pushez souvent pour sauvegarder et partager. La clé d'une bonne collaboration, c'est une synchronisation fréquente et une communication claire avec l'équipe !

⏭️ [Authentification : Clés SSH et Personal Access Tokens (PAT)](/module-05-git-a-plusieurs/07-gerer-les-cles-ssh-et-pat.md)
