# 5.4. Tagging et releases

## Marquer les moments importants de votre projet

Imaginez que vous développez une application et que vous venez de terminer une version stable que vous souhaitez publier. Comment indiquer clairement ce point spécifique dans l'historique de votre projet ? Comment retrouver facilement cette version particulière parmi des centaines de commits ?

C'est là que les **tags** Git entrent en jeu !

## Qu'est-ce qu'un tag Git ?

Un **tag** est simplement un pointeur vers un commit spécifique, mais contrairement aux branches, il ne se déplace pas. C'est comme planter un drapeau sur un commit en disant : "Ce point est important, donnons-lui un nom facile à retenir."

Les tags sont généralement utilisés pour marquer :
- Des versions de release (v1.0, v2.1.3)
- Des jalons importants du projet
- Des états stables du code

## Types de tags

Git propose deux types de tags :

1. **Tags légers** : simple pointeur vers un commit
2. **Tags annotés** : objets complets dans la base de données Git, contenant des métadonnées supplémentaires

Pour les releases, on utilise généralement des tags annotés car ils stockent des informations importantes comme :
- Le nom du créateur du tag
- La date de création
- Un message descriptif

## Créer des tags

### Créer un tag léger

La syntaxe la plus simple :

```bash
git tag v1.0
```

Cette commande crée un tag nommé "v1.0" qui pointe vers le commit actuel (HEAD).

### Créer un tag annoté (recommandé pour les releases)

```bash
git tag -a v1.0 -m "Version 1.0 - Première release stable"
```

L'option `-a` signifie "annoté" et `-m` vous permet d'ajouter un message descriptif.

### Tagger un commit passé

Vous pouvez aussi tagger un commit antérieur en spécifiant son hash :

```bash
git tag -a v0.9 -m "Version bêta" 9fceb02
```

Où `9fceb02` est le début du hash du commit que vous souhaitez tagger.

## Visualiser les tags

### Lister tous les tags

```bash
git tag
```

Vous verrez une liste comme :
```
v0.8
v0.9
v1.0
```

### Lister les tags avec un filtre

Pour voir tous les tags qui commencent par "v1." :

```bash
git tag -l "v1.*"
```

### Voir les détails d'un tag

Pour voir les informations détaillées d'un tag annoté :

```bash
git show v1.0
```

Cela affichera des informations comme :
```
tag v1.0
Tagger: Votre Nom <votre@email.com>
Date:   Mer Mai 5 17:30:45 2025 +0200

Version 1.0 - Première release stable

commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
Author: Votre Nom <votre@email.com>
Date:   Mer Mai 5 17:25:36 2025 +0200

    Dernier commit avant la v1.0
```

## Pousser des tags vers un dépôt distant

Par défaut, la commande `git push` n'envoie pas les tags vers votre dépôt distant. Pour les partager, vous devez le faire explicitement.

### Pousser un tag spécifique

```bash
git push origin v1.0
```

### Pousser tous les tags

```bash
git push origin --tags
```

## Checkout d'un tag

Pour examiner le code tel qu'il était au moment du tag :

```bash
git checkout v1.0
```

⚠️ **Attention** : Cela vous met dans un état "détaché" (detached HEAD). Tout commit que vous feriez ne serait attaché à aucune branche. Si vous souhaitez faire des modifications, créez plutôt une branche :

```bash
git checkout -b branche-fix-v1.0 v1.0
```

Cette commande crée une nouvelle branche nommée "branche-fix-v1.0" basée sur l'état du code au tag "v1.0".

## Supprimer des tags

### Supprimer un tag local

```bash
git tag -d v1.0
```

### Supprimer un tag distant

```bash
git push origin --delete v1.0
```

## Convention de nommage des tags

Une convention populaire pour les tags de version est le **Semantic Versioning** (SemVer) avec le format `vX.Y.Z` où :

- **X** : Version majeure (changements incompatibles avec les versions précédentes)
- **Y** : Version mineure (nouvelles fonctionnalités compatibles avec les versions précédentes)
- **Z** : Patch (corrections de bugs compatibles)

Exemples :
- `v1.0.0` : Première version stable
- `v1.0.1` : Correction de bugs sur la v1.0.0
- `v1.1.0` : Ajout de nouvelles fonctionnalités
- `v2.0.0` : Changements majeurs qui peuvent casser la compatibilité

## Des tags aux releases

### Qu'est-ce qu'une release ?

Une **release** est un concept plus large qu'un simple tag. Une release comprend généralement :
- Un tag Git qui marque le code
- Des fichiers compilés/packagés (binaires, archives)
- Des notes de release expliquant les changements
- Des instructions de mise à jour

### Créer une release sur GitHub/GitLab

Sur les plateformes comme GitHub ou GitLab, vous pouvez transformer un tag en release complète :

1. Créez un tag dans Git et poussez-le
2. Sur l'interface web, allez dans la section "Releases"
3. Créez une nouvelle release basée sur votre tag
4. Ajoutez un titre, une description et des fichiers binaires (si nécessaire)

### Notes de release efficaces

De bonnes notes de release incluent généralement :
- Un résumé des changements majeurs
- Une liste des nouvelles fonctionnalités
- Les bugs corrigés
- Les changements qui peuvent affecter les utilisateurs existants
- Des instructions de mise à jour

## Cas pratiques d'utilisation

### Scénario 1 : Créer une release de votre application

1. Vous finalisez tous les développements prévus pour la v1.0
2. Vous créez un tag annoté :
   ```bash
   git tag -a v1.0 -m "Version 1.0 - Description des fonctionnalités principales"
   ```
3. Vous poussez le tag :
   ```bash
   git push origin v1.0
   ```
4. Vous créez une release sur GitHub/GitLab basée sur ce tag

### Scénario 2 : Retrouver un état antérieur

Votre application a un bug en production et vous suspectez qu'il n'était pas présent dans la v1.2 :
1. Vous vérifiez le code à ce tag :
   ```bash
   git checkout v1.2
   ```
2. Vous confirmez que le bug n'était pas présent
3. Vous créez une branche pour corriger le bug dans la version actuelle :
   ```bash
   git checkout main
   git checkout -b fix-bug-xyz
   ```

### Scénario 3 : Backport d'un correctif

Vous avez corrigé un bug critique sur la branche principale, mais vous devez aussi l'appliquer à une ancienne version en production :
1. Vous identifiez le commit du correctif
2. Vous créez une branche basée sur l'ancien tag de release :
   ```bash
   git checkout -b fix-for-v1.5 v1.5
   ```
3. Vous appliquez le correctif avec cherry-pick
4. Vous taguez cette correction :
   ```bash
   git tag -a v1.5.1 -m "Correctif de sécurité pour la v1.5"
   ```

## Bonnes pratiques

1. **Utilisez toujours des tags annotés pour les releases** (`-a`)
2. **Suivez une convention de nommage cohérente** (comme SemVer)
3. **Écrivez des messages descriptifs** expliquant ce qui est important dans cette version
4. **Ne modifiez jamais un tag publié** - créez plutôt un nouveau tag
5. **Poussez vos tags** vers le dépôt distant pour les partager avec l'équipe
6. **Documentez vos releases** avec des notes claires

## En résumé

Les tags Git sont un moyen simple mais puissant de marquer des points importants dans l'historique de votre projet. Ils facilitent la navigation dans l'historique, le déploiement de versions et la communication sur l'évolution de votre logiciel.

Combinés avec de bonnes pratiques de release, les tags transforment votre historique Git en une chronologie claire du développement de votre projet, ce qui est inestimable tant pour les développeurs que pour les utilisateurs.
