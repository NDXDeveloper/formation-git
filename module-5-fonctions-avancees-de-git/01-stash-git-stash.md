# 5.1. Stash : mettre de côté temporairement (git stash)

## Le problème : interrompre son travail

Imaginez cette situation : vous travaillez sur une nouvelle fonctionnalité et votre code est en plein chantier. Soudain, on vous demande de corriger un bug urgent sur la branche principale. Que faire de vos modifications non terminées ? Vous ne voulez pas les committer car elles ne sont pas prêtes, mais vous devez changer de branche pour résoudre le bug.

C'est exactement pour ce genre de situation que Git a créé la commande `git stash` !

## Qu'est-ce que git stash ?

`git stash` est comme une "pochette secrète" temporaire où vous pouvez ranger vos modifications en cours sans avoir à créer un commit. Cela vous permet de :

- Mettre de côté votre travail actuel
- Revenir à un état propre (HEAD)
- Travailler sur autre chose
- Récupérer vos modifications plus tard

C'est un peu comme si vous mettiez votre travail dans un tiroir pour garder votre bureau propre pendant que vous travaillez sur une tâche urgente.

## Commandes de base du stash

### Mettre de côté vos modifications

Pour stocker vos modifications actuelles :

```bash
git stash
```

ou avec un message descriptif (recommandé) :

```bash
git stash save "Description de ce que je suis en train de faire"
```

Cette commande :
1. Prend toutes vos modifications non committées
2. Les enregistre temporairement
3. Remet votre répertoire de travail dans l'état du dernier commit

Votre terminal affichera quelque chose comme :
```
Saved working directory and index state "Description de ce que je suis en train de faire"
```

### Voir la liste des stashs

Pour voir tous les stashs que vous avez créés :

```bash
git stash list
```

Exemple de résultat :
```
stash@{0}: On feature-login: Description de ce que je suis en train de faire
stash@{1}: On feature-login: Autre modification sauvegardée précédemment
```

### Récupérer vos modifications stashées

Une fois que vous avez terminé de travailler sur le bug urgent, vous pouvez récupérer vos modifications mises de côté :

```bash
git stash apply
```

Cette commande applique le stash le plus récent (`stash@{0}`) sans le supprimer de la liste des stashs.

Pour appliquer un stash spécifique, indiquez son identifiant :

```bash
git stash apply stash@{1}
```

### Récupérer et supprimer le stash

Si vous voulez récupérer vos modifications et supprimer le stash en même temps :

```bash
git stash pop
```

C'est comme `apply` + suppression du stash.

### Supprimer un stash

Pour supprimer un stash sans l'appliquer :

```bash
git stash drop stash@{1}
```

Pour supprimer tous les stashs :

```bash
git stash clear
```

## Cas pratiques d'utilisation

### Scénario 1 : Interruption urgente

1. Vous travaillez sur la fonctionnalité A
2. Un bug urgent est signalé
3. Vous exécutez `git stash save "Fonctionnalité A en cours"`
4. Vous basculez sur la branche main : `git checkout main`
5. Vous créez une branche pour le correctif : `git checkout -b hotfix`
6. Vous corrigez le bug, committez et fusionnez le correctif
7. Vous revenez à votre branche : `git checkout feature-A`
8. Vous récupérez votre travail : `git stash pop`

### Scénario 2 : Nettoyer temporairement votre espace de travail

Parfois, vous voulez simplement vérifier à quoi ressemble votre code dans son état propre, sans vos modifications en cours :

1. `git stash`
2. Examinez l'état du code
3. `git stash pop` pour récupérer vos modifications

### Scénario 3 : Déplacer des modifications vers une autre branche

Vous avez commencé à travailler mais vous êtes sur la mauvaise branche :

1. `git stash`
2. `git checkout correct-branch`
3. `git stash pop`

## Options utiles

### Stasher seulement certains fichiers

```bash
git stash push -m "Message" chemin/vers/fichier1.txt chemin/vers/fichier2.txt
```

### Inclure les fichiers non suivis (untracked)

Par défaut, `git stash` ignore les fichiers non suivis (que vous n'avez jamais ajoutés avec `git add`).

Pour les inclure :

```bash
git stash -u
```

ou

```bash
git stash --include-untracked
```

### Voir le contenu d'un stash

Pour voir ce qui se trouve dans un stash particulier :

```bash
git stash show stash@{0}
```

Pour voir les différences ligne par ligne :

```bash
git stash show -p stash@{0}
```

## Points importants à retenir

1. Les stashs sont locaux et ne sont pas transférés lorsque vous poussez vers un dépôt distant
2. Les stashs peuvent être appliqués sur n'importe quelle branche, pas seulement celle où ils ont été créés
3. Un stash peut contenir des conflits lors de son application, tout comme un merge
4. Les stashs ne sont pas destinés au stockage à long terme de code - utilisez des branches pour cela

## En résumé

`git stash` est un outil formidable pour mettre temporairement de côté votre travail sans créer de commits inachevés. C'est parfait pour les interruptions dans votre flux de travail, pour garder votre historique de commits propre, et pour déplacer des modifications entre les branches.

Maintenant que vous maîtrisez `git stash`, vous pouvez travailler de manière plus flexible et réagir rapidement aux changements de priorités sans perdre votre travail en cours !
