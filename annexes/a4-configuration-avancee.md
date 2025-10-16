🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Annexes
## Configuration Git avancée

Cette annexe vous guide dans la configuration avancée de Git pour optimiser votre productivité, personnaliser votre environnement, et automatiser vos workflows. Toutes les configurations présentées ici sont optionnelles mais recommandées pour une utilisation professionnelle de Git.

---

## Les niveaux de configuration

Git utilise trois niveaux de configuration, appliqués dans cet ordre de priorité (du plus spécifique au plus général) :

### 1. Configuration locale (--local)

**Fichier :** `.git/config` (dans le dépôt)
**Portée :** Uniquement pour le dépôt actuel
**Priorité :** La plus haute

```bash
# Configurer localement
git config --local user.name "Nom pour ce projet"

# Voir la configuration locale
git config --local --list
```

**Utilité :** Configuration spécifique à un projet (email professionnel vs personnel, hooks spéciaux, etc.)

### 2. Configuration globale (--global)

**Fichier :** `~/.gitconfig` ou `~/.config/git/config` (home directory)
**Portée :** Tous vos dépôts sur cette machine
**Priorité :** Moyenne

```bash
# Configurer globalement
git config --global user.email "votre.email@example.com"

# Voir la configuration globale
git config --global --list
```

**Utilité :** Vos préférences personnelles sur cette machine.

### 3. Configuration système (--system)

**Fichier :** `/etc/gitconfig` (Linux/Mac) ou `C:\Program Files\Git\etc\gitconfig` (Windows)
**Portée :** Tous les utilisateurs de la machine
**Priorité :** La plus basse

```bash
# Configurer au niveau système (nécessite admin/sudo)
sudo git config --system core.editor "nano"

# Voir la configuration système
git config --system --list
```

**Utilité :** Configuration pour toute une organisation ou machine partagée.

### Visualiser toute la configuration

```bash
# Voir toute la configuration (tous niveaux)
git config --list

# Voir d'où vient chaque paramètre
git config --list --show-origin

# Voir la configuration avec les scopes
git config --list --show-scope
```

---

## Configuration de l'identité

### Configuration de base

```bash
# Nom affiché dans les commits
git config --global user.name "Prénom Nom"

# Email affiché dans les commits
git config --global user.email "votre.email@example.com"
```

### Identités multiples (pro/perso)

**Situation :** Vous travaillez sur des projets professionnels et personnels.

**Solution 1 : Configuration locale par projet**

```bash
# Pour un projet professionnel
cd projet-travail
git config --local user.email "prenom.nom@entreprise.com"

# Pour un projet personnel
cd projet-perso
git config --local user.email "perso@gmail.com"
```

**Solution 2 : Configuration conditionnelle (recommandée)**

Créez `~/.gitconfig` avec :

```ini
[user]
    name = Prénom Nom
    email = perso@gmail.com

# Configuration pour les projets dans ~/travail/
[includeIf "gitdir:~/travail/"]
    path = ~/.gitconfig-travail

# Configuration pour GitHub
[includeIf "hasconfig:remote.*.url:git@github.com:entreprise/**"]
    path = ~/.gitconfig-travail
```

Créez `~/.gitconfig-travail` avec :

```ini
[user]
    email = prenom.nom@entreprise.com
```

### Signature GPG des commits

Pour signer cryptographiquement vos commits :

```bash
# Générer une clé GPG (si vous n'en avez pas)
gpg --full-generate-key

# Lister vos clés GPG
gpg --list-secret-keys --keyid-format=long

# Configurer Git pour utiliser votre clé
git config --global user.signingkey VOTRE_CLE_GPG

# Signer automatiquement tous les commits
git config --global commit.gpgsign true

# Signer automatiquement tous les tags
git config --global tag.gpgsign true
```

---

## Configuration de l'éditeur

### Éditeurs courants

```bash
# Visual Studio Code
git config --global core.editor "code --wait"

# Sublime Text
git config --global core.editor "subl -n -w"

# Atom
git config --global core.editor "atom --wait"

# Vim
git config --global core.editor "vim"

# Nano
git config --global core.editor "nano"

# Emacs
git config --global core.editor "emacs"

# Notepad++ (Windows)
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

### Configuration de l'éditeur de différences

```bash
# Utiliser VSCode comme difftool
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'

