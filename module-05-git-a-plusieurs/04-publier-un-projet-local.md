🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 4. Publier un projet local : git remote add et git push -u

### La situation de départ

Imaginez : vous avez créé un projet sur votre ordinateur, vous avez fait plusieurs commits, et maintenant vous voulez le partager en ligne. Comment faire ?

C'est exactement la situation inverse du clonage. Au lieu de partir d'un dépôt distant pour créer un dépôt local, vous partez d'un dépôt local pour créer un dépôt distant.

**Votre projet local existe déjà**
```bash
mon-projet/
  ├── .git/          # Dépôt Git initialisé
  ├── index.html
  ├── style.css
  └── script.js
```

Vous avez déjà fait des commits, mais tout est uniquement sur votre machine. Vous voulez maintenant :
- Sauvegarder votre code en ligne
- Le partager avec d'autres
- Collaborer avec une équipe
- Avoir une copie de secours

### Étape 1 : Créer un dépôt distant vide

Avant de connecter votre projet local, vous devez créer un dépôt vide sur GitHub, GitLab ou Bitbucket.

**Sur GitHub :**

1. Connectez-vous à votre compte GitHub
2. Cliquez sur le `+` en haut à droite, puis "New repository"
3. Donnez un nom à votre dépôt (idéalement le même que votre dossier local)
4. Ajoutez une description (optionnel)
5. Choisissez Public ou Private
6. **IMPORTANT** : Ne cochez PAS les options "Add a README", "Add .gitignore" ou "Choose a license"
7. Cliquez sur "Create repository"

**Pourquoi ne rien ajouter ?**

Si vous cochez ces options, GitHub créera des commits dans le dépôt distant. Votre dépôt local et le distant auront alors des historiques différents, ce qui compliquera la synchronisation. Pour publier un projet existant, le dépôt distant doit être complètement vide.

**Sur GitLab :**

1. Cliquez sur "New project" puis "Create blank project"
2. Donnez un nom au projet
3. Choisissez la visibilité
4. Décochez "Initialize repository with a README"
5. Cliquez sur "Create project"

**Sur Bitbucket :**

1. Cliquez sur "Create" puis "Repository"
2. Entrez le nom du dépôt
3. Choisissez la visibilité
4. Décochez "Include a README"
5. Cliquez sur "Create repository"

Après la création, la plateforme vous affiche généralement des instructions. Nous allons suivre celles pour "push an existing repository".

### Étape 2 : Copier l'URL du dépôt distant

Une fois le dépôt créé, vous verrez une URL. Copiez-la. Elle ressemblera à :

**HTTPS :**
```
https://github.com/votre-nom/mon-projet.git
```

**SSH :**
```
git@github.com:votre-nom/mon-projet.git
```

Pour l'instant, utilisez l'URL HTTPS qui est plus simple pour débuter.

### Étape 3 : Connecter votre dépôt local au distant avec git remote add

Dans votre terminal, positionnez-vous dans votre projet local :

```bash
cd ~/Documents/mon-projet
```

Puis ajoutez le lien vers le dépôt distant :

```bash
git remote add origin https://github.com/votre-nom/mon-projet.git
```

**Décortiquons cette commande :**

