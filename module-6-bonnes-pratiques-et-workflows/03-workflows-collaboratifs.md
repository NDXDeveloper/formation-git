# 6.3. Workflows collaboratifs (Git Flow, GitHub Flow)

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Qu'est-ce qu'un workflow Git ?

Un workflow Git est une **recommandation sur la façon d'utiliser Git** pour accomplir un travail de manière cohérente et productive. C'est comme une "recette" qui définit comment :

- Organiser vos branches
- Intégrer les changements
- Gérer les versions
- Coordonner le travail en équipe

Les workflows ne sont pas imposés par Git, mais sont des **conventions d'équipe** qui améliorent la collaboration.

## Pourquoi adopter un workflow standardisé ?

Lorsque vous travaillez seul, vous pouvez suivre votre propre méthode. Mais en équipe, un workflow standardisé offre de nombreux avantages :

- **Réduction des conflits** et des erreurs d'intégration
- **Simplicité de collaboration** (tout le monde suit les mêmes règles)
- **Historique plus propre** et plus facile à comprendre
- **Processus prévisibles** pour les déploiements et releases
- **Onboarding plus rapide** des nouveaux membres de l'équipe

Voyons les deux workflows les plus populaires : Git Flow et GitHub Flow.

## Git Flow : le workflow complet

Git Flow est un workflow conçu par Vincent Driessen en 2010. Il définit un modèle de branches strict, particulièrement adapté aux projets avec des cycles de release formels.

### Structure des branches dans Git Flow

Git Flow utilise deux branches principales à durée de vie illimitée :

- **`main`** (ou `master`) : contient uniquement le code de production stable
- **`develop`** : branche d'intégration pour les fonctionnalités en développement

Et trois types de branches temporaires :

- **`feature/`** : pour développer de nouvelles fonctionnalités
- **`release/`** : pour préparer une nouvelle version
- **`hotfix/`** : pour corriger rapidement des bugs critiques en production

### Fonctionnement de Git Flow

