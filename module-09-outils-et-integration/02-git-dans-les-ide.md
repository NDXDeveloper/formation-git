🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 9 : Outils et intégration

## 2. Git dans les IDE (VSCode, IntelliJ, PyCharm)

### Introduction

Un **IDE** (Integrated Development Environment, ou environnement de développement intégré) est un logiciel qui regroupe tous les outils nécessaires pour coder : éditeur de texte, débogueur, terminal, et bien plus. La bonne nouvelle, c'est que la plupart des IDE modernes intègrent Git directement !

Cela signifie que vous pouvez :
- Faire des commits sans quitter votre éditeur
- Voir quels fichiers ont été modifiés en un coup d'œil
- Comparer des versions de fichiers côte à côte
- Gérer vos branches directement depuis votre espace de travail
- Résoudre des conflits avec des outils visuels intégrés

Dans cette section, nous explorerons comment utiliser Git dans trois IDE très populaires : **Visual Studio Code**, **IntelliJ IDEA** et **PyCharm**. Même si vous n'utilisez pas ces IDE spécifiquement, les concepts s'appliquent à la plupart des éditeurs modernes.

---

### Git dans Visual Studio Code (VSCode)

**Visual Studio Code** (ou VSCode) est un éditeur de code gratuit et open-source développé par Microsoft. C'est l'un des éditeurs les plus populaires au monde, utilisé par des millions de développeurs.

#### Installation et configuration

**Git est automatiquement détecté**
Si Git est installé sur votre système, VSCode le détecte automatiquement. Aucune configuration supplémentaire n'est nécessaire dans la plupart des cas.

**Vérifier que Git est reconnu**
1. Ouvrez VSCode
2. Appuyez sur `Ctrl+Shift+P` (ou `Cmd+Shift+P` sur Mac) pour ouvrir la palette de commandes
3. Tapez "Git: Version" et appuyez sur Entrée
4. Si Git est bien installé, la version s'affichera dans le coin inférieur droit

**Configurer votre identité**
Si ce n'est pas déjà fait, configurez votre nom et email (utilisés pour les commits) :
1. Ouvrez le terminal intégré (`Ctrl+ù` ou menu Terminal → Nouveau terminal)
2. Tapez les commandes suivantes :
```
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"
```

#### Interface Git de VSCode

**Barre latérale Source Control**
Cliquez sur l'icône de branchement dans la barre latérale gauche (troisième icône depuis le haut, ou `Ctrl+Shift+G`). C'est ici que se passe toute la magie Git !

Vous y trouverez :
- La liste des fichiers modifiés
- Des boutons pour les actions courantes (commit, push, pull)
- La branche actuelle
- Le nombre de modifications en attente

**Indicateurs visuels dans l'éditeur**
VSCode affiche des couleurs dans la marge gauche de l'éditeur :
- **Barre verte** : lignes ajoutées
- **Barre bleue** : lignes modifiées
- **Petit triangle rouge** : lignes supprimées

Ces indicateurs vous montrent en temps réel ce qui a changé depuis le dernier commit.

**Indicateurs dans l'explorateur de fichiers**
Dans l'explorateur de fichiers (barre latérale), les fichiers ont des lettres et couleurs :
- **M** (modifié) en orange : fichier modifié
- **U** (untracked) en vert : nouveau fichier non suivi
- **D** (deleted) en rouge : fichier supprimé
- **C** (conflict) : fichier en conflit

#### Opérations Git de base dans VSCode

**Initialiser ou cloner un dépôt**

Pour un nouveau projet :
1. Ouvrez un dossier vide
2. Cliquez sur "Initialize Repository" dans la vue Source Control
3. Votre dossier devient un dépôt Git !

Pour cloner un dépôt existant :
1. `Ctrl+Shift+P` → "Git: Clone"
2. Collez l'URL du dépôt
3. Choisissez un dossier de destination

**Voir les modifications (git status)**
Simplement ouvrir la vue Source Control (`Ctrl+Shift+G`) vous montre tous les fichiers modifiés. C'est l'équivalent visuel de `git status`.

**Ajouter des fichiers au staging (git add)**
Dans la vue Source Control :
- Survolez un fichier et cliquez sur le `+` qui apparaît
- Ou cliquez sur le `+` à côté de "Changes" pour tout ajouter d'un coup
- Les fichiers passent de la section "Changes" à "Staged Changes"

