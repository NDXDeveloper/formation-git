🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Annexes
## Glossaire Git

Ce glossaire définit tous les termes techniques utilisés dans Git. Les définitions sont volontairement simples et accessibles pour faciliter la compréhension, même si vous débutez. Les termes sont classés par ordre alphabétique.

---

## A

### Add (Ajouter)
Action d'ajouter des fichiers modifiés à la **staging area** (zone d'index) en préparation d'un commit. Commande : `git add fichier.txt`

### Alias
Raccourci personnalisé pour une commande Git. Par exemple, créer `git st` comme raccourci pour `git status`.

### Amend (Modifier)
Action de modifier le dernier commit pour y ajouter des changements oubliés ou corriger le message. Commande : `git commit --amend`

### Ancestor (Ancêtre)
Un commit parent ou grand-parent d'un autre commit. Dans l'historique, tous les commits précédents sont des ancêtres du commit actuel.

### Archive
Création d'une archive (ZIP, TAR) du code à un moment donné, sans l'historique Git. Commande : `git archive`

---

## B

### Bare Repository (Dépôt nu)
Un dépôt Git sans working directory, utilisé uniquement comme serveur central. Contient seulement l'historique Git, pas de fichiers de travail.

### Bisect
Outil pour trouver le commit qui a introduit un bug en testant automatiquement différents commits par dichotomie. Commande : `git bisect`

### Blame
Commande qui montre qui a modifié chaque ligne d'un fichier et dans quel commit. Utile pour comprendre l'origine d'un changement. Commande : `git blame fichier.txt`

### Branch (Branche)
Une ligne de développement indépendante. Permet de travailler sur des fonctionnalités sans affecter le code principal. Imaginez-la comme une version parallèle de votre projet.

**Exemple :** La branche `main` contient le code en production, tandis que la branche `feature/login` contient le développement du système de connexion.

### Branch Protection (Protection de branche)
Règles configurées sur GitHub/GitLab pour empêcher les modifications directes sur certaines branches (comme `main`), forçant l'utilisation de Pull Requests.

---

## C

### Cache
Voir **Staging Area**. Terme parfois utilisé pour désigner la zone d'index.

### Checkout
Commande pour changer de branche ou récupérer des fichiers d'un commit spécifique. Commande : `git checkout nom-branche` (ancienne syntaxe, remplacée par `git switch`)

### Cherry-pick
Action de copier un commit spécifique d'une branche vers une autre, sans merger toute la branche. Commande : `git cherry-pick abc1234`

**Exemple :** Vous voulez uniquement le commit de correction de bug d'une branche experimentale, pas toutes les autres modifications.

### Clone
Action de copier un dépôt distant complet vers votre ordinateur, incluant tout l'historique. Commande : `git clone URL`

### Commit
Un "instantané" de votre projet à un moment donné. Enregistre les modifications avec un message descriptif, un auteur, une date et un identifiant unique (hash).

**Analogie :** Comme une sauvegarde d'un jeu vidéo, vous pouvez y revenir plus tard.

### Commit Hash (ou SHA)
Identifiant unique d'un commit, une longue chaîne de caractères (ex: `a3f5d8c`). Permet de référencer précisément un commit.

### Commit Message
Le texte descriptif qui accompagne un commit, expliquant ce qui a été modifié et pourquoi.

**Exemple :** `"Fix: Correction du bug de login avec email invalide"`

### Conflict (Conflit)
Situation où Git ne peut pas fusionner automatiquement des modifications car deux versions différentes existent pour les mêmes lignes de code. Nécessite une intervention manuelle.

### Conflicting Files (Fichiers en conflit)
Fichiers contenant des modifications incompatibles lors d'un merge ou rebase, marqués par Git avec `<<<<<<<`, `=======`, et `>>>>>>>`.

---

## D

### Detached HEAD
État où HEAD pointe vers un commit spécifique au lieu d'une branche. Peut arriver après un `git checkout [commit-hash]`. Les commits faits dans cet état peuvent être perdus si non sauvegardés dans une branche.

### Diff (Différence)
La comparaison entre deux versions du code, montrant les lignes ajoutées et supprimées. Commande : `git diff`

### Dirty (Sale)
Terme désignant un working directory avec des modifications non commitées. Opposé de "clean" (propre).

---

## F

### Fast-forward
Type de merge où la branche de destination avance simplement son pointeur sans créer de commit de merge, car il n'y a pas de divergence.

