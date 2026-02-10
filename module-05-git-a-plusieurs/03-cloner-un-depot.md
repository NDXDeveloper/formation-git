🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 3. Cloner un dépôt (git clone)

### Qu'est-ce que le clonage ?

**Cloner** un dépôt Git signifie créer une copie complète d'un projet distant sur votre ordinateur. C'est comme télécharger un projet entier avec tout son historique, toutes ses branches et tous ses commits.

Contrairement à un simple téléchargement de fichiers, le clonage vous donne :
- Tous les fichiers du projet dans leur état actuel
- L'historique complet des modifications
- Toutes les branches disponibles
- Un lien automatique avec le dépôt distant d'origine

### Pourquoi cloner un dépôt ?

**Rejoindre un projet existant**
Votre équipe travaille déjà sur un projet hébergé sur GitHub ? Clonez-le pour commencer à contribuer.

**Étudier du code**
Vous avez trouvé un projet open source intéressant ? Clonez-le pour explorer le code en détail sur votre machine.

**Utiliser un projet**
Vous voulez utiliser une bibliothèque ou un outil disponible sur GitHub ? Clonez-le pour l'installer et l'utiliser.

**Contribuer à l'open source**
Pour proposer des améliorations à un projet public, vous devez d'abord le cloner.

### La commande git clone

La syntaxe de base est très simple :

```bash
git clone <url-du-dépôt>
```

Par exemple :

```bash
git clone https://github.com/utilisateur/mon-projet.git
```

Cette commande va :
1. Créer un nouveau dossier nommé `mon-projet`
2. Télécharger tous les fichiers du projet
3. Récupérer tout l'historique Git
4. Configurer automatiquement le dépôt distant (appelé `origin`)

### Où trouver l'URL d'un dépôt ?

Sur **GitHub**, **GitLab** ou **Bitbucket**, lorsque vous êtes sur la page d'un dépôt, cherchez un bouton vert "Code", "Clone" ou similaire. En cliquant dessus, vous verrez l'URL à utiliser.

Il existe généralement deux types d'URL :

**HTTPS** (recommandé pour débuter)
```
https://github.com/utilisateur/projet.git
```
Simple à utiliser, nécessite votre nom d'utilisateur et mot de passe (ou token).

**SSH**
```
git@github.com:utilisateur/projet.git
```
Plus sécurisé et pratique, mais nécessite de configurer des clés SSH au préalable.

Pour l'instant, utilisez HTTPS. Nous verrons SSH plus tard dans ce module.

### Exemples concrets de clonage

**Cloner un projet dans le dossier courant**

```bash
cd ~/Documents  
git clone https://github.com/torvalds/linux.git
```

Cela crée un dossier `linux` dans `~/Documents` contenant le code source du noyau Linux.

**Cloner avec un nom de dossier personnalisé**

Si vous voulez que le dossier créé ait un nom différent :

```bash
git clone https://github.com/utilisateur/projet.git mon-dossier-perso
```

Le projet sera cloné dans un dossier nommé `mon-dossier-perso` au lieu de `projet`.

**Cloner une branche spécifique**

Par défaut, Git clone la branche principale. Pour cloner une branche spécifique :

```bash
git clone -b nom-de-branche https://github.com/utilisateur/projet.git
```

**Clonage superficiel (shallow clone)**

Pour les très gros projets, si vous voulez seulement les fichiers récents sans tout l'historique :

```bash
git clone --depth 1 https://github.com/utilisateur/projet.git
```

Cela télécharge uniquement le dernier commit, ce qui est beaucoup plus rapide. Utile pour tester rapidement un projet ou si vous n'avez pas besoin de l'historique complet.

### Que se passe-t-il après le clonage ?

Une fois le clonage terminé, entrez dans le dossier créé :

```bash
cd mon-projet
```

Vous pouvez maintenant :

**Voir l'état du dépôt**
```bash
git status
```

**Consulter l'historique**
```bash
git log
```

**Voir les branches disponibles**
```bash
git branch -a
```

**Vérifier le dépôt distant configuré**
```bash
git remote -v
```

Vous verrez quelque chose comme :
```
origin  https://github.com/utilisateur/projet.git (fetch)  
origin  https://github.com/utilisateur/projet.git (push)
```

Le nom `origin` est le nom par défaut donné au dépôt distant d'où vous avez cloné. C'est une convention standard dans Git.

### Différence entre télécharger et cloner

**Télécharger le ZIP depuis GitHub**
Si vous cliquez sur "Download ZIP" sur GitHub, vous obtenez juste les fichiers actuels. Vous n'avez :
- Aucun historique Git
- Aucune connexion au dépôt distant
- Pas de possibilité de contribuer facilement

**Cloner avec git clone**
Vous obtenez un véritable dépôt Git fonctionnel avec :
- L'historique complet
- Toutes les branches
- La connexion au dépôt distant
- La possibilité de contribuer et synchroniser

