# 3.2. Créer, lister, supprimer des branches

Maintenant que vous comprenez ce qu'est une branche, voyons comment les manipuler concrètement. Ces opérations font partie des tâches quotidiennes lorsque vous travaillez avec Git.

## Lister les branches existantes

Avant de créer une nouvelle branche, il est souvent utile de savoir quelles branches existent déjà dans votre dépôt.

```bash
git branch
```

Cette commande affiche toutes les branches locales. La branche active (celle sur laquelle vous travaillez actuellement) est marquée par un astérisque (`*`).

Pour voir à la fois les branches locales et distantes (celles qui existent sur le serveur distant, comme GitHub) :

```bash
git branch -a
```

## Créer une nouvelle branche

Pour créer une nouvelle branche, utilisez la commande suivante :

```bash
git branch nom-de-la-branche
```

Par exemple, si vous voulez créer une branche pour développer une fonctionnalité de connexion :

```bash
git branch feature-login
```

**Important** : Cette commande crée uniquement la branche mais ne vous y place pas automatiquement. Vous restez sur votre branche actuelle.

### Créer et basculer en une seule commande

Pour créer une branche et y basculer immédiatement, utilisez :

```bash
git checkout -b nom-de-la-branche
```

Ou, avec Git version 2.23 ou supérieure :

```bash
git switch -c nom-de-la-branche
```

Par exemple :

```bash
git checkout -b feature-login
# ou
git switch -c feature-login
```

## Bonnes pratiques pour nommer les branches

Il est recommandé d'utiliser des noms descriptifs pour vos branches. Des conventions courantes incluent :

- `feature/nom-fonctionnalité` pour les nouvelles fonctionnalités
- `bugfix/description-bug` pour les corrections de bugs
- `hotfix/description` pour les corrections urgentes
- `docs/description` pour les modifications de documentation

Par exemple : `feature/user-authentication` ou `bugfix/login-error`

## Supprimer une branche

Une fois que vous avez terminé de travailler sur une branche (généralement après l'avoir fusionnée dans la branche principale), vous pouvez la supprimer :

```bash
git branch -d nom-de-la-branche
```

Par exemple :

```bash
git branch -d feature-login
```

Git vous protège en vérifiant que la branche a bien été fusionnée avant de la supprimer. Si vous essayez de supprimer une branche qui n'a pas été fusionnée, Git affichera une erreur.

Si vous êtes absolument sûr de vouloir supprimer une branche non fusionnée (peut-être parce que vous abandonnez les modifications), vous pouvez forcer la suppression :

```bash
git branch -D nom-de-la-branche
```

Attention : cette action est irréversible si vous n'avez pas poussé cette branche vers un dépôt distant !

## Renommer une branche

Si vous souhaitez renommer la branche sur laquelle vous travaillez actuellement :

```bash
git branch -m nouveau-nom
```

Pour renommer une branche différente de celle sur laquelle vous êtes :

```bash
git branch -m ancien-nom nouveau-nom
```

## Exercice pratique

Voici un petit exercice pour vous familiariser avec la gestion des branches :

1. Créez une nouvelle branche appelée `exercice-branches`
2. Listez toutes vos branches pour vérifier qu'elle a bien été créée
3. Basculez vers cette nouvelle branche
4. Créez un fichier texte simple (par exemple avec `echo "Test de branches" > test.txt`)
5. Ajoutez et committez ce fichier
6. Retournez à votre branche principale (`main` ou `master`)
7. Vérifiez que le fichier `test.txt` n'est pas visible dans cette branche
8. Supprimez la branche `exercice-branches` (vous devrez utiliser `-D` car vous n'avez pas fusionné les changements)

Cet exercice vous permettra de visualiser comment les branches maintiennent des environnements de travail séparés.

Dans la prochaine section, nous verrons comment naviguer entre les branches avec `git checkout` et `git switch`.
