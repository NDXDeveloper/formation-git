# 5.3. Reflog et récupération de commits

## Le filet de sécurité de Git

Vous est-il déjà arrivé de supprimer accidentellement du code important ? Ou peut-être avez-vous fait une erreur lors d'un rebase ou d'un reset ? Bonne nouvelle : Git garde presque toujours une trace de ce que vous faites et offre un moyen de récupérer vos données !

Dans cette section, nous allons découvrir `git reflog`, un outil puissant qui fonctionne comme une "boîte noire" de votre dépôt Git - enregistrant toutes vos actions et vous permettant de revenir à n'importe quel état précédent, même après des opérations qui semblent destructives.

## Qu'est-ce que le Reflog ?

Le "reference log" (ou reflog) est un mécanisme interne de Git qui enregistre tous les changements de position de HEAD dans votre dépôt. En termes simples :

- Il garde une trace de **chaque action** qui modifie l'état de votre dépôt
- Il inclut les commits, checkout, resets, merges, rebases, etc.
- C'est comme un **historique de navigation** de vos actions Git
- Il est **strictement local** et n'est jamais partagé avec d'autres dépôts

Contrairement à `git log` qui montre l'historique des commits, `git reflog` montre l'historique de vos actions Git.

## Comment accéder au Reflog

La commande de base est simple :

```bash
git reflog
```

Vous verrez quelque chose comme ceci :

```
734713b (HEAD -> main) HEAD@{0}: commit: Ajout de la fonctionnalité X
8a3d12f HEAD@{1}: commit: Correction du bug Y
4e8d7f2 HEAD@{2}: checkout: moving from feature to main
2c5e8c9 HEAD@{3}: commit: Travail sur la feature
...
```

Chaque ligne contient :
- Un identifiant de commit
- Une notation `HEAD@{n}` qui indique la position dans l'historique des actions
- Le type d'action effectuée (commit, checkout, reset, etc.)
- Une description de l'action

## Scénarios de récupération courants

### 1. Récupérer après un reset accidentel

Imaginez que vous venez de faire un `git reset --hard` et que vous réalisez que vous avez perdu des commits importants :

```bash
# Vous avez fait ceci par accident
git reset --hard HEAD~3  # Supprime les 3 derniers commits

# Panique ! Comment récupérer ces commits ?
```

Solution :

1. Consultez le reflog pour voir vos actions récentes :
   ```bash
   git reflog
   ```

2. Trouvez l'état avant votre reset (par exemple, `HEAD@{4}`)

3. Récupérez cet état :
   ```bash
   git reset --hard HEAD@{4}
   ```

Et voilà ! Vos commits sont de retour.

### 2. Récupérer une branche supprimée

Vous avez supprimé une branche par erreur :

```bash
git branch -D ma-super-feature  # Suppression accidentelle
```

Pour la récupérer :

1. Consultez le reflog :
   ```bash
   git reflog
   ```

2. Trouvez le dernier commit de cette branche (cherchez quelque chose comme "checkout: moving from ma-super-feature to main")

3. Recréez la branche à partir de ce point :
   ```bash
   git branch ma-super-feature HEAD@{7}  # Utilisez le numéro approprié
   ```

### 3. Récupérer après un rebase raté

Un rebase peut parfois mal tourner et vous faire perdre des commits. Pour récupérer :

1. Consultez le reflog :
   ```bash
   git reflog
   ```

2. Identifiez l'état avant le rebase (souvent marqué "checkout: moving to...")

3. Revenez à cet état :
   ```bash
   git reset --hard HEAD@{11}  # Utilisez le numéro approprié
   ```

## Options utiles du Reflog

### Afficher plus de détails

Pour voir plus d'informations dans le reflog :

```bash
git reflog --verbose
```

### Filtrer par date

Pour voir les actions des dernières 24 heures :

```bash
git reflog --since="24 hours ago"
```

Ou une autre période :

```bash
git reflog --since="2 days ago"
git reflog --since="1 week ago"
```

### Voir le reflog d'une branche spécifique

```bash
git reflog show feature-branch
```

## Comment Git stocke le Reflog

Le reflog est stocké dans votre dépôt Git local, dans des fichiers comme :
- `.git/logs/HEAD`
- `.git/logs/refs/heads/main`

Ces fichiers sont de simples fichiers texte qui enregistrent les mouvements de chaque référence.

## Limites du Reflog

Il est important de connaître les limites du reflog :

1. **Expiration des entrées** : Par défaut, les entrées du reflog expirent après 90 jours pour les commits accessibles, et 30 jours pour les commits non accessibles.

2. **Local uniquement** : Le reflog n'est jamais poussé vers des dépôts distants - il est strictement local.

3. **Pas de reflog pour les nouveaux clones** : Si vous clonez un dépôt, vous commencez avec un reflog vide.

4. **Suppression lors du garbage collecting** : La commande `git gc` peut supprimer les objets inaccessibles après leur expiration du reflog.

## Autres commandes de récupération

### Trouver des commits "perdus"

Si vous ne trouvez pas ce que vous cherchez dans le reflog, vous pouvez essayer :

```bash
git fsck --lost-found
```

Cette commande vérifie la base de données Git et trouve les objets sans référence (dangling).

### Examiner un commit spécifique

Une fois que vous avez trouvé un commit que vous souhaitez récupérer, vous pouvez l'examiner :

```bash
git show <commit-hash>
```

### Créer une branche à partir d'un commit perdu

```bash
git branch nouvelle-branche-de-recuperation <commit-hash>
```

## Bonnes pratiques de prévention

Bien que le reflog soit un excellent filet de sécurité, voici quelques pratiques pour éviter d'avoir à l'utiliser :

1. **Committer fréquemment** : Des commits petits et fréquents facilitent la récupération.

2. **Créer des branches** : Travaillez sur des branches séparées pour les expérimentations risquées.

3. **Faire des backups** : Pour les changements importants, considérez de pousser une branche de sauvegarde vers le dépôt distant.

4. **Utiliser `--dry-run`** : Certaines commandes Git offrent une option `--dry-run` pour voir ce qui se passerait sans réellement l'exécuter.

## En résumé

Le reflog est comme une machine à remonter le temps pour votre dépôt Git. Il vous permet de :
- Récupérer des commits perdus
- Restaurer des branches supprimées
- Annuler des opérations destructives comme reset, rebase ou merge
- Explorer l'historique complet de vos actions dans Git

C'est un outil extrêmement puissant qui peut vous sauver dans des situations qui sembleraient autrement désespérées. Rappelez-vous : avec Git, presque rien n'est jamais vraiment perdu - il suffit de savoir où chercher !
