# 5.2. Cherry-pick : sélectionner des commits spécifiques

## Qu'est-ce que le Cherry-pick ?

Imaginez un panier de cerises. Au lieu de prendre tout le panier, vous choisissez uniquement les plus belles cerises, une par une. C'est exactement ce que fait `git cherry-pick` : il vous permet de sélectionner des commits spécifiques provenant de n'importe quelle branche et de les appliquer à votre branche actuelle.

En termes simples, cherry-pick permet de :
- Copier un commit spécifique
- L'appliquer à votre branche actuelle
- Créer un nouveau commit avec les mêmes modifications

## Pourquoi utiliser le Cherry-pick ?

Voici quelques situations où le cherry-pick est particulièrement utile :

1. **Récupérer un correctif spécifique** : vous avez besoin d'un correctif de bug qui existe sur une autre branche
2. **Extraire des fonctionnalités** : vous voulez récupérer certaines parties d'une branche sans la fusionner entièrement
3. **Corriger des erreurs de branche** : vous avez commité sur la mauvaise branche
4. **Réorganiser l'historique** : vous souhaitez réorganiser l'ordre des commits

## Comment utiliser git cherry-pick

### Syntaxe de base

```bash
git cherry-pick <commit-hash>
```

Où `<commit-hash>` est l'identifiant unique du commit que vous voulez copier (cet identifiant ressemble à `a1b2c3d...`).

### Étapes pour faire un cherry-pick

1. **Identifier le commit** que vous souhaitez récupérer :
   ```bash
   git log
   ```

   ou pour voir les commits d'une autre branche :
   ```bash
   git log autre-branche
   ```

2. **Copier l'identifiant du commit** (hash) que vous souhaitez récupérer. Par exemple : `a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6`

3. **Basculer sur la branche de destination** où vous voulez appliquer ce commit :
   ```bash
   git checkout ma-branche-cible
   ```

4. **Appliquer le commit** avec cherry-pick :
   ```bash
   git cherry-pick a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
   ```

5. Git va créer un **nouveau commit** sur votre branche actuelle avec les mêmes modifications que le commit original.

### Exemple visuel

Supposons que vous ayez cette situation :

```
Branche main :     A---B---C---D
                    \
Branche feature :    E---F---G---H
```

Si vous voulez appliquer le commit G à la branche main :

1. `git checkout main` (vous passez à la branche main)
2. `git cherry-pick <hash-de-G>` (vous appliquez G)

Le résultat sera :

```
Branche main :     A---B---C---D---G'
                    \
Branche feature :    E---F---G---H
```

Notez que G' est un nouveau commit qui contient les mêmes modifications que G, mais avec un identifiant différent.

## Options utiles de cherry-pick

### Récupérer plusieurs commits d'un coup

Pour cherry-picker plusieurs commits consécutifs :

```bash
git cherry-pick <commit1>..<commit2>
```

Cela va appliquer tous les commits après `commit1` jusqu'à `commit2` inclus.

Pour cherry-picker plusieurs commits non consécutifs :

```bash
git cherry-pick <commit1> <commit2> <commit3>
```

### Éditer le message de commit

Pour modifier le message de commit pendant le cherry-pick :

```bash
git cherry-pick -e <commit-hash>
```

### Cherry-pick sans commit automatique

Si vous voulez appliquer les modifications sans créer de commit automatiquement :

```bash
git cherry-pick -n <commit-hash>
```

Cela vous permet de vérifier les changements et de les committer vous-même quand vous êtes prêt.

## Gérer les conflits lors d'un cherry-pick

Comme pour un merge, un cherry-pick peut générer des conflits si les modifications du commit sélectionné sont incompatibles avec votre branche actuelle.

Si un conflit survient :

1. Git va vous alerter que le cherry-pick n'a pas pu être complété
2. Vous devrez résoudre les conflits dans les fichiers concernés
3. Ajouter les fichiers résolus avec `git add <fichier>`
4. Continuer le cherry-pick avec :
   ```bash
   git cherry-pick --continue
   ```

Ou si vous préférez abandonner l'opération :
```bash
git cherry-pick --abort
```

## Cas pratiques d'utilisation

### Scénario 1 : Récupérer un correctif de bug urgent

1. Un bug urgent est corrigé sur la branche de développement
2. Vous devez appliquer ce correctif sur la branche de production sans fusionner toutes les autres modifications
3. Utilisez cherry-pick pour appliquer uniquement le commit du correctif

### Scénario 2 : Vous avez commité sur la mauvaise branche

1. Vous réalisez que vous avez fait des commits sur `main` au lieu de votre branche de fonctionnalité
2. Créez une nouvelle branche à partir de `main` avant ces commits
3. Cherry-pickez les commits sur cette nouvelle branche
4. Réinitialisez la branche `main` à son état précédent

### Scénario 3 : Récupérer une fonctionnalité spécifique

Vous travaillez sur un projet où différentes équipes développent des fonctionnalités sur différentes branches :
1. L'équipe B a développé une fonctionnalité utile pour votre tâche
2. Vous n'avez besoin que de cette fonctionnalité spécifique, pas du reste de leur travail
3. Utilisez cherry-pick pour récupérer uniquement les commits pertinents

## Bonnes pratiques

1. **Évitez de cherry-picker des merges** : cela peut créer des complications
2. **Utilisez des messages clairs** : ajoutez "(cherry-picked from commit xyz)" dans vos messages pour garder une trace
3. **Préférez un rebase ou un merge** quand vous voulez récupérer tous les commits d'une branche
4. **Documentez vos actions** : notez quels commits ont été cherry-pickés où, surtout dans un environnement d'équipe

## Points importants à retenir

1. Cherry-pick crée de **nouveaux commits** avec de nouveaux identifiants
2. Ces commits contiennent les mêmes modifications mais sont techniquement différents des originaux
3. Utilisez cette fonctionnalité avec parcimonie pour éviter de dupliquer trop de code
4. C'est un excellent outil pour des cas spécifiques, mais pas pour le flux de travail quotidien

## En résumé

`git cherry-pick` est un outil puissant qui vous permet d'appliquer sélectivement des modifications d'une branche à une autre. C'est comme si vous aviez un super pouvoir pour extraire précisément ce dont vous avez besoin, quand vous en avez besoin.

Utilisez-le judicieusement et vous aurez une flexibilité incroyable pour gérer votre code et corriger des problèmes sans perturber le flux de développement normal.
