# 7.1. Utilisation de Git avec des GUI (Sourcetree, GitKraken, VSCode, etc.)

Les interfaces graphiques (GUI - Graphical User Interface) pour Git vous permettent de visualiser et de gérer vos dépôts sans avoir à mémoriser toutes les commandes. Elles sont particulièrement utiles pour les débutants ou pour visualiser des historiques complexes.

## Pourquoi utiliser une interface graphique ?

- **Visualisation intuitive** : voir clairement les branches, commits et leur relation
- **Actions simplifiées** : effectuer des opérations complexes en quelques clics
- **Réduction de la courbe d'apprentissage** : moins de commandes à mémoriser
- **Meilleure compréhension** : vision globale de l'état du projet

## Les principales interfaces graphiques pour Git

### Sourcetree

![Sourcetree](https://bitbucket.org/blog/wp-content/uploads/2021/06/sourcetree_UI_overview-1.png)

**Points forts :**
- Gratuit et disponible pour Windows et macOS
- Interface claire et bien organisée
- Intégration avec Bitbucket, GitHub et GitLab
- Visualisation détaillée de l'historique

**Installation et démarrage :**
1. Téléchargez Sourcetree depuis le [site officiel](https://www.sourcetreeapp.com/)
2. Installez et lancez l'application
3. Dans l'écran d'accueil, cliquez sur "Ajouter" puis "Cloner" pour cloner un dépôt distant, ou "Créer" pour initialiser un nouveau dépôt
4. Naviguez dans votre dépôt via l'interface principale

**Opérations courantes :**
- **Commit** : Sélectionnez les fichiers dans la zone "Fichiers en attente", ajoutez un message et cliquez sur "Commit"
- **Branche** : Cliquez sur le bouton "Branche" dans la barre d'outils
- **Merge** : Cliquez droit sur une branche et sélectionnez "Merge"

### GitKraken

![GitKraken](https://www.gitkraken.com/img/og-image-gitkraken-git-gui.png)

**Points forts :**
- Interface moderne et intuitive
- Disponible sur Windows, macOS et Linux
- Graphique de branches très visuel avec code couleur
- Fonction d'annulation intégrée pour presque toutes les actions
- Version gratuite pour les projets publics

**Installation et démarrage :**
1. Téléchargez GitKraken depuis le [site officiel](https://www.gitkraken.com/)
2. Suivez les instructions d'installation et lancez l'application
3. Connectez-vous ou créez un compte GitKraken
4. Ouvrez, clonez ou créez un dépôt depuis l'écran d'accueil

**Opérations courantes :**
- **Staging/Unstaging** : Faites glisser les fichiers ou utilisez les boutons +/-
- **Commit** : Saisissez un message en bas à gauche et cliquez sur "Commit"
- **Branches** : Cliquez sur l'icône de branche dans la barre latérale gauche

### Visual Studio Code (avec extension Git)

![VSCode Git](https://code.visualstudio.com/assets/docs/sourcecontrol/overview/overview.png)

**Points forts :**
- Intégré à votre éditeur de code
- Léger et rapide
- Fonctionnalités Git essentielles sans quitter votre environnement
- Extensions supplémentaires disponibles pour plus de fonctionnalités

**Installation et utilisation :**
1. VSCode inclut Git par défaut, mais vous pouvez installer des extensions comme "GitLens" pour des fonctionnalités avancées
2. Accédez à l'onglet Source Control dans la barre latérale (icône de branche)
3. Gérez vos modifications directement dans l'interface

**Opérations courantes :**
- **Stage/Unstage** : Cliquez sur le + à côté du fichier
- **Commit** : Écrivez votre message et cliquez sur ✓
- **Branches** : Cliquez sur le nom de la branche actuelle dans la barre d'état en bas

### GitHub Desktop

![GitHub Desktop](https://desktop.github.com/images/github-desktop-screenshot-windows.png)

**Points forts :**
- Très simple d'utilisation pour les débutants
- Parfaite intégration avec GitHub
- Interface minimaliste axée sur les tâches essentielles
- Gratuit et open-source

**Installation et démarrage :**
1. Téléchargez GitHub Desktop depuis le [site officiel](https://desktop.github.com/)
2. Installez et connectez-vous à votre compte GitHub
3. Clonez un dépôt existant ou créez-en un nouveau

**Opérations courantes :**
- **Commit** : Sélectionnez les fichiers, ajoutez un message et cliquez sur "Commit to [branche]"
- **Push/Pull** : Utilisez les boutons en haut de l'interface
- **Branches** : Utilisez le menu déroulant "Current Branch"

## Choisir la bonne interface graphique

Le choix d'une interface graphique dépend de plusieurs facteurs :

1. **Votre système d'exploitation** : certaines GUI ne sont disponibles que sur certains OS
2. **Votre workflow** : certains outils sont mieux adaptés à certains workflows (ex: GitHub Desktop pour GitHub)
3. **Votre niveau d'expertise** : certaines interfaces sont plus simples, d'autres plus complètes
4. **Vos préférences visuelles** : l'aspect et l'organisation de l'interface peuvent influencer votre productivité

## Conseils pour bien utiliser les GUI Git

- **Comprenez les actions avant de cliquer** : savoir ce que fait chaque bouton pour éviter les erreurs
- **Utilisez la visualisation de l'historique** : c'est l'un des grands avantages des GUI
- **Explorez les préférences** : personnalisez l'interface selon vos besoins
- **N'abandonnez pas la ligne de commande** : certaines opérations avancées sont parfois plus rapides ou uniquement disponibles en ligne de commande

## Exercice pratique

1. Téléchargez et installez une des interfaces graphiques mentionnées ci-dessus
2. Clonez un de vos dépôts existants ou créez-en un nouveau
3. Réalisez les opérations suivantes avec l'interface :
   - Créez un fichier et faites un commit
   - Créez une nouvelle branche et basculez dessus
   - Modifiez un fichier et committez le changement
   - Revenez sur la branche principale et fusionnez votre nouvelle branche
   - Examinez l'historique des commits

---

En utilisant ces interfaces graphiques, vous découvrirez probablement que certaines tâches Git deviennent beaucoup plus intuitives. Cependant, gardez à l'esprit que comprendre les concepts sous-jacents reste essentiel pour résoudre des problèmes complexes ou des situations inhabituelles.