**Exemple :** Si `main` n'a pas bougé depuis que vous avez créé votre branche, Git peut simplement "avancer" main.

### Fetch
Action de télécharger les changements d'un dépôt distant sans les appliquer localement. Commande : `git fetch`

**Différence avec pull :** `fetch` télécharge, `pull` télécharge ET applique.

### Fork
Copie complète d'un dépôt sur votre compte GitHub/GitLab. Permet de travailler librement sans affecter le projet original. Essentiel pour contribuer à l'open source.

---

## G

### Git
Le système de contrôle de version créé par Linus Torvalds en 2005. Permet de suivre l'historique des modifications de fichiers et de collaborer.

### Git Flow
Une méthodologie populaire d'organisation des branches pour le travail en équipe, avec des branches `main`, `develop`, `feature/*`, `release/*`, et `hotfix/*`.

### GitHub / GitLab / Bitbucket
Plateformes web qui hébergent des dépôts Git distants et ajoutent des fonctionnalités comme les Pull Requests, issues, wiki, etc.

### .gitignore
Fichier spécial listant les fichiers et dossiers que Git doit ignorer (ne pas suivre). Typiquement : `node_modules/`, `.env`, `*.log`

### Gitk
Interface graphique intégrée à Git pour visualiser l'historique. Commande : `gitk`

---

## H

### Hash
Voir **Commit Hash**. Identifiant unique généré pour chaque commit.

### HEAD
Pointeur spécial indiquant le commit actuel sur lequel vous travaillez. Généralement, HEAD pointe vers une branche qui pointe vers un commit.

**Exemple :** `HEAD -> main -> abc1234`

### Hook
Script automatique qui s'exécute à certains moments (avant commit, après push, etc.). Permet d'automatiser des tâches. Situés dans `.git/hooks/`

### Hotfix
Correction urgente d'un bug en production. Dans Git Flow, une branche `hotfix/*` part de `main` pour corriger rapidement un problème critique.

---

## I

### Index
Voir **Staging Area**. Autre nom pour la zone d'index où les fichiers sont préparés avant le commit.

### Init (Initialiser)
Action de créer un nouveau dépôt Git dans un dossier. Commande : `git init`

---

## L

### LFS (Large File Storage)
Extension de Git pour gérer efficacement les gros fichiers (images, vidéos, etc.) qui ne sont pas bien adaptés au fonctionnement standard de Git.

### Local Repository (Dépôt local)
Votre copie personnelle d'un dépôt Git sur votre ordinateur, avec tout l'historique.

### Log
Historique des commits. Commande : `git log`

---

## M

### Main (ou Master)
Nom de la branche principale par défaut. Contient généralement le code stable en production. Anciennement appelée `master`, maintenant souvent renommée `main`.

### Merge (Fusionner)
Action de combiner les modifications de deux branches. Crée un commit de merge qui a deux parents.

**Exemple :** Fusionner `feature/login` dans `main` pour intégrer la nouvelle fonctionnalité.

### Merge Conflict
Voir **Conflict**. Situation nécessitant une résolution manuelle lors d'un merge.

### Merge Commit
Commit spécial créé lors d'un merge, ayant deux commits parents (un de chaque branche fusionnée).

---

## O

### Origin
Nom par défaut donné au dépôt distant principal lorsque vous clonez un projet. Commande : `git remote -v` pour voir l'URL d'origin.

---

## P

### Patch
Fichier contenant les différences (diff) entre deux versions, pouvant être appliqué sur un autre dépôt. Commande : `git format-patch`

### Pull
Action de récupérer les changements d'un dépôt distant ET de les appliquer localement. Équivaut à `git fetch` + `git merge`. Commande : `git pull`

### Pull Request (PR) / Merge Request (MR)
Demande de fusion de vos modifications dans un projet. Permet la revue de code avant intégration. Utilisé sur GitHub (Pull Request) et GitLab (Merge Request).

### Push
Action d'envoyer vos commits locaux vers un dépôt distant. Commande : `git push`

---

## R

### Rebase
Action de déplacer ou réappliquer des commits sur une nouvelle base. Réécrit l'historique pour créer une ligne de développement plus linéaire.

**Différence avec merge :** Le rebase réécrit l'historique, le merge le préserve.

### Reflog (Reference Log)
Journal de toutes les actions effectuées dans le dépôt, même celles qui ne sont plus visibles dans l'historique. Commande : `git reflog`

