🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 5. Gérer les dépôts distants (git remote)

### Introduction

Vous savez maintenant comment cloner un dépôt et publier votre code. Mais comment gérer efficacement vos connexions aux dépôts distants ? Comment voir les dépôts configurés, en ajouter, en supprimer, ou les modifier ?

La commande `git remote` est votre outil de gestion central pour toutes ces opérations. C'est comme un carnet d'adresses pour vos dépôts distants.

### Qu'est-ce qu'un "remote" exactement ?

Un **remote** (distant) est simplement un raccourci qui pointe vers l'URL d'un dépôt Git distant. Au lieu de taper une URL complète à chaque fois, vous utilisez un nom court.

**Sans remote :**
```bash
git push https://github.com/utilisateur/projet.git main
```

**Avec remote :**
```bash
git push origin main
```

C'est beaucoup plus simple ! Le remote "origin" est juste un alias pour l'URL complète.

### Lister les dépôts distants

**Commande de base :**
```bash
git remote
```

Affiche simplement les noms des distants :
```
origin
```

**Commande détaillée :**
```bash
git remote -v
```

L'option `-v` (verbose) affiche les URLs complètes :
```
origin  https://github.com/utilisateur/projet.git (fetch)
origin  https://github.com/utilisateur/projet.git (push)
```

**Comprendre fetch et push :**

Vous voyez deux lignes pour `origin` :
- **(fetch)** : URL utilisée pour récupérer (télécharger) des données
- **(push)** : URL utilisée pour envoyer (publier) des données

Dans la plupart des cas, ces deux URLs sont identiques. Mais vous pourriez avoir des URLs différentes si, par exemple, vous lisez depuis un dépôt public mais écrivez vers votre fork privé.

### Ajouter un dépôt distant

**Syntaxe :**
```bash
git remote add <nom> <url>
```

**Exemple :**
```bash
git remote add origin https://github.com/utilisateur/projet.git
```

**Ajouter plusieurs distants :**

Vous n'êtes pas limité à un seul distant. Vous pouvez en avoir plusieurs :

```bash
# Distant principal sur GitHub
git remote add origin https://github.com/vous/projet.git

# Distant secondaire sur GitLab
git remote add gitlab https://gitlab.com/vous/projet.git

# Distant du dépôt officiel (pour l'open source)
git remote add upstream https://github.com/projet-original/projet.git
```

Vérifiez :
```bash
git remote -v
```

Résultat :
```
origin    https://github.com/vous/projet.git (fetch)
origin    https://github.com/vous/projet.git (push)
gitlab    https://gitlab.com/vous/projet.git (fetch)
gitlab    https://gitlab.com/vous/projet.git (push)
upstream  https://github.com/projet-original/projet.git (fetch)
upstream  https://github.com/projet-original/projet.git (push)
```

### Convention de nommage des distants

Bien que vous puissiez nommer vos distants comme vous voulez, il existe des conventions :

**origin**
Le distant principal, celui d'où vous avez cloné ou vers lequel vous poussez habituellement. C'est le nom par défaut créé par `git clone`.

**upstream**
Utilisé dans le contexte de fork. C'est le dépôt original que vous avez forké. Vous récupérez les mises à jour depuis `upstream` et poussez vers `origin` (votre fork).

**Exemples de workflow avec fork :**
```
upstream = projet original (lecture seule pour vous)
origin = votre fork (lecture et écriture)
```

**Autres noms courants :**
- `production` : serveur de production
- `staging` : serveur de test
- `backup` : copie de sauvegarde
- `gitlab`, `bitbucket` : pour identifier la plateforme

### Voir les détails d'un distant

**Commande :**
```bash
git remote show <nom>
```

**Exemple :**
```bash
git remote show origin
```

Affiche des informations détaillées :
```
* remote origin
  Fetch URL: https://github.com/utilisateur/projet.git
  Push  URL: https://github.com/utilisateur/projet.git
  HEAD branch: main
  Remote branches:
    main     tracked
    develop  tracked
    feature  tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local ref configured for 'git push':
    main pushes to main (up to date)
```

**Ces informations vous disent :**
- Les URLs utilisées
- La branche principale du distant
- Toutes les branches distantes suivies
- Les branches locales liées aux branches distantes
- L'état de synchronisation

### Renommer un dépôt distant

**Syntaxe :**
```bash
git remote rename <ancien-nom> <nouveau-nom>
```

**Exemple :**
```bash
git remote rename origin github
git remote rename gitlab primary
```

Vérifiez :
```bash
git remote -v
```

**Quand renommer ?**

