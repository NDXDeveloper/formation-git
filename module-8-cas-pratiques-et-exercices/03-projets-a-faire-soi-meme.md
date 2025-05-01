# 8.3. Projets à faire soi-même

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

Après avoir exploré les scénarios courants et participé aux ateliers guidés, il est temps de mettre en pratique vos compétences Git de manière autonome. Cette section vous propose plusieurs projets personnels de difficulté croissante que vous pouvez réaliser par vous-même pour renforcer votre maîtrise de Git.

## Pourquoi faire des projets personnels ?

La pratique autonome est essentielle pour plusieurs raisons :

- Elle renforce votre **confiance** dans l'utilisation de Git
- Elle vous permet de **personnaliser** votre approche
- Elle vous confronte à des problèmes que vous devez **résoudre par vous-même**
- Elle vous prépare à utiliser Git dans vos **propres projets** futurs

## Projet 1 : Portfolio personnel versionné

### Objectif
Créer et maintenir un site web portfolio personnel en utilisant Git pour suivre toutes les modifications.

### Étapes suggérées

1. **Initialisation**
   - Créez un nouveau dépôt sur GitHub nommé "mon-portfolio"
   - Clonez-le sur votre machine locale
   - Créez une structure de base pour un site web portfolio

2. **Développement par fonctionnalités**
   - Pour chaque section de votre portfolio (accueil, projets, compétences, contact), créez une branche dédiée
   - Développez la section sur sa branche
   - Fusionnez-la dans main une fois terminée

3. **Publication**
   - Utilisez GitHub Pages pour publier votre portfolio
   - Le dépôt servira à la fois de système de versioning et d'hébergement

4. **Maintenance**
   - Créez des branches pour chaque mise à jour future
   - Utilisez des tags pour marquer les versions importantes
   - Maintenez un historique propre avec des messages de commit descriptifs

### Compétences Git exercées
- Gestion de branches
- Commits atomiques
- Publication via Git
- Tagging de versions

## Projet 2 : Journal de bord technique

### Objectif
Créer un journal de notes techniques en Markdown que vous maintiendrez au fil du temps avec Git.

### Étapes suggérées

1. **Mise en place**
   - Créez un dépôt "tech-journal"
   - Organisez-le par dossiers thématiques (ex: "git", "html", "css", "javascript")

2. **Rédaction et versioning**
   - Ajoutez régulièrement des notes sur ce que vous apprenez
   - Committez fréquemment avec des messages descriptifs
   - Utilisez des branches pour les sujets sur lesquels vous travaillez en parallèle

3. **Collaboration (optionnel)**
   - Invitez des amis à contribuer à votre journal
   - Pratiquez les Pull Requests et les revues de code
   - Gérez les conflits de fusion qui pourraient survenir

4. **Raffinement**
   - Revenez sur d'anciennes notes pour les améliorer
   - Utilisez `git blame` pour voir l'historique de chaque ligne
   - Expérimentez avec `git rebase` pour nettoyer l'historique si nécessaire

### Compétences Git exercées
- Suivi de contenu textuel
- Historique de modifications
- Collaboration asynchrone
- Manipulation avancée de l'historique

## Projet 3 : Mini-projet logiciel avec versioning sémantique

### Objectif
Développer une petite application (comme une calculatrice, un gestionnaire de tâches ou un jeu simple) en utilisant un workflow Git professionnel avec versioning sémantique.

### Étapes suggérées

1. **Configuration initiale**
   - Créez un dépôt pour votre application
   - Établissez une structure de projet claire
   - Configurez un fichier .gitignore approprié

2. **Développement avec Git Flow**
   - Initialisez Git Flow dans votre projet
   - Utilisez des branches feature/ pour chaque fonctionnalité
   - Créez des branches release/ pour préparer les versions
   - Utilisez des branches hotfix/ pour les corrections urgentes

