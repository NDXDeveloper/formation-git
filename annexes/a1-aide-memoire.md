🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Annexes
## Aide-mémoire des commandes Git

Cet aide-mémoire regroupe toutes les commandes Git essentielles organisées par thématique. Gardez ce document à portée de main pour une consultation rapide lors de votre travail quotidien avec Git.

---

## Configuration initiale

### Configurer votre identité

```bash
# Configuration globale (pour tous vos projets)
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Configuration locale (pour un projet spécifique)
git config --local user.name "Autre Nom"
git config --local user.email "autre.email@example.com"

# Voir toute la configuration
git config --list

# Voir une valeur spécifique
git config user.name
```

### Configurer l'éditeur par défaut

```bash
# Utiliser VSCode
git config --global core.editor "code --wait"

# Utiliser Nano
git config --global core.editor "nano"

# Utiliser Vim
git config --global core.editor "vim"
```

### Autres configurations utiles

```bash
# Activer la coloration
git config --global color.ui auto

# Définir la branche par défaut
git config --global init.defaultBranch main

# Configurer le comportement de pull
git config --global pull.rebase false  # merge (par défaut)
git config --global pull.rebase true   # rebase
```

---

## Initialisation et clonage

### Créer un nouveau dépôt

```bash
# Initialiser un dépôt dans le dossier actuel
git init

# Initialiser un dépôt avec un nom de branche spécifique
git init --initial-branch=main

# Initialiser dans un nouveau dossier
git init nom-du-projet
cd nom-du-projet
```

### Cloner un dépôt existant

```bash
# Cloner un dépôt distant
git clone https://github.com/utilisateur/depot.git

# Cloner dans un dossier spécifique
git clone https://github.com/utilisateur/depot.git mon-dossier

# Cloner seulement la dernière version (plus rapide)
git clone --depth 1 https://github.com/utilisateur/depot.git

# Cloner une branche spécifique
git clone -b nom-branche https://github.com/utilisateur/depot.git
```

---

## Commandes de base

### Voir l'état du dépôt

```bash
# État complet
git status

# État court
git status -s
git status --short

# Voir les fichiers ignorés aussi
git status --ignored
```

### Ajouter des fichiers à la staging area

```bash
# Ajouter un fichier spécifique
git add fichier.txt

# Ajouter plusieurs fichiers
git add fichier1.txt fichier2.txt

# Ajouter tous les fichiers modifiés et nouveaux
git add .
git add --all
git add -A

# Ajouter tous les fichiers d'un dossier
git add dossier/

# Ajouter de manière interactive
git add -i
git add --interactive

# Ajouter seulement une partie d'un fichier
git add -p fichier.txt
git add --patch fichier.txt
```

### Créer des commits

```bash
# Commit avec message en ligne
git commit -m "Message du commit"

# Commit avec message dans l'éditeur
git commit

# Ajouter tous les fichiers modifiés et commiter
git commit -am "Message du commit"

# Modifier le dernier commit (avant push)
git commit --amend

# Modifier le dernier commit sans changer le message
git commit --amend --no-edit

# Commit vide (utile pour déclencher CI/CD)
git commit --allow-empty -m "Trigger CI"
```

---

## Voir l'historique

### Commandes de base

```bash
# Historique complet
git log

# Historique simplifié (une ligne par commit)
git log --oneline

# Historique avec graphe des branches
git log --graph

# Historique de toutes les branches
git log --all

# Combinaison recommandée
git log --oneline --graph --all --decorate
```

### Options de filtrage

```bash
# Limiter le nombre de commits affichés
git log -n 5
git log -5

# Voir les commits depuis une date
git log --since="2024-01-01"
git log --since="2 weeks ago"
git log --after="2024-01-01"

# Voir les commits jusqu'à une date
git log --until="2024-12-31"
git log --before="yesterday"

# Voir les commits d'un auteur
git log --author="Nom"

# Chercher dans les messages de commit
git log --grep="fix"

# Chercher dans le code
git log -S"fonction_recherchée"

# Voir l'historique d'un fichier
git log fichier.txt
git log --follow fichier.txt  # Suit les renommages
```

### Voir les détails des commits

```bash
# Voir un commit spécifique
git show abc1234

# Voir le dernier commit
git show HEAD

# Voir l'avant-dernier commit
git show HEAD~1
git show HEAD^

# Voir les changements d'un commit
git show abc1234 --stat
```

---

## Voir les différences

