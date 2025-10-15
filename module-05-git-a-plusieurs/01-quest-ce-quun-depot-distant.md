🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 1. Qu'est-ce qu'un dépôt distant ?

### Introduction

Jusqu'à présent, vous avez travaillé avec Git en local sur votre ordinateur. Tous vos commits, branches et historique restaient sur votre machine. Mais l'un des grands avantages de Git est de permettre la **collaboration** : plusieurs développeurs peuvent travailler sur le même projet, partager leur code et synchroniser leurs modifications.

C'est là qu'interviennent les **dépôts distants** (ou *remote repositories* en anglais).

### Définition simple

Un **dépôt distant** est une version de votre projet Git qui est hébergée sur Internet ou sur un réseau. C'est comme une copie de votre dépôt local, mais stockée ailleurs, accessible par vous et d'autres personnes.

Pensez-y comme à un **espace de stockage partagé** pour votre code, similaire à Google Drive ou Dropbox, mais spécialement conçu pour le code avec toutes les fonctionnalités de Git.

### Pourquoi utiliser un dépôt distant ?

Les dépôts distants offrent plusieurs avantages majeurs :

**Collaboration en équipe**
Plusieurs développeurs peuvent travailler sur le même projet simultanément. Chacun clone le dépôt distant sur sa machine, fait ses modifications localement, puis partage son travail avec les autres via le dépôt distant.

**Sauvegarde et sécurité**
Si votre ordinateur tombe en panne ou est volé, votre code n'est pas perdu. Il existe une copie complète sur le serveur distant avec tout l'historique.

**Accès depuis n'importe où**
Vous pouvez accéder à votre projet depuis n'importe quel ordinateur. Vous travaillez sur votre ordinateur portable au bureau, puis continuez le soir depuis votre ordinateur personnel.

**Open Source et partage**
Vous pouvez rendre votre projet public pour que d'autres puissent le consulter, l'utiliser ou même y contribuer.

**Intégration continue et déploiement**
Les dépôts distants peuvent être connectés à des outils automatisés qui testent votre code, créent des versions ou déploient votre application automatiquement.

### Comment ça fonctionne ?

Le principe est simple :

1. **Votre dépôt local** : C'est votre espace de travail personnel sur votre ordinateur
2. **Le dépôt distant** : C'est la version partagée hébergée sur un serveur
3. **Synchronisation** : Vous échangez régulièrement des modifications entre les deux

Voici le cycle typique de travail :

```
Vous récupérez les nouveautés du dépôt distant (git pull)
         ↓
Vous travaillez en local : modifications, commits
         ↓
Vous envoyez vos commits vers le dépôt distant (git push)
         ↓
Vos collègues peuvent alors récupérer vos modifications
```

### Les plateformes d'hébergement

Pour créer un dépôt distant, vous avez besoin d'un serveur. Heureusement, plusieurs plateformes offrent ce service gratuitement :

**GitHub** (le plus populaire)
Propriété de Microsoft, GitHub est la plateforme la plus utilisée au monde. Elle offre des dépôts publics gratuits illimités et des dépôts privés également. C'est la référence pour l'open source.

**GitLab**
Alternative complète à GitHub avec des fonctionnalités avancées de CI/CD intégrées. Vous pouvez également l'installer sur vos propres serveurs.

**Bitbucket**
Propriété d'Atlassian, Bitbucket s'intègre bien avec d'autres outils Atlassian comme Jira. Offre des dépôts privés gratuits pour les petites équipes.

Ces plateformes offrent bien plus qu'un simple hébergement : gestion de projets, suivi de bugs, revue de code, wikis, et bien plus encore.

### Analogie pour bien comprendre

Imaginez que vous écrivez un livre :

- **Votre dépôt local** = le manuscrit sur votre ordinateur personnel
- **Le dépôt distant** = la version stockée dans le cloud partagée avec votre éditeur et co-auteurs
- **git push** = envoyer votre dernier chapitre au cloud
- **git pull** = récupérer les corrections de votre éditeur

Tout le monde travaille sur sa propre copie, mais il existe une version de référence commune dans le cloud que tout le monde peut consulter et mettre à jour.

### Dépôt local vs dépôt distant

| Aspect | Dépôt local | Dépôt distant |
|--------|-------------|---------------|
| Emplacement | Votre ordinateur | Serveur en ligne |
| Accès | Vous uniquement | Vous et autres (selon permissions) |
| Vitesse | Très rapide | Dépend de la connexion Internet |
| Sauvegarde | Non sauvegardé (risque de perte) | Sauvegardé et sécurisé |
| Collaboration | Impossible | Possible |

### Points importants à retenir

Le dépôt distant n'est pas un concept compliqué : c'est simplement une copie de votre dépôt qui vit ailleurs, sur Internet. Vous gardez le contrôle total de votre dépôt local et vous choisissez quand synchroniser avec le distant.

Git permet d'avoir plusieurs dépôts distants pour un même projet local. Par exemple, vous pourriez avoir un dépôt sur GitHub pour partager publiquement et un autre sur le serveur de votre entreprise pour le travail interne.

Même si un dépôt distant est "distant", vous n'avez pas besoin d'être constamment connecté à Internet. Vous pouvez travailler hors ligne et synchroniser plus tard quand vous retrouvez une connexion.

### Dans la suite du module

Maintenant que vous comprenez ce qu'est un dépôt distant, nous allons voir comment :

- Créer et utiliser des dépôts sur GitHub, GitLab ou Bitbucket
- Cloner un dépôt distant existant
- Connecter votre projet local à un dépôt distant
- Envoyer et récupérer des modifications
- Collaborer avec d'autres développeurs

Le travail avec les dépôts distants est au cœur de la collaboration moderne avec Git. Une fois que vous maîtriserez ces concepts, vous pourrez participer à des projets d'équipe et même contribuer à des projets open source !

⏭️ [Introduction à GitHub, GitLab et Bitbucket](/module-05-git-a-plusieurs/02-introduction-plateformes-git.md)
