# 4.4. Pousser et tirer des changements (git push, git pull, git fetch)

Maintenant que vous savez cloner un dépôt, voyons comment synchroniser votre travail avec le dépôt distant. Cette synchronisation se fait dans deux directions :
- **Vers le dépôt distant** : pousser vos modifications
- **Depuis le dépôt distant** : récupérer les modifications des autres

## L'analogie de la bibliothèque (suite)

Reprenons notre analogie de la bibliothèque :
- **git push** : C'est comme rapporter un livre à la bibliothèque après y avoir ajouté vos notes et améliorations.
- **git pull** : C'est comme emprunter à nouveau un livre pour voir les notes que d'autres lecteurs y ont ajoutées.
- **git fetch** : C'est comme consulter le catalogue de la bibliothèque pour voir les mises à jour, sans emprunter le livre.

## Git Push : Partager votre travail

### Qu'est-ce que git push ?

La commande `git push` envoie vos commits locaux vers le dépôt distant. C'est ainsi que vous partagez votre travail avec les autres membres de l'équipe.

### Étapes pour pousser vos modifications

1. **Faire des modifications** dans votre code
2. **Ajouter** ces modifications à la zone de staging avec `git add`
3. **Commiter** ces modifications avec `git commit`
4. **Pousser** ces commits vers GitHub avec `git push`

### Syntaxe de base

```bash
git push <nom-distant> <nom-branche>
```

- `<nom-distant>` est généralement `origin` (le nom par défaut du dépôt d'où vous avez cloné)
- `<nom-branche>` est le nom de la branche que vous voulez pousser (ex: `main` ou `master`)

### Exemple simple

```bash
# Après avoir fait vos modifications, ajouts et commits
git push origin main
```

### Premier push d'une nouvelle branche

Si vous avez créé une nouvelle branche localement, pour la pousser la première fois :

```bash
git push -u origin ma-nouvelle-branche
```

L'option `-u` (ou `--set-upstream`) établit une relation de suivi entre votre branche locale et la branche distante. Après cela, vous pourrez simplement utiliser `git push` sans arguments.

## Git Pull : Récupérer et fusionner les changements

### Qu'est-ce que git pull ?

La commande `git pull` récupère les dernières modifications du dépôt distant et les fusionne automatiquement dans votre branche locale. C'est comme faire un `git fetch` suivi d'un `git merge` en une seule commande.

### Quand utiliser git pull ?

- Avant de commencer à travailler chaque jour
- Avant de pousser vos propres modifications
- Quand vous savez que d'autres ont fait des changements

### Syntaxe de base

```bash
git pull <nom-distant> <nom-branche>
```

### Exemple simple

```bash
# Pour récupérer et fusionner les derniers changements de la branche main
git pull origin main
```

### Gestion des conflits lors d'un pull

Parfois, `git pull` peut générer des conflits si les modifications distantes entrent en conflit avec vos modifications locales. Git vous demandera de résoudre ces conflits manuellement avant de terminer l'opération.

Nous verrons la résolution de conflits plus en détail dans une autre section, mais voici les bases :
1. Git marquera les zones de conflit dans vos fichiers
2. Vous devrez éditer ces fichiers pour choisir quelles modifications conserver
3. Puis faire un `git add` des fichiers modifiés
4. Et enfin `git commit` pour finaliser la fusion

## Git Fetch : Récupérer sans fusionner

### Qu'est-ce que git fetch ?

La commande `git fetch` récupère toutes les modifications du dépôt distant, mais ne les fusionne pas automatiquement dans votre travail local. C'est une façon plus prudente de voir ce qui a changé avant de décider quoi faire.

### Pourquoi utiliser git fetch au lieu de git pull ?

- Pour vérifier les changements distants sans affecter votre travail local
- Pour examiner les modifications avant de les fusionner
- Pour mettre à jour plusieurs branches distantes en une seule fois

### Syntaxe de base

```bash
git fetch <nom-distant>
```

### Exemple simple

```bash
# Récupérer tous les changements de 'origin' sans les fusionner
git fetch origin
```

Après un `git fetch`, vous pouvez :
- Examiner les changements avec `git log origin/main`
- Fusionner manuellement avec `git merge origin/main`
- Ou utiliser `git checkout` pour explorer les changements

## Comparaison : Pull vs Fetch

| Commande | Action | Utilisation |
|---------|--------|------------|
| `git pull` | Récupère ET fusionne | Quand vous voulez rapidement mettre à jour votre code |
| `git fetch` | Récupère SANS fusionner | Quand vous voulez examiner les changements avant de les intégrer |

## Workflow typique en équipe

Voici un workflow quotidien typique lorsque vous travaillez en équipe :

1. **Commencez la journée** avec un `git pull` pour obtenir les derniers changements
2. **Travaillez** sur vos tâches, en faisant des commits réguliers
3. **Avant de pousser**, faites un nouveau `git pull` pour intégrer les changements récents
4. **Résolvez les conflits** s'il y en a
5. **Poussez vos changements** avec `git push`

## Conseils pratiques

### Toujours pull avant de push

Pour éviter les conflits, prenez l'habitude de toujours faire un `git pull` avant de faire un `git push`. Cela vous permet d'intégrer les changements des autres à votre travail local avant de partager le vôtre.

### Utilisez des branches pour les fonctionnalités

Créez des branches séparées pour chaque fonctionnalité ou correction. Cela rend la collaboration plus facile et plus sûre.

```bash
# Créer et changer de branche
git checkout -b nouvelle-fonctionnalite

# Travailler, commiter...

# Pousser la nouvelle branche
git push -u origin nouvelle-fonctionnalite
```

### Vérifiez votre statut avant chaque opération

Utilisez `git status` pour voir où vous en êtes avant de faire un push ou un pull :

```bash
git status
```

## Erreurs courantes et solutions

### "Failed to push some refs"

Cette erreur survient souvent quand quelqu'un d'autre a poussé des changements que vous n'avez pas encore intégrés.

**Solution** : Faites d'abord un `git pull`, puis réessayez votre `git push`.

### "Merge conflict"

Des conflits apparaissent quand Git ne peut pas automatiquement fusionner des modifications.

**Solution** : Éditez les fichiers marqués comme conflictuels, puis :
```bash
git add <fichiers-résolus>
git commit -m "Résolution des conflits"
git push
```

### "Your branch is ahead of 'origin/main' by X commits"

Cela signifie que vous avez des commits locaux que vous n'avez pas encore poussés.

**Solution** : Utilisez `git push` pour partager ces commits, si vous êtes prêt.

## Résumé

- **git push** : Envoie vos commits vers le dépôt distant
- **git pull** : Récupère et fusionne les changements distants en une étape
- **git fetch** : Récupère les changements distants sans fusionner automatiquement

Maîtriser ces trois commandes est essentiel pour collaborer efficacement avec d'autres développeurs. Elles constituent le cœur de la synchronisation entre votre travail et celui de vos collègues.

---

Dans la prochaine section, nous verrons comment configurer et utiliser les clés SSH pour une connexion plus sécurisée et plus pratique avec GitHub.
