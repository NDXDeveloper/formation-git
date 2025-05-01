# 7.2. Intégration Git dans les IDE

Les Environnements de Développement Intégrés (IDE) modernes offrent généralement une intégration native avec Git, vous permettant de gérer votre code source sans quitter votre espace de travail. Cette intégration rend le développement plus fluide et plus efficace.

## Avantages de l'intégration Git dans les IDE

- **Tout-en-un** : gérez votre code et son versionnement au même endroit
- **Contextualisation** : visualisez qui a modifié quelles lignes directement dans l'éditeur
- **Productivité** : exécutez des commandes Git sans changer d'application
- **Feedback immédiat** : voyez l'état de vos fichiers (modifiés, ajoutés, etc.) en temps réel

## Visual Studio Code et Git

Visual Studio Code (VSCode) est un éditeur de code populaire avec une excellente intégration Git.

### Fonctionnalités Git intégrées dans VSCode

![VSCode Git](https://code.visualstudio.com/assets/docs/sourcecontrol/overview/overview.png)

- **Indicateurs de changement** : barres colorées dans la gouttière indiquant les modifications
- **Contrôle de source** : onglet dédié pour gérer les commits, branches, etc.
- **Terminal intégré** : accès à la ligne de commande Git si nécessaire
- **Diff visuel** : comparaison côte à côte des modifications

### Utilisation de base de Git dans VSCode

1. **Ouvrir le contrôle de source** : cliquez sur l'icône de branche dans la barre latérale ou utilisez `Ctrl+Shift+G`
2. **Visualiser les changements** : les fichiers modifiés apparaissent dans la section "Changes"
3. **Stage des fichiers** : cliquez sur le "+" à côté du nom du fichier
4. **Commit** : ajoutez un message et cliquez sur la coche ✓
5. **Push/Pull** : utilisez les icônes ↑ (push) et ↓ (pull) dans la barre d'état

### Extensions Git pour VSCode

- **GitLens** : fonctionnalités Git avancées (blame, historique, etc.)
- **Git History** : visualisation graphique de l'historique Git
- **Git Graph** : affichage visuel des branches et commits

## IntelliJ IDEA (et autres IDE JetBrains)

Les IDE de JetBrains (IntelliJ IDEA, PyCharm, WebStorm, etc.) offrent une intégration Git complète.

![IntelliJ Git](https://www.jetbrains.com/help/img/idea/2023.1/git_operations_popup.png)

### Fonctionnalités Git dans les IDE JetBrains

- **Menu Git** : accès à toutes les opérations Git courantes
- **Commit Tool Window** : fenêtre dédiée pour les commits
- **Log** : visualisation détaillée de l'historique
- **Branch Manager** : gestion visuelle des branches
- **Merge Tool** : outil puissant pour résoudre les conflits

### Utilisation de base de Git dans IntelliJ

1. **Accéder au menu Git** : cliquez sur `VCS` > `Git` dans la barre de menu
2. **Commit** : `VCS` > `Commit` ou utilisez le raccourci `Ctrl+K`
3. **Push** : `VCS` > `Git` > `Push` ou `Ctrl+Shift+K`
4. **Pull** : `VCS` > `Git` > `Pull` ou `Ctrl+T`
5. **Branches** : `VCS` > `Git` > `Branches` ou cliquez sur le nom de la branche dans la barre d'état

## Eclipse et Git (EGit)

Eclipse utilise EGit pour l'intégration avec Git.

![Eclipse EGit](https://wiki.eclipse.org/images/7/7e/Egit-0.9-getstarted-commit.png)

### Fonctionnalités Git dans Eclipse

- **Perspective Git** : vue dédiée pour les opérations Git
- **Décorateurs** : icônes indiquant l'état des fichiers
- **History View** : historique des commits
- **Synchronize View** : vue pour synchroniser avec le dépôt distant

### Utilisation de base de Git dans Eclipse

1. **Ajouter à l'index** : clic droit sur fichier > `Team` > `Add to Index`
2. **Commit** : clic droit > `Team` > `Commit`
3. **Push** : clic droit > `Team` > `Push to Upstream`
4. **Pull** : clic droit > `Team` > `Pull`
5. **Voir l'historique** : clic droit > `Team` > `Show in History`

## Visual Studio et Git

Microsoft Visual Studio offre également une intégration Git native.

![Visual Studio Git](https://learn.microsoft.com/en-us/visualstudio/version-control/media/vs-2022/git-changes-window.png?view=vs-2022)

### Fonctionnalités Git dans Visual Studio

- **Fenêtre Git Changes** : visualisation des modifications et des commits
- **Explorateur de solutions** : indicateurs d'état sur les fichiers
- **Branch Picker** : sélecteur de branches dans la barre d'outils
- **Git Repository Window** : fenêtre pour explorer le dépôt

### Utilisation de base de Git dans Visual Studio

1. **Ouvrir Git Changes** : `View` > `Git Changes` ou cliquez sur l'icône Git dans la barre d'état
2. **Stage des fichiers** : cliquez sur le "+" à côté du fichier
3. **Commit** : entrez un message et cliquez sur "Commit All"
4. **Push/Pull** : utilisez les boutons dans la fenêtre Git Changes
5. **Gérer les branches** : utilisez le sélecteur de branches en haut de la fenêtre Git Changes

## Astuces pour utiliser Git dans les IDE

1. **Apprenez les raccourcis clavier** : ils peuvent accélérer considérablement votre workflow
2. **Utilisez la visualisation des différences** : comparez visuellement les versions des fichiers
3. **Exploitez les outils de résolution de conflits** : la plupart des IDE ont des outils visuels pour résoudre les conflits
4. **Consultez l'historique des lignes** : voyez qui a modifié une ligne spécifique et quand (blame/annotate)
5. **Personnalisez les préférences Git** : adaptez l'expérience à vos besoins

## Exercice pratique

1. Ouvrez votre IDE préféré et vérifiez que l'intégration Git est activée
2. Créez ou clonez un dépôt Git
3. Modifiez quelques fichiers et observez comment l'IDE indique les modifications
4. Utilisez l'interface de l'IDE pour:
   - Voir les différences entre la version actuelle et la dernière version committée
   - Stage et commit des modifications
   - Créer une nouvelle branche
   - Fusionner deux branches
5. Explorez les fonctionnalités avancées comme l'historique du fichier ou le blame

---

L'intégration de Git dans votre IDE vous fait gagner du temps et simplifie votre workflow quotidien. Bien que les interfaces diffèrent entre les IDE, les concepts de base restent les mêmes. N'hésitez pas à explorer toutes les fonctionnalités Git offertes par votre IDE - vous découvrirez probablement des outils qui rendront votre travail avec Git encore plus efficace.