# Utiliser VSCode comme mergetool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

---

## Configuration des couleurs

### Activer la coloration

```bash
# Activer la coloration automatique
git config --global color.ui auto

# Toujours activer les couleurs
git config --global color.ui always

# Désactiver les couleurs
git config --global color.ui false
```

### Personnaliser les couleurs

```bash
# Couleurs pour git status
git config --global color.status.changed "yellow bold"
git config --global color.status.untracked "red bold"
git config --global color.status.added "green bold"

# Couleurs pour git diff
git config --global color.diff.meta "yellow bold"
git config --global color.diff.old "red bold"
git config --global color.diff.new "green bold"

# Couleurs pour git branch
git config --global color.branch.current "yellow reverse"
git config --global color.branch.local "yellow"
git config --global color.branch.remote "green"
```

**Couleurs disponibles :** normal, black, red, green, yellow, blue, magenta, cyan, white

**Attributs disponibles :** bold, dim, ul (underline), blink, reverse

---

## Configuration des branches

### Branche par défaut

```bash
# Définir 'main' comme nom de branche par défaut
git config --global init.defaultBranch main

# Ou 'develop' si vous préférez
git config --global init.defaultBranch develop
```

### Configuration du comportement de pull

```bash
# Pull avec merge (comportement par défaut)
git config --global pull.rebase false

# Pull avec rebase (recommandé pour un historique linéaire)
git config --global pull.rebase true

# Pull avec fast-forward uniquement (sûr)
git config --global pull.ff only
```

### Configuration du comportement de push

```bash
# Push seulement la branche actuelle
git config --global push.default simple

# Push toutes les branches correspondantes
git config --global push.default matching

# Push et créer la branche distante si elle n'existe pas
git config --global push.autoSetupRemote true

# Pousser les tags avec les commits
git config --global push.followTags true
```

---

## Alias Git puissants

Les alias permettent de créer des raccourcis pour les commandes fréquentes.

### Alias de base

```bash
# Raccourcis pour les commandes courantes
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# Utilisation : git co main (au lieu de git checkout main)
```

### Alias pour l'historique

```bash
# Historique visuel compact
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"

# Historique avec les fichiers modifiés
git config --global alias.ll "log --pretty=format:'%C(yellow)%h%Cred%d %Creset%s%Cblue [%cn]' --decorate --numstat"

# Historique simple
git config --global alias.ls "log --pretty=format:'%C(yellow)%h %C(blue)%ad%C(red)%d %C(reset)%s%C(green) [%cn]' --decorate --date=short"

# Graphe de toutes les branches
git config --global alias.graph "log --graph --oneline --all --decorate"
```

### Alias pour les modifications

```bash
# Voir les fichiers modifiés
git config --global alias.changed "diff --name-only"

# Voir les modifications stagées
git config --global alias.staged "diff --staged"

# Annuler les modifications
git config --global alias.unstage "restore --staged"
git config --global alias.discard "restore"

# Dernier commit
git config --global alias.last "log -1 HEAD --stat"

# Amend rapide (modifier le dernier commit)
git config --global alias.amend "commit --amend --no-edit"
```

### Alias pour les branches

```bash
# Lister toutes les branches
git config --global alias.branches "branch -a"

# Branches triées par date
git config --global alias.recent "branch --sort=-committerdate"

# Supprimer les branches mergées
git config --global alias.cleanup "!git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d"

# Créer et changer de branche
git config --global alias.cob "checkout -b"
```

### Alias pour les dépôts distants

```bash
# Pull avec rebase
git config --global alias.up "pull --rebase"

# Fetch et rebase
git config --global alias.sync "!git fetch && git rebase"

# Push avec force-with-lease (plus sûr)
git config --global alias.pushf "push --force-with-lease"
```

### Alias pour l'analyse

