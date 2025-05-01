# 3.6. Rebasage : git rebase

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

Le rebasage (ou "rebase" en anglais) est une alternative à la fusion pour intégrer les changements d'une branche à une autre. Bien que plus complexe à première vue, cette technique permet de maintenir un historique plus propre et linéaire.

## Qu'est-ce que le rebasage ?

Alors que la fusion (merge) crée un nouveau commit qui combine deux branches, le rebasage **réécrit l'historique** en déplaçant une série de commits vers une nouvelle "base".

En termes simples, le rebasage permet de dire : "Prends mes modifications dans cette branche et applique-les comme si j'avais commencé à travailler à partir du dernier commit de l'autre branche."

## Comprendre visuellement le rebasage

### Avant rebase

```
      A---B---C (feature)
     /
D---E---F---G (main)
```

### Après rebase

```
              A'--B'--C' (feature)
             /
D---E---F---G (main)
```

Notez que les commits A, B et C ont été recréés (d'où les apostrophes) sur la base du commit G. Les commits originaux existent toujours, mais sont généralement "perdus" car plus aucune branche ne les référence.

## Quand utiliser rebase vs merge ?

- **Utilisez merge** lorsque vous souhaitez préserver l'historique exact des branches et montrer quand des branches ont été fusionnées.
- **Utilisez rebase** lorsque vous souhaitez garder un historique propre et linéaire, particulièrement pour les branches de fonctionnalités avant de les fusionner dans la branche principale.

## Comment effectuer un rebasage

### Rebase basique

La syntaxe de base est la suivante :

```bash
git checkout feature  # Allez sur la branche à rebaser
git rebase main       # Rebasez la branche actuelle sur main
```

Ces commandes prennent tous les commits qui existent uniquement dans la branche `feature` et les "rejoue" sur le dessus de la branche `main`.

### Exemple pratique

1. Créez une branche de fonctionnalité et ajoutez quelques commits
   ```bash
   git switch -c feature/nouvelle-fonction
   # Modifiez des fichiers
   git add .
   git commit -m "Première partie de la fonction"
   # Modifiez d'autres fichiers
   git add .
   git commit -m "Deuxième partie de la fonction"
   ```

2. Entre-temps, la branche principale a avancé avec d'autres commits
   ```bash
   git switch main
   # Modifiez des fichiers
   git add .
   git commit -m "Mise à jour sur main"
   ```

3. Revenez à votre branche et rebasez-la sur main
   ```bash
   git switch feature/nouvelle-fonction
   git rebase main
   ```

4. Une fois le rebase terminé, vous pouvez fusionner proprement dans main
   ```bash
   git switch main
   git merge feature/nouvelle-fonction  # Ce sera une fusion fast-forward
   ```

## Gérer les conflits pendant un rebase

Comme pour une fusion, des conflits peuvent survenir pendant un rebase. La principale différence est que Git résout les conflits **commit par commit** plutôt que tous à la fois.

Lorsqu'un conflit apparaît :

1. Git s'arrête et vous indique quel commit cause le conflit
2. Vous résolvez les conflits comme d'habitude (éditer les fichiers et supprimer les marqueurs de conflit)
3. Vous ajoutez les fichiers résolus : `git add fichier-resolu.txt`
4. Vous poursuivez le rebase : `git rebase --continue`

Si les choses deviennent trop compliquées, vous pouvez toujours annuler l'opération :
```bash
git rebase --abort
```

## Fonctionnalités avancées du rebase

### Rebase interactif

Le rebase interactif est un outil puissant qui vous permet de modifier l'historique de commits avant de les appliquer à la nouvelle base :

```bash
git rebase -i main
```

Cette commande ouvre un éditeur où vous pouvez :
- Réorganiser l'ordre des commits
- Supprimer des commits
- Fusionner plusieurs commits en un seul (squash)
- Modifier les messages de commit
- Et plus encore...

### Exemple de rebase interactif

Quand vous exécutez `git rebase -i main`, vous verrez quelque chose comme :

```
pick abc1234 Premier commit
pick def5678 Deuxième commit
pick ghi9012 Troisième commit

# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# ... (et plus d'options)
```

Pour fusionner le deuxième et le troisième commit, modifiez le fichier comme suit :

```
pick abc1234 Premier commit
pick def5678 Deuxième commit
squash ghi9012 Troisième commit
```

Enregistrez et fermez l'éditeur. Git vous demandera alors de fournir un message pour le commit combiné.

## Mise en garde importante

⚠️ **Le rebasage réécrit l'historique de Git**. Cela signifie que :

1. **Ne jamais rebaser des branches qui ont été partagées avec d'autres** (poussées sur un dépôt distant que d'autres personnes utilisent). Cela créerait des duplications de commits qui compliqueraient la vie de tout le monde.

2. Si vous avez déjà poussé une branche et que vous la rebasez ensuite, vous devrez utiliser `git push --force` pour la pousser à nouveau. Utilisez cette commande avec précaution car elle peut effacer le travail des autres.

## Bonnes pratiques pour le rebasage

1. **Rebasez les branches locales** avant de les pousser vers un dépôt partagé
2. **Rebasez régulièrement** vos branches de fonctionnalités sur la branche principale pour éviter les conflits massifs
3. **Utilisez le rebase interactif** pour nettoyer votre historique de commits avant de partager votre travail
4. **N'utilisez jamais rebase sur des branches publiques** (main, develop, etc.) ou toute branche que d'autres personnes utilisent

## Exercice pratique

Voici un exercice pour vous familiariser avec le rebasage :

1. Créez une nouvelle branche depuis main
   ```bash
   git switch -c feature/test-rebase
   ```

2. Ajoutez deux ou trois commits
   ```bash
   # Modifiez des fichiers, puis :
   git add .
   git commit -m "Premier commit sur feature/test-rebase"
   # Répétez pour d'autres commits
   ```

3. Revenez à main et créez un nouveau commit
   ```bash
   git switch main
   # Modifiez des fichiers, puis :
   git add .
   git commit -m "Nouveau commit sur main"
   ```

4. Rebasez votre branche de fonctionnalité
   ```bash
   git switch feature/test-rebase
   git rebase main
   ```

5. Explorez l'historique pour voir la différence
   ```bash
   git log --graph --oneline
   ```

## Résumé

- **Merge** crée un nouveau commit qui combine des branches et préserve l'historique complet
- **Rebase** réécrit l'historique pour créer une ligne de commits plus propre et linéaire
- Le rebasage est idéal pour les branches de fonctionnalités locales
- Évitez de rebaser des branches que vous avez déjà partagées avec d'autres

En maîtrisant à la fois les techniques de fusion et de rebasage, vous pourrez choisir la stratégie la plus adaptée à chaque situation, ce qui vous permettra de maintenir un historique Git propre et compréhensible.

Dans le prochain module, nous explorerons comment travailler avec des dépôts distants et collaborer avec d'autres développeurs.

⏭️ [Module 4 : Git à plusieurs – Git et GitHub](/module-4-git-a-plusieurs-git-et-github/README.md)
