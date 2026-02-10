🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 8 : Dépannage et résolution de problèmes

## 5. Erreurs fréquentes et solutions

### Introduction

Tout le monde rencontre des erreurs avec Git, même les développeurs expérimentés ! Cette section regroupe les erreurs les plus courantes et leurs solutions. Considérez-la comme votre guide de premiers secours Git.

---

## Erreurs de commit

### Erreur 1 : "Please tell me who you are"

**Message d'erreur :**
```
*** Please tell me who you are.

Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```

**Cause :** Vous n'avez pas configuré votre identité Git.

**Solution :**
```bash
# Configurer votre nom
git config --global user.name "Votre Nom"

# Configurer votre email
git config --global user.email "votre.email@example.com"

# Vérifier la configuration
git config --global --list
```

**Note :** Utilisez `--global` pour configurer pour tous vos projets, ou omettez-le pour configurer uniquement le projet actuel.

---

### Erreur 2 : "Changes not staged for commit"

**Message d'erreur :**
```
Changes not staged for commit:
  modified:   fichier.txt
```

**Cause :** Vous avez modifié des fichiers mais ne les avez pas ajoutés au staging area.

**Solution :**
```bash
# Ajouter les fichiers modifiés
git add fichier.txt

# Ou ajouter tous les fichiers modifiés
git add .

# Puis commiter
git commit -m "Votre message"
```

**Alternative rapide :**
```bash
# Commiter directement tous les fichiers modifiés (déjà trackés)
git commit -am "Votre message"
```

**Note :** Le `-a` n'ajoute que les fichiers **déjà trackés**. Les nouveaux fichiers doivent être ajoutés avec `git add`.

---

### Erreur 3 : "nothing to commit, working tree clean"

**Message :**
```
nothing to commit, working tree clean
```

**Cause :** Vous essayez de faire un commit mais il n'y a aucune modification.

**Ce n'est pas vraiment une erreur !** Cela signifie que tout est déjà sauvegardé.

**Vérification :**
```bash
# Voir l'état actuel
git status

# Voir s'il y a des modifications non trackées
git status --ignored
```

---

### Erreur 4 : Oublier le message de commit

**Message d'erreur :**
```
Aborting commit due to empty commit message.
```

**Cause :** Vous avez fait `git commit` sans `-m` et fermé l'éditeur sans écrire de message.

**Solutions :**

**Option 1 : Utiliser -m directement**
```bash
git commit -m "Votre message descriptif"
```

**Option 2 : Configurer un éditeur plus confortable**
```bash
# Utiliser VS Code
git config --global core.editor "code --wait"

# Utiliser nano (plus simple que vim)
git config --global core.editor "nano"

# Utiliser Notepad sur Windows
git config --global core.editor "notepad"
```

---

### Erreur 5 : Mauvais message de commit

**Situation :** Vous venez de commiter avec un message incorrect ou avec une faute.

**Solution (dernier commit uniquement) :**
```bash
# Modifier le message du dernier commit
git commit --amend -m "Nouveau message corrigé"
```

**⚠️ Attention :** Ne faites jamais `--amend` sur un commit déjà partagé (après un `git push`).

---

## Erreurs de push et pull

### Erreur 6 : "failed to push some refs"

**Message d'erreur :**
```
! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'origin'  
hint: Updates were rejected because the tip of your current branch is behind
```

**Cause :** Quelqu'un d'autre a poussé des modifications avant vous. Votre branche locale est en retard.

**Solution (méthode recommandée) :**
```bash
# Récupérer et fusionner les modifications distantes
git pull origin main

# Si des conflits apparaissent, les résoudre
# Puis pousser
git push origin main
```

**Alternative avec rebase :**
```bash
# Récupérer et rebaser vos commits
git pull --rebase origin main

# Pousser
git push origin main
```

**⚠️ À éviter (sauf si vous savez ce que vous faites) :**
```bash
# Force push - écrase l'historique distant
git push --force
```