```bash
# Différences entre working directory et staging area
git diff

# Différences entre staging area et dernier commit
git diff --staged
git diff --cached

# Différences entre working directory et dernier commit
git diff HEAD

# Différences entre deux commits
git diff abc1234 def5678

# Différences entre deux branches
git diff branche1 branche2

# Différences pour un fichier spécifique
git diff fichier.txt

# Statistiques des différences
git diff --stat

# Différences mot par mot (au lieu de ligne par ligne)
git diff --word-diff
```

---

## Gestion des branches

### Créer et lister des branches

```bash
# Lister les branches locales
git branch

# Lister toutes les branches (locales et distantes)
git branch -a
git branch --all

# Lister les branches distantes uniquement
git branch -r

# Créer une nouvelle branche
git branch nom-branche

# Créer une branche depuis un commit spécifique
git branch nom-branche abc1234
```

### Changer de branche

```bash
# Changer de branche (ancienne syntaxe)
git checkout nom-branche

# Changer de branche (nouvelle syntaxe, recommandée)
git switch nom-branche

# Créer et changer de branche
git checkout -b nouvelle-branche
git switch -c nouvelle-branche

# Revenir à la branche précédente
git checkout -
git switch -
```

### Renommer et supprimer des branches

```bash
# Renommer la branche actuelle
git branch -m nouveau-nom

# Renommer une autre branche
git branch -m ancien-nom nouveau-nom

# Supprimer une branche (sécurisé, refuse si non mergée)
git branch -d nom-branche

# Forcer la suppression d'une branche
git branch -D nom-branche

# Supprimer une branche distante
git push origin --delete nom-branche
```

---

## Fusionner et rebaser

### Merge (fusion)

```bash
# Merger une branche dans la branche actuelle
git merge nom-branche

# Merge sans fast-forward (crée toujours un commit de merge)
git merge --no-ff nom-branche

# Merge avec message personnalisé
git merge nom-branche -m "Message de merge"

# Annuler un merge en cours
git merge --abort

# Voir les branches déjà mergées
git branch --merged

# Voir les branches non mergées
git branch --no-merged
```

### Rebase

```bash
# Rebaser la branche actuelle sur une autre
git rebase nom-branche

# Rebase interactif (pour nettoyer l'historique)
git rebase -i HEAD~5

# Continuer après résolution de conflit
git rebase --continue

# Passer un commit problématique
git rebase --skip

# Annuler un rebase en cours
git rebase --abort

# Rebaser et préserver les merges
git rebase -p nom-branche
```

---

## Annuler et corriger

### Annuler des modifications

```bash
# Annuler les modifications d'un fichier (non stagé)
git restore fichier.txt
git checkout -- fichier.txt  # ancienne syntaxe

# Annuler toutes les modifications non stagées
git restore .

# Retirer un fichier de la staging area
git restore --staged fichier.txt
git reset HEAD fichier.txt  # ancienne syntaxe

# Annuler les modifications stagées ET non stagées
git restore --staged --worktree fichier.txt
```

### Reset (réinitialiser)

```bash
# Annuler le dernier commit (garder les changements)
git reset --soft HEAD~1

# Annuler le dernier commit (déstagé les changements)
git reset --mixed HEAD~1
git reset HEAD~1  # mixed est le défaut

# Annuler le dernier commit (supprimer les changements)
git reset --hard HEAD~1

# Revenir à un commit spécifique
git reset --hard abc1234

# Annuler les 3 derniers commits
git reset --hard HEAD~3
```

### Revert (annuler un commit publié)

```bash
# Créer un nouveau commit qui annule un commit
git revert abc1234

# Annuler plusieurs commits
git revert abc1234 def5678

# Annuler un merge
git revert -m 1 abc1234

# Annuler sans créer de commit immédiatement
git revert --no-commit abc1234
```

---

## Stash (mise de côté)

```bash
# Mettre de côté les modifications
git stash
git stash push

# Stash avec un message
git stash save "Message descriptif"
git stash push -m "Message descriptif"

# Stash incluant les fichiers non suivis
git stash -u
git stash --include-untracked

# Stash incluant tout (même les fichiers ignorés)
git stash -a
git stash --all

# Lister les stashes
git stash list

# Voir le contenu d'un stash
git stash show
git stash show -p  # avec le diff

# Appliquer le dernier stash (le garde dans la liste)
git stash apply

# Appliquer un stash spécifique
git stash apply stash@{2}

# Appliquer et supprimer le dernier stash
git stash pop

# Supprimer le dernier stash
git stash drop

# Supprimer un stash spécifique
git stash drop stash@{2}

# Supprimer tous les stashes
git stash clear

# Créer une branche depuis un stash
git stash branch nom-branche
```