![Schéma Git Flow](https://nvie.com/img/git-model@2x.png)

1. **Développement de fonctionnalités** :
   ```bash
   # Création d'une branche de fonctionnalité à partir de develop
   git checkout develop
   git checkout -b feature/ma-nouvelle-fonctionnalite

   # Après développement, fusion dans develop
   git checkout develop
   git merge --no-ff feature/ma-nouvelle-fonctionnalite
   git branch -d feature/ma-nouvelle-fonctionnalite
   ```

2. **Préparation d'une release** :
   ```bash
   # Création d'une branche de release
   git checkout develop
   git checkout -b release/1.0.0

   # Corrections de bugs mineurs dans cette branche
   # ...

   # Fusion dans main ET develop
   git checkout main
   git merge --no-ff release/1.0.0
   git tag -a v1.0.0

   git checkout develop
   git merge --no-ff release/1.0.0

   git branch -d release/1.0.0
   ```

3. **Hotfix (correction urgente)** :
   ```bash
   # Création d'une branche hotfix depuis main
   git checkout main
   git checkout -b hotfix/1.0.1

   # Correction du bug
   # ...

   # Fusion dans main ET develop
   git checkout main
   git merge --no-ff hotfix/1.0.1
   git tag -a v1.0.1

   git checkout develop
   git merge --no-ff hotfix/1.0.1

   git branch -d hotfix/1.0.1
   ```

### Avantages de Git Flow

- **Structure claire** et bien définie
- **Adapté aux versions multiples** en production
- **Séparation nette** entre développement et production
- **Processus de release** bien défini

### Inconvénients de Git Flow

- **Complexe** pour les petits projets ou les équipes réduites
- **Nombreuses branches** à gérer
- **Moins adapté** au déploiement continu
- **Courbe d'apprentissage** plus élevée

### Outils pour Git Flow

Des outils comme l'extension [git-flow](https://github.com/nvie/gitflow) ou des plugins pour les IDE peuvent automatiser ces opérations :

```bash
# Installation
# Sur macOS
brew install git-flow

# Sur Windows avec Git for Windows
git flow init
```

## GitHub Flow : le workflow simplifié

GitHub Flow est une alternative plus légère proposée par GitHub. Il s'agit d'un workflow beaucoup plus simple, adapté aux équipes pratiquant l'intégration continue et le déploiement continu.

### Structure des branches dans GitHub Flow

GitHub Flow n'utilise qu'une seule branche principale :

- **`main`** (ou `master`) : contient le code déployable en production

### Fonctionnement de GitHub Flow

![Schéma GitHub Flow](https://guides.github.com/activities/hello-world/branching.png)

1. **Créer une branche** à partir de `main` :
   ```bash
   git checkout main
   git checkout -b ma-fonctionnalite
   ```

2. **Développer** et faire des commits réguliers :
   ```bash
   git add .
   git commit -m "feat: ajouter ma fonctionnalité"
   ```

3. **Pousser la branche** vers le dépôt distant :
   ```bash
   git push -u origin ma-fonctionnalite
   ```

4. **Ouvrir une Pull Request** sur GitHub pour discussion

5. **Revue de code, tests** et discussions sur la Pull Request

6. **Fusionner** dans `main` après validation :
   ```bash
   git checkout main
   git merge --no-ff ma-fonctionnalite
   git push origin main
   ```

7. **Déployer** en production (souvent automatiquement)

### Avantages de GitHub Flow

- **Simplicité** : facile à comprendre et à mettre en œuvre
- **Rapide** : idéal pour des itérations courtes
- **Adapté au déploiement continu**
- **Moins de branches** à gérer
- **Intégration parfaite** avec GitHub

### Inconvénients de GitHub Flow

- **Moins structuré** pour les projets avec releases formelles
- **Gestion des versions** moins évidente
- **Peut être problématique** pour les projets maintenus en parallèle
- **Dépend des Pull Requests** (fonctionne mieux avec GitHub)

## Quel workflow choisir ?

Le choix du workflow dépend de plusieurs facteurs :

### Choisissez Git Flow si :

- Votre projet a des **cycles de release formels**
- Vous maintenez **plusieurs versions** en parallèle
- Vous avez un **processus de validation** rigoureux
- Votre équipe est **assez grande** et structurée

### Choisissez GitHub Flow si :

- Vous pratiquez l'**intégration continue**
- Vous déployez **fréquemment** (parfois plusieurs fois par jour)
- Votre équipe est **petite à moyenne**
- Vous utilisez **GitHub** comme plateforme

### Autres workflows populaires

- **GitLab Flow** : un compromis entre Git Flow et GitHub Flow
- **Trunk-Based Development** : tout le monde travaille sur une branche principale
- **OneFlow** : une alternative à Git Flow avec une structure simplifiée

## Mise en place dans votre équipe

Pour adopter un workflow dans votre équipe :

1. **Choisissez** le workflow qui correspond à vos besoins
2. **Documentez-le** clairement pour votre équipe
3. **Formez** les membres de l'équipe
4. **Automatisez** autant que possible avec des hooks ou des outils CI/CD
5. **Soyez flexible** et adaptez-le au fil du temps

## Exercice pratique

Essayez de simuler un petit projet avec un collègue ou seul en suivant ces étapes :

1. Pour Git Flow :
   - Initialisez un dépôt avec la structure Git Flow
   - Créez une fonctionnalité, préparez une release
   - Simulez un hotfix

2. Pour GitHub Flow :
   - Créez un dépôt sur GitHub
   - Créez une branche pour une fonctionnalité
   - Faites une Pull Request et fusionnez-la

Cela vous aidera à comprendre concrètement comment fonctionnent ces workflows.

## Conclusion

Les workflows Git sont comme des recettes de cuisine : il n'y a pas de solution universelle, mais des approches adaptées à différents contextes.

Quelle que soit votre choix, l'important est que votre équipe :
1. **Comprenne** le workflow choisi
2. **L'applique** de manière cohérente
3. **L'adapte** à ses besoins spécifiques

N'hésitez pas à commencer simple, puis à évoluer vers plus de complexité au fur et à mesure que votre projet et votre équipe grandissent !

⏭️ [Revue de code avec Pull Requests](/module-6-bonnes-pratiques-et-workflows/04-revue-de-code-avec-pr.md)