**Utilité :** Permet de récupérer des commits "perdus" après un reset ou autre erreur.

### Remote (Distant)
Un dépôt Git hébergé sur un serveur (GitHub, GitLab, etc.). Sert de point central pour la collaboration.

### Remote Tracking Branch
Copie locale de l'état d'une branche distante, comme `origin/main`. Met à jour avec `git fetch`.

### Repository (Dépôt)
Un projet suivi par Git, contenant tous les fichiers et l'historique complet des modifications. Peut être local (sur votre ordinateur) ou distant (sur un serveur).

### Reset
Commande pour annuler des commits en déplaçant le pointeur HEAD et/ou la branche actuelle. Commande : `git reset`

**Options :**
- `--soft` : Garde les modifications stagées
- `--mixed` : Garde les modifications non stagées
- `--hard` : Supprime tout (dangereux !)

### Resolve (Résoudre)
Action de corriger manuellement un conflit en choisissant quelle version du code garder.

### Restore
Commande moderne pour annuler des modifications ou retirer des fichiers de la staging area. Commande : `git restore fichier.txt`

### Revert
Commande pour annuler un commit en créant un nouveau commit inverse. Contrairement à `reset`, ne réécrit pas l'historique. Commande : `git revert abc1234`

---

## S

### SHA (Secure Hash Algorithm)
Voir **Commit Hash**. Algorithme utilisé pour générer les identifiants uniques des commits.

### Squash
Action de combiner plusieurs commits en un seul, généralement lors d'un rebase interactif. Utile pour nettoyer l'historique avant de partager.

**Exemple :** Transformer 5 petits commits "fix", "wip", "typo" en un seul commit propre "Add: Login feature".

### Staging Area (Zone d'index)
Zone intermédiaire entre votre working directory et le repository. Les fichiers ajoutés avec `git add` y sont placés avant d'être committés.

**Analogie :** Comme une zone de préparation où vous organisez ce qui sera dans la prochaine photo (commit).

### Stash
Action de mettre temporairement de côté des modifications non commitées pour travailler sur autre chose. Commande : `git stash`

**Utilité :** Vous travaillez sur une fonctionnalité mais devez rapidement corriger un bug sur une autre branche.

### Submodule
Un dépôt Git inclus dans un autre dépôt Git. Permet d'utiliser un projet externe comme dépendance tout en gardant son propre historique.

### Switch
Commande moderne pour changer de branche, plus claire que `checkout`. Commande : `git switch nom-branche`

---

## T

### Tag
Marqueur permanent pointant vers un commit spécifique, généralement utilisé pour marquer des versions (v1.0.0, v2.0.0). Commande : `git tag v1.0.0`

**Types :**
- **Léger** : Simple pointeur vers un commit
- **Annoté** : Contient un message, auteur, date (recommandé)

### Three-way Merge
Type de merge qui utilise trois points : le commit de la branche actuelle, le commit de la branche à merger, et leur ancêtre commun.

### Tracked Files (Fichiers suivis)
Fichiers que Git surveille activement. Opposé de "untracked" (non suivis).

### Tracking Branch
Branche locale configurée pour suivre une branche distante. Permet d'utiliser `git push` et `git pull` sans spécifier la branche.

### Tree
Dans Git, structure de données représentant un dossier et son contenu à un moment donné.

### Trunk
Terme parfois utilisé pour désigner la branche principale (équivalent de `main`), surtout dans "Trunk-Based Development".

---

## U

### Uncommitted Changes (Modifications non commitées)
Modifications présentes dans le working directory ou la staging area mais pas encore enregistrées dans un commit.

### Untracked Files (Fichiers non suivis)
Fichiers présents dans votre dossier de travail mais que Git ne surveille pas encore. Apparaissent en rouge dans `git status`.

### Upstream
1. La branche distante que votre branche locale suit
2. Le dépôt original d'un projet que vous avez forké

**Exemple :** Après un fork, le projet original est appelé "upstream" tandis que votre fork est "origin".

---

## W

### Working Directory (Répertoire de travail)
Le dossier sur votre ordinateur contenant les fichiers actuels du projet. C'est là que vous éditez les fichiers.

**Les trois zones de Git :**
1. Working Directory (où vous travaillez)
2. Staging Area (préparation)
3. Repository (historique)