Le `--force` peut faire perdre le travail des autres ! Utilisez-le seulement sur vos branches personnelles.

---

### Erreur 7 : "refusing to merge unrelated histories"

**Message d'erreur :**
```
fatal: refusing to merge unrelated histories
```

**Cause :** Vous essayez de fusionner deux dépôts qui n'ont aucun commit en commun (par exemple, un dépôt local et un dépôt GitHub initialisé séparément).

**Solution :**
```bash
# Forcer la fusion d'historiques non liés
git pull origin main --allow-unrelated-histories

# Ou lors d'un merge
git merge origin/main --allow-unrelated-histories
```

**Après cette commande :** Vous devrez probablement résoudre des conflits si les deux dépôts ont des fichiers avec le même nom.

---

### Erreur 8 : "Your branch is ahead of 'origin/main' by X commits"

**Message :**
```
Your branch is ahead of 'origin/main' by 3 commits.
  (use "git push" to publish your local commits)
```

**Cause :** Vous avez des commits locaux qui ne sont pas encore sur le serveur distant.

**Ce n'est pas une erreur !** C'est juste une information.

**Solution :**
```bash
# Pousser vos commits
git push origin main
```

---

### Erreur 9 : "Permission denied (publickey)"

**Message d'erreur :**
```
Permission denied (publickey).  
fatal: Could not read from remote repository.
```

**Cause :** Git ne peut pas s'authentifier avec le serveur distant (problème de clé SSH ou de token).

**Solutions selon le protocole :**

**Pour SSH (git@github.com:...) :**

**Étape 1 : Vérifier si vous avez une clé SSH**
```bash
ls -al ~/.ssh
```

Cherchez des fichiers comme `id_rsa.pub` ou `id_ed25519.pub`.

**Étape 2 : Générer une clé SSH (si nécessaire)**
```bash
ssh-keygen -t ed25519 -C "votre.email@example.com"
```

**Étape 3 : Copier la clé publique**
```bash
# Linux/macOS
cat ~/.ssh/id_ed25519.pub

# Windows
type %USERPROFILE%\.ssh\id_ed25519.pub
```

**Étape 4 : Ajouter la clé à GitHub/GitLab**
- Aller dans Settings → SSH Keys
- Coller la clé publique