3. **Versioning sémantique**
   - Suivez le standard [SemVer](https://semver.org/) (X.Y.Z)
     - X : version majeure (changements incompatibles)
     - Y : version mineure (ajouts de fonctionnalités compatibles)
     - Z : correctifs (corrections de bugs)
   - Taguez chaque version avec des notes détaillées

4. **Documentation**
   - Maintenez un fichier CHANGELOG.md qui liste les changements par version
   - Utilisez le fichier README.md pour documenter l'installation et l'utilisation
   - Assurez-vous que la documentation reste synchronisée avec le code

### Compétences Git exercées
- Workflow Git Flow complet
- Tagging et versioning sémantique
- Gestion de projet via Git
- Intégration de la documentation dans le workflow

## Projet 4 : Contribuer à un projet open source

### Objectif
Faire une contribution, même mineure, à un vrai projet open source sur GitHub.

### Étapes suggérées

1. **Trouver un projet**
   - Recherchez des projets avec le tag "good first issue"
   - Choisissez un projet qui vous intéresse et correspond à vos compétences
   - Lisez le fichier CONTRIBUTING.md s'il existe

2. **Préparer votre environnement**
   - Forkez le dépôt sur votre compte GitHub
   - Clonez votre fork sur votre machine locale
   - Configurez le dépôt original comme remote "upstream"
   ```bash
   git remote add upstream https://github.com/auteur-original/projet.git
   ```

3. **Développer votre contribution**
   - Créez une branche pour votre travail
   - Faites les modifications nécessaires
   - Suivez les conventions de code et de commit du projet

4. **Soumettre votre contribution**
   - Poussez votre branche vers votre fork
   - Créez une Pull Request vers le projet original
   - Répondez aux commentaires des mainteneurs si nécessaire

### Compétences Git exercées
- Workflow de fork
- Synchronisation entre forks
- Pull Requests dans un contexte réel
- Collaboration avec une communauté

## Défi avancé : Recréer un projet existant avec Git

### Objectif
Choisissez un petit projet personnel existant que vous avez développé sans Git, et recréez son historique de développement.

### Étapes suggérées

1. **Analyse préliminaire**
   - Identifiez les grandes étapes du développement de votre projet
   - Planifiez une série de commits qui racontera l'histoire du développement

2. **Création de l'historique**
   - Partez de zéro avec un nouveau dépôt
   - Reconstruisez le projet étape par étape, en faisant des commits significatifs
   - Utilisez `git commit --date="YYYY-MM-DD HH:MM:SS"` pour définir les dates des commits

3. **Simulation de workflows**
   - Créez des branches pour simuler différentes fonctionnalités
   - Fusionnez-les pour reconstituer un historique réaliste
   - Ajoutez des tags pour marquer les versions importantes

### Compétences Git exercées
- Compréhension profonde de l'historique Git
- Manipulation avancée de commits
- Structuration intentionnelle du développement

## Conseils pour tous les projets

1. **Commencez petit** et augmentez progressivement la complexité
2. **Prenez des notes** sur les problèmes rencontrés et les solutions trouvées
3. **Consultez la documentation** Git quand vous êtes bloqué
4. **Expérimentez** sans crainte - c'est comme ça qu'on apprend !
5. **Partagez** vos projets et discutez de votre workflow avec d'autres développeurs

## Journal de progression

Pour chaque projet, tenez un petit journal de votre progression :

1. Quelles commandes Git avez-vous utilisées le plus souvent ?
2. Quels problèmes avez-vous rencontrés et comment les avez-vous résolus ?
3. Comment votre flux de travail a-t-il évolué au cours du projet ?
4. Quelles habitudes avez-vous développées ?

Ce journal vous aidera à identifier vos forces et les domaines où vous pourriez avoir besoin de plus de pratique.

## Ressources complémentaires

Si vous avez besoin d'inspiration pour vos projets :

- [GitHub Explore](https://github.com/explore) - Découvrez des projets intéressants
- [First Contributions](https://github.com/firstcontributions/first-contributions) - Guide pour faire votre première contribution open source
- [App Ideas](https://github.com/florinpop17/app-ideas) - Collection d'idées d'applications à développer

---

Ces projets personnels sont conçus pour vous aider à renforcer et approfondir vos compétences Git de manière autonome. Rappelez-vous que la maîtrise vient avec la pratique régulière. Dans la prochaine section, vous pourrez tester vos connaissances avec un quiz de fin de module.

⏭️ [Quiz de fin de module](/module-8-cas-pratiques-et-exercices/04-quiz-de-fin-de-module.md)