**Créer un commit (git commit)**
1. Assurez-vous que vos fichiers sont dans "Staged Changes"
2. Tapez votre message de commit dans la zone de texte en haut
3. Appuyez sur `Ctrl+Entrée` ou cliquez sur l'icône de validation (✓)
4. Votre commit est créé !

**Voir l'historique (git log)**
VSCode n'a pas d'affichage d'historique très sophistiqué par défaut, mais vous pouvez :
- Cliquer sur l'icône d'horloge à côté d'un fichier pour voir son historique
- Ou installer l'extension "Git Graph" (fortement recommandée !) qui affiche un graphe complet de vos commits

**Pousser et tirer (git push / git pull)**
Dans la barre de statut en bas :
- Cliquez sur l'icône de synchronisation (deux flèches en cercle) pour faire `git pull` puis `git push`
- Ou utilisez les boutons dans la vue Source Control

#### Gestion des branches dans VSCode

**Voir la branche actuelle**
En bas à gauche de la fenêtre, vous voyez le nom de votre branche actuelle (ex: `main`, `develop`).

**Changer de branche**
1. Cliquez sur le nom de la branche en bas à gauche
2. Une liste des branches disponibles s'ouvre
3. Sélectionnez la branche vers laquelle vous voulez basculer

**Créer une nouvelle branche**
1. Cliquez sur le nom de la branche en bas à gauche
2. Sélectionnez "+ Create new branch..."
3. Entrez le nom de la nouvelle branche
4. Vous basculez automatiquement sur cette nouvelle branche

**Fusionner des branches**
1. Basculez sur la branche de destination (celle qui va recevoir les modifications)
2. `Ctrl+Shift+P` → "Git: Merge Branch..."
3. Sélectionnez la branche à fusionner
4. Si tout se passe bien, la fusion est automatique !

#### Résolution de conflits dans VSCode

Lorsqu'un conflit survient, VSCode le détecte et vous aide à le résoudre :

1. **Identification du conflit**
   Les fichiers en conflit apparaissent avec un `C` dans la vue Source Control

2. **Ouverture du fichier**
   Ouvrez le fichier en conflit. VSCode affiche les conflits avec des couleurs :
   - Bleu/vert : vos modifications (Current Change)
   - Rouge/jaune : les modifications entrantes (Incoming Change)

3. **Résolution visuelle**
   Au-dessus de chaque conflit, VSCode affiche des boutons :
   - **Accept Current Change** : garder votre version
   - **Accept Incoming Change** : prendre la version entrante
   - **Accept Both Changes** : garder les deux
   - **Compare Changes** : voir une comparaison détaillée

4. **Finalisation**
   Une fois tous les conflits résolus :
   - Sauvegardez le fichier
   - Ajoutez-le au staging (cliquez sur `+`)
   - Créez un commit pour finaliser la fusion

#### Extensions Git recommandées pour VSCode

**Git Graph**
Affiche un graphe visuel magnifique de votre historique Git. Indispensable !
- Installation : Recherchez "Git Graph" dans l'onglet Extensions
- Utilisation : Cliquez sur "Git Graph" dans la barre de statut

**GitLens**
Enrichit VSCode avec des fonctionnalités Git avancées : blame inline, comparaisons, historique détaillé...
- Affiche l'auteur et la date de chaque ligne de code
- Permet de comparer facilement différentes versions
- Offre une vue détaillée de l'historique

**Git History**
Permet de visualiser et rechercher dans l'historique Git, comparer des branches et commits.

#### Astuces et raccourcis VSCode

**Raccourcis clavier pratiques**
- `Ctrl+Shift+G` : Ouvrir la vue Source Control
- `Ctrl+Entrée` (dans le message de commit) : Créer le commit
- `Ctrl+Shift+P` puis "Git" : Accéder à toutes les commandes Git

**Comparer des fichiers**
- Dans l'historique d'un fichier, cliquez sur un commit pour voir les différences
- Ou faites un clic droit sur un fichier modifié → "Open Changes"

