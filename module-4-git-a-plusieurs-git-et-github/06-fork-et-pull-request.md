# 4.6. Fork et Pull Request (PR)

## Introduction aux contributions sur GitHub

Jusqu'à présent, nous avons vu comment travailler avec vos propres dépôts. Mais comment contribuer à des projets qui ne vous appartiennent pas ? C'est là qu'interviennent les concepts de **Fork** et de **Pull Request** (PR).

## Qu'est-ce qu'un Fork ?

Un **Fork** est votre propre copie personnelle d'un dépôt appartenant à quelqu'un d'autre. C'est comme si vous preniez une photocopie d'un livre pour pouvoir écrire vos propres annotations sans modifier l'original.

Avec un fork, vous pouvez :
- Expérimenter librement sans affecter le projet original
- Proposer des améliorations au projet original
- Utiliser le code d'un autre projet comme point de départ pour le vôtre

## Qu'est-ce qu'une Pull Request (PR) ?

Une **Pull Request** (ou demande de tirage) est une façon formelle de proposer vos modifications au projet original. C'est comme dire : "Voici les changements que j'ai faits dans ma copie, aimeriez-vous les intégrer dans votre version ?"

## L'analogie du restaurant

Imaginez un restaurant avec sa recette secrète :
- Le **dépôt original** est le livre de recettes du chef
- **Forker** le dépôt, c'est comme prendre une copie de la recette pour l'essayer chez vous
- **Modifier** le code, c'est comme ajouter votre touche personnelle à la recette
- Créer une **Pull Request**, c'est comme suggérer vos améliorations au chef
- Le chef peut alors **accepter** vos suggestions, les **refuser**, ou vous demander de les **modifier**

## Workflow complet : Fork et Pull Request

Voyons maintenant le processus complet pour contribuer à un projet open source :

### Étape 1 : Forker le dépôt

1. Rendez-vous sur la page GitHub du projet auquel vous souhaitez contribuer
2. Cliquez sur le bouton **Fork** en haut à droite de la page
3. GitHub va créer une copie du dépôt sur votre compte

![Bouton Fork sur GitHub](https://i.imgur.com/EXEMPLE_FORK.png)

### Étape 2 : Cloner votre fork

Une fois le fork créé, clonez-le sur votre ordinateur :

```bash
git clone git@github.com:VOTRE-PSEUDO/nom-du-projet.git
cd nom-du-projet
```

> **Important** : Vous clonez VOTRE fork, pas le dépôt original !

### Étape 3 : Configurer le remote "upstream"

Pour pouvoir synchroniser votre fork avec le dépôt original (appelé "upstream" par convention), ajoutez-le comme remote :

```bash
git remote add upstream git@github.com:PROPRIETAIRE-ORIGINAL/nom-du-projet.git
```

Vous aurez ainsi deux remotes :
- `origin` : votre fork (où vous pouvez pousser)
- `upstream` : le dépôt original (où vous ne pouvez pas pousser directement)

### Étape 4 : Créer une branche pour votre contribution

Créez toujours une nouvelle branche pour chaque contribution :

```bash
git checkout -b ma-nouvelle-fonctionnalite
```

Choisissez un nom descriptif qui reflète ce que vous êtes en train de faire.

### Étape 5 : Faire vos modifications

Travaillez normalement sur votre code, en faisant des commits réguliers :

```bash
# Modifiez des fichiers...
git add fichier-modifie.js
git commit -m "Ajout de la fonctionnalité X qui résout le problème Y"
```

### Étape 6 : Synchroniser avec le dépôt original (facultatif mais recommandé)

Avant de soumettre votre contribution, assurez-vous d'être à jour avec le dépôt original :

```bash
# Récupérer les derniers changements du dépôt original
git fetch upstream

# Fusionner ces changements dans votre branche
git merge upstream/main
```

Si des conflits apparaissent, résolvez-les avant de continuer.

### Étape 7 : Pousser votre branche vers votre fork

```bash
git push origin ma-nouvelle-fonctionnalite
```

### Étape 8 : Créer la Pull Request

1. Allez sur votre fork sur GitHub
2. Vous verrez un bandeau suggérant de créer une Pull Request avec votre nouvelle branche
3. Cliquez sur le bouton **Compare & pull request**

![Bouton Compare & pull request](https://i.imgur.com/EXEMPLE_PR.png)

### Étape 9 : Remplir le formulaire de Pull Request

1. Donnez un **titre clair** à votre PR
2. Rédigez une **description détaillée** :
   - Ce que fait votre contribution
   - Pourquoi c'est utile
   - Comment l'avez-vous testée
   - Tout détail pertinent pour les reviewers
3. Cliquez sur **Create pull request**

## Le cycle de vie d'une Pull Request

Une fois votre PR créée, voici ce qui peut se passer :

### 1. Revue de code (Code Review)

Les mainteneurs du projet vont examiner votre code et peuvent :
- Laisser des commentaires avec des questions ou suggestions
- Approuver vos changements
- Demander des modifications

### 2. Discussion et modifications

Si des modifications sont demandées :
1. Retournez sur votre branche locale
2. Faites les changements nécessaires
3. Committez et poussez à nouveau
4. La PR se mettra à jour automatiquement

```bash
# Faire des modifications suite aux commentaires
git add fichier-modifie.js
git commit -m "Correction suite aux commentaires dans la PR"
git push origin ma-nouvelle-fonctionnalite
```

### 3. Tests automatisés

De nombreux projets ont des tests automatisés qui s'exécutent sur votre PR. Assurez-vous qu'ils passent tous.

### 4. Fusion (Merge)

Si votre contribution est acceptée, un mainteneur va la fusionner dans le projet principal. Félicitations !

## Bonnes pratiques pour les Pull Requests

### Avant de soumettre

- **Lisez les guidelines** du projet (souvent dans les fichiers CONTRIBUTING.md ou README.md)
- **Recherchez** si quelqu'un d'autre a déjà proposé la même chose
- **Testez** bien votre code
- **Faites des commits logiques** avec des messages clairs

### Dans votre Pull Request

- **Un seul changement** par PR : ne mélangez pas plusieurs fonctionnalités non liées
- **Soyez précis** dans la description
- **Répondez rapidement** aux commentaires des reviewers
- **Soyez respectueux** et ouvert aux suggestions

## Cas particuliers et conseils

### Mettre à jour une PR avec de nouveaux changements du dépôt original

Si le dépôt original a beaucoup évolué pendant que vous travailliez sur votre PR :

```bash
git fetch upstream
git rebase upstream/main
git push -f origin ma-nouvelle-fonctionnalite
```

> **Note** : Le `-f` (force) est nécessaire après un rebase. Utilisez-le avec précaution !

### Fermer une PR

Si vous décidez d'abandonner votre contribution, vous pouvez simplement fermer la PR sur l'interface GitHub.

### Contribuer à nouveau après une première PR

Pour faire une nouvelle contribution après une PR acceptée :

1. Synchronisez votre fork avec le dépôt original
2. Créez une nouvelle branche
3. Recommencez le processus

## Résumé

- **Fork** : Votre copie personnelle d'un dépôt GitHub
- **Pull Request** : Proposition formelle de modification au projet original
- Le workflow complet implique plusieurs étapes : fork, clone, branche, modification, push, PR
- Restez réceptif aux retours pendant le processus de revue

---

Félicitations ! Vous connaissez maintenant les bases de la collaboration sur GitHub. Ces compétences sont essentielles pour participer à des projets open source ou travailler en équipe sur des projets de développement.

Dans le prochain module, nous explorerons des fonctions plus avancées de Git qui vous aideront à travailler encore plus efficacement.