```bash
# Qui a contribué le plus
git config --global alias.contributors "shortlog -sn"

# Statistiques du dépôt
git config --global alias.stats "!git log --shortstat --oneline | grep -E 'fil(e|es) changed' | awk '{files+=$1; inserted+=$4; deleted+=$6} END {print \"Files changed:\", files, \"Insertions:\", inserted, \"Deletions:\", deleted}'"

# Trouver les commits volumineux
git config --global alias.big "!git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | awk '/^blob/ {print substr($0,6)}' | sort --numeric-sort --key=2 | tail -20"
```

### Alias complexes avec fonctions shell

```bash
# Créer une branche et pousser immédiatement
git config --global alias.new "!f() { git checkout -b $1 && git push -u origin $1; }; f"

# Rechercher dans les commits
git config --global alias.find "!f() { git log --all --grep=$1; }; f"

# Nettoyer complètement
git config --global alias.wipe "!git clean -fd && git reset --hard HEAD"
```

---

## Configuration des différences (diff)

### Algorithme de diff

```bash
# Utiliser l'algorithme histogram (plus lisible)
git config --global diff.algorithm histogram

# Autres options : myers (défaut), minimal, patience
```

### Coloration des déplacements

```bash
# Mettre en évidence les lignes déplacées
git config --global diff.colorMoved default

# Options : no, default, plain, blocks, zebra, dimmed-zebra
```

### Ignorer les changements d'espaces

```bash
# Ignorer les changements d'espaces dans les diffs
git config --global diff.ignoreSubmodules dirty

# Ignorer complètement les espaces
git config --global core.whitespace trailing-space,space-before-tab
```

### Configuration des outils de diff externes

```bash
# Utiliser Beyond Compare
git config --global diff.tool bc
git config --global difftool.bc.path "c:/Program Files/Beyond Compare 4/bcomp.exe"

# Utiliser Meld
git config --global diff.tool meld
git config --global difftool.meld.path "/usr/bin/meld"
```

---

## Configuration des merges

### Stratégie de merge

```bash
# Désactiver le fast-forward par défaut
git config --global merge.ff false

# Toujours créer un commit de merge
git config --global merge.commit yes

# Style de conflit (montrer l'ancêtre commun)
git config --global merge.conflictstyle diff3
```

### Merge tool

```bash
# Configurer VSCode comme merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Ne pas créer de fichiers .orig après merge
git config --global mergetool.keepBackup false
```

### Rerere (Reuse Recorded Resolution)

```bash
# Activer rerere (réutilise les résolutions de conflits)
git config --global rerere.enabled true

# Rerere avec auto-staging
git config --global rerere.autoupdate true
```

**Utilité :** Si vous résolvez le même conflit plusieurs fois (ex: merge puis rebase), Git réappliquera automatiquement votre résolution.

---

## Configuration de la performance

### Cache de credentials

```bash
# Cache les credentials pour 1 heure (Linux/Mac)
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'

# Utiliser le trousseau système (Mac)
git config --global credential.helper osxkeychain

# Utiliser le gestionnaire Windows
git config --global credential.helper wincred

# Utiliser le gestionnaire Git Credential Manager
git config --global credential.helper manager
```

### Optimisation du garbage collection

```bash
# Nettoyer automatiquement
git config --global gc.auto 256

# Agressif mais plus lent
git config --global gc.aggressive true
```

### Parallélisation

```bash
# Utiliser plusieurs threads pour les opérations
git config --global pack.threads 0  # 0 = auto-detect

# Paralléliser le fetch
git config --global fetch.parallel 0  # 0 = auto-detect
```

### Delta compression

```bash
# Niveau de compression (0-9, 9 = max)
git config --global core.compression 9

# Taille de la fenêtre delta
git config --global pack.windowMemory 256m
```

---

## Configuration de la sécurité

### Protection contre le force push

```bash
# Refuser les force push sur certaines branches
git config --global receive.denyNonFastForwards true
```

### Vérification des URLs

```bash
# Bloquer les URLs dangereuses
git config --global protocol.file.allow user
git config --global protocol.ext.allow user
```

### Configuration SSH

```bash
# Spécifier la clé SSH à utiliser
git config --global core.sshCommand "ssh -i ~/.ssh/id_rsa_github"
```

---

## Configuration des hooks

Les hooks sont des scripts qui s'exécutent à certains moments Git.

### Emplacement des hooks

