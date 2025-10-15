🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 2. Introduction à GitHub, GitLab et Bitbucket

### Vue d'ensemble

Maintenant que vous savez ce qu'est un dépôt distant, il est temps de choisir où héberger vos projets. Les trois principales plateformes sont **GitHub**, **GitLab** et **Bitbucket**. Toutes les trois offrent des services similaires, mais chacune a ses particularités.

La bonne nouvelle ? Les concepts Git que vous apprenez fonctionnent de la même manière sur les trois plateformes. Une fois que vous maîtrisez l'une, passer à une autre est très facile.

### GitHub – La plateforme la plus populaire

**Qu'est-ce que GitHub ?**

GitHub est la plateforme d'hébergement de code la plus utilisée au monde. Lancée en 2008 et acquise par Microsoft en 2018, elle héberge plus de 100 millions de dépôts et compte des dizaines de millions de développeurs.

**Points forts de GitHub**

**Communauté massive**
GitHub est le réseau social des développeurs. La plupart des projets open source sont hébergés ici. C'est l'endroit idéal pour découvrir du code, apprendre et contribuer.

**Visibilité**
Un profil GitHub actif est devenu un véritable portfolio pour les développeurs. Les recruteurs consultent régulièrement les profils GitHub pour évaluer les candidats.

**GitHub Pages**
Vous pouvez héberger gratuitement des sites web statiques directement depuis vos dépôts. Parfait pour des documentations, blogs ou portfolios.

**GitHub Actions**
Système d'intégration continue (CI/CD) intégré qui permet d'automatiser vos workflows : tests automatiques, déploiements, etc.

**Intégrations nombreuses**
Des milliers d'applications tierces s'intègrent avec GitHub : outils de gestion de projet, services de déploiement, analyses de code, etc.

**Offres disponibles**

- **Gratuit** : dépôts publics et privés illimités, GitHub Actions avec des limites généreuses
- **GitHub Pro** : fonctionnalités avancées pour les développeurs individuels
- **GitHub Team & Enterprise** : pour les organisations avec besoins avancés

**Idéal pour**

Les projets open source, les développeurs souhaitant se constituer un portfolio public, les équipes de toutes tailles, les débutants (énorme quantité de ressources d'apprentissage).

### GitLab – La plateforme tout-en-un

**Qu'est-ce que GitLab ?**

GitLab est une plateforme complète lancée en 2011. Elle se distingue par son approche "DevOps complet" où tout le cycle de développement peut être géré au même endroit.

**Points forts de GitLab**

**CI/CD puissant et intégré**
GitLab offre l'un des meilleurs systèmes d'intégration continue du marché, inclus gratuitement dès le départ. Les pipelines CI/CD sont très flexibles et puissants.

**Auto-hébergement possible**
GitLab propose une version Community Edition que vous pouvez installer sur vos propres serveurs. Parfait pour les entreprises ayant des contraintes de sécurité strictes.

**Gestion de projet intégrée**
Boards Kanban, jalons, épopées, gestion du temps... GitLab offre des outils de gestion de projet très complets nativement intégrés.

**Wikis et documentation**
Chaque projet dispose d'un wiki intégré pour documenter votre code facilement.

**Registre de conteneurs**
GitLab inclut un registre Docker intégré pour stocker vos images de conteneurs.

**Offres disponibles**

- **Free** : fonctionnalités essentielles avec CI/CD inclus
- **Premium** : fonctionnalités avancées pour les équipes
- **Ultimate** : ensemble complet pour les grandes organisations
- **Self-hosted** : versions Community et Enterprise à installer soi-même

**Idéal pour**

Les équipes qui veulent une solution complète DevOps, les organisations nécessitant un hébergement privé, les projets avec des besoins CI/CD importants, les entreprises voulant tout centraliser.

### Bitbucket – L'outil des équipes Atlassian

**Qu'est-ce que Bitbucket ?**

Bitbucket est la plateforme d'hébergement Git d'Atlassian, créée en 2008. Elle s'intègre parfaitement avec l'écosystème Atlassian (Jira, Confluence, Trello).

**Points forts de Bitbucket**

**Intégration Atlassian**
Si votre équipe utilise déjà Jira pour la gestion de projet ou Confluence pour la documentation, Bitbucket s'intègre parfaitement. Vous pouvez lier des commits à des tickets Jira automatiquement.

**Bitbucket Pipelines**
Système CI/CD intégré, simple à configurer et efficace pour les besoins standards.

**Branches et permissions avancées**
Contrôle très fin des permissions par branche, idéal pour les processus de révision de code stricts.

**Gratuit pour petites équipes**
L'offre gratuite est généreuse pour les petites équipes (jusqu'à 5 utilisateurs).

**Support de Mercurial (historique)**
Bitbucket supportait à l'origine Mercurial en plus de Git, bien que ce support ait été arrêté en 2020.

**Offres disponibles**

- **Free** : gratuit jusqu'à 5 utilisateurs
- **Standard** : pour les équipes grandissantes
- **Premium** : fonctionnalités entreprise complètes

**Idéal pour**

Les équipes utilisant déjà l'écosystème Atlassian, les petites équipes (l'offre gratuite est très généreuse), les entreprises avec des besoins de permissions complexes.

### Tableau comparatif