- Vous trouvez qu'un nom est peu clair
- Vous voulez standardiser les noms dans votre équipe
- Vous avez plusieurs distants et voulez mieux les distinguer

**Attention :** Renommer un distant ne change rien sur le serveur, c'est uniquement local. Cela affecte aussi les noms de branches distantes : `origin/main` devient `github/main`.

### Changer l'URL d'un distant

**Syntaxe :**
```bash
git remote set-url <nom> <nouvelle-url>
```

**Exemple - Passer de HTTPS à SSH :**
```bash
git remote set-url origin git@github.com:utilisateur/projet.git
```

**Exemple - Corriger une URL erronée :**
```bash
git remote set-url origin https://github.com/bon-utilisateur/projet.git
```

**Vérifier le changement :**
```bash
git remote -v
```

**Cas d'usage courants :**

**Migration de compte :**
Vous avez changé de nom d'utilisateur GitHub
```bash
git remote set-url origin https://github.com/nouveau-nom/projet.git
```

**Changement de protocole :**
Vous voulez passer de HTTPS à SSH pour ne plus taper vos identifiants
```bash
git remote set-url origin git@github.com:utilisateur/projet.git
```

**Changement de plateforme :**
Vous migrez de GitHub vers GitLab
```bash
git remote set-url origin https://gitlab.com/utilisateur/projet.git
```

**Migration de serveur d'entreprise :**
L'entreprise change de serveur Git
```bash
git remote set-url origin https://nouveau-serveur.entreprise.com/projet.git
```

### Supprimer un dépôt distant

**Syntaxe :**
```bash
git remote remove <nom>
```

ou

```bash
git remote rm <nom>
```

**Exemple :**
```bash
git remote remove backup
```

**Quand supprimer un distant ?**

- Le dépôt distant n'existe plus
- Vous ne travaillez plus avec ce distant
- Vous voulez nettoyer des distants de test
- Vous réorganisez vos connexions

**Important :** Supprimer un distant est une opération locale uniquement. Cela ne supprime pas le dépôt sur le serveur, juste la référence dans votre dépôt local.

### Modifier les URLs fetch et push séparément

Dans des cas avancés, vous pouvez avoir des URLs différentes pour fetch et push :

**URL fetch uniquement :**
```bash
git remote set-url origin https://github.com/utilisateur/projet.git
```

**URL push uniquement :**
```bash
git remote set-url --push origin git@github.com:utilisateur/projet.git
```

**Cas d'usage :**

Vous pouvez cloner (fetch) depuis un dépôt public en HTTPS, mais pousser (push) vers votre fork en SSH :
```bash
git remote add origin https://github.com/projet-public/code.git
git remote set-url --push origin git@github.com:votre-fork/code.git
```

### Scénarios pratiques complets

**Scénario 1 : Contribuer à un projet open source**

Vous voulez contribuer à un projet populaire :

```bash
# 1. Forker le projet sur GitHub (via l'interface)

# 2. Cloner votre fork
git clone https://github.com/vous/projet.git
cd projet

# 3. Ajouter le dépôt original comme upstream
git remote add upstream https://github.com/auteur-original/projet.git

# 4. Vérifier vos distants
git remote -v
# origin    -> votre fork (lecture/écriture)
# upstream  -> projet original (lecture seule)

# 5. Récupérer les mises à jour du projet original
git fetch upstream

# 6. Fusionner dans votre branche
git merge upstream/main

# 7. Pousser vers votre fork
git push origin main
```

**Scénario 2 : Corriger une mauvaise URL après clonage**

Vous avez cloné avec la mauvaise URL :

```bash
# Vérifier l'URL actuelle
git remote -v

# Corriger l'URL
git remote set-url origin https://bonne-url.git

# Vérifier la correction
git remote -v

# Tester avec un fetch
git fetch origin
```

**Scénario 3 : Maintenir plusieurs copies miroirs**

Votre projet doit être sur GitHub (public) et GitLab (entreprise) :

```bash
# Ajouter GitHub
git remote add github https://github.com/vous/projet.git

# Ajouter GitLab
git remote add gitlab https://gitlab.entreprise.com/vous/projet.git

# Pousser vers les deux
git push github main
git push gitlab main

# Ou créer un alias pour pousser partout (voir section alias)
```

**Scénario 4 : Passer de HTTPS à SSH**

Vous en avez assez de taper vos identifiants :