**Dossier :** `.git/hooks/` (dans chaque dépôt)

### Template de hooks global

```bash
# Créer un dossier pour vos templates
mkdir -p ~/.git-templates/hooks

# Configurer Git pour utiliser ce template
git config --global init.templateDir ~/.git-templates
```

### Hooks courants

**1. pre-commit** : Avant chaque commit

```bash
#!/bin/sh
# Exemple : Vérifier le code avant commit

echo "Running linter..."
npm run lint || {
    echo "Linting failed. Commit aborted."
    exit 1
}

echo "Running tests..."
npm test || {
    echo "Tests failed. Commit aborted."
    exit 1
}
```

**2. commit-msg** : Valider le message de commit

```bash
#!/bin/sh
# Exemple : Forcer un format de message

commit_msg=$(cat "$1")

# Vérifier que le message commence par un type
if ! echo "$commit_msg" | grep -qE "^(feat|fix|docs|style|refactor|test|chore):"; then
    echo "Error: Commit message must start with a type (feat, fix, docs, etc.)"
    echo "Example: feat: Add user authentication"
    exit 1
fi
```

**3. pre-push** : Avant chaque push

```bash
#!/bin/sh
# Exemple : Empêcher le push de code non testé

echo "Running tests before push..."
npm test || {
    echo "Tests failed. Push aborted."
    exit 1
}
```

**4. post-checkout** : Après changement de branche

```bash
#!/bin/sh
# Exemple : Installer les dépendances après changement de branche

if [ -f "package.json" ]; then
    echo "Installing dependencies..."
    npm install
fi
```

### Rendre les hooks exécutables

```bash
chmod +x .git/hooks/pre-commit
```

### Outils pour gérer les hooks

**Husky (JavaScript/Node.js)**
```bash
npm install husky --save-dev
npx husky install
npx husky add .git/hooks/pre-commit "npm test"
```

**pre-commit (Python)**
```bash
pip install pre-commit
pre-commit install
```

---

## Configuration des submodules

```bash
# Mise à jour récursive des submodules
git config --global submodule.recurse true

# Fetch en parallèle
git config --global submodule.fetchJobs 8
```

---

## Configuration du pager

```bash
# Désactiver le pager pour certaines commandes
git config --global pager.branch false
git config --global pager.tag false

# Utiliser less avec des options
git config --global core.pager 'less -RFX'

# Utiliser delta (amélioration de diff)
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
```

---

## Configuration .gitattributes

Le fichier `.gitattributes` contrôle le comportement de Git pour certains fichiers.

### Créer .gitattributes

```bash
# À la racine du projet
touch .gitattributes
```

### Normalisation des fins de ligne

```
# Normaliser les fins de ligne en LF (Linux/Mac)
* text=auto eol=lf

# Windows conserve CRLF
*.bat text eol=crlf

# Binaires (ne pas toucher)
*.png binary
*.jpg binary
*.pdf binary
```

### Git LFS (Large File Storage)

```
# Gérer les gros fichiers avec LFS
*.psd filter=lfs diff=lfs merge=lfs -text
*.ai filter=lfs diff=lfs merge=lfs -text
*.mp4 filter=lfs diff=lfs merge=lfs -text
```

### Merge drivers personnalisés

```
# Ne jamais merger certains fichiers (toujours garder la version actuelle)
package-lock.json merge=ours
```

### Diff personnalisés

```
# Utiliser un diff spécial pour certains fichiers
*.md diff=markdown
*.ipynb diff=jupyternotebook
```

---

## Templates de messages de commit

### Créer un template

```bash
# Créer un fichier template
nano ~/.gitmessage.txt
```

**Contenu de `~/.gitmessage.txt` :**

```
# Type: Résumé (50 caractères max)
# |<----  Utiliser max 50 caractères  ---->|

# Expliquer POURQUOI ce changement est nécessaire (72 caractères par ligne)
# |<----   Essayer de limiter chaque ligne à 72 caractères    ---->|

# Fournir des liens vers les issues, tickets, etc.
# Issue: #

# Types valides:
# feat:     Nouvelle fonctionnalité
# fix:      Correction de bug
# docs:     Documentation uniquement
# style:    Formatage (pas de changement de code)
# refactor: Refactorisation
# test:     Ajout de tests
# chore:    Maintenance
```

