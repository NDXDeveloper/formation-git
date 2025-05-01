# 3.4. Fusion de branches (git merge)

Une fois que vous avez développé et testé une fonctionnalité sur une branche séparée, vous voudrez intégrer ces changements dans votre branche principale. Cette opération s'appelle une **fusion** (ou "merge" en anglais). Voyons comment cela fonctionne.

## Comprendre la fusion

La fusion consiste à prendre les modifications d'une branche et à les appliquer à une autre branche. C'est comme dire à Git : "Prends tout le travail fait dans la branche A et ajoute-le à la branche B."

Il existe principalement deux types de fusions :
- **Fast-forward** (avance rapide) : quand il n'y a pas eu de nouveaux commits sur la branche de destination depuis que vous avez créé votre branche
- **Merge commit** (commit de fusion) : quand les deux branches ont évolué en parallèle

## Procédure de fusion

### Étape 1 : Basculer vers la branche de destination

Avant tout, vous devez vous placer sur la branche qui va **recevoir** les modifications. Généralement, c'est votre branche principale :

```bash
git checkout main
# ou
git switch main
```

### Étape 2 : Fusionner la branche

Ensuite, utilisez la commande `git merge` suivie du nom de la branche que vous souhaitez fusionner **dans** votre branche actuelle :

```bash
git merge nom-de-la-branche-source
```

Par exemple, pour fusionner une branche de fonctionnalité dans main :

```bash
git merge feature/nouvelle-fonctionnalite
```

### Exemple de fusion fast-forward

Voici comment se déroule une fusion "fast-forward" :

```
Avant la fusion:
      A---B---C (feature)
     /
D---E (main)

Après la fusion:
      A---B---C (main, feature)
     /
D---E
```

Git déplace simplement le pointeur de la branche main vers le dernier commit de la branche feature.

### Exemple de fusion avec commit de fusion

Voici comment se déroule une fusion qui nécessite un commit de fusion :

```
Avant la fusion:
      A---B---C (feature)
     /
D---E---F---G (main)

Après la fusion:
      A---B---C
     /         \
D---E---F---G---H (main)
```

Git crée un nouveau commit H qui combine les changements des deux branches.

## Options de fusion

### Forcer un commit de fusion

Même si une fusion fast-forward est possible, vous pouvez forcer la création d'un commit de fusion avec l'option `--no-ff` (no fast-forward) :

```bash
git merge --no-ff feature/nouvelle-fonctionnalite
```

Cela peut être utile pour garder une trace claire de l'historique de vos branches.

### Message de fusion personnalisé

Vous pouvez spécifier un message pour le commit de fusion :

```bash
git merge feature/nouvelle-fonctionnalite -m "Fusion de la fonctionnalité X dans main"
```

## Annuler une fusion

Si vous venez de faire une fusion et que vous souhaitez l'annuler :

```bash
git merge --abort
```

Cette commande fonctionne uniquement si la fusion n'est pas encore terminée (par exemple s'il y a des conflits).

Si la fusion est déjà complétée, vous pouvez revenir à l'état précédent avec :

```bash
git reset --hard ORIG_HEAD
```

ORIG_HEAD est une référence que Git crée automatiquement avant des opérations majeures comme une fusion.

## Bonnes pratiques pour les fusions

1. **Toujours s'assurer que la branche de destination est à jour** avant de fusionner
2. **Tester votre code** sur la branche source avant de fusionner
3. **Fusionner régulièrement** la branche principale dans vos branches de fonctionnalités pour limiter les conflits
4. **Faire une sauvegarde** si vous n'êtes pas sûr (soit avec `git branch backup/avant-fusion`, soit en poussant toutes vos branches vers un dépôt distant)

## Exercice pratique

Essayons une fusion dans un petit exercice :

1. Assurez-vous d'être sur votre branche principale
   ```bash
   git switch main
   ```

2. Créez une nouvelle branche de fonctionnalité
   ```bash
   git switch -c feature/bouton-contact
   ```

3. Ajoutez un nouveau fichier ou modifiez un fichier existant
   ```bash
   echo "Ceci est un nouveau bouton de contact" > bouton-contact.txt
   ```

4. Committez vos changements
   ```bash
   git add bouton-contact.txt
   git commit -m "Ajout du bouton de contact"
   ```

5. Retournez à la branche principale
   ```bash
   git switch main
   ```

6. Fusionnez votre branche de fonctionnalité
   ```bash
   git merge feature/bouton-contact
   ```

7. Vérifiez que la fusion a fonctionné
   ```bash
   git log --graph --oneline
   ```

Cela devrait vous montrer un graphe de l'historique avec votre fusion.

Dans la prochaine section, nous verrons comment gérer les conflits qui peuvent survenir lors d'une fusion.
