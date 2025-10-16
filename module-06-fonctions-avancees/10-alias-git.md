🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 10. Alias Git pour la productivité

### Introduction

Imaginez que vous devez taper "git status" cinquante fois par jour, ou "git log --oneline --graph --decorate --all" à chaque fois que vous voulez voir l'historique. Fatigant, non ? C'est là qu'interviennent les **alias Git** !

Un **alias** est un raccourci personnalisé pour une commande Git. C'est comme créer vos propres "touches rapides" pour les commandes que vous utilisez souvent. Au lieu de taper une longue commande, vous tapez juste 2-3 lettres.

**Exemples :**
- `git st` au lieu de `git status`
- `git co` au lieu de `git checkout`
- `git lg` au lieu de `git log --oneline --graph --all`

Les alias peuvent vous faire gagner des **heures** par mois et rendre votre travail avec Git beaucoup plus agréable ! ⚡

---

### Qu'est-ce qu'un alias Git ?

Un alias Git est :
- Un **raccourci personnalisé** pour une commande Git
- Stocké dans votre configuration Git (fichier `.gitconfig`)
- **Global** (tous vos dépôts) ou **local** (un seul dépôt)
- Peut être simple ou complexe (avec des paramètres, des fonctions)

**Analogie :** C'est comme créer un contact dans votre téléphone. Au lieu de taper le numéro complet à chaque fois, vous tapez juste le nom.

---

### Créer des alias

Il existe deux façons de créer des alias : via la ligne de commande ou en éditant le fichier de configuration.

#### Méthode 1 : Ligne de commande (recommandé pour débuter)

```bash
git config --global alias.<nom-alias> '<commande>'
```

**Exemples simples :**

```bash
# Créer un alias 'st' pour 'status'
git config --global alias.st status

# Créer un alias 'co' pour 'checkout'
git config --global alias.co checkout

# Créer un alias 'br' pour 'branch'
git config --global alias.br branch

# Créer un alias 'ci' pour 'commit'
git config --global alias.ci commit
```

**Utilisation :**

```bash
git st        # équivalent à git status
git co main   # équivalent à git checkout main
git br -a     # équivalent à git branch -a
git ci -m "Message"  # équivalent à git commit -m "Message"
```

#### Méthode 2 : Éditer .gitconfig directement

```bash
# Ouvrir le fichier de configuration
git config --global --edit
```

**Ajouter dans le fichier :**

```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
```

**Avantages :** Plus rapide pour ajouter plusieurs alias d'un coup, plus lisible.

---

### Types d'alias

#### Alias simples

Les alias les plus basiques remplacent simplement une commande par une autre :

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
```

#### Alias avec options

Vous pouvez inclure des options dans vos alias :

```bash
# Voir les branches avec plus d'infos
git config --global alias.bv 'branch -v'

# Commit avec ajout automatique
git config --global alias.ciam 'commit -am'

# Status court
git config --global alias.s 'status -s'

# Log avec graphe
git config --global alias.lg 'log --oneline --graph --decorate --all'
```

**Utilisation :**

```bash
git s              # Status court
git ciam "Message" # Add all + commit
git lg             # Log graphique magnifique
```

#### Alias avec des fonctions shell

Pour des alias plus complexes, utilisez une fonction shell avec `!` :

```bash
git config --global alias.undo '!git reset --soft HEAD~1'
```

Le `!` indique à Git d'exécuter la commande comme un script shell plutôt qu'une commande Git.

**Exemples avancés :**

```bash
# Créer une branche et switcher dessus
git config --global alias.cob '!git checkout -b'

# Supprimer les branches mergées
git config --global alias.bclean '!git branch --merged | grep -v "\*" | xargs -n 1 git branch -d'

# Voir les contributeurs triés par nombre de commits
git config --global alias.contributors '!git shortlog -sn'
```

---

### Les alias essentiels (pour débutants)

Voici une collection d'alias que tout le monde devrait avoir :

#### Commandes de base plus courtes

```ini
[alias]
    # Commandes courtes
    st = status
    s = status -s
    co = checkout
    br = branch
    ci = commit

    # Ajout et commit
    a = add
    aa = add --all
    cm = commit -m
    cam = commit -am

    # Pousser et tirer
    ps = push
    pl = pull

    # Voir les différences
    d = diff
    dc = diff --cached