**Terminal intégré**
Besoin d'une commande Git avancée ? Ouvrez le terminal intégré (`Ctrl+ù`) et tapez vos commandes comme d'habitude. Le meilleur des deux mondes !

---

### Git dans IntelliJ IDEA

**IntelliJ IDEA** est un IDE puissant développé par JetBrains, principalement utilisé pour Java, mais qui supporte de nombreux autres langages. L'intégration Git d'IntelliJ est réputée pour être l'une des meilleures du marché.

#### Configuration initiale

**Vérifier que Git est détecté**
1. Allez dans `File → Settings` (Windows/Linux) ou `IntelliJ IDEA → Preferences` (Mac)
2. Naviguez vers `Version Control → Git`
3. IntelliJ affiche le chemin vers l'exécutable Git
4. Cliquez sur "Test" pour vérifier que tout fonctionne

**Configurer votre identité Git**
Si ce n'est pas déjà fait, configurez votre nom et email :
1. Ouvrez le terminal intégré (onglet en bas)
2. Tapez :
```
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"
```

#### Interface Git d'IntelliJ

**Fenêtre Git**
En bas de l'IDE, vous trouverez l'onglet "Git" (ou "Commit" dans les versions récentes). C'est votre centre de contrôle Git.

Vous y accédez en cliquant sur l'onglet ou avec `Alt+9` (Windows/Linux) ou `Cmd+9` (Mac).

**Structure de la fenêtre Git**
- **Log** : Historique visuel de tous vos commits sous forme de graphe
- **Console** : Sortie des commandes Git exécutées
- **Branches** : Vue de toutes vos branches locales et distantes

**Indicateurs visuels dans l'éditeur**
IntelliJ affiche dans la marge gauche :
- **Barre verte** : nouvelles lignes
- **Barre bleue** : lignes modifiées
- **Triangle gris** : lignes supprimées

Cliquez sur ces barres pour voir un diff détaillé et même annuler des modifications ligne par ligne !

**Couleurs dans l'explorateur de projet**
- **Bleu** : fichiers modifiés
- **Vert** : nouveaux fichiers (untracked)
- **Rouge** : fichiers qui ont des conflits
- **Gris** : fichiers ignorés (.gitignore)

#### Opérations Git de base dans IntelliJ

**Initialiser ou cloner un dépôt**

Pour initialiser :
1. `VCS → Enable Version Control Integration`
2. Sélectionnez "Git"

Pour cloner :
1. `File → New → Project from Version Control`
2. Entrez l'URL du dépôt
3. Choisissez le dossier de destination

**Voir les modifications**
1. Ouvrez l'onglet "Commit" sur le côté gauche (`Ctrl+K` ou `Cmd+K`)
2. Vous voyez tous les fichiers modifiés organisés par catégorie
3. Cliquez sur un fichier pour voir le diff détaillé

**Créer un commit**
1. Ouvrez l'onglet "Commit" (`Ctrl+K` / `Cmd+K`)
2. Cochez les fichiers que vous voulez committer
3. Tapez votre message de commit
4. Cliquez sur "Commit" (ou "Commit and Push" pour committer et pousser en une seule action)

**Avant de committer** : IntelliJ peut effectuer des vérifications automatiques :
- Analyse du code
- Reformatage automatique
- Optimisation des imports
- Vérification TODO

**Pousser et tirer**
- **Push** : `VCS → Git → Push` ou `Ctrl+Shift+K` / `Cmd+Shift+K`
- **Pull** : `VCS → Git → Pull` ou `Ctrl+T` / `Cmd+T`

**Voir l'historique**
1. Ouvrez l'onglet "Git" en bas (`Alt+9` / `Cmd+9`)
2. Vous voyez un magnifique graphe de vos commits
3. Cliquez sur un commit pour voir les fichiers modifiés
4. Double-cliquez sur un fichier pour voir le diff complet

#### Gestion des branches dans IntelliJ

**Widget de branche**
En bas à droite de la fenêtre, vous voyez le nom de la branche actuelle (ex: `main`). C'est le "widget de branche".

**Changer de branche**
1. Cliquez sur le widget de branche
2. Sélectionnez la branche vers laquelle vous voulez basculer
3. Choisissez "Checkout"