**Configurer Git pour utiliser ce template :**

```bash
git config --global commit.template ~/.gitmessage.txt
```

---

## Configuration complète recommandée

Voici une configuration complète et optimisée à copier-coller :

```bash
# ===== IDENTITÉ =====
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# ===== ÉDITEUR =====
git config --global core.editor "code --wait"

# ===== BRANCHE PAR DÉFAUT =====
git config --global init.defaultBranch main

# ===== COMPORTEMENT PULL/PUSH =====
git config --global pull.rebase true
git config --global push.default simple
git config --global push.autoSetupRemote true
git config --global push.followTags true

# ===== COULEURS =====
git config --global color.ui auto

# ===== DIFF ET MERGE =====
git config --global diff.algorithm histogram
git config --global diff.colorMoved default
git config --global merge.conflictstyle diff3
git config --global rerere.enabled true
git config --global rerere.autoupdate true

# ===== ALIAS =====
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'restore --staged'
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.last 'log -1 HEAD --stat'
git config --global alias.amend 'commit --amend --no-edit'

# ===== SÉCURITÉ =====
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'

# ===== PERFORMANCE =====
git config --global core.preloadindex true
git config --global core.fscache true
git config --global gc.auto 256

# ===== MERGE TOOL =====
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
git config --global mergetool.keepBackup false

# ===== SUBMODULES =====
git config --global submodule.recurse true

# ===== COMMIT TEMPLATE =====
# git config --global commit.template ~/.gitmessage.txt
```

---

## Configuration par projet

### .git/config local

Chaque projet peut avoir sa propre configuration dans `.git/config`.

**Exemple pour un projet professionnel :**

```bash
cd projet-travail

# Email professionnel pour ce projet
git config --local user.email "prenom.nom@entreprise.com"

# Hooks spécifiques
git config --local core.hooksPath .githooks

# Remote supplémentaire
git config --local remote.upstream.url "https://github.com/entreprise/projet.git"
```

---

## Voir et modifier la configuration

### Commandes utiles

```bash
# Voir toute la configuration
git config --list

# Voir d'où vient chaque paramètre
git config --list --show-origin

# Voir une valeur spécifique
git config user.name

# Éditer directement le fichier
git config --global --edit

# Supprimer une configuration
git config --global --unset alias.co

# Supprimer une section entière
git config --global --remove-section alias
```

### Fichier de configuration manuel

Vous pouvez aussi éditer directement `~/.gitconfig` :

```ini
[user]
    name = Prénom Nom
    email = votre.email@example.com

[core]
    editor = code --wait
    autocrlf = input

[init]
    defaultBranch = main

[pull]
    rebase = true

[push]
    default = simple
    autoSetupRemote = true

[alias]
    co = checkout
    br = branch
    ci = commit
    st = status
    lg = log --graph --oneline --all --decorate

[color]
    ui = auto

[diff]
    algorithm = histogram
    colorMoved = default

[merge]
    conflictstyle = diff3

[rerere]
    enabled = true
    autoupdate = true
```

---

## Troubleshooting de la configuration

### Problèmes courants

**1. Mauvais nom/email dans les commits**

```bash
# Vérifier la configuration actuelle
git config user.name
git config user.email

# Corriger
git config --global user.name "Bon Nom"
git config --global user.email "bon.email@example.com"

# Modifier le dernier commit
git commit --amend --reset-author
```

**2. Éditeur qui ne s'ouvre pas**

```bash
# Vérifier l'éditeur configuré
git config core.editor

# Reconfigurer avec un éditeur simple
git config --global core.editor "nano"
```

**3. Problèmes de fins de ligne Windows/Linux**

```bash
# Windows
git config --global core.autocrlf true

# Linux/Mac
git config --global core.autocrlf input
```

**4. Credentials non sauvegardés**

```bash
# Activer le cache
git config --global credential.helper cache

# Ou utiliser le gestionnaire système
git config --global credential.helper manager  # Windows
git config --global credential.helper osxkeychain  # Mac
```