| Critère | GitHub | GitLab | Bitbucket |
|---------|--------|--------|-----------|
| **Popularité** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Communauté open source** | Énorme | Grande | Moyenne |
| **CI/CD intégré** | Oui (Actions) | Oui (excellent) | Oui (Pipelines) |
| **Offre gratuite** | Très bonne | Très bonne | Bonne (5 users) |
| **Auto-hébergement** | Non (sauf Enterprise) | Oui | Non |
| **Interface utilisateur** | Moderne, simple | Complète, dense | Claire, fonctionnelle |
| **Documentation** | Excellente | Excellente | Très bonne |
| **Intégration Jira** | Via apps | Native | Native |
| **Pages web statiques** | Oui (Pages) | Oui (Pages) | Non |
| **Registre conteneurs** | Oui | Oui | Non intégré |

### Quelle plateforme choisir ?

**Choisissez GitHub si...**

- Vous débutez et voulez rejoindre la plus grande communauté
- Vous travaillez sur un projet open source
- Vous voulez maximiser la visibilité de votre code
- Vous cherchez à construire un portfolio public
- Vous voulez accéder à une énorme quantité de projets existants

**Choisissez GitLab si...**

- Vous avez besoin d'outils DevOps complets et intégrés
- Vous voulez une solution CI/CD puissante gratuitement
- Vous devez héberger votre propre instance pour des raisons de sécurité
- Vous préférez une plateforme tout-en-un
- Vous aimez avoir de nombreuses fonctionnalités disponibles

**Choisissez Bitbucket si...**

- Votre équipe utilise déjà Jira, Confluence ou d'autres outils Atlassian
- Vous êtes une petite équipe (moins de 5 personnes)
- Vous avez besoin de contrôles de permissions avancés
- Vous voulez une intégration transparente avec l'écosystème Atlassian

### Créer un compte

Pour les trois plateformes, la création de compte est simple et gratuite :

**GitHub**
Rendez-vous sur [github.com](https://github.com), cliquez sur "Sign up" et suivez les instructions. Vous aurez besoin d'une adresse email et devrez choisir un nom d'utilisateur unique.

**GitLab**
Allez sur [gitlab.com](https://gitlab.com), cliquez sur "Register" ou "Sign up". Vous pouvez vous inscrire avec votre email ou utiliser un compte Google, GitHub ou autres.

**Bitbucket**
Visitez [bitbucket.org](https://bitbucket.org), cliquez sur "Get it free" et créez votre compte Atlassian. Ce compte vous donnera accès à tous les produits Atlassian.

### Interface de base

Bien que chaque plateforme ait sa propre interface, vous retrouverez des éléments communs :

**Page d'accueil / Dashboard**
Vue d'ensemble de vos projets, activité récente, notifications.

**Dépôts (Repositories)**
Liste de tous vos projets Git. Vous pouvez créer, supprimer, rechercher des dépôts.

**Profil**
Votre page personnelle montrant vos projets, contributions, activité.

**Paramètres**
Configuration de votre compte, clés SSH, tokens d'accès, intégrations.

**Explorateur / Discover**
Découvrir des projets publics, tendances, sujets populaires.

### Points communs entre les plateformes

Malgré leurs différences, ces trois plateformes partagent les fonctionnalités essentielles :

- Hébergement illimité de dépôts Git
- Gestion des permissions et collaborateurs
- Pull Requests / Merge Requests pour la revue de code
- Issues pour le suivi des bugs et fonctionnalités
- Wikis pour la documentation
- Intégration continue (CI/CD)
- Webhooks pour l'automatisation
- API pour l'intégration avec d'autres outils

### Peut-on utiliser plusieurs plateformes ?

Absolument ! Vous pouvez parfaitement :

- Avoir un compte sur les trois plateformes
- Héberger différents projets sur différentes plateformes
- Même avoir plusieurs dépôts distants pour un seul projet local

Par exemple, vous pourriez avoir votre projet principal sur GitHub pour la visibilité, tout en ayant une copie miroir sur GitLab pour utiliser leur CI/CD.

### Conseils pour débuter

**Commencez avec GitHub**
Si vous hésitez, commencez par GitHub. C'est la plateforme la plus documentée et la plus utilisée. Vous trouverez d'innombrables tutoriels et réponses à vos questions.

**Explorez les projets existants**
Avant de créer vos propres dépôts, explorez des projets populaires. Observez comment ils sont organisés, comment les équipes collaborent, comment sont écrits les messages de commit.

**Configurez votre profil**
Ajoutez une photo, une bio, vos liens. Un profil soigné rend votre présence plus professionnelle.

**Activez l'authentification à deux facteurs**
Pour la sécurité de votre compte, activez la 2FA dès le début.

### Dans la suite

Maintenant que vous connaissez les principales plateformes, nous allons voir concrètement comment :

- Cloner un dépôt existant depuis ces plateformes
- Créer votre premier dépôt distant
- Connecter votre projet local à un dépôt distant
- Publier votre code en ligne

Le choix de la plateforme est important, mais ne vous bloquez pas trop : vous pourrez toujours migrer votre code d'une plateforme à une autre si nécessaire. L'important est de commencer !

⏭️ [Cloner un dépôt (git clone)](/module-05-git-a-plusieurs/03-cloner-un-depot.md)