**Créer une nouvelle branche**
1. Cliquez sur le widget de branche
2. Sélectionnez "New Branch..."
3. Entrez le nom de la branche
4. Cochez "Checkout branch" pour basculer dessus immédiatement

**Fusionner des branches**
1. Basculez sur la branche de destination
2. Cliquez sur le widget de branche
3. Sélectionnez la branche à fusionner
4. Choisissez "Merge into Current"

**Comparer des branches**
1. Cliquez sur le widget de branche
2. Sélectionnez une branche
3. Choisissez "Compare with Current"
4. IntelliJ affiche tous les fichiers différents entre les deux branches

#### Résolution de conflits dans IntelliJ

IntelliJ possède l'un des meilleurs outils de résolution de conflits :

1. **Détection automatique**
   Quand un conflit survient, IntelliJ affiche une notification et surligne les fichiers en rouge

2. **Ouverture du résolveur de conflits**
   - Cliquez droit sur le fichier en conflit → "Git → Resolve Conflicts"
   - Ou acceptez la notification qui propose de résoudre les conflits

3. **Interface de résolution en trois panneaux**
   IntelliJ ouvre une vue avec trois colonnes :
   - **Gauche** : votre version (local)
   - **Centre** : le résultat fusionné (que vous éditez)
   - **Droite** : la version entrante (remote)

4. **Résolution facilitée**
   - Des boutons `>>` et `<<` permettent d'accepter des modifications avec un clic
   - Vous pouvez aussi éditer manuellement le panneau central
   - Les conflits non résolus sont surlignés en rouge

5. **Finalisation**
   - Cliquez sur "Apply" quand tous les conflits sont résolés
   - IntelliJ marque automatiquement le fichier comme résolu
   - Créez un commit pour finaliser

#### Fonctionnalités Git avancées d'IntelliJ

**Annotate (Git Blame)**
1. Clic droit dans la marge de l'éditeur → "Annotate with Git Blame"
2. Chaque ligne affiche l'auteur, la date et le commit qui l'a modifiée
3. Cliquez sur l'annotation pour voir le commit complet

**Local History (historique local)**
IntelliJ maintient un historique local de vos modifications, même avant les commits :
1. Clic droit sur un fichier → "Local History → Show History"
2. Vous pouvez revenir à n'importe quelle version sauvegardée
3. C'est un filet de sécurité supplémentaire !

**Shelf (équivalent de git stash)**
Pour mettre de côté temporairement vos modifications :
1. `VCS → Shelve Changes`
2. Nommez votre "shelf"
3. Plus tard : `VCS → Unshelve` pour récupérer vos modifications

**Cherry-pick visuel**
1. Dans l'historique Git, cliquez droit sur un commit
2. Sélectionnez "Cherry-Pick"
3. IntelliJ applique ce commit sur votre branche actuelle

#### Astuces et raccourcis IntelliJ

**Raccourcis clavier essentiels**
- `Ctrl+K` / `Cmd+K` : Ouvrir la fenêtre de commit
- `Ctrl+T` / `Cmd+T` : Pull
- `Ctrl+Shift+K` / `Cmd+Shift+K` : Push
- `Alt+9` / `Cmd+9` : Ouvrir/fermer la fenêtre Git
- `Alt+\`` : Menu rapide VCS

**Revenir en arrière rapidement**
Dans l'éditeur, cliquez sur les barres de couleur dans la marge gauche :
- Une popup s'affiche avec les modifications
- Cliquez sur l'icône de flèche pour annuler cette modification spécifique
- Pratique pour annuler juste quelques lignes !

**Comparaison avec le dépôt distant**
`VCS → Git → Compare with Branch` vous permet de comparer votre version locale avec n'importe quelle branche distante avant de faire un pull.

---

### Git dans PyCharm

**PyCharm** est l'IDE de JetBrains spécialisé pour Python. Bonne nouvelle : puisqu'il fait partie de la famille JetBrains (comme IntelliJ), **l'intégration Git est identique** !

#### Similitudes avec IntelliJ

PyCharm utilise exactement la même interface Git qu'IntelliJ IDEA :
- Même fenêtre Git avec le graphe des commits
- Même outil de résolution de conflits
- Mêmes raccourcis clavier
- Même widget de branche en bas à droite
- Même fenêtre de commit (`Ctrl+K` / `Cmd+K`)

