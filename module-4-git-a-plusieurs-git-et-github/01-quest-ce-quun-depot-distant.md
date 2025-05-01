# 4.1. Qu'est-ce qu'un dépôt distant ?

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Définition simple

Un **dépôt distant** (*remote repository* en anglais) est simplement une copie de votre projet Git qui est hébergée sur Internet ou sur un réseau, plutôt que sur votre ordinateur local. Pensez-y comme à une "sauvegarde en ligne" de votre projet, mais avec beaucoup plus de fonctionnalités !

## Pourquoi utiliser un dépôt distant ?

Les dépôts distants offrent plusieurs avantages essentiels :

1. **Collaboration** : Ils permettent à plusieurs personnes de travailler sur le même projet, chacune depuis son propre ordinateur.

2. **Sauvegarde** : Si votre ordinateur tombe en panne, vous ne perdez pas votre travail puisqu'une copie existe en ligne.

3. **Accessibilité** : Vous pouvez accéder à votre code depuis n'importe quel ordinateur connecté à Internet.

4. **Visibilité** : Les dépôts publics permettent de partager votre travail avec la communauté des développeurs.

## Analogie pour mieux comprendre

Imaginez que vous écrivez un livre avec plusieurs co-auteurs :

- Votre **dépôt local** est comme le manuscrit sur votre bureau personnel.
- Le **dépôt distant** est comme une bibliothèque centrale où tous les co-auteurs peuvent déposer leurs chapitres terminés et récupérer les chapitres des autres.
- Quand vous avez fini d'écrire un chapitre, vous l'envoyez à la bibliothèque centrale (`git push`).
- Quand vous voulez voir les chapitres écrits par vos collègues, vous les récupérez depuis la bibliothèque (`git pull`).

## Les principaux hébergeurs de dépôts distants

Les dépôts distants sont généralement hébergés sur des plateformes spécialisées :

- **GitHub** : Le plus populaire, avec plus de 100 millions de dépôts. C'est celui que nous utiliserons dans ce tutoriel.
- **GitLab** : Une alternative complète qui propose aussi des outils de CI/CD intégrés.
- **Bitbucket** : Particulièrement apprécié pour l'intégration avec d'autres outils Atlassian.
- **Azure DevOps** : La solution de Microsoft pour les entreprises.

## Comment interagir avec un dépôt distant ?

Git propose plusieurs commandes pour communiquer avec les dépôts distants :

- `git clone` : Copier un dépôt distant sur votre ordinateur
- `git push` : Envoyer vos commits locaux vers le dépôt distant
- `git pull` : Récupérer les changements du dépôt distant et les fusionner avec votre travail local
- `git fetch` : Récupérer les changements du dépôt distant sans les fusionner automatiquement

## Représentation visuelle

```
  Votre ordinateur                             Internet
┌────────────────────┐                  ┌────────────────────┐
│                    │                  │                    │
│   Dépôt local      │      push        │   Dépôt distant    │
│   (.git)           │ ─────────────>   │   (GitHub)         │
│                    │                  │                    │
│                    │      pull        │                    │
│                    │ <─────────────   │                    │
└────────────────────┘                  └────────────────────┘
```

## Points importants à retenir

- Un dépôt distant n'est pas simplement un "backup" - c'est un outil de collaboration essentiel.
- Vous pouvez avoir plusieurs dépôts distants liés à un même dépôt local.
- Les dépôts distants peuvent être publics (visibles par tous) ou privés (accessibles uniquement aux personnes autorisées).
- La communication entre votre dépôt local et le dépôt distant se fait via des commandes Git spécifiques.

Dans la prochaine section, nous allons découvrir GitHub, la plateforme la plus populaire pour héberger des dépôts distants, et voir comment y créer votre premier dépôt.

⏭️ [Introduction à GitHub (ou GitLab, Bitbucket)](/module-4-git-a-plusieurs-git-et-github/02-introduction-a-github.md)