### Working Tree
Voir **Working Directory**. Autre nom pour le répertoire de travail.

### Worktree
Fonctionnalité permettant d'avoir plusieurs working directories pour le même dépôt, chacun sur une branche différente. Commande : `git worktree`

---

## Concepts fondamentaux illustrés

### Les trois états d'un fichier

```
┌──────────────────┐
│ Working Directory│  (Modifications en cours)
│    git add →     │
└──────────────────┘
         ↓
┌──────────────────┐
│  Staging Area    │  (Préparation du commit)
│   git commit →   │
└──────────────────┘
         ↓
┌──────────────────┐
│   Repository     │  (Historique permanent)
└──────────────────┘
```

### Relation entre HEAD, branche et commit

```
HEAD → main → abc1234 (commit)
```

HEAD pointe vers une branche, qui pointe vers un commit.

### Fork vs Clone

```
Dépôt Original (upstream)
         ↓ Fork
Votre Fork (origin)
         ↓ Clone
Votre ordinateur (local)
```

---

## Termes souvent confondus

### Fetch vs Pull
- **Fetch** : Télécharge les changements sans les appliquer
- **Pull** : Télécharge ET applique les changements (fetch + merge)

### Reset vs Revert
- **Reset** : Réécrit l'historique (dangereux si partagé)
- **Revert** : Crée un nouveau commit annulant un ancien (sûr)

### Merge vs Rebase
- **Merge** : Combine les branches, préserve l'historique
- **Rebase** : Réapplique les commits, historique linéaire

### Checkout vs Switch
- **Checkout** : Ancienne commande polyvalente (changer de branche, restaurer fichiers)
- **Switch** : Nouvelle commande spécialisée pour changer de branche

### Restore vs Reset
- **Restore** : Annuler des modifications de fichiers
- **Reset** : Déplacer HEAD et annuler des commits

### Clone vs Fork
- **Clone** : Copie locale d'un dépôt
- **Fork** : Copie distante sur votre compte GitHub/GitLab

### Origin vs Upstream
- **Origin** : Votre dépôt distant principal (souvent votre fork)
- **Upstream** : Le dépôt original d'un projet forké

---

## Acronymes et abréviations

| Acronyme | Signification | Explication |
|----------|---------------|-------------|
| **CI/CD** | Continuous Integration/Continuous Deployment | Intégration et déploiement continus |
| **PR** | Pull Request | Demande de fusion sur GitHub |
| **MR** | Merge Request | Demande de fusion sur GitLab |
| **SHA** | Secure Hash Algorithm | Algorithme de hash pour les commits |
| **VCS** | Version Control System | Système de contrôle de version |
| **SCM** | Source Control Management | Gestion du contrôle de source |
| **WIP** | Work In Progress | Travail en cours |
| **LFS** | Large File Storage | Stockage de gros fichiers |
| **HEAD** | - | Pointeur vers le commit actuel |
| **ORIG_HEAD** | - | Position de HEAD avant une opération dangereuse |

---

## Symboles et caractères spéciaux

| Symbole | Signification | Exemple |
|---------|---------------|---------|
| `~` | Parent (n générations en arrière) | `HEAD~3` = 3 commits en arrière |
| `^` | Premier parent | `HEAD^` = commit parent |
| `..` | Range de commits | `main..feature` = commits dans feature mais pas main |
| `...` | Différence symétrique | `main...feature` = commits différents entre les deux |
| `@` | Raccourci pour HEAD | `@~1` = `HEAD~1` |
| `:` | Séparateur de chemin | `HEAD:fichier.txt` = fichier dans HEAD |
| `--` | Sépare options et fichiers | `git checkout -- fichier.txt` |

---

## États d'un fichier dans Git

```
Non suivi (Untracked)
      ↓ git add
Suivi et Stagé (Staged)
      ↓ git commit
Commité (Committed)
      ↓ modification
Modifié (Modified)
      ↓ git add
Stagé (Staged)
      ↓ git commit
Commité (Committed)
```

---

## Niveaux de configuration

Git a trois niveaux de configuration, par ordre de priorité :

1. **Local** (`.git/config`) : Spécifique au dépôt actuel
2. **Global** (`~/.gitconfig`) : Pour l'utilisateur sur la machine
3. **System** (`/etc/gitconfig`) : Pour tous les utilisateurs de la machine

Commandes :
```bash
git config --local   # Configuration du dépôt  
git config --global  # Configuration utilisateur  
git config --system  # Configuration système
```

