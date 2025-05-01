# 4.3. Cloner un dépôt (git clone)

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Qu'est-ce que le clonage ?

**Cloner** un dépôt, c'est créer une copie complète d'un projet Git existant sur votre ordinateur. Cette copie inclut :
- Tous les fichiers du projet
- Tout l'historique des commits
- Toutes les branches
- Tous les tags

Le clonage est souvent la première étape lorsque vous voulez contribuer à un projet existant ou simplement explorer son code.

## L'analogie de la bibliothèque

Imaginez GitHub comme une bibliothèque avec des milliers de livres (projets). Le clonage, c'est comme emprunter un livre pour l'emmener chez vous. Vous obtenez votre propre exemplaire sur lequel vous pouvez travailler tranquillement.

## Comment cloner un dépôt

### Étape 1 : Trouver l'URL du dépôt à cloner

Sur GitHub, rendez-vous sur la page du dépôt que vous souhaitez cloner. Cliquez sur le bouton vert `Code` et vous verrez une URL. Vous avez deux options :

- **HTTPS** : Plus simple pour débuter (format : `https://github.com/utilisateur/projet.git`)
- **SSH** : Plus sécurisé mais nécessite une configuration supplémentaire (format : `git@github.com:utilisateur/projet.git`)

Pour les débutants, commencez avec HTTPS. Nous verrons l'option SSH dans une section ultérieure.

![Bouton Clone sur GitHub](https://i.imgur.com/EXEMPLE_IMAGE.png)

### Étape 2 : Exécuter la commande git clone

Ouvrez votre terminal (Command Prompt, PowerShell, Terminal, etc.) et naviguez vers le dossier où vous souhaitez placer le projet. Puis exécutez :

```bash
git clone https://github.com/utilisateur/projet.git
```

Remplacez l'URL par celle que vous avez copiée depuis GitHub.

### Étape 3 : Attendre la fin du téléchargement

Git va télécharger tous les fichiers du projet. Vous verrez quelque chose comme :

```
Cloning into 'projet'...
remote: Enumerating objects: 1463, done.
remote: Counting objects: 100% (1463/1463), done.
remote: Compressing objects: 100% (739/739), done.
remote: Total 1463 (delta 724), reused 1463 (delta 724), pack-reused 0
Receiving objects: 100% (1463/1463), 2.56 MiB | 3.27 MiB/s, done.
Resolving deltas: 100% (724/724), done.
```

### Étape 4 : Naviguer dans le dépôt cloné

Une fois le clonage terminé, un nouveau dossier portant le nom du projet aura été créé. Accédez-y avec :

```bash
cd nom-du-projet
```

Vous pouvez maintenant explorer le code, créer des branches, faire des commits, etc.

## Exemples pratiques

### Exemple 1 : Cloner un projet open source populaire

Clonons le projet "React" de Facebook :

```bash
git clone https://github.com/facebook/react.git
```

### Exemple 2 : Cloner dans un dossier spécifique

Si vous voulez que le dossier créé ait un nom différent de celui du projet :

```bash
git clone https://github.com/utilisateur/projet.git mon-dossier-personnalise
```

## Points importants à comprendre

### Le dossier .git est cloné

Quand vous clonez un dépôt, vous obtenez également le dossier `.git` qui contient tout l'historique du projet. C'est ce qui vous permet de voir tous les commits passés et de travailler avec toutes les branches.

### La connexion au dépôt distant est automatique

Lorsque vous clonez un dépôt, Git configure automatiquement une connexion avec le dépôt d'origine, appelé `origin`. Cela vous permettra plus tard de facilement pousser vos modifications ou récupérer les mises à jour.

Pour voir cette connexion, vous pouvez utiliser :

```bash
git remote -v
```

Vous devriez voir quelque chose comme :

```
origin  https://github.com/utilisateur/projet.git (fetch)
origin  https://github.com/utilisateur/projet.git (push)
```

### Différence entre télécharger et cloner

- **Télécharger un ZIP** depuis GitHub ne vous donne que les fichiers actuels, sans l'historique Git.
- **Cloner** vous donne les fichiers ET tout l'historique Git, vous permettant de contribuer au projet.

## Que faire après avoir cloné ?

Une fois que vous avez cloné un dépôt, vous pouvez :

1. Explorer le code pour comprendre comment il fonctionne
2. Créer une nouvelle branche pour travailler sur des fonctionnalités ou des corrections
3. Faire des modifications, les commiter, et les pousser vers GitHub
4. Récupérer les dernières modifications faites par d'autres contributeurs

Nous verrons ces actions dans les prochaines sections du tutoriel.

## Résolution de problèmes courants

### "Permission denied"
Si vous voyez une erreur de permission lors du clonage, assurez-vous que :
- Le dépôt n'est pas privé
- Vous avez correctement copié l'URL
- Vous êtes connecté à Internet

### Dépôt trop volumineux
Pour les très grands projets, le clonage peut prendre du temps. Si le projet est trop volumineux, vous pouvez envisager un "clone superficiel" :

```bash
git clone --depth=1 https://github.com/utilisateur/projet.git
```

Cela ne télécharge que le commit le plus récent, ce qui est beaucoup plus rapide.

---

Dans la prochaine section, nous apprendrons comment pousser vos modifications locales vers GitHub et récupérer les changements des autres contributeurs.

⏭️ [Pousser et tirer des changements (git push, git pull, git fetch)](/module-4-git-a-plusieurs-git-et-github/04-pousser-et-tirer-des-changements.md)