```

#### Historique et log améliorés

```ini
[alias]
    # Log simple et lisible
    l = log --oneline
    lg = log --oneline --graph --decorate --all

    # Log avec plus de détails
    ll = log --pretty=format:"%C(yellow)%h%Cred%d %Creset%s%Cblue [%an]" --decorate --numstat

    # Voir le dernier commit
    last = log -1 HEAD

    # Historique d'un fichier
    filelog = log -u
```

#### Alias pratiques du quotidien

```ini
[alias]
    # Annuler le dernier commit (garde les modifications)
    undo = reset --soft HEAD~1

    # Annuler les modifications non commitées
    unstage = reset HEAD --
    discard = checkout --

    # Voir les branches
    branches = branch -a

    # Voir les tags
    tags = tag -l

    # Voir les remotes
    remotes = remote -v
```

---

### Alias avancés et puissants

#### Alias avec paramètres

```bash
# Créer et checkout une nouvelle branche
git config --global alias.cob '!f() { git checkout -b "$1"; }; f'

# Utilisation :
git cob feature/new-feature
```

**Explication :**
- `!f() { ... }; f` : Crée une fonction shell temporaire
- `"$1"` : Premier paramètre passé à l'alias
- La fonction est appelée immédiatement

**Autres exemples :**

```bash
# Commit avec préfixe automatique
git config --global alias.feat '!f() { git commit -m "feat: $1"; }; f'
git config --global alias.fix '!f() { git commit -m "fix: $1"; }; f'

# Utilisation :
git feat "Add login feature"  # commit: "feat: Add login feature"
git fix "Correct typo"        # commit: "fix: Correct typo"
```

#### Alias pour nettoyer le dépôt

```ini
[alias]
    # Supprimer les branches mergées (sauf main/develop)
    bclean = "!f() { git branch --merged ${1-main} | grep -v " ${1-main}$" | xargs git branch -d; }; f"

    # Nettoyer les branches distantes obsolètes
    prune = fetch --prune

    # Supprimer tous les fichiers non trackés
    cleanup = clean -fd
```

#### Alias pour améliorer l'affichage

```ini
[alias]
    # Arbre magnifique de l'historique
    tree = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative

    # Graphe ASCII de l'historique
    graph = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all

    # Voir qui a modifié quoi
    who = shortlog -sne

    # Statistiques du dépôt
    stats = shortlog -sn --all --no-merges
```

#### Alias pour la recherche

```ini
[alias]
    # Trouver un commit par message
    find = "!git log --pretty=format:'%C(yellow)%h %Cblue%ad %Creset%s%Cgreen [%an]' --date=short --grep"

    # Trouver dans les fichiers
    grep = grep -Ii

    # Chercher dans l'historique
    search = "!f() { git log -S\"$1\" --oneline; }; f"
```

#### Alias pour travailler avec les branches

```ini
[alias]
    # Voir toutes les branches avec leur dernier commit
    bv = branch -v
    bvv = branch -vv

    # Supprimer une branche locale et distante
    bdel = "!f() { git branch -d $1; git push origin --delete $1; }; f"

    # Renommer la branche actuelle
    rename = branch -m

    # Voir les branches récentes
    recent = "!git for-each-ref --count=10 --sort=-committerdate refs/heads/ --format='%(refname:short)'"