---

## Dépôts distants

### Gérer les remotes

```bash
# Lister les dépôts distants
git remote
git remote -v  # avec les URLs

# Ajouter un dépôt distant
git remote add origin https://github.com/user/repo.git

# Voir les informations d'un remote
git remote show origin

# Renommer un remote
git remote rename origin upstream

# Changer l'URL d'un remote
git remote set-url origin https://nouvelle-url.git

# Supprimer un remote
git remote remove origin
git remote rm origin
```

### Synchroniser avec les dépôts distants

```bash
# Récupérer les changements (sans appliquer)
git fetch
git fetch origin
git fetch --all  # Tous les remotes

# Récupérer et appliquer les changements
git pull
git pull origin main

# Pull avec rebase
git pull --rebase
git pull --rebase origin main

# Pousser vers le dépôt distant
git push
git push origin main

# Pousser et définir l'upstream
git push -u origin main
git push --set-upstream origin main

# Pousser toutes les branches
git push --all

# Pousser les tags
git push --tags

# Forcer le push (dangereux !)
git push --force

# Forcer le push de manière plus sûre
git push --force-with-lease

# Supprimer une branche distante
git push origin --delete nom-branche
```

---

## Tags (étiquettes)

```bash
# Lister les tags
git tag

# Lister les tags avec un pattern
git tag -l "v1.*"

# Créer un tag léger
git tag v1.0.0

# Créer un tag annoté (recommandé)
git tag -a v1.0.0 -m "Version 1.0.0"

# Créer un tag sur un commit ancien
git tag -a v1.0.0 abc1234 -m "Version 1.0.0"

# Voir les informations d'un tag
git show v1.0.0

# Pousser un tag vers le remote
git push origin v1.0.0

# Pousser tous les tags
git push --tags
git push origin --tags

# Supprimer un tag local
git tag -d v1.0.0

# Supprimer un tag distant
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0

# Checkout un tag
git checkout v1.0.0
```

---

## Cherry-pick

```bash
# Appliquer un commit spécifique sur la branche actuelle
git cherry-pick abc1234

# Cherry-pick plusieurs commits
git cherry-pick abc1234 def5678

# Cherry-pick une série de commits
git cherry-pick abc1234..def5678

# Cherry-pick sans créer de commit
git cherry-pick --no-commit abc1234

# Continuer après résolution de conflit
git cherry-pick --continue

# Annuler un cherry-pick en cours
git cherry-pick --abort
```

---

## Reflog (journal de référence)

```bash
# Voir tout l'historique des actions
git reflog

# Voir le reflog d'une branche spécifique
git reflog show nom-branche

# Voir les 10 dernières entrées
git reflog -10

# Revenir à un état précédent
git reset --hard HEAD@{2}

# Créer une branche depuis une entrée du reflog
git branch branche-recuperee HEAD@{5}
```

---

## Recherche et débogage

### Rechercher dans le code

```bash
# Chercher un texte dans les fichiers suivis
git grep "texte à chercher"

# Chercher avec numéros de ligne
git grep -n "texte"

# Chercher un mot entier uniquement
git grep -w "mot"

# Chercher dans un commit spécifique
git grep "texte" abc1234

# Compter les occurrences
git grep -c "texte"
```

### Blâme (qui a modifié quoi)

```bash
# Voir qui a modifié chaque ligne
git blame fichier.txt

# Blame avec plus d'informations
git blame -L 10,20 fichier.txt  # Lignes 10 à 20

# Blame en ignorant les espaces
git blame -w fichier.txt
```

### Bisect (trouver un bug)

```bash
# Démarrer une session bisect
git bisect start

# Marquer le commit actuel comme mauvais
git bisect bad

# Marquer un ancien commit comme bon
git bisect good abc1234

# Git va vous proposer des commits à tester
# Après chaque test, marquez :
git bisect good  # si le commit est bon
git bisect bad   # si le commit est mauvais

# Terminer la session bisect
git bisect reset

# Bisect automatisé avec un script
git bisect run ./test-script.sh
```

---

## Nettoyage

