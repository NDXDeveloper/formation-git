🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 9 : Outils et intégration

## 3. Extensions Git utiles

### Introduction

Git est déjà puissant tel quel, mais la communauté a développé des milliers d'**extensions** (aussi appelées plugins ou add-ons) qui ajoutent des fonctionnalités supplémentaires. Ces extensions peuvent vous faire gagner du temps, améliorer votre productivité, et rendre certaines tâches complexes beaucoup plus simples.

Dans cette section, nous explorerons les extensions les plus utiles pour différents environnements :
- Extensions pour éditeurs de code (notamment VSCode)
- Extensions pour navigateurs (GitHub, GitLab)
- Outils en ligne de commande
- Applications complémentaires

Vous n'avez pas besoin d'installer toutes ces extensions ! Commencez par quelques-unes qui correspondent à vos besoins, et ajoutez-en d'autres au fur et à mesure.

---

### Extensions pour Visual Studio Code

VSCode possède un écosystème d'extensions très riche. Voici les extensions Git indispensables et celles qui peuvent vraiment améliorer votre expérience.

#### Git Graph ⭐ (Indispensable)

**Ce qu'elle fait :**
Git Graph affiche un graphe visuel magnifique de votre historique Git, directement dans VSCode. C'est comme avoir une interface graphique Git intégrée à votre éditeur.

**Pourquoi l'installer :**
VSCode n'a pas de visualisation d'historique sophistiquée par défaut. Git Graph comble ce manque avec un graphe interactif où vous pouvez :
- Voir toutes vos branches et leur évolution
- Identifier facilement où les branches ont divergé ou fusionné
- Cliquer sur un commit pour voir les fichiers modifiés
- Effectuer des actions Git (checkout, merge, rebase) directement depuis le graphe

**Comment l'utiliser :**
1. Installez l'extension "Git Graph" depuis le marketplace
2. Cliquez sur "Git Graph" dans la barre de statut en bas
3. Ou ouvrez la palette de commandes (`Ctrl+Shift+P`) et tapez "Git Graph: View Git Graph"

**Fonctionnalités clés :**
- Graphe coloré avec les branches bien visibles
- Filtrage par branche, auteur, date
- Recherche dans les commits
- Vue détaillée de chaque commit avec diff
- Actions contextuelles (clic droit sur un commit)

**Niveau de difficulté :** Débutant - très facile à utiliser

---

#### GitLens ⭐ (Très recommandé)

**Ce qu'elle fait :**
GitLens enrichit considérablement les capacités Git de VSCode. Elle affiche qui a modifié chaque ligne de code, quand, et pourquoi (le message de commit).

**Pourquoi l'installer :**
GitLens répond à des questions courantes en développement :
- "Qui a écrit cette ligne de code ?"
- "Pourquoi ce code a été modifié ?"
- "Quand cette fonction a été ajoutée ?"

Elle ajoute ces informations directement dans l'éditeur, sans avoir à chercher dans l'historique.

**Fonctionnalités clés :**

**Blame inline**
En bas de chaque ligne, GitLens affiche discrètement l'auteur et la date de la dernière modification. Survolez ces informations pour voir le message de commit complet.

**File History**
Visualisez l'historique complet d'un fichier : tous les commits qui l'ont modifié, avec un aperçu des changements.

**Compare**
Comparez facilement :
- Le fichier actuel avec une version précédente
- Deux branches entre elles
- Deux commits spécifiques

**Current Line Blame**
Dans la barre de statut, GitLens affiche des informations sur la ligne où se trouve votre curseur.

**Line History View**
Panneau latéral qui montre l'historique de la ligne sélectionnée : tous les commits qui l'ont touchée.

**Comment l'utiliser :**
1. Installez l'extension "GitLens" depuis le marketplace
2. Ouvrez un fichier suivi par Git
3. Les annotations apparaissent automatiquement !
4. Explorez les panneaux latéraux GitLens (icône dans la barre latérale gauche)