```

---

### Configuration complète recommandée

Voici une configuration d'alias complète et moderne :

```ini
[alias]
    # === Commandes de base ===
    st = status
    s = status -s
    co = checkout
    cob = checkout -b
    br = branch
    ci = commit
    cm = commit -m
    cam = commit -am

    # === Ajout ===
    a = add
    aa = add --all
    ap = add --patch

    # === Log et historique ===
    l = log --oneline
    lg = log --oneline --graph --decorate --all
    last = log -1 HEAD

    # Historique magnifique avec couleurs
    tree = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

    # === Diff ===
    d = diff
    dc = diff --cached
    ds = diff --stat

    # === Reset et undo ===
    undo = reset --soft HEAD~1
    unstage = reset HEAD --
    discard = checkout --

    # === Branches ===
    branches = branch -a
    bv = branch -v
    recent = "!git for-each-ref --count=10 --sort=-committerdate refs/heads/ --format='%(refname:short)'"

    # === Remote ===
    remotes = remote -v

    # === Stash ===
    sl = stash list
    sp = stash pop
    ss = stash save

    # === Tags ===
    tags = tag -l

    # === Utilitaires ===
    who = shortlog -sne
    stats = shortlog -sn --all --no-merges
    contributors = shortlog -sn

    # === Aliases avancés ===
    # Conventional commits
    feat = "!f() { git commit -m \"feat: $1\"; }; f"
    fix = "!f() { git commit -m \"fix: $1\"; }; f"
    docs = "!f() { git commit -m \"docs: $1\"; }; f"

    # Créer et push une branche
    pushnew = "!git push -u origin $(git branch --show-current)"

    # Amend sans changer le message
    amend = commit --amend --no-edit

    # Voir les fichiers ignorés
    ignored = ls-files -o -i --exclude-standard
```

---

### Installer cette configuration

#### Option 1 : Copier-coller

```bash
git config --global --edit
```

Copiez-collez la configuration ci-dessus.

#### Option 2 : Script d'installation

Créez un fichier `install-git-aliases.sh` :

```bash
#!/bin/bash

echo "Installation des alias Git..."

# Commandes de base
git config --global alias.st status
git config --global alias.s "status -s"
git config --global alias.co checkout
git config --global alias.cob "checkout -b"
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.cm "commit -m"
git config --global alias.cam "commit -am"

# Log
git config --global alias.l "log --oneline"
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.last "log -1 HEAD"

# Diff
git config --global alias.d diff
git config --global alias.dc "diff --cached"

# Reset
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.unstage "reset HEAD --"

# Branches
git config --global alias.branches "branch -a"
git config --global alias.bv "branch -v"

echo "✅ Alias installés avec succès !"
echo "Testez avec : git st"
```

**Utilisation :**

```bash
chmod +x install-git-aliases.sh
./install-git-aliases.sh
```

---

### Alias spécifiques par workflow

#### Pour un workflow Git Flow

```ini
[alias]
    # Démarrer une feature
    feature = "!f() { git checkout develop && git pull && git checkout -b feature/$1; }; f"

    # Terminer une feature
    feature-done = "!f() { git checkout develop && git merge --no-ff feature/$1 && git branch -d feature/$1; }; f"

    # Hotfix
    hotfix = "!f() { git checkout main && git pull && git checkout -b hotfix/$1; }; f"
```

#### Pour un workflow GitHub Flow

```ini
[alias]
    # Synchroniser avec main et créer une branche
    new = "!f() { git checkout main && git pull && git checkout -b $1; }; f"

    # Mettre à jour la branche actuelle depuis main
    sync = "!git fetch origin && git rebase origin/main"

    # Push et créer la PR (avec GitHub CLI)
    pr = "!git push -u origin $(git branch --show-current) && gh pr create"
```

#### Pour le déploiement

```ini
[alias]
    # Tag et release
    release = "!f() { git tag -a $1 -m \"Release $1\" && git push origin $1; }; f"

    # Voir la version actuelle
    version = describe --tags --abbrev=0

    # Déployer en production
    deploy-prod = "!git checkout main && git pull && git push production main"
```

---

### Alias pour le débogage

```ini
[alias]
    # Voir les modifications récentes
    wtf = log --pretty=format:'%Cred%h %Creset- %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --since='1 week ago'

    # Trouver qui a cassé le code
    blame-line = "!f() { git blame -L $2,$2 $1; }; f"

    # Voir l'historique d'un fichier supprimé
    find-deleted = "!git log --diff-filter=D --summary | grep"

    # Chercher un bug (bisect simplifié)
    bisect-start-auto = "!f() { git bisect start && git bisect bad && git bisect good $1; }; f"
```

---

### Gérer vos alias

#### Voir tous vos alias

```bash
git config --global --get-regexp alias
```

**Ou créez un alias pour ça :**

```bash
git config --global alias.aliases "config --get-regexp alias"