```bash
# Supprimer les fichiers non suivis
git clean -f

# Supprimer les fichiers ET dossiers non suivis
git clean -fd

# Voir ce qui serait supprimé (simulation)
git clean -n
git clean --dry-run

# Supprimer aussi les fichiers ignorés
git clean -x

# Nettoyer de manière interactive
git clean -i

# Optimiser le dépôt (garbage collection)
git gc

# Garbage collection aggressif
git gc --aggressive

# Vérifier l'intégrité du dépôt
git fsck
```

---

## Sous-modules

```bash
# Ajouter un sous-module
git submodule add https://github.com/user/repo.git chemin/

# Initialiser les sous-modules après clone
git submodule init

# Mettre à jour les sous-modules
git submodule update

# Cloner avec les sous-modules
git clone --recurse-submodules https://github.com/user/repo.git

# Mettre à jour tous les sous-modules
git submodule update --remote

# Voir l'état des sous-modules
git submodule status
```

---

## Alias (raccourcis)

### Créer des alias

```bash
# Créer un alias
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# Alias pour log formaté
git config --global alias.lg "log --oneline --graph --all --decorate"

# Alias pour voir les branches
git config --global alias.branches "branch -a"

# Alias pour unstage
git config --global alias.unstage "restore --staged"

# Alias pour dernier commit
git config --global alias.last "log -1 HEAD --stat"

# Utilisation
git co main       # au lieu de git checkout main
git lg            # au lieu de git log --oneline --graph --all
```

---

## Commandes avancées

### Archive

```bash
# Créer une archive du projet
git archive --format=zip HEAD > projet.zip

# Archive d'une branche spécifique
git archive --format=tar branche > projet.tar

# Archive d'un commit spécifique
git archive --format=zip abc1234 > version.zip
```

### Worktree (arbres de travail multiples)

```bash
# Créer un worktree (dossier de travail parallèle)
git worktree add ../chemin nom-branche

# Lister les worktrees
git worktree list

# Supprimer un worktree
git worktree remove ../chemin

# Nettoyer les worktrees obsolètes
git worktree prune
```

### Rerere (réutiliser les résolutions de conflits)

```bash
# Activer rerere
git config --global rerere.enabled true

# Git se souviendra de vos résolutions de conflits
# et les réappliquera automatiquement
```

---

## Options globales utiles

```bash
# Options applicables à de nombreuses commandes

# Mode verbeux (plus de détails)
git [commande] --verbose
git [commande] -v

# Mode silencieux
git [commande] --quiet
git [commande] -q

# Mode simulation (ne fait rien, montre ce qui serait fait)
git [commande] --dry-run
git [commande] -n

# Forcer l'opération
git [commande] --force
git [commande] -f
```

---

## Aide et documentation

```bash
# Aide générale
git help

# Aide sur une commande spécifique
git help commit
git commit --help

# Aide courte (options principales)
git commit -h

# Version de Git
git --version

# Manuel complet (ouvre dans le navigateur)
git help -w commit
```

---

## Raccourcis de navigation dans l'historique

```bash
# HEAD : Le commit actuel
HEAD

# Parent de HEAD (commit précédent)
HEAD~1
HEAD^
HEAD~

# Arrière-grand-parent
HEAD~2
HEAD^^

# 5 commits en arrière
HEAD~5

# Référence relative depuis une branche
main~3

# Premier parent d'un merge (dans un merge, il y a 2 parents)
HEAD^1

# Deuxième parent d'un merge
HEAD^2
```

---

## Patterns de .gitignore

```bash
# Fichier spécifique
fichier.txt

# Tous les fichiers avec extension
*.log
*.tmp

# Tous les fichiers dans un dossier
node_modules/
build/

# Tous les .txt sauf un
*.txt
!important.txt

# Fichiers dans tous les dossiers avec ce nom
**/logs/

# Fichiers à la racine uniquement
/fichier.txt

# Pattern complexe
debug[0-9].log

# Commentaire dans .gitignore
# Ceci est un commentaire
```

---

## Résolution de problèmes courants

### Problème : Changements non voulus dans un commit

```bash
# Annuler le dernier commit (garder les changements)
git reset --soft HEAD~1

# Ou modifier le commit
git commit --amend
```

### Problème : Mauvaise branche

```bash
# Créer une branche avec le travail actuel
git branch nouvelle-branche

# Revenir sur la bonne branche
git checkout branche-correcte

# Supprimer les commits de la mauvaise branche
git reset --hard origin/branche-correcte
```