**Note pour débutants :** GitLens affiche beaucoup d'informations. Si vous trouvez cela trop chargé, vous pouvez désactiver certaines fonctionnalités dans les paramètres. Commencez simplement par profiter du blame inline.

**Niveau de difficulté :** Débutant à intermédiaire

---

#### Git History

**Ce qu'elle fait :**
Permet de visualiser et rechercher dans l'historique Git, avec une interface simple et claire.

**Pourquoi l'installer :**
Alternative ou complément à Git Graph, Git History offre :
- Historique d'un fichier spécifique
- Comparaison entre branches
- Recherche dans l'historique avec filtres
- Informations sur les auteurs et statistiques

**Comment l'utiliser :**
1. Installez l'extension "Git History"
2. Clic droit sur un fichier → "Git: View File History"
3. Ou ouvrez la palette de commandes et tapez "Git: View History"

**Niveau de difficulté :** Débutant

---

#### Git Blame

**Ce qu'elle fait :**
Affiche directement dans l'éditeur qui a modifié chaque ligne de code, ligne par ligne.

**Pourquoi l'installer (si vous n'utilisez pas GitLens) :**
Si GitLens est trop complet pour vous, Git Blame offre uniquement la fonctionnalité de blame de manière simple et légère.

**Comment l'utiliser :**
1. Installez l'extension "Git Blame"
2. Les informations de blame s'affichent dans la barre de statut
3. Commande pour activer/désactiver : "Git Blame: Toggle"

**Niveau de difficulté :** Débutant

---

#### GitIgnore

**Ce qu'elle fait :**
Aide à créer et gérer vos fichiers `.gitignore` avec des modèles pré-faits pour différents langages et frameworks.

**Pourquoi l'installer :**
Créer un bon `.gitignore` peut être fastidieux. Cette extension vous donne accès à des centaines de modèles testés et maintenus par la communauté.

**Comment l'utiliser :**
1. Installez l'extension "gitignore"
2. Palette de commandes → "Add gitignore"
3. Choisissez votre langage ou framework (Python, Node.js, Java, etc.)
4. Un `.gitignore` approprié est généré !

**Exemple :** Sélectionnez "Python" et vous obtiendrez automatiquement un fichier qui ignore `__pycache__/`, `*.pyc`, `venv/`, etc.

**Niveau de difficulté :** Débutant

---

#### Git Stash

**Ce qu'elle fait :**
Facilite la gestion de votre stash Git avec une interface visuelle.

**Pourquoi l'installer :**
Le stash est très utile, mais peut être confus en ligne de commande. Cette extension rend le stash visuel et facile :
- Voir tous vos stashes
- Appliquer ou supprimer un stash avec un clic
- Prévisualiser le contenu d'un stash

**Comment l'utiliser :**
1. Installez l'extension "Git Stash"
2. Une nouvelle vue "Stash" apparaît dans la barre latérale Source Control
3. Cliquez sur les stashes pour les gérer

**Niveau de difficulté :** Débutant à intermédiaire

---

#### Conventional Commits

**Ce qu'elle fait :**
Aide à écrire des messages de commit suivant la convention "Conventional Commits" (format standardisé : `type(scope): message`).

**Pourquoi l'installer :**
Les messages de commit bien formatés facilitent :
- La lecture de l'historique
- La génération automatique de changelogs
- La collaboration en équipe

**Comment l'utiliser :**
1. Installez l'extension "Conventional Commits"
2. Quand vous créez un commit, cliquez sur l'icône de l'extension
3. Un assistant vous guide pour créer un message formaté correctement

**Exemple de format :**
```
feat(auth): add password reset functionality  
fix(api): correct response status code  
docs(readme): update installation instructions
```

**Niveau de difficulté :** Intermédiaire

---

#### Git Automator

**Ce qu'elle fait :**
Automatise des tâches Git répétitives avec des raccourcis et commandes personnalisables.