---

## Vocabulaire du workflow collaboratif

### Code Review (Revue de code)
Processus où d'autres développeurs examinent et commentent votre code avant qu'il soit mergé.

### Continuous Integration (Intégration continue)
Pratique de merger fréquemment le code dans la branche principale, avec des tests automatiques.

### Feature Branch (Branche de fonctionnalité)
Branche créée pour développer une nouvelle fonctionnalité spécifique, généralement nommée `feature/nom-fonctionnalite`.

### Release (Version)
Publication d'une nouvelle version du logiciel, généralement taguée (ex: v2.1.0).

### Semantic Versioning (Versionnement sémantique)
Convention de numérotation : MAJOR.MINOR.PATCH (ex: 2.1.3)
- MAJOR : Changements incompatibles
- MINOR : Nouvelles fonctionnalités
- PATCH : Corrections de bugs

### Sprint
Période de développement fixe (souvent 2 semaines) dans les méthodes agiles.

---

## Expressions courantes

### "Commit early, commit often"
**Traduction :** Commitez tôt, commitez souvent  
**Signification :** Mieux vaut faire de nombreux petits commits qu'un énorme commit. Facilite le suivi et le debugging.  

### "Don't push to main"
**Traduction :** Ne poussez pas sur main  
**Signification :** Travaillez sur des branches séparées et utilisez des Pull Requests plutôt que de pousser directement sur la branche principale.  

### "Merge hell"
**Traduction :** L'enfer du merge  
**Signification :** Situation où de nombreux conflits apparaissent lors d'un merge, souvent due à un manque de synchronisation entre branches.  

### "Detached HEAD state"
**Signification :** État où vous travaillez directement sur un commit au lieu d'une branche. Les modifications peuvent être perdues.

### "Fast-forward merge"
**Signification :** Merge simple où la branche de destination avance simplement sans créer de commit de merge.

### "Rebase onto"
**Signification :** Déplacer une série de commits sur une nouvelle base, réécrivant l'historique.

### "Cherry-picking commits"
**Traduction :** Picorage de commits  
**Signification :** Sélectionner et appliquer uniquement certains commits spécifiques d'une branche.  

---

## Bonnes pratiques de vocabulaire

### Messages de commit

**Préfixes courants :**
- `Add:` Ajout de fonctionnalité
- `Fix:` Correction de bug
- `Update:` Mise à jour
- `Remove:` Suppression
- `Refactor:` Refactorisation
- `Docs:` Documentation
- `Style:` Formatage
- `Test:` Tests
- `Chore:` Maintenance

**Exemple :** `Fix: Correction du crash lors du login avec email vide`

---

## Ressources pour approfondir

### Documentation officielle
- **Git Glossary** : https://git-scm.com/docs/gitglossary
- **Git Reference** : https://git-scm.com/docs

### Apprendre le vocabulaire
- Utilisez ce glossaire régulièrement
- Lisez le code et les messages de commit d'autres projets
- Participez à des discussions sur Git
- Pratiquez en utilisant les termes corrects

---

## Comment utiliser ce glossaire

1. **Première lecture** : Parcourez tout le glossaire pour avoir une vue d'ensemble
2. **Référence rapide** : Consultez-le quand vous rencontrez un terme inconnu
3. **Révision** : Relisez régulièrement pour ancrer les concepts
4. **Pratique** : Utilisez les termes corrects dans vos conversations et documentation

**💡 Conseil :** Gardez ce glossaire ouvert dans un onglet de votre navigateur pendant que vous apprenez Git. La compréhension du vocabulaire est essentielle pour maîtriser Git !

---

## Conclusion

Le vocabulaire Git peut sembler intimidant au début, mais chaque terme a une raison d'être et devient naturel avec la pratique. N'hésitez pas à :

- ✅ Revenir à ce glossaire quand nécessaire
- ✅ Utiliser les termes corrects dans votre équipe
- ✅ Poser des questions si quelque chose n'est pas clair
- ✅ Contribuer à enrichir ce glossaire avec vos propres notes

**Rappelez-vous :** Même les développeurs expérimentés consultent la documentation. L'important n'est pas de tout mémoriser, mais de savoir où trouver l'information quand vous en avez besoin.

Bonne découverte du monde Git ! 🚀

⏭️ [Ressources complémentaires](/annexes/a3-ressources.md)