**Pour HTTPS (https://github.com/...) :**

Utiliser un Personal Access Token (PAT) :
```bash
# GitHub vous demandera username et password
# Username : votre nom d'utilisateur
# Password : collez votre PAT (pas votre mot de passe !)
```

**Alternative : Changer de HTTPS vers SSH (ou inverse)**
```bash
# Voir l'URL actuelle
git remote -v

# Changer vers SSH
git remote set-url origin git@github.com:user/repo.git

# Changer vers HTTPS
git remote set-url origin https://github.com/user/repo.git
```

---

## Erreurs de branches

### Erreur 10 : "error: pathspec 'branch-name' did not match any file(s)"

**Message d'erreur :**
```
error: pathspec 'feature' did not match any file(s) known to git
```

**Cause :** Vous essayez de basculer vers une branche qui n'existe pas localement.

**Solutions :**

**Si la branche existe sur le serveur distant :**
```bash
# Récupérer toutes les branches distantes
git fetch

# Créer une branche locale qui suit la branche distante
git checkout -b feature origin/feature

# Ou plus simplement (Git moderne)
git checkout feature
```

**Si la branche n'existe nulle part :**
```bash
# Lister toutes les branches locales
git branch

# Lister toutes les branches distantes
git branch -r

# Créer une nouvelle branche
git checkout -b feature
```

---

### Erreur 11 : "Your local changes would be overwritten by checkout"

**Message d'erreur :**
```
error: Your local changes to the following files would be overwritten by checkout:
    fichier.txt
Please commit your changes or stash them before you switch branches.
```

**Cause :** Vous avez des modifications non commitées qui entreraient en conflit avec la branche vers laquelle vous voulez basculer.

**Solutions :**

**Option 1 : Commiter les modifications**
```bash
git add .  
git commit -m "Sauvegarde avant changement de branche"  
git checkout autre-branche
```

**Option 2 : Mettre de côté temporairement (stash)**
```bash
# Mettre de côté
git stash

# Changer de branche
git checkout autre-branche

# Plus tard, récupérer les modifications
git checkout branche-originale  
git stash pop
```

**Option 3 : Abandonner les modifications**
```bash
# ⚠️ Cela supprime définitivement vos modifications !
git checkout -- fichier.txt

# Ou pour tous les fichiers
git reset --hard
```

---

### Erreur 12 : "You are not currently on a branch"

**Message :**
```
You are in 'detached HEAD' state.
```

**Cause :** Vous avez fait un checkout sur un commit spécifique plutôt que sur une branche.

**Solution :** Voir la section "Résoudre detached HEAD" du module 8.1.

**Solution rapide :**
```bash
# Si vous voulez garder vos modifications
git checkout -b nouvelle-branche

# Sinon, revenir sur une branche existante
git checkout main
```

---

## Erreurs de conflits

### Erreur 13 : Conflits de merge

**Message :**
```
CONFLICT (content): Merge conflict in fichier.txt  
Automatic merge failed; fix conflicts and then commit the result.
```

**Cause :** Git ne peut pas fusionner automatiquement car deux versions modifient les mêmes lignes.

**Solution :**

**Étape 1 : Identifier les fichiers en conflit**
```bash
git status
```

Vous verrez :
```
Unmerged paths:
  both modified:   fichier.txt
```

**Étape 2 : Ouvrir le fichier et résoudre les conflits**

Le fichier contient des marqueurs :
```
<<<<<<< HEAD
Votre version
=======
La version de l'autre branche
>>>>>>> feature-branch
```

**Étape 3 : Éditer le fichier**

Supprimez les marqueurs et gardez le bon code :
```
Version finale après résolution
```

**Étape 4 : Marquer comme résolu et terminer le merge**
```bash
git add fichier.txt  
git commit -m "Résolution des conflits"
```

**Outils visuels pour aider :**
```bash
# Utiliser l'outil de merge par défaut
git mergetool

# Ou configurer VS Code
git config --global merge.tool vscode  
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

**Abandonner le merge en cours :**
```bash
git merge --abort
```

---

### Erreur 14 : "You have unmerged paths"

**Message :**
```
You have unmerged paths.
  (fix conflicts and run "git commit")
```

**Cause :** Vous êtes au milieu d'un merge avec des conflits non résolus.

**Solution :**
```bash
# Voir quels fichiers sont en conflit
git status

# Résoudre chaque conflit
# Puis ajouter les fichiers résolus
git add fichier-resolu.txt

# Terminer le merge
git commit
```

---

## Erreurs de fichiers

### Erreur 15 : "Fatal: Not a git repository"

**Message d'erreur :**
```
fatal: not a git repository (or any of the parent directories): .git
```

**Cause :** Vous n'êtes pas dans un dépôt Git, ou le dossier `.git` a été supprimé.

**Solutions :**

**Si vous êtes dans le mauvais dossier :**
```bash
# Naviguer vers votre projet
cd /chemin/vers/votre/projet

# Vérifier que c'est bien un dépôt Git
ls -la
```

Vous devez voir un dossier `.git`.

**Si le dossier .git n'existe pas :**
```bash
# Initialiser un nouveau dépôt
git init
```

---

### Erreur 16 : Fichier supprimé par erreur

**Situation :** Vous avez supprimé un fichier et fait un commit.

**Solutions :**

**Si vous n'avez pas encore commité :**
```bash
# Restaurer le fichier
git restore fichier.txt

# Ou avec l'ancienne syntaxe
git checkout -- fichier.txt
```

**Si vous avez commité la suppression :**
```bash
# Trouver le dernier commit où le fichier existait
git log -- fichier.txt

# Récupérer le fichier de ce commit
git checkout <hash-du-commit> -- fichier.txt

# Commiter la restauration
git add fichier.txt  
git commit -m "Restauration de fichier.txt"
```

**Récupération depuis un commit spécifique :**
```bash
# Récupérer la version d'il y a 3 commits
git checkout HEAD~3 -- fichier.txt
```

---

### Erreur 17 : "The following untracked working tree files would be overwritten"

**Message d'erreur :**
```
error: The following untracked working tree files would be overwritten by checkout:
    fichier.txt
Please move or remove them before you switch branches.
```

**Cause :** Vous avez un fichier non tracké qui a le même nom qu'un fichier dans la branche cible.

**Solutions :**

**Option 1 : Renommer/déplacer le fichier**
```bash
mv fichier.txt fichier.txt.backup  
git checkout autre-branche
```

**Option 2 : Supprimer le fichier (si pas important)**
```bash
rm fichier.txt  
git checkout autre-branche
```

**Option 3 : Commiter le fichier d'abord**
```bash
git add fichier.txt  
git commit -m "Ajout de fichier.txt"  
git checkout autre-branche
```

---

## Erreurs de configuration

### Erreur 18 : "LF will be replaced by CRLF"

**Message d'avertissement :**
```
warning: LF will be replaced by CRLF in fichier.txt.  
The file will have its original line endings in your working directory
```

**Cause :** Différences de fin de ligne entre systèmes d'exploitation (Windows vs Linux/Mac).

**Solution :**

**Configuration recommandée (une seule fois) :**
```bash
# Sur Windows
git config --global core.autocrlf true

# Sur Mac/Linux
git config --global core.autocrlf input
```

**Ce n'est généralement pas grave**, c'est juste un avertissement. Git s'occupe de la conversion automatiquement.

---

### Erreur 19 : ".gitignore ne fonctionne pas"

**Situation :** Vous avez ajouté un fichier à `.gitignore` mais Git le suit toujours.

**Cause :** Le fichier était déjà tracké avant d'être ajouté au `.gitignore`.

**Solution :**
```bash
# Retirer le fichier du tracking (mais le garder localement)
git rm --cached fichier-a-ignorer.txt

# Ou pour un dossier
git rm -r --cached dossier/

# Commiter
git commit -m "Mise à jour du .gitignore"
```

**Vérifier que .gitignore fonctionne :**
```bash
# Tester si un fichier serait ignoré
git check-ignore -v fichier.txt
```

---

### Erreur 20 : Problèmes d'encodage ou de caractères bizarres

**Symptômes :** Caractères étranges dans les noms de fichiers, accents mal affichés.

**Solutions :**

**Pour les noms de fichiers :**
```bash
# Permettre les caractères Unicode dans les noms
git config --global core.quotepath false
```

**Pour le contenu des fichiers :**

Vérifier l'encodage de votre éditeur (devrait être UTF-8).

---

## Erreurs diverses

### Erreur 21 : "Git is not recognized as an internal or external command"

**Cause (Windows) :** Git n'est pas dans le PATH système.

**Solutions :**

**Option 1 : Réinstaller Git** et cocher "Add Git to PATH"

**Option 2 : Ajouter manuellement au PATH**
1. Chercher l'installation de Git (généralement `C:\Program Files\Git\cmd`)
2. Ajouter ce chemin aux variables d'environnement Windows

**Redémarrer le terminal** après modification.

---

### Erreur 22 : Opération trop lente

**Symptômes :** Git est très lent, surtout sur Windows.

**Solutions :**

**1. Désactiver l'antivirus pour le dossier .git**

**2. Activer le cache de fichiers :**
```bash
git config --global core.preloadindex true  
git config --global core.fscache true
```

**3. Utiliser Git Bash au lieu de CMD/PowerShell**

**4. Nettoyer le dépôt :**
```bash
git gc --aggressive
```

---

### Erreur 23 : "fatal: unable to auto-detect email address"

**Message d'erreur :**
```
fatal: unable to auto-detect email address (got 'user@machine.(none)')
```

**Cause :** Git ne peut pas deviner votre email automatiquement.

**Solution :**
```bash
git config --global user.email "votre.email@example.com"  
git config --global user.name "Votre Nom"
```

---

### Erreur 24 : Problème avec les sous-modules

**Message :**
```
fatal: no submodule mapping found in .gitmodules
```

**Solutions :**

**Initialiser les sous-modules :**
```bash
git submodule init  
git submodule update
```

**Ou en une commande lors du clone :**
```bash
git clone --recurse-submodules https://github.com/user/repo.git
```

---

## Commandes de diagnostic

Quand vous êtes perdu, ces commandes vous aident à comprendre la situation :

```bash
# Voir l'état actuel
git status

# Voir l'historique récent
git log --oneline -10

# Voir toutes les références
git reflog

# Voir les branches
git branch -a

# Voir la configuration
git config --list

# Voir les remotes
git remote -v

# Vérifier l'intégrité du dépôt
git fsck

# Voir ce qui a changé
git diff
```

---

## Tableau récapitulatif des erreurs courantes

| Erreur | Cause principale | Solution rapide |
|--------|------------------|-----------------|
| Please tell me who you are | Config manquante | `git config --global user.name/email` |
| Failed to push | Branche en retard | `git pull` puis `git push` |
| Permission denied | Authentification | Configurer SSH ou PAT |
| Merge conflict | Modifications conflictuelles | Éditer le fichier, `git add`, `git commit` |
| Detached HEAD | Checkout d'un commit | `git checkout -b nouvelle-branche` |
| Not a git repository | Mauvais dossier | `cd` vers le bon projet ou `git init` |
| .gitignore ne marche pas | Fichier déjà tracké | `git rm --cached fichier` |
| Untracked files would be overwritten | Fichier en conflit | Déplacer, supprimer, ou commiter |

---

## Philosophie de débogage Git

Quand vous rencontrez une erreur :

1. **Lisez le message d'erreur** : Git donne souvent la solution directement
2. **Ne paniquez pas** : Presque tout est réversible dans Git
3. **Utilisez `git status`** : C'est votre boussole
4. **Cherchez sur Google** : Vous n'êtes pas le premier à avoir ce problème
5. **Faites des sauvegardes** : Créez une branche avant les manipulations risquées
6. **Demandez de l'aide** : Votre équipe ou les communautés en ligne

---

## Ressources pour aller plus loin

- **Documentation officielle** : https://git-scm.com/doc
- **Oh Shit, Git!?!** : https://ohshitgit.com/ (guide des erreurs courantes)
- **Stack Overflow** : Tag "git" pour les questions spécifiques
- **Git Flight Rules** : Guide des situations d'urgence

---

## Points clés à retenir

- **La plupart des erreurs Git sont réversibles** : Le reflog est votre filet de sécurité
- **Lisez toujours les messages d'erreur** : Git vous dit souvent quoi faire
- **git status est votre meilleur ami** : Utilisez-le constamment
- **En cas de doute, créez une branche** : Vous pourrez toujours revenir en arrière
- **Les conflits sont normaux** : Ils font partie du travail collaboratif
- **N'ayez pas peur d'expérimenter** : Vous apprendrez en faisant des erreurs

### En résumé

Les erreurs Git font partie de l'apprentissage. Chaque erreur que vous résolvez vous rend plus compétent. Gardez ce guide à portée de main, et rappelez-vous : même les experts Git consultent régulièrement la documentation et cherchent des solutions en ligne. L'important n'est pas de ne jamais faire d'erreurs, mais de savoir comment les résoudre !

⏭️ [Module 9 : Outils et intégration](/module-09-outils-et-integration/README.md)