**Pourquoi l'installer :**
Pour les utilisateurs avancés qui veulent automatiser leur workflow Git. Permet de créer des séquences de commandes Git personnalisées.

**Niveau de difficulté :** Avancé

---

### Extensions pour navigateurs

Les plateformes comme GitHub, GitLab et Bitbucket peuvent être améliorées avec des extensions de navigateur.

#### Octotree (GitHub & GitLab)

**Ce qu'elle fait :**
Ajoute une barre latérale avec l'arborescence des fichiers sur GitHub et GitLab, comme dans un IDE.

**Pourquoi l'installer :**
Naviguer dans un dépôt GitHub devient aussi facile que dans VSCode. Plus besoin de cliquer sur chaque dossier pour explorer la structure du projet.

**Fonctionnalités :**
- Arborescence des fichiers pliable/dépliable
- Recherche rapide de fichiers
- Support des Pull Requests
- Bookmarks pour vos dépôts favoris

**Disponible pour :** Chrome, Firefox, Edge, Safari, Opera

**Version :** Gratuite (avec version Pro payante pour fonctionnalités avancées)

**Niveau de difficulté :** Débutant

---

#### Refined GitHub

**Ce qu'elle fait :**
Améliore l'interface GitHub avec des dizaines de petites améliorations qui rendent l'expérience plus agréable.

**Pourquoi l'installer :**
Si vous passez beaucoup de temps sur GitHub, cette extension ajoute plein de fonctionnalités utiles :
- Boutons et raccourcis supplémentaires
- Meilleure visualisation des diffs
- Amélioration de la lisibilité
- Fonctionnalités cachées exposées
- Corrections de bugs de l'interface GitHub

**Exemples d'améliorations :**
- Attendre la CI avant de merger
- Voir les fichiers modifiés dans les PRs plus clairement
- Liens rapides vers des actions courantes
- Marquer des notifications comme lues rapidement

**Disponible pour :** Chrome, Firefox, Edge

**Version :** Gratuite et open-source

**Niveau de difficulté :** Débutant

---

#### Enhanced GitHub

**Ce qu'elle fait :**
Affiche la taille des fichiers et permet de télécharger des fichiers individuels depuis GitHub.

**Pourquoi l'installer :**
Par défaut, GitHub ne montre pas la taille des fichiers et ne permet pas de télécharger un seul fichier (il faut cloner tout le dépôt). Cette extension corrige ces limitations.

**Disponible pour :** Chrome, Firefox, Opera

**Version :** Gratuite

**Niveau de difficulté :** Débutant

---

#### GitZip for GitHub

**Ce qu'elle fait :**
Permet de télécharger un dossier ou fichier spécifique d'un dépôt GitHub sans cloner tout le dépôt.

**Pourquoi l'installer :**
Très utile quand vous voulez juste un exemple de code ou un fichier spécifique d'un gros projet.

**Comment l'utiliser :**
1. Installez l'extension
2. Sur GitHub, double-cliquez sur un dossier
3. Un bouton de téléchargement apparaît !

**Disponible pour :** Chrome, Firefox

**Version :** Gratuite

**Niveau de difficulté :** Débutant

---

#### Sourcegraph

**Ce qu'elle fait :**
Ajoute la navigation de code intelligente sur GitHub, GitLab et Bitbucket : allez à la définition, trouvez les références, comme dans un IDE.

**Pourquoi l'installer :**
Transforme votre navigateur en IDE pour lire du code :
- Cliquez sur une fonction pour voir sa définition
- Trouvez toutes les utilisations d'une variable
- Navigation de code intelligente

**Disponible pour :** Chrome, Firefox, Safari

**Version :** Gratuite (avec version Enterprise pour les organisations)

**Niveau de difficulté :** Intermédiaire

---

### Outils en ligne de commande

Au-delà des extensions d'éditeurs et navigateurs, il existe des outils CLI (Command Line Interface) qui enrichissent Git.