```bash
# 1. Configurer vos clés SSH (voir section authentification)

# 2. Vérifier l'URL actuelle
git remote -v
# origin  https://github.com/vous/projet.git

# 3. Changer pour SSH
git remote set-url origin git@github.com:vous/projet.git

# 4. Vérifier
git remote -v
# origin  git@github.com:vous/projet.git

# 5. Tester
git fetch origin
```

### Erreurs courantes et solutions

**"fatal: remote origin already exists"**

Vous essayez d'ajouter un distant qui existe déjà. Solutions :

```bash
# Soit supprimer puis rajouter
git remote remove origin
git remote add origin <nouvelle-url>

# Soit changer l'URL directement
git remote set-url origin <nouvelle-url>
```

**"fatal: No such remote: 'origin'"**

Le distant n'existe pas. Vérifiez les distants disponibles :
```bash
git remote -v
```

Puis ajoutez le bon distant :
```bash
git remote add origin <url>
```

**"error: Could not remove config section 'remote.origin'"**

Le distant n'existe pas ou a déjà été supprimé. Listez vos distants pour vérifier :
```bash
git remote -v
```

### Commandes récapitulatives

```bash
# Lister les distants (noms uniquement)
git remote

# Lister avec URLs
git remote -v

# Voir les détails d'un distant
git remote show origin

# Ajouter un distant
git remote add <nom> <url>

# Renommer un distant
git remote rename <ancien> <nouveau>

# Changer l'URL
git remote set-url <nom> <nouvelle-url>

# Supprimer un distant
git remote remove <nom>

# Récupérer depuis un distant spécifique
git fetch <nom>

# Pousser vers un distant spécifique
git push <nom> <branche>
```

### Organisation recommandée

**Pour un projet personnel simple :**
```
origin -> GitHub/GitLab personnel
```

**Pour un projet en équipe :**
```
origin -> Dépôt principal de l'équipe
```

**Pour contribuer à l'open source :**
```
origin   -> Votre fork
upstream -> Projet original
```

**Pour un projet déployé :**
```
origin     -> GitHub (code source)
production -> Serveur de production
staging    -> Serveur de test
```

### Bonnes pratiques

**Nommez clairement vos distants**

Utilisez des noms qui reflètent leur rôle :
```bash
git remote add github-perso https://...
git remote add github-travail https://...
```

**Documentez vos distants**

Si votre projet a plusieurs distants, documentez-les dans le README :
```markdown
## Dépôts distants
- `origin`: Dépôt principal (GitHub)
- `upstream`: Projet original forké
- `gitlab`: Copie miroir sur GitLab entreprise
```

**Vérifiez régulièrement**

Prenez l'habitude de vérifier vos distants :
```bash
git remote -v
```

Surtout après clonage ou avant de pousser du code important.

**Nettoyez les distants obsolètes**

Si un distant n'est plus utilisé, supprimez-le :
```bash
git remote remove ancien-serveur
```

**Soyez cohérent dans l'équipe**

Si vous travaillez en équipe, standardisez les noms de distants. Tout le monde devrait utiliser les mêmes conventions.

### Distants et branches

Les distants sont liés aux branches distantes. Quand vous avez un distant `origin`, vous verrez des branches comme :
- `origin/main`
- `origin/develop`
- `origin/feature-x`

Si vous renommez le distant :
```bash
git remote rename origin github
```

Les branches deviennent :
- `github/main`
- `github/develop`
- `github/feature-x`

### Configuration avancée

Les distants sont stockés dans `.git/config` :

```bash
cat .git/config
```

Vous verrez :
```ini
[remote "origin"]
    url = https://github.com/utilisateur/projet.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```

Vous pouvez éditer ce fichier directement, mais il est préférable d'utiliser les commandes `git remote` pour éviter les erreurs.

### Ce qu'il faut retenir

La commande `git remote` est votre interface de gestion pour les connexions distantes :

- **Lister** : `git remote -v` pour voir vos connexions
- **Ajouter** : `git remote add` pour créer une nouvelle connexion
- **Modifier** : `git remote set-url` pour changer une URL
- **Renommer** : `git remote rename` pour clarifier
- **Supprimer** : `git remote remove` pour nettoyer

Les distants sont simplement des raccourcis vers des URLs. Vous pouvez en avoir plusieurs et les gérer comme bon vous semble. C'est une configuration locale qui n'affecte que votre dépôt.

Avec une bonne gestion de vos distants, vous pouvez facilement travailler avec plusieurs dépôts, contribuer à l'open source, maintenir des copies miroirs, et collaborer efficacement avec différentes équipes.

⏭️ [Synchroniser : git fetch, git pull, git push](/module-05-git-a-plusieurs/06-synchroniser-fetch-pull-push.md)