# Utilisation
git aliases
```

#### Supprimer un alias

```bash
git config --global --unset alias.nom-alias
```

**Exemple :**

```bash
git config --global --unset alias.st
```

#### Modifier un alias

Recréez-le simplement :

```bash
git config --global alias.st "status -s"
# Remplace l'ancien alias 'st'
```

---

### Alias dans le shell (bash/zsh)

Vous pouvez aussi créer des alias dans votre shell pour rendre Git encore plus rapide :

#### Dans ~/.bashrc ou ~/.zshrc

```bash
# Alias Git ultra-courts
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git pull'
alias gco='git checkout'
alias gb='git branch'
alias gd='git diff'
alias gl='git log --oneline'

# Avec autocomplétion (bash)
complete -o default -o nospace -F _git g
```

**Utilisation :**

```bash
g st        # git status
gs          # git status
ga .        # git add .
gc -m "msg" # git commit -m "msg"
```

---

### Alias populaires de la communauté

Voici des alias populaires utilisés par de nombreux développeurs :

```ini
[alias]
    # Git obliterate - supprime complètement un fichier de l'historique
    obliterate = "!f() { git filter-branch --force --index-filter \"git rm --cached --ignore-unmatch $1\" --prune-empty --tag-name-filter cat -- --all; }; f"

    # Visualiser les contributions
    activity = "!git log --since='1 month ago' --author=\"$(git config user.name)\" --oneline"

    # Export vers archive
    export = "!f() { git archive --format=zip --output=\"${1}.zip\" HEAD; }; f"

    # Compter les lignes de code
    count = "!git ls-files | xargs wc -l"

    # Voir les fichiers les plus modifiés
    churn = "!git log --all -M -C --name-only --format='format:' \"$@\" | sort | grep -v '^$' | uniq -c | sort -n"

    # Alias fantaisistes
    yolo = "!git add -A && git commit -m \"$(curl -s whatthecommit.com/index.txt)\""
    oops = commit --amend --no-edit
```

---

### Bonnes pratiques

#### ✅ Commencez simple

Ne créez pas 50 alias d'un coup. Commencez par :

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
```

Ajoutez-en au fur et à mesure de vos besoins.

#### ✅ Utilisez des noms mnémoniques

```bash
# ✅ Bon : facile à retenir
git config --global alias.st status
git config --global alias.undo "reset --soft HEAD~1"

# ❌ Mauvais : cryptique
git config --global alias.x status
git config --global alias.zz "reset --soft HEAD~1"
```

#### ✅ Documentez vos alias complexes

```ini
[alias]
    # Supprimer toutes les branches mergées dans main
    # Usage: git bclean
    bclean = "!git branch --merged main | grep -v 'main' | xargs git branch -d"
```

#### ✅ Partagez avec votre équipe

Créez un fichier `git-aliases.txt` dans votre projet avec les alias recommandés.

#### ✅ Sauvegardez votre configuration

```bash
# Sauvegarder
cp ~/.gitconfig ~/gitconfig-backup

# Ou versionner
git init ~/dotfiles
cp ~/.gitconfig ~/dotfiles/
cd ~/dotfiles && git add . && git commit -m "Backup git config"
```

#### ❌ N'écrasez pas les commandes Git natives

```bash
# ❌ Mauvais : écrase 'git log'
git config --global alias.log "log --oneline"

# ✅ Bon : crée un nouvel alias
git config --global alias.l "log --oneline"
```

#### ❌ Ne rendez pas les alias trop complexes

Si un alias fait plus de 3 lignes, créez plutôt un script séparé.

---

### Tester et déboguer les alias

#### Voir ce qu'un alias exécute

```bash
git config --global --get alias.nom-alias
```

**Exemple :**

```bash
git config --global --get alias.lg
# log --oneline --graph --decorate --all
```

#### Tester un alias en mode debug

```bash
git -c alias.test='!echo "Test: $@"' test arg1 arg2
# Test: arg1 arg2
```

#### Exécuter un alias avec trace

```bash
GIT_TRACE=1 git votre-alias
```

---

### Configuration avancée : Conditional includes