#### Git-flow (AVH Edition)

**Ce qu'il fait :**
Ajoute des commandes Git pour faciliter le workflow Git Flow (branches feature/, release/, hotfix/).

**Pourquoi l'installer :**
Si vous utilisez le workflow Git Flow, ces commandes simplifient la création et la gestion des branches :
```bash
git flow feature start nouvelle-fonctionnalite  
git flow feature finish nouvelle-fonctionnalite  
git flow release start 1.0.0
```

**Installation :**
- **macOS** : `brew install git-flow-avh`
- **Linux** : `apt-get install git-flow` (Ubuntu/Debian)
- **Windows** : Inclus dans Git for Windows

**Niveau de difficulté :** Intermédiaire

---

#### Tig

**Ce qu'il fait :**
Interface en ligne de commande (texte) pour visualiser l'historique Git de manière interactive.

**Pourquoi l'installer :**
Pour ceux qui préfèrent le terminal mais veulent une meilleure visualisation que `git log`. Tig offre :
- Navigation interactive dans l'historique
- Visualisation de branches en mode texte
- Recherche et filtrage rapides
- Léger et rapide

**Installation :**
- **macOS** : `brew install tig`
- **Linux** : `apt-get install tig`
- **Windows** : Disponible via Git Bash ou WSL

**Utilisation :**
Tapez simplement `tig` dans votre dépôt Git.

**Niveau de difficulté :** Intermédiaire

---

#### Hub / GitHub CLI (gh)

**Ce qu'il fait :**
Étend Git avec des commandes GitHub directement depuis le terminal.

**Pourquoi l'installer :**
Gérez GitHub sans quitter votre terminal :
- Créer des Pull Requests : `gh pr create`
- Cloner vos dépôts : `gh repo clone mon-repo`
- Voir les issues : `gh issue list`
- Fork des projets : `gh repo fork`

**Installation :**
- **macOS** : `brew install gh`
- **Linux** : Voir instructions sur cli.github.com
- **Windows** : Installeur disponible sur cli.github.com

**Niveau de difficulté :** Intermédiaire

---

#### Delta

**Ce qu'il fait :**
Améliore l'affichage des diffs Git avec de la coloration syntaxique, des changements ligne par ligne mis en évidence, et un rendu plus lisible.

**Pourquoi l'installer :**
Les diffs Git par défaut peuvent être difficiles à lire. Delta les rend magnifiques et clairs :
- Coloration syntaxique du code
- Mise en évidence des mots modifiés (pas seulement les lignes)
- Numéros de ligne
- Thèmes personnalisables

**Installation :**
- **macOS** : `brew install git-delta`
- **Linux** : Voir instructions sur github.com/dandavison/delta
- **Windows** : Télécharger depuis les releases GitHub

**Configuration :**
Après installation, configurez Git pour utiliser Delta :
```bash
git config --global core.pager delta  
git config --global interactive.diffFilter delta --color-only
```

**Niveau de difficulté :** Intermédiaire

---

#### Git-extras

**Ce qu'il fait :**
Collection de 60+ commandes Git supplémentaires pour des tâches courantes.

**Pourquoi l'installer :**
Ajoute des commandes très utiles que Git n'a pas par défaut :
- `git undo` : annuler le dernier commit (garde les modifications)
- `git summary` : statistiques sur le dépôt
- `git effort` : voir quels fichiers ont le plus de commits
- `git authors` : lister tous les contributeurs
- `git changelog` : générer un changelog automatique

**Installation :**
- **macOS** : `brew install git-extras`
- **Linux** : `apt-get install git-extras`
- **Windows** : Disponible via Git Bash

**Niveau de difficulté :** Intermédiaire à avancé

---

#### LazyGit

**Ce qu'il fait :**
Interface utilisateur en terminal (TUI - Text User Interface) simple et rapide pour Git.