### Exemple complet : cloner et explorer un projet

Imaginons que vous voulez explorer le code de Vue.js :

```bash
# 1. Cloner le projet
git clone https://github.com/vuejs/core.git vue-js

# 2. Entrer dans le dossier
cd vue-js

# 3. Voir où vous êtes
git status

# 4. Consulter l'historique récent
git log --oneline -10

# 5. Voir toutes les branches disponibles
git branch -a

# 6. Vérifier la configuration du distant
git remote -v
```

Vous avez maintenant le code complet de Vue.js sur votre machine et vous pouvez l'explorer, le modifier localement, ou même contribuer au projet.

### Erreurs courantes et solutions

**"fatal: destination path 'projet' already exists"**
Un dossier avec ce nom existe déjà. Supprimez-le, déplacez-vous ailleurs, ou utilisez un nom différent :
```bash
git clone https://github.com/user/projet.git projet-v2
```

**"fatal: unable to access ... Could not resolve host"**
Problème de connexion Internet ou URL incorrecte. Vérifiez :
- Votre connexion Internet
- Que l'URL est correcte (copiez-collez depuis GitHub)
- Que le dépôt existe et est accessible

**"Username for 'https://github.com':"**
Git vous demande de vous authentifier. Pour HTTPS :
- Entrez votre nom d'utilisateur GitHub
- Pour le mot de passe, utilisez un Personal Access Token (PAT) et non votre mot de passe de compte

Nous verrons l'authentification en détail plus loin dans ce module.

**Clonage très lent**
Pour les gros projets, le clonage peut prendre du temps. Vous pouvez :
- Utiliser `--depth 1` pour un clonage superficiel
- Être patient, cela ne se fait qu'une fois
- Vérifier votre connexion Internet

### Cloner des dépôts privés

Pour cloner un dépôt privé, vous devez avoir :
- L'autorisation d'accès au dépôt (le propriétaire doit vous ajouter comme collaborateur)
- Vos identifiants configurés (username/token ou clés SSH)

Le processus est le même, mais Git vous demandera de vous authentifier :

```bash
git clone https://github.com/entreprise/projet-prive.git
# Username: votre-nom
# Password: votre-token-personnel
```

### Cloner vs Fork

Sur GitHub, vous verrez aussi un bouton "Fork". Quelle est la différence ?

**Clone**
Crée une copie locale sur votre ordinateur. Le dépôt distant reste celui d'origine.

**Fork**
Crée une copie du dépôt sous votre compte GitHub. Vous avez alors votre propre version du projet sur GitHub.

Le workflow classique pour contribuer à un projet open source :
1. **Fork** le projet sur GitHub (copie sur votre compte)
2. **Clone** votre fork sur votre ordinateur
3. Faites vos modifications
4. Poussez vers votre fork
5. Créez une Pull Request vers le projet original

### Bonnes pratiques

**Organisez vos projets**
Créez une structure de dossiers cohérente :
```
~/Projets/
  ├── personnel/
  ├── travail/
  └── open-source/
```

Clonez ensuite dans le dossier approprié :
```bash
cd ~/Projets/open-source  
git clone https://github.com/projet/cool.git
```

**Vérifiez toujours après le clonage**
Prenez l'habitude de faire :
```bash
cd projet-clone  
git status  
git remote -v
```

Cela vous confirme que tout s'est bien passé.

**Lisez le README**
Après avoir cloné un projet, lisez toujours le fichier `README.md`. Il contient généralement :
- La description du projet
- Comment l'installer et l'utiliser
- Comment contribuer
- Les dépendances nécessaires

**Ne modifiez pas directement la branche main**
Quand vous commencez à travailler sur le projet cloné, créez une nouvelle branche pour vos modifications :
```bash
git checkout -b ma-nouvelle-fonctionnalite
```

### Mise à jour d'un dépôt cloné

Une fois que vous avez cloné un dépôt, il reste sur votre ordinateur avec la version que vous avez téléchargée. Pour récupérer les nouvelles modifications du dépôt distant :

```bash
git pull
```

Nous verrons cette commande en détail dans les prochaines sections.

### Ce qu'il faut retenir

Le clonage est l'action de démarrage la plus courante quand vous travaillez avec Git en équipe. C'est simple :

1. Trouvez l'URL du dépôt sur GitHub/GitLab/Bitbucket
2. Utilisez `git clone <url>`
3. Vous avez maintenant une copie complète et fonctionnelle du projet

Le clonage ne se fait qu'une seule fois par projet. Ensuite, vous utiliserez `git pull` et `git push` pour synchroniser vos modifications.

Avec `git clone`, vous pouvez facilement rejoindre des projets existants, contribuer à l'open source, ou simplement explorer le code de milliers de projets disponibles publiquement.

⏭️ [Publier un projet local : git remote add et git push -u](/module-05-git-a-plusieurs/04-publier-un-projet-local.md)