Utilisez des alias différents selon le dépôt :

**~/.gitconfig :**

```ini
[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work

[includeIf "gitdir:~/personal/"]
    path = ~/.gitconfig-personal
```

**~/.gitconfig-work :**

```ini
[alias]
    ci = commit -s  # Avec sign-off pour le travail
```

**~/.gitconfig-personal :**

```ini
[alias]
    ci = commit    # Sans sign-off pour perso
```

---

### Exemples de workflows complets

#### Workflow du matin

```bash
# Alias pour démarrer la journée
git config --global alias.morning '!git checkout main && git pull && git fetch --prune'

# Utilisation
git morning
# ✅ Vous êtes à jour et prêt à travailler !
```

#### Workflow de fin de journée

```bash
# Alias pour terminer la journée
git config --global alias.eod '!git add -A && git commit -m "EOD: Work in progress" && git push'

# Utilisation
git eod
# ✅ Votre travail est sauvegardé !
```

#### Workflow de review

```bash
# Voir ce qui a changé aujourd'hui
git config --global alias.today 'log --since=midnight --author="$(git config user.name)" --oneline'

# Utilisation
git today
```

---

### Récapitulatif des alias essentiels

Voici les alias que vous devriez avoir au minimum :

```bash
# Les 10 alias indispensables
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.cm "commit -m"
git config --global alias.l "log --oneline"
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.d diff
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.aliases "config --get-regexp alias"
```

---

### Ressources et inspirations

#### Dépôts populaires d'alias Git

- **Oh My Zsh Git plugin** : Collection massive d'alias
- **Dotfiles sur GitHub** : Recherchez "dotfiles" pour voir les configs d'autres développeurs
- **Git wiki** : Liste d'alias communautaires

#### Outils complémentaires

- **Oh My Zsh** : Framework qui inclut des alias Git automatiques
- **Git extras** : Collection de scripts et d'alias Git utiles
- **Tig** : Interface texte pour Git (alternative aux alias pour certaines tâches)

---

### Tableau récapitulatif des catégories d'alias

| Catégorie | Exemples | Usage |
|-----------|----------|-------|
| **Commandes courtes** | st, co, br, ci | Économiser la frappe |
| **Log amélioré** | lg, tree, last | Mieux visualiser l'historique |
| **Workflow** | feat, fix, pr | Conventional commits, automatisation |
| **Nettoyage** | bclean, prune | Maintenance du dépôt |
| **Recherche** | find, search, who | Déboguer et investiguer |
| **Utilitaires** | undo, amend, oops | Corriger les erreurs |

---

### Conclusion

Les **alias Git** sont un moyen simple et puissant d'améliorer votre productivité. Ils vous permettent de :

**Points clés à retenir :**

- Les alias transforment des commandes longues en raccourcis courts
- Créez des alias avec `git config --global alias.nom 'commande'`
- Commencez avec des alias simples (st, co, br)
- Ajoutez progressivement des alias plus complexes selon vos besoins
- Documentez et partagez vos alias avec votre équipe
- Sauvegardez votre `.gitconfig`

**Pour commencer immédiatement :**

```bash
# Installer les 5 alias essentiels
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.l "log --oneline"
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.undo "reset --soft HEAD~1"

# Tester
git st
```

**Gain de temps estimé :**

Si vous faites 100 commandes Git par jour :
- Sans alias : ~10 minutes de frappe
- Avec alias : ~3 minutes de frappe
- **Gain : 7 minutes/jour = 30 heures/an !** ⏱️

Avec des alias bien configurés, Git devient plus rapide, plus agréable et plus productif. Prenez 15 minutes pour configurer vos alias aujourd'hui, vous gagnerez des heures dans le futur ! 🚀

---

**Fin du Module 6 : Fonctions avancées** ✅

Félicitations ! Vous maîtrisez maintenant les outils avancés de Git : stash, cherry-pick, rebase interactif, reflog, bisect, blame, tags, submodules, hooks et alias. Vous êtes prêt à utiliser Git comme un pro ! 🎉

⏭️ [Module 7 : Bonnes pratiques et workflows](/module-07-bonnes-pratiques-et-workflows/README.md)