- `git remote add` : ajoute un nouveau dépôt distant
- `origin` : le nom que vous donnez à ce distant (c'est une convention, on utilise toujours "origin" pour le distant principal)
- L'URL : l'adresse du dépôt distant que vous venez de créer

**Qu'est-ce que "origin" ?**

"origin" est simplement un alias, un raccourci pour l'URL complète. Au lieu de taper `https://github.com/votre-nom/mon-projet.git` à chaque fois, vous pourrez simplement écrire `origin`.

C'est comme enregistrer un contact dans votre téléphone : au lieu de composer le numéro complet, vous tapez juste le nom.

### Vérifier la connexion

Pour vérifier que le distant a bien été ajouté :

```bash
git remote -v
```

Vous devriez voir :
```
origin  https://github.com/votre-nom/mon-projet.git (fetch)
origin  https://github.com/votre-nom/mon-projet.git (push)
```

Cela confirme que Git connaît maintenant votre dépôt distant et peut communiquer avec lui.

### Étape 4 : Envoyer votre code avec git push -u

Maintenant que la connexion est établie, envoyez votre code sur le dépôt distant :

```bash
git push -u origin main
```

Ou si votre branche principale s'appelle `master` :

```bash
git push -u origin master
```

**Décortiquons cette commande :**

- `git push` : envoie (pousse) vos commits vers le distant
- `-u` : (ou `--set-upstream`) crée un lien de suivi entre votre branche locale et distante
- `origin` : le nom du dépôt distant (celui qu'on a défini avec `git remote add`)
- `main` : le nom de la branche à envoyer

**Première fois : authentification**

Lors de votre premier push, Git vous demandera de vous authentifier :

```
Username for 'https://github.com': votre-nom
Password for 'https://votre-nom@github.com':
```

**ATTENTION :** Pour le mot de passe, n'utilisez PAS votre mot de passe GitHub !
Vous devez utiliser un **Personal Access Token (PAT)**.

Nous verrons comment créer un PAT dans la section sur l'authentification. Pour l'instant, sachez que c'est un mot de passe spécial que GitHub génère pour vous.

### Le rôle de -u (--set-upstream)

L'option `-u` (raccourci de `--set-upstream`) est très importante lors du premier push.

**Ce qu'elle fait :**

Elle crée un lien de suivi entre votre branche locale `main` et la branche distante `origin/main`. Une fois ce lien établi, Git sait que ces deux branches "vont ensemble".

**Avantages pour la suite :**

Après avoir utilisé `-u` une première fois, vous pourrez simplement faire :

```bash
git push
```

Sans préciser `origin main`. Git saura automatiquement où envoyer vos commits.

Vous pourrez aussi faire :

```bash
git pull
```

Sans préciser d'où récupérer les modifications. Git utilisera automatiquement le lien de suivi.

**Analogie :**

C'est comme configurer une destination favorite dans votre GPS. La première fois, vous entrez l'adresse complète (`-u`). Ensuite, vous pouvez juste dire "Va à ma destination favorite" sans répéter l'adresse.

### Que se passe-t-il lors du push ?

Quand vous exécutez `git push -u origin main`, Git :

1. Se connecte au dépôt distant sur GitHub/GitLab/Bitbucket
2. Vérifie vos permissions (authentification)
3. Envoie tous vos commits de la branche `main`
4. Crée la branche `main` sur le distant
5. Établit le lien de suivi entre les deux branches
6. Confirme que tout s'est bien passé

Vous verrez quelque chose comme :
```
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (15/15), 1.25 KiB | 1.25 MiB/s, done.
Total 15 (delta 3), reused 0 (delta 0)
To https://github.com/votre-nom/mon-projet.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

### Vérifier que tout a fonctionné

**Dans le terminal :**

```bash
git branch -vv
```

Vous devriez voir :
```
* main 7a3b2c1 [origin/main] Votre dernier message de commit
```

Le `[origin/main]` confirme que votre branche locale `main` suit la branche distante `origin/main`.

**Sur GitHub/GitLab/Bitbucket :**

Rafraîchissez la page de votre dépôt. Vous devriez maintenant voir :
- Tous vos fichiers
- Votre historique de commits
- Votre fichier README s'il existe

Félicitations ! Votre projet est maintenant en ligne !

### Exemple complet pas à pas

Imaginons que vous avez créé un site web simple :

```bash
# 1. Créer le projet et initialiser Git
mkdir mon-portfolio
cd mon-portfolio
git init

# 2. Créer des fichiers
echo "# Mon Portfolio" > README.md
echo "<h1>Bienvenue</h1>" > index.html
echo "body { font-family: Arial; }" > style.css

# 3. Faire le premier commit
git add .
git commit -m "Initial commit: structure de base"

# 4. Ajouter plus de contenu
echo "<p>Présentation</p>" >> index.html
git add index.html
git commit -m "Ajout de la présentation"

# À ce stade, tout est uniquement en local

# 5. Créer un dépôt vide sur GitHub (via l'interface web)

# 6. Connecter le local au distant
git remote add origin https://github.com/votre-nom/mon-portfolio.git

# 7. Vérifier la connexion
git remote -v

# 8. Envoyer le code
git push -u origin main

# 9. Vérifier que tout est en place
git branch -vv
```

Votre portfolio est maintenant accessible à `https://github.com/votre-nom/mon-portfolio` !

### Pushs suivants

Après le premier push avec `-u`, les prochains pushs sont beaucoup plus simples :

```bash
# Vous faites des modifications
echo "<footer>Contact</footer>" >> index.html

# Vous commitez
git add index.html
git commit -m "Ajout du footer"

# Vous poussez simplement avec
git push

# Git sait automatiquement envoyer vers origin/main
```

### Erreurs courantes et solutions

**"fatal: 'origin' does not appear to be a git repository"**

Vous n'avez pas encore ajouté le distant. Faites :
```bash
git remote add origin <url>
```

**"error: src refspec main does not match any"**

Votre branche s'appelle peut-être `master` et non `main`, ou vous n'avez pas encore de commits. Vérifiez :
```bash
git branch
```

Si vous êtes sur `master` :
```bash
git push -u origin master
```

Si vous n'avez aucune branche, faites au moins un commit :
```bash
git commit -m "Initial commit"
```

**"error: failed to push some refs to..."**

Le dépôt distant n'est pas vide, il contient déjà des commits. Deux solutions :

Soit vous avez accidentellement coché "Add README" lors de la création. Créez un nouveau dépôt vide.

Soit vous devez d'abord récupérer ces commits :
```bash
git pull origin main --allow-unrelated-histories
git push -u origin main
```

**"remote: Permission denied"**

Vous n'avez pas les droits d'écriture sur ce dépôt, ou vos identifiants sont incorrects. Vérifiez :
- L'URL du dépôt
- Vos identifiants (token)
- Vos permissions sur le dépôt

### Gérer plusieurs dépôts distants

Vous pouvez connecter votre projet local à plusieurs dépôts distants :

```bash
# Distant principal sur GitHub
git remote add origin https://github.com/vous/projet.git

# Distant secondaire sur GitLab
git remote add gitlab https://gitlab.com/vous/projet.git

# Voir tous vos distants
git remote -v

# Pousser vers GitHub
git push origin main

# Pousser vers GitLab
git push gitlab main
```

C'est utile pour :
- Avoir une copie miroir sur plusieurs plateformes
- Séparer code public (GitHub) et privé (GitLab d'entreprise)
- Travailler avec différentes équipes

### Changer ou supprimer un distant

**Voir les détails d'un distant :**
```bash
git remote show origin
```

**Changer l'URL d'un distant :**
```bash
git remote set-url origin https://nouvelle-url.git
```

**Renommer un distant :**
```bash
git remote rename origin nouveau-nom
```

**Supprimer un distant :**
```bash
git remote remove origin
```

### Bonnes pratiques

**Vérifiez avant de pousser**

Prenez l'habitude de vérifier l'état de votre dépôt avant de pousser :
```bash
git status
git log --oneline -5
```

Assurez-vous que vous poussez bien ce que vous voulez pousser.

**Messages de commit clairs**

Une fois en ligne, vos messages de commit sont visibles par tous les collaborateurs. Écrivez des messages clairs et descriptifs.

**Ne poussez pas de données sensibles**

Avant de pousser, vérifiez que vous n'avez pas :
- De mots de passe en clair
- De clés API
- De données personnelles sensibles

Utilisez `.gitignore` pour exclure les fichiers de configuration contenant des secrets.

**Poussez régulièrement**

Ne laissez pas s'accumuler des dizaines de commits avant de pousser. Poussez régulièrement pour :
- Sauvegarder votre travail
- Permettre aux autres de voir vos avancées
- Éviter les conflits trop complexes

### Résumé des commandes

```bash
# Créer le lien vers le distant
git remote add origin <url>

# Vérifier le distant
git remote -v

# Premier push avec suivi
git push -u origin main

# Pushs suivants
git push
```

### Ce qu'il faut retenir

Publier un projet local se fait en deux étapes simples :

1. **Connecter** : `git remote add origin <url>` - créer le lien
2. **Envoyer** : `git push -u origin main` - publier le code

L'option `-u` lors du premier push est importante : elle simplifie tous les pushs futurs.

Une fois votre projet publié, il est sauvegardé en ligne, accessible de partout, et prêt pour la collaboration. Vous pouvez maintenant travailler dessus depuis n'importe quel ordinateur, le partager avec d'autres, ou même en faire un projet open source !

⏭️ [Gérer les dépôts distants (git remote)](/module-05-git-a-plusieurs/05-gerer-les-depots-distants.md)
