# 3.3. git checkout et git switch

Une fois que vous avez créé vos branches, vous aurez besoin de naviguer entre elles. Dans Git, il existe deux commandes principales pour cette tâche : `git checkout` et `git switch`. Voyons comment les utiliser et leurs différences.

## La commande `git checkout`

Historiquement, `git checkout` a été la commande principale pour basculer entre les branches. C'est une commande polyvalente qui peut faire plusieurs choses :

### Basculer vers une branche existante

```bash
git checkout nom-de-la-branche
```

Par exemple, pour passer à la branche principale :

```bash
git checkout main
# ou
git checkout master  # selon le nom de votre branche principale
```

Lorsque vous exécutez cette commande, Git :
1. Met à jour les fichiers dans votre répertoire de travail pour qu'ils correspondent à la version dans la branche cible
2. Déplace le pointeur HEAD pour qu'il pointe vers la branche spécifiée

### Créer et basculer vers une nouvelle branche

Comme nous l'avons vu dans la section précédente, vous pouvez aussi créer et basculer vers une nouvelle branche en une seule commande :

```bash
git checkout -b nouvelle-branche
```

Par exemple :

```bash
git checkout -b feature/page-contact
```

### Revenir à un commit spécifique

Vous pouvez également utiliser `checkout` pour revenir à un état précédent de votre projet en spécifiant l'identifiant d'un commit :

```bash
git checkout abc1234  # où abc1234 est l'identifiant du commit
```

Cela vous place dans un état appelé "detached HEAD" (tête détachée), ce qui signifie que vous n'êtes plus sur une branche nommée.

## La commande `git switch` (Git 2.23+)

En 2019, Git a introduit la commande `git switch` pour rendre les choses plus claires. Cette commande a été créée spécifiquement pour la navigation entre branches, tandis que d'autres fonctionnalités de `checkout` ont été déplacées vers d'autres commandes (comme `git restore`).

### Basculer vers une branche existante

```bash
git switch nom-de-la-branche
```

Par exemple :

```bash
git switch main
```

### Créer et basculer vers une nouvelle branche

```bash
git switch -c nouvelle-branche
```

Par exemple :

```bash
git switch -c feature/page-contact
```

Le `-c` signifie "create" (créer).

### Revenir à la branche précédente

Une astuce pratique : vous pouvez rapidement revenir à la branche précédente avec :

```bash
git switch -
```

C'est similaire à la commande `cd -` dans le terminal, qui vous ramène au répertoire précédent.

## Quelle commande utiliser ?

Si vous utilisez une version récente de Git (2.23 ou plus récente), il est recommandé d'utiliser :
- `git switch` pour naviguer entre les branches
- `git restore` pour annuler des changements (nous verrons cette commande plus tard)

Si vous utilisez une version plus ancienne ou si vous êtes déjà habitué à `git checkout`, celle-ci fonctionne parfaitement aussi.

## Précautions à prendre lors du changement de branche

Avant de changer de branche, assurez-vous que :

1. **Votre travail est sauvegardé** : Git ne vous laissera pas changer de branche si vous avez des modifications non commitées qui entreraient en conflit avec les fichiers de la branche cible. Vous devrez soit committer vos changements, soit les mettre de côté (stash).

2. **Vous êtes dans le bon répertoire** : Assurez-vous d'être à la racine de votre dépôt Git, ou dans un sous-répertoire de celui-ci.

## Vérifier votre branche actuelle

Pour savoir sur quelle branche vous vous trouvez actuellement :

```bash
git status
```

La première ligne de la sortie indiquera votre branche actuelle : "On branch nom-de-la-branche"

Vous pouvez également utiliser :

```bash
git branch
```

La branche avec un astérisque (*) est votre branche actuelle.

## Exercice pratique

Essayons ces commandes dans un petit exercice :

1. Assurez-vous d'être sur votre branche principale (`main` ou `master`)
2. Créez et basculez vers une nouvelle branche appelée `feature/page-accueil` :
   ```bash
   git switch -c feature/page-accueil
   # ou
   git checkout -b feature/page-accueil
   ```
3. Vérifiez que vous êtes bien sur cette nouvelle branche avec `git status`
4. Basculez vers la branche principale :
   ```bash
   git switch main
   # ou
   git checkout main
   ```
5. Revenez à la branche `feature/page-accueil` en utilisant la commande pour revenir à la branche précédente :
   ```bash
   git switch -
   ```

Dans la prochaine section, nous verrons comment fusionner les changements d'une branche vers une autre avec `git merge`.