**Pourquoi l'installer :**
Pour ceux qui aiment le terminal mais veulent une interface visuelle légère. LazyGit offre :
- Navigation au clavier
- Visualisation de l'état Git
- Opérations Git courantes facilitées
- Très rapide et léger

**Installation :**
- **macOS** : `brew install lazygit`
- **Linux** : Voir instructions sur github.com/jesseduffield/lazygit
- **Windows** : `scoop install lazygit`

**Utilisation :**
Tapez `lazygit` dans votre dépôt Git.

**Niveau de difficulté :** Intermédiaire

---

### Applications complémentaires

#### Pre-commit

**Ce qu'il fait :**
Framework pour gérer les hooks Git pre-commit facilement. Permet de lancer automatiquement des vérifications avant chaque commit.

**Pourquoi l'installer :**
Évitez de committer du code qui ne respecte pas les standards :
- Vérification du formatage (black, prettier, etc.)
- Analyse de code (linters)
- Tests automatiques
- Vérification de fichiers sensibles

**Installation :**
```bash
pip install pre-commit
```

**Utilisation basique :**
1. Créez un fichier `.pre-commit-config.yaml` dans votre projet
2. Configurez les hooks désirés
3. Exécutez `pre-commit install`
4. Les vérifications s'exécutent automatiquement avant chaque commit !

**Niveau de difficulté :** Intermédiaire à avancé

---

#### Commitizen

**Ce qu'il fait :**
Outil interactif pour créer des commits suivant la convention Conventional Commits.

**Pourquoi l'installer :**
Comme l'extension VSCode Conventional Commits, mais utilisable depuis le terminal et avec n'importe quel éditeur.

**Installation :**
```bash
npm install -g commitizen
```

**Utilisation :**
Remplacez `git commit` par `git cz` et un assistant interactif vous guide.

**Niveau de difficulté :** Intermédiaire

---

### Comment choisir ses extensions

Face à tant d'options, comment décider quelles extensions installer ?

#### Commencez petit

Ne surchargez pas votre environnement ! Commencez avec 2-3 extensions essentielles et ajoutez-en d'autres au fur et à mesure que vous identifiez vos besoins.

**Pour débuter, installez au minimum :**
1. **Git Graph** (VSCode) - pour visualiser l'historique
2. **GitLens** (VSCode) - pour comprendre l'historique du code
3. **Octotree** (navigateur) - si vous utilisez GitHub

#### Identifiez vos besoins

Posez-vous ces questions :
- "Quelle tâche Git me prend le plus de temps ?"
- "Qu'est-ce qui me frustre dans mon workflow actuel ?"
- "Qu'est-ce que je dois faire souvent et qui pourrait être simplifié ?"

Cherchez ensuite une extension qui résout ce problème spécifique.

#### Testez et évaluez

Installez une extension, testez-la pendant quelques jours, puis décidez :
- Est-ce qu'elle m'aide vraiment ?
- Est-ce qu'elle complique les choses ou les simplifie ?
- Est-ce que je l'utilise régulièrement ?

Si la réponse est non, désinstallez-la. Mieux vaut quelques extensions bien utilisées que des dizaines qui encombrent.

#### Considérez les performances

Certaines extensions, surtout pour VSCode, peuvent ralentir votre éditeur. Si vous remarquez des ralentissements :
- Vérifiez quelles extensions sont actives
- Désactivez celles que vous n'utilisez pas régulièrement
- Choisissez des extensions légères

#### Privilégiez les extensions populaires et maintenues

Vérifiez :
- Le nombre d'installations / téléchargements
- La date de dernière mise à jour
- Les notes et avis des utilisateurs
- La réactivité du développeur aux issues

Une extension populaire et activement maintenue a plus de chances d'être fiable et compatible.

---

### Suggestions par profil

#### Pour les débutants

Vous découvrez Git et voulez juste les essentiels :
- **Git Graph** : pour comprendre l'historique visuellement
- **GitIgnore** : pour créer facilement des .gitignore
- **Octotree** : pour naviguer sur GitHub plus facilement