Si vous maîtrisez Git dans IntelliJ, vous maîtrisez Git dans PyCharm, et vice-versa !

#### Spécificités Python et Git

**Fichiers à ignorer pour Python**
Lors de l'initialisation d'un dépôt Git dans PyCharm, l'IDE suggère automatiquement d'ajouter un `.gitignore` pour Python qui exclut :
- `__pycache__/` : dossiers de cache Python
- `*.pyc` : fichiers compilés Python
- `.pytest_cache/` : cache de pytest
- `venv/`, `env/`, `.venv/` : environnements virtuels
- `.idea/` : fichiers de configuration PyCharm
- `*.egg-info/` : fichiers d'installation de packages

**Détection des environnements virtuels**
PyCharm détecte automatiquement si vous utilisez un environnement virtuel (venv) et vous rappelle de ne PAS le committer. Les environnements virtuels doivent toujours être dans le `.gitignore` !

**Integration avec requirements.txt**
Quand vous installez des packages Python, PyCharm peut automatiquement mettre à jour votre `requirements.txt`. Pensez à committer ce fichier car il fait partie du code source de votre projet !

#### Configuration recommandée pour Python

**Toujours créer un .gitignore**
Lors de la création d'un nouveau projet Python :
1. PyCharm propose d'initialiser un dépôt Git
2. Acceptez et laissez PyCharm créer le `.gitignore` automatiquement
3. Vérifiez que les dossiers `venv/` ou `.venv/` sont bien listés

**Fichiers à committer dans un projet Python**
✅ À committer :
- Code source (`.py`)
- `requirements.txt` ou `Pipfile`
- `README.md`
- Fichiers de configuration
- Tests
- Documentation

❌ À NE PAS committer :
- Environnements virtuels (`venv/`, `env/`)
- Fichiers compilés (`__pycache__/`, `*.pyc`)
- Configuration locale (`.env` avec des secrets)
- Bases de données de développement (`.sqlite3`, `.db`)
- Fichiers de l'IDE (`.idea/`)

#### Workflow Git typique pour un projet Python

1. **Initialisation**
   - Créer un projet Python dans PyCharm
   - Initialiser Git : `VCS → Enable Version Control Integration`
   - Laisser PyCharm créer le `.gitignore`

2. **Premier commit**
   - Créer votre environnement virtuel
   - Installer vos dépendances
   - Créer/mettre à jour `requirements.txt`
   - Committer : code source + requirements.txt + .gitignore

3. **Développement**
   - Créer une branche pour chaque fonctionnalité
   - Committer régulièrement
   - Pousser vers le dépôt distant

4. **Collaboration**
   - Un collègue clone le dépôt
   - Il crée son propre environnement virtuel local (non commité)
   - Il installe les dépendances depuis `requirements.txt` : `pip install -r requirements.txt`
   - Il peut commencer à développer !

---

### Comparaison : VSCode vs IntelliJ/PyCharm

| Critère | VSCode | IntelliJ / PyCharm |
|---------|--------|-------------------|
| **Prix** | Gratuit et open-source | IntelliJ Community (gratuit) / Ultimate & PyCharm (payant) |
| **Léger / Rapide** | Très léger et rapide | Plus lourd, mais très puissant |
| **Intégration Git native** | Bonne, améliorable avec extensions | Excellente, très complète |
| **Graphe des commits** | Nécessite extension (Git Graph) | Intégré et magnifique |
| **Résolution de conflits** | Correcte | Excellente (3 panneaux) |
| **Courbe d'apprentissage** | Facile | Moyenne |
| **Personnalisation** | Très personnalisable (extensions) | Moins de personnalisation nécessaire |
| **Pour débutants** | Excellent | Très bon |

#### Quelle IDE choisir ?

**Choisissez VSCode si :**
- Vous voulez un éditeur gratuit et léger
- Vous travaillez sur plusieurs langages
- Vous aimez personnaliser votre environnement
- Vous avez un ordinateur avec ressources limitées
- Vous débutez et voulez quelque chose de simple

**Choisissez IntelliJ si :**
- Vous développez principalement en Java/Kotlin
- Vous voulez la meilleure intégration Git du marché
- Vous appréciez un IDE complet "out of the box"
- Les performances de votre machine le permettent