### Problème : Conflit de merge

```bash
# Voir les fichiers en conflit
git status

# Éditer les fichiers pour résoudre
# puis
git add fichier-resolu.txt
git commit

# Ou annuler le merge
git merge --abort
```

### Problème : Commit perdu

```bash
# Trouver le commit dans le reflog
git reflog

# Récupérer le commit
git cherry-pick abc1234
# ou
git reset --hard abc1234
```

---

## Configuration recommandée pour débutants

Copiez-collez ces commandes pour une configuration optimale :

```bash
# Identité
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Éditeur (choisissez celui que vous préférez)
git config --global core.editor "code --wait"

# Branche par défaut
git config --global init.defaultBranch main

# Coloration
git config --global color.ui auto

# Pull avec rebase par défaut
git config --global pull.rebase true

# Alias utiles
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.unstage "restore --staged"

# Rerere (réutiliser les résolutions de conflits)
git config --global rerere.enabled true

# Afficher les conflits de manière plus claire
git config --global merge.conflictstyle diff3
```

---

## Commandes par ordre de fréquence d'utilisation

### Quotidien (tous les jours)
```bash
git status
git add .
git commit -m "message"
git push
git pull
git log --oneline
```

### Hebdomadaire (plusieurs fois par semaine)
```bash
git branch
git checkout -b nouvelle-branche
git merge branche
git stash
git stash pop
git diff
```

### Mensuel (de temps en temps)
```bash
git rebase
git cherry-pick
git reset
git revert
git tag
git fetch --all
```

### Occasionnel (situations spéciales)
```bash
git reflog
git bisect
git clean
git rebase -i
git blame
git remote add
```

---

## Commandes par niveau

### Niveau Débutant 🌱
```bash
git init
git clone
git add
git commit
git status
git log
git push
git pull
git branch
git checkout
```

### Niveau Intermédiaire 🌿
```bash
git merge
git diff
git stash
git reset
git restore
git remote
git fetch
git tag
git log --graph
```

### Niveau Avancé 🌳
```bash
git rebase
git rebase -i
git cherry-pick
git reflog
git bisect
git blame
git revert
git clean
git worktree
```

### Niveau Expert 🌟
```bash
git filter-branch
git gc
git fsck
git rerere
git submodule
git archive
git bundle
git replace
```

---

## Tableau récapitulatif des commandes essentielles

| Commande | Description | Exemple |
|----------|-------------|---------|
| `git init` | Initialiser un dépôt | `git init` |
| `git clone` | Cloner un dépôt | `git clone URL` |
| `git add` | Ajouter à la staging area | `git add .` |
| `git commit` | Créer un commit | `git commit -m "msg"` |
| `git status` | Voir l'état | `git status` |
| `git log` | Voir l'historique | `git log --oneline` |
| `git diff` | Voir les différences | `git diff` |
| `git branch` | Gérer les branches | `git branch nom` |
| `git checkout` | Changer de branche | `git checkout nom` |
| `git merge` | Fusionner | `git merge branche` |
| `git pull` | Récupérer et appliquer | `git pull origin main` |
| `git push` | Envoyer les commits | `git push origin main` |
| `git stash` | Mettre de côté | `git stash` |
| `git reset` | Annuler des commits | `git reset HEAD~1` |
| `git revert` | Annuler (sûr) | `git revert abc1234` |

---

## Conclusion

Cet aide-mémoire couvre les commandes Git les plus importantes. Vous n'avez pas besoin de toutes les mémoriser ! Voici comment l'utiliser efficacement :

1. **Débutant** : Concentrez-vous sur les commandes quotidiennes
2. **Pratique** : Référez-vous à cet aide-mémoire régulièrement
3. **Progression** : Ajoutez progressivement des commandes à votre arsenal
4. **Personnalisation** : Créez vos propres alias pour les commandes fréquentes

**💡 Conseil :** Gardez ce document facilement accessible (favoris du navigateur, fichier sur le bureau, ou même imprimé à côté de votre écran).

**🎯 Prochaines étapes :**
- Utilisez cet aide-mémoire dans vos projets quotidiens
- Testez les commandes que vous ne connaissez pas encore
- Créez vos propres alias pour vos commandes favorites
- Partagez cet aide-mémoire avec votre équipe !

Bonne utilisation de Git ! 🚀

⏭️ [Glossaire Git](/annexes/a2-glossaire.md)