#### Pour les développeurs intermédiaires

Vous maîtrisez les bases et voulez plus de productivité :
- Tout ce qui précède, plus :
- **GitLens** : pour le blame et l'historique détaillé
- **Refined GitHub** : si vous travaillez beaucoup avec GitHub
- **Delta** : pour des diffs plus lisibles

#### Pour les développeurs avancés

Vous voulez automatiser et optimiser au maximum :
- Tout ce qui précède, plus :
- **GitHub CLI (gh)** : pour gérer GitHub depuis le terminal
- **Pre-commit** : pour automatiser les vérifications
- **LazyGit** ou **Tig** : pour une interface terminal rapide
- **Git-extras** : pour des commandes supplémentaires

#### Pour le travail en équipe

Vous collaborez sur des projets avec plusieurs personnes :
- **GitLens** : pour voir qui a écrit quoi
- **Conventional Commits** : pour standardiser les messages
- **GitHub CLI** : pour gérer les Pull Requests efficacement
- **Refined GitHub** : pour améliorer la revue de code

---

### Bonnes pratiques avec les extensions

#### Documentez vos choix en équipe

Si vous travaillez en équipe, documentez :
- Quelles extensions sont recommandées
- Comment les configurer
- Pourquoi elles sont utiles pour votre workflow

Un fichier `CONTRIBUTING.md` ou `.vscode/extensions.json` peut lister les extensions recommandées.

#### Restez à jour

Les extensions évoluent ! Mettez-les à jour régulièrement :
- Nouvelles fonctionnalités
- Corrections de bugs
- Améliorations de performances

La plupart des éditeurs proposent une mise à jour automatique.

#### Apprenez les raccourcis

Beaucoup d'extensions offrent des raccourcis clavier. Prenez le temps de les apprendre pour être encore plus productif.

#### N'oubliez pas les bases

Les extensions sont des outils, pas des béquilles. Assurez-vous de comprendre ce qu'elles font en arrière-plan. Si une extension cesse de fonctionner, vous devez pouvoir accomplir la même tâche manuellement.

---

### Ressources pour découvrir de nouvelles extensions

**Pour VSCode :**
- Le marketplace VSCode : cherchez "git" et triez par popularité
- Site web : marketplace.visualstudio.com

**Pour navigateurs :**
- Chrome Web Store / Firefox Add-ons
- Recherchez "GitHub" ou "Git"

**Pour CLI :**
- Awesome Git : github.com/dictcp/awesome-git (liste communautaire)
- GitHub trending : voir les projets Git populaires

**Blogs et articles :**
- Dev.to : articles sur les workflows Git
- Medium : tutoriels et recommandations
- Reddit (r/git, r/vscode) : discussions et découvertes

---

### Conclusion

Les extensions Git peuvent transformer votre expérience de développement. Qu'il s'agisse de visualiser l'historique plus clairement, d'automatiser des tâches répétitives, ou de standardiser votre workflow, il existe probablement une extension pour cela.

**Recommandations finales :**

**Essentielles (pour tous) :**
- Git Graph (VSCode)
- Octotree (navigateur, si vous utilisez GitHub)

**Très utiles :**
- GitLens (VSCode)
- Refined GitHub (navigateur)
- Delta (terminal)

**Pour aller plus loin :**
- GitHub CLI (terminal)
- Pre-commit (automatisation)
- LazyGit (terminal)

Commencez avec les essentielles, puis explorez selon vos besoins et votre curiosité. Chaque développeur a son propre ensemble d'outils préférés - construisez le vôtre !

Dans la section suivante, nous verrons comment Git s'intègre avec les systèmes d'intégration continue (CI/CD) et de déploiement automatisé.

⏭️ [Intégration continue (CI/CD) et Git](/module-09-outils-et-integration/04-integration-continue-et-git.md)