**Choisissez PyCharm si :**
- Vous développez principalement en Python
- Vous voulez des outils Python avancés (débogueur, tests, etc.)
- Vous appréciez une intégration Git excellente
- Vous êtes prêt à investir dans un outil professionnel (version payante)

---

### Conseils généraux pour Git dans les IDE

#### Comprendre ce que fait l'IDE

Même si les IDE facilitent l'utilisation de Git, **il est important de comprendre ce qui se passe en arrière-plan**. Les IDE exécutent les mêmes commandes Git que vous taperiez dans un terminal.

Par exemple :
- Quand vous cliquez sur "Commit", l'IDE exécute `git commit`
- Quand vous cliquez sur "Push", c'est `git push` qui est exécuté
- Le graphe de commits ? C'est `git log --graph` en visuel

Comprendre ces concepts vous permet de :
- Déboguer quand quelque chose ne fonctionne pas
- Utiliser Git en ligne de commande si nécessaire
- Mieux maîtriser votre outil

#### Consulter l'historique des commandes

La plupart des IDE montrent les commandes Git exécutées :
- **VSCode** : Ouvrez l'Output (View → Output) et sélectionnez "Git" dans le menu déroulant
- **IntelliJ/PyCharm** : Onglet "Console" dans la fenêtre Git

C'est éducatif et utile pour comprendre ce que l'IDE fait !

#### Combiner IDE et terminal

N'ayez pas peur d'utiliser le terminal intégré pour des commandes spécifiques :
- Les IDE sont parfaits pour les opérations courantes
- Le terminal est idéal pour les commandes avancées ou les scripts

Par exemple :
- Utilisez l'IDE pour committer et pousser
- Utilisez le terminal pour un rebase interactif complexe
- Utilisez l'IDE pour visualiser l'historique
- Utilisez le terminal pour un `git bisect`

#### Faire attention aux fichiers sensibles

Les IDE rendent si facile de faire des commits qu'on peut accidentellement committer des fichiers sensibles :
- Mots de passe dans des fichiers de configuration
- Clés API dans le code
- Fichiers volumineux

**Bonnes pratiques :**
- Toujours vérifier les fichiers avant de committer
- Utiliser un `.gitignore` complet
- Ne jamais committer de secrets en dur dans le code
- Utiliser des variables d'environnement pour les configurations sensibles

#### Rester vigilant avec les gros commits

Les IDE permettent de committer facilement des dizaines de fichiers d'un coup. Mais souvenez-vous : **les commits atomiques sont meilleurs**.

Au lieu d'un gros commit avec 20 fichiers :
- Faites plusieurs petits commits logiques
- Chaque commit doit représenter une unité de travail cohérente
- Les messages de commit doivent être descriptifs

Les IDE facilitent le "staging partiel" : vous pouvez sélectionner individuellement les fichiers ou même les portions de fichiers à committer.

---

### Conclusion

Les IDE modernes offrent une intégration Git excellente qui rend le contrôle de version accessible et visuel. Que vous utilisiez **VSCode**, **IntelliJ IDEA** ou **PyCharm**, vous avez accès à des outils puissants :

- **Visualisation claire** de vos modifications et de l'historique
- **Opérations courantes** en quelques clics (commit, push, pull, branches)
- **Résolution de conflits** avec des outils visuels intuitifs
- **Indicateurs visuels** qui montrent l'état de vos fichiers en temps réel

Les trois IDE ont leurs forces :
- **VSCode** : léger, gratuit, très personnalisable
- **IntelliJ/PyCharm** : intégration Git supérieure, outils professionnels

L'important est de :
1. **Comprendre les concepts Git** (les modules précédents vous y ont préparé)
2. **Utiliser les outils de votre IDE** pour les opérations courantes
3. **Ne pas hésiter à utiliser le terminal** pour les opérations avancées
4. **Toujours vérifier ce que vous committez**

Avec ces connaissances, vous êtes équipé pour utiliser Git efficacement dans votre environnement de développement quotidien. Dans la prochaine section, nous explorerons les extensions et outils complémentaires qui peuvent encore améliorer votre expérience Git.

⏭️ [Extensions Git utiles](/module-09-outils-et-integration/03-extensions-git-utiles.md)