### Réinitialiser la configuration

```bash
# Supprimer toute la configuration globale
rm ~/.gitconfig

# Ou réinitialiser un paramètre spécifique
git config --global --unset user.name
```

---

## Configuration d'équipe

### Fichier .gitconfig partagé

Pour standardiser la configuration dans une équipe, créez un fichier `.gitconfig` dans le dépôt :

```ini
# .gitconfig-team (à la racine du projet)
[alias]
    sync = !git fetch && git rebase
    cleanup = !git branch --merged | grep -v '\\*\\|main\\|develop' | xargs -n 1 git branch -d

[core]
    autocrlf = input

[pull]
    rebase = true
```

**Chaque membre de l'équipe l'inclut :**

```bash
git config --global include.path /chemin/vers/.gitconfig-team
```

### Documentation de la configuration

Créez un `CONTRIBUTING.md` dans votre projet :

```markdown
# Configuration Git recommandée

Avant de contribuer, configurez Git :

```bash
git config user.email "votre.email@entreprise.com"
git config pull.rebase true
```

Hooks à installer :
- `pre-commit` : Lint et tests
- `commit-msg` : Validation du format
```

---

## Outils de configuration avancée

### Git Configuration Manager

Plusieurs outils facilitent la gestion de configurations complexes :

**1. Git Config Manager**
- Interface graphique pour gérer les configurations
- Gestion de profils multiples

**2. Dotfiles**
- Versionner votre `.gitconfig` dans un dépôt
- Synchroniser entre machines

**3. Chezmoi**
- Gestionnaire de dotfiles
- Templates et configurations conditionnelles

---

## Bonnes pratiques

### 1. Versionnez votre configuration

```bash
# Créer un dépôt dotfiles
mkdir ~/dotfiles
cd ~/dotfiles
cp ~/.gitconfig gitconfig
git init
git add gitconfig
git commit -m "Initial: Git configuration"
```

### 2. Documentez vos alias

Ajoutez des commentaires dans votre `.gitconfig` :

```ini
[alias]
    # Historique visuel
    lg = log --graph --oneline --all --decorate

    # Sync avec upstream
    sync = !git fetch upstream && git rebase upstream/main
```

### 3. Séparez les configurations

- **Globale** : Préférences personnelles
- **Locale** : Spécificités du projet
- **Équipe** : Standards partagés

### 4. Testez avant de partager

Testez toujours vos alias et configurations avant de les recommander à votre équipe.

### 5. Gardez la configuration simple

Ne surchargez pas avec des alias complexes que vous n'utiliserez jamais. Commencez simple et ajoutez au fur et à mesure.

---

## Checklist de configuration

Pour un setup complet et optimisé :

- [ ] Identité configurée (nom et email)
- [ ] Éditeur configuré
- [ ] Branche par défaut définie (main)
- [ ] Pull en rebase activé
- [ ] Couleurs activées
- [ ] 5-10 alias utiles configurés
- [ ] Merge conflictstyle en diff3
- [ ] Rerere activé
- [ ] Cache de credentials configuré
- [ ] Template de commit créé (optionnel)
- [ ] Hooks installés (optionnel)
- [ ] Configuration sauvegardée/versionnée

---

## Conclusion

La configuration de Git est un processus continu. Commencez avec une configuration de base, puis personnalisez au fur et à mesure selon vos besoins et habitudes.

**Principes clés :**
- ✅ Configurez au niveau approprié (local, global, system)
- ✅ Documentez vos configurations
- ✅ Partagez les bonnes pratiques avec votre équipe
- ✅ Testez avant de déployer largement
- ✅ Restez simple et pragmatique

**Pour débuter immédiatement :**
Copiez-collez la section "Configuration complète recommandée" ci-dessus, adaptez avec votre nom/email, et vous êtes prêt à travailler efficacement avec Git !

**Pour aller plus loin :**
- Explorez les hooks pour automatiser
- Créez des alias pour vos workflows spécifiques
- Synchronisez votre configuration entre machines
- Partagez vos meilleures configurations avec la communauté

Bonne configuration et bon usage de Git ! 🚀

⏭️ Retour au [Sommaire](/SOMMAIRE.md)
