🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 8. Submodules et sous-projets

### Introduction

Imaginez que vous construisez une maison et que vous voulez utiliser des éléments préfabriqués comme des fenêtres ou des portes. Vous ne voulez pas les fabriquer vous-même, mais les intégrer dans votre construction. C'est exactement ce que font les **submodules** Git !

Un **submodule** est un dépôt Git complet intégré à l'intérieur d'un autre dépôt Git. Cela permet d'inclure et de gérer des projets externes comme dépendances, tout en gardant leurs historiques séparés.

**Cas d'usage typiques :**
- Intégrer une bibliothèque que vous maintenez séparément
- Partager du code commun entre plusieurs projets
- Gérer des thèmes ou plugins dans un projet web
- Inclure des dépendances qui ne sont pas sur npm/pip/etc.

---

### Qu'est-ce qu'un submodule ?

Un submodule est :
- Un **dépôt Git complet** à l'intérieur d'un autre dépôt
- Pointé vers un **commit spécifique** du dépôt externe
- Référencé dans le fichier `.gitmodules` du projet parent
- **Indépendant** : il a son propre historique, ses propres branches

**Analogie :** C'est comme avoir un raccourci vers un autre projet, mais avec le contrôle précis de la version que vous utilisez.

**Exemple de structure :**

```
mon-projet/               ← Dépôt principal
├── .git/
├── .gitmodules          ← Fichier de configuration des submodules
├── src/
├── lib/
│   └── ma-bibliotheque/ ← Submodule (dépôt Git séparé)
│       ├── .git/
│       └── ...
└── themes/
    └── mon-theme/       ← Autre submodule
        ├── .git/
        └── ...
```

---

### Pourquoi utiliser des submodules ?

#### Avantages

✅ **Versionnement précis** : Vous contrôlez exactement quelle version de la dépendance vous utilisez

✅ **Séparation des préoccupations** : Chaque projet garde son propre historique

✅ **Réutilisabilité** : Partagez du code entre plusieurs projets

✅ **Mises à jour contrôlées** : Mettez à jour les dépendances quand vous le décidez

✅ **Développement parallèle** : Travaillez sur le projet principal et les dépendances simultanément

#### Inconvénients

❌ **Complexité** : Plus difficile à comprendre et à utiliser que des dépendances classiques

❌ **Commandes supplémentaires** : Nécessite des commandes spéciales pour cloner et mettre à jour

❌ **Risque d'oubli** : On peut oublier de pousser les changements du submodule

❌ **Courbe d'apprentissage** : Les débutants peuvent être perdus

---

### Créer un submodule

#### Ajouter un submodule existant

```bash
git submodule add <url-du-depot> <chemin-local>
```

**Exemple pratique :**

```bash
# Ajouter une bibliothèque JavaScript
git submodule add https://github.com/user/awesome-lib.git lib/awesome-lib

# Git crée :
# 1. Le dossier lib/awesome-lib avec le contenu du dépôt
# 2. Le fichier .gitmodules avec la configuration
# 3. Une entrée dans l'index du dépôt parent
```

**Ce qui se passe :**

1. Git clone le dépôt dans le chemin spécifié
2. Un fichier `.gitmodules` est créé (ou mis à jour)
3. Le submodule est ajouté à l'index du dépôt parent

**Contenu du fichier `.gitmodules` :**

```ini
[submodule "lib/awesome-lib"]
    path = lib/awesome-lib
    url = https://github.com/user/awesome-lib.git
```

#### Commiter l'ajout du submodule

```bash
git add .gitmodules lib/awesome-lib
git commit -m "Add awesome-lib submodule"
```

**Important :** Le dépôt parent ne stocke PAS le contenu du submodule, seulement :
- Le chemin du submodule
- L'URL du dépôt distant
- Le commit spécifique (hash) utilisé

---

### Cloner un projet avec submodules

Quand quelqu'un clone votre projet, les submodules ne sont **pas clonés automatiquement** !

#### Méthode 1 : Clone puis initialisation

```bash
# 1. Cloner le projet principal
git clone https://github.com/user/mon-projet.git
cd mon-projet

# 2. Les dossiers de submodules existent mais sont vides !
ls lib/awesome-lib  # Vide

# 3. Initialiser les submodules
git submodule init

# 4. Récupérer le contenu
git submodule update
```

#### Méthode 2 : Clone récursif (recommandé)

```bash
git clone --recursive https://github.com/user/mon-projet.git
```

ou (syntaxe alternative) :

```bash
git clone --recurse-submodules https://github.com/user/mon-projet.git
```

Cette commande clone le projet ET tous ses submodules en une seule fois ! 🎉

#### Si vous avez déjà cloné sans --recursive

```bash
# Vous êtes dans un projet déjà cloné avec des submodules vides
git submodule update --init --recursive
```

Cette commande :
- `--init` : Initialise les submodules
- `--recursive` : Initialise aussi les submodules des submodules (si applicable)

---

### Travailler avec des submodules

#### Voir l'état des submodules

```bash
git submodule status
```

**Sortie typique :**

```
 a1b2c3d4e5f6 lib/awesome-lib (v1.2.0)
 e4f5g6h7i8j9 themes/mon-theme (heads/main)
```

**Signification des préfixes :**
- Espace : Submodule à jour
- `-` : Submodule non initialisé
- `+` : Le commit actuel du submodule diffère de celui enregistré dans le projet parent
- `U` : Submodule avec des conflits de merge

#### Entrer dans un submodule

Les submodules sont des dépôts Git normaux :

```bash
cd lib/awesome-lib
git status
git log
git branch
# Toutes les commandes Git fonctionnent !
```

#### Modifier un submodule

**Scénario :** Vous devez corriger un bug dans une bibliothèque.

```bash
# 1. Entrer dans le submodule
cd lib/awesome-lib

# 2. Créer une branche pour vos modifications
git checkout -b fix-bug

# 3. Faire vos modifications
# ... éditer des fichiers ...

# 4. Commiter dans le submodule
git add .
git commit -m "fix: Correct calculation bug"

# 5. Pousser vers le dépôt distant du submodule
git push origin fix-bug

# 6. Revenir au projet parent
cd ../..

# 7. Le projet parent voit que le submodule a changé
git status
# On branch main
# Changes not staged for commit:
#   modified:   lib/awesome-lib (new commits)

# 8. Commiter la nouvelle référence dans le projet parent
git add lib/awesome-lib
git commit -m "Update awesome-lib to fix bug"

# 9. Pousser le projet parent
git push
```

**Point crucial :** Vous devez commiter ET pousser :
1. D'abord dans le submodule
2. Puis dans le projet parent

---

### Mettre à jour un submodule

#### Mettre à jour vers le dernier commit

```bash
# Entrer dans le submodule
cd lib/awesome-lib

# Récupérer les dernières modifications
git fetch
git merge origin/main
# ou git pull origin main

# Revenir au projet parent
cd ../..

# Commiter la mise à jour
git add lib/awesome-lib
git commit -m "Update awesome-lib to latest version"
```

#### Mettre à jour tous les submodules en une commande

```bash
git submodule update --remote
```

Cette commande met à jour tous les submodules vers le dernier commit de leur branche par défaut.

**Pour mettre à jour un submodule spécifique :**

```bash
git submodule update --remote lib/awesome-lib
```

#### Configurer la branche de suivi

Par défaut, Git utilise la branche `main` (ou `master`). Pour changer :

```bash
git config -f .gitmodules submodule.lib/awesome-lib.branch develop
```

Maintenant `git submodule update --remote` suivra la branche `develop`.

---

### Supprimer un submodule

Supprimer un submodule est plus complexe qu'on ne le pense !

**Étapes complètes :**

```bash
# 1. Désenregistrer le submodule
git submodule deinit lib/awesome-lib

# 2. Supprimer le dossier du submodule du dépôt
git rm lib/awesome-lib

# 3. Commiter la suppression
git commit -m "Remove awesome-lib submodule"

# 4. Supprimer le dossier .git/modules (optionnel mais recommandé)
rm -rf .git/modules/lib/awesome-lib
```

**Pourquoi c'est compliqué ?**

Git stocke les informations des submodules à plusieurs endroits :
- `.gitmodules`
- `.git/config`
- `.git/modules/`

La commande `git submodule deinit` nettoie la plupart, mais pas tout.

---

### Cas d'usage pratiques

#### Cas 1 : Bibliothèque partagée entre plusieurs projets

**Structure :**

```
projet-web/
└── lib/
    └── shared-utils/  ← Submodule

projet-mobile/
└── lib/
    └── shared-utils/  ← Même submodule

projet-desktop/
└── lib/
    └── shared-utils/  ← Même submodule
```

**Avantage :** Modifiez `shared-utils` une fois, tous les projets peuvent l'utiliser.

```bash
# Dans chaque projet
git submodule add https://github.com/company/shared-utils.git lib/shared-utils
```

#### Cas 2 : Thèmes WordPress

```bash
# Projet WordPress
cd wp-content/themes

# Ajouter un thème personnalisé comme submodule
git submodule add https://github.com/company/custom-theme.git custom-theme
```

**Avantage :** Développez et versionnez le thème indépendamment du site WordPress.

#### Cas 3 : Microservices avec dépendances communes

```bash
# Service API
git submodule add https://github.com/company/common-models.git shared/models
git submodule add https://github.com/company/common-auth.git shared/auth

# Service Web
git submodule add https://github.com/company/common-models.git shared/models
git submodule add https://github.com/company/common-ui.git shared/ui
```

#### Cas 4 : Documentation séparée

```bash
# Projet principal
git submodule add https://github.com/company/docs.git documentation
```

**Avantage :** L'équipe de documentation travaille indépendamment.

---

### Commandes avancées

#### Exécuter une commande dans tous les submodules

```bash
git submodule foreach <commande>
```

**Exemples :**

```bash
# Voir le statut de tous les submodules
git submodule foreach git status

# Faire un pull dans tous les submodules
git submodule foreach git pull origin main

# Créer une branche dans tous les submodules
git submodule foreach git checkout -b feature/new-feature

# Afficher la dernière ligne du log de chaque submodule
git submodule foreach 'git log -1 --oneline'
```

#### Cloner avec des submodules en parallèle

```bash
git clone --recursive --jobs 4 https://github.com/user/projet.git
```

Télécharge 4 submodules en parallèle pour aller plus vite.

#### Mettre à jour et fusionner automatiquement

```bash
git submodule update --remote --merge
```

ou

```bash
git submodule update --remote --rebase
```

---

### Pièges courants et solutions

#### Piège 1 : Oublier de pousser le submodule

**Problème :**

```bash
# Vous modifiez le submodule
cd lib/awesome-lib
git commit -m "Fix bug"

# Vous committez dans le projet parent
cd ../..
git add lib/awesome-lib
git commit -m "Update lib"
git push  # ❌ Mais vous oubliez de pousser le submodule !
```

**Conséquence :** Vos collègues ne peuvent pas récupérer le commit du submodule (il n'existe que localement).

**Solution :**

```bash
# Toujours pousser le submodule en premier
cd lib/awesome-lib
git push

# Puis pousser le projet parent
cd ../..
git push
```

**Prévention automatique :**

```bash
# Configurer Git pour vérifier
git config --global push.recurseSubmodules check
# Git refuse le push si un submodule n'est pas poussé

# Ou mieux : pousser automatiquement
git config --global push.recurseSubmodules on-demand
```

#### Piège 2 : Submodule en detached HEAD

**Problème :**

Les submodules pointent vers un commit spécifique, pas une branche.

```bash
cd lib/awesome-lib
git branch
# * (HEAD detached at a1b2c3d)
```

**Solution :**

Toujours créer une branche avant de modifier :

```bash
cd lib/awesome-lib
git checkout -b my-changes
# Maintenant vous êtes sur une branche
```

#### Piège 3 : Conflits de merge sur les submodules

**Problème :**

```bash
git pull
# CONFLICT (submodule): Merge conflict in lib/awesome-lib
```

**Solution :**

```bash
# 1. Entrer dans le submodule
cd lib/awesome-lib

# 2. Résoudre comme un conflit normal
git fetch
git merge origin/main
# Résoudre les conflits si nécessaire

# 3. Revenir au projet parent
cd ../..

# 4. Indiquer que c'est résolu
git add lib/awesome-lib
git commit
```

#### Piège 4 : Cloner sans --recursive

**Problème :** Les dossiers de submodules sont vides après le clone.

**Solution :**

```bash
git submodule update --init --recursive
```

---

### Alternative : Git Subtree

**Git subtree** est une alternative aux submodules, plus simple mais moins flexible.

#### Différences principales

| Submodules | Subtree |
|------------|---------|
| Dépôt séparé | Code intégré directement |
| Commit spécifique | Historique fusionné |
| Commandes spéciales requises | Commandes Git normales |
| Modifications trackées séparément | Modifications dans le projet principal |
| Plus complexe | Plus simple |
| Meilleur pour développement actif | Meilleur pour code stable |

#### Utilisation basique de subtree

```bash
# Ajouter un subtree
git subtree add --prefix=lib/awesome-lib \
  https://github.com/user/awesome-lib.git main --squash

# Mettre à jour
git subtree pull --prefix=lib/awesome-lib \
  https://github.com/user/awesome-lib.git main --squash

# Pousser des modifications vers le projet externe
git subtree push --prefix=lib/awesome-lib \
  https://github.com/user/awesome-lib.git main
```

**Quand utiliser subtree plutôt que submodules ?**
- Dépendance stable, rarement mise à jour
- Équipe peu expérimentée avec Git
- Vous voulez simplifier le workflow

---

### Configuration avancée

#### Configurer un alias pour les submodules

```bash
# Alias pour mettre à jour tous les submodules
git config --global alias.supdate 'submodule update --remote --merge'

# Utilisation
git supdate
```

#### Ignorer les modifications d'un submodule

Si vous ne voulez pas que Git vous avertisse des modifications dans un submodule :

```bash
git config -f .gitmodules submodule.lib/awesome-lib.ignore all
```

Options pour `ignore` :
- `all` : Ignore toutes les modifications
- `dirty` : Ignore les modifications non commitées
- `untracked` : Ignore les fichiers non trackés

---

### Workflow complet avec submodules

Voici un workflow typique pour un projet utilisant des submodules :

#### 1. Configuration initiale

```bash
# Créer le projet principal
mkdir mon-projet
cd mon-projet
git init

# Ajouter des submodules
git submodule add https://github.com/user/lib1.git lib/lib1
git submodule add https://github.com/user/lib2.git lib/lib2

# Commiter
git add .
git commit -m "Initial commit with submodules"

# Créer le dépôt distant et pousser
git remote add origin https://github.com/user/mon-projet.git
git push -u origin main
```

#### 2. Nouveau développeur clone le projet

```bash
git clone --recursive https://github.com/user/mon-projet.git
cd mon-projet
```

#### 3. Travailler sur un submodule

```bash
# Entrer dans le submodule
cd lib/lib1

# Créer une branche
git checkout -b feature/new-feature

# Développer
# ... modifications ...
git add .
git commit -m "feat: Add new feature"

# Pousser le submodule
git push origin feature/new-feature

# Revenir au projet parent
cd ../..

# Mettre à jour la référence
git add lib/lib1
git commit -m "Update lib1 with new feature"
git push
```

#### 4. Mettre à jour les submodules

```bash
# Récupérer les dernières modifications
git pull

# Mettre à jour les submodules
git submodule update --recursive --remote

# Si des modifications ont eu lieu
git add lib/
git commit -m "Update submodules"
git push
```

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git submodule add <url> <path>` | Ajoute un submodule |
| `git clone --recursive <url>` | Clone avec les submodules |
| `git submodule init` | Initialise les submodules |
| `git submodule update` | Récupère le contenu des submodules |
| `git submodule update --init --recursive` | Initialise et récupère tout |
| `git submodule update --remote` | Met à jour vers le dernier commit |
| `git submodule status` | Affiche l'état des submodules |
| `git submodule foreach <cmd>` | Exécute une commande dans chaque submodule |
| `git submodule deinit <path>` | Désenregistre un submodule |
| `git rm <path>` | Supprime un submodule |

---

### Conseils et bonnes pratiques

✅ **Utilisez --recursive lors du clone** : Configurez un alias pour ne jamais oublier

```bash
alias gclone='git clone --recursive'
```

✅ **Committez les submodules en premier** : Toujours pousser le submodule avant le projet parent

✅ **Documentez l'utilisation des submodules** : Créez un README expliquant comment cloner et mettre à jour

✅ **Utilisez push.recurseSubmodules** :

```bash
git config --global push.recurseSubmodules on-demand
```

✅ **Créez des branches dans les submodules** : Évitez le detached HEAD

✅ **Vérifiez régulièrement le statut** :

```bash
git submodule foreach git status
```

❌ **N'utilisez pas de submodules pour npm/pip packages** : Utilisez plutôt les gestionnaires de paquets natifs

❌ **Évitez les submodules imbriqués** : C'est très complexe à gérer

❌ **Ne modifiez pas directement sans branche** : Toujours créer une branche avant de modifier

---

### Déboguer les problèmes de submodules

#### Réinitialiser un submodule corrompu

```bash
# Supprimer complètement
rm -rf lib/awesome-lib
rm -rf .git/modules/lib/awesome-lib

# Réinitialiser
git submodule update --init lib/awesome-lib
```

#### Voir les différences de commit

```bash
# Comparer les commits de submodule
git diff lib/awesome-lib
```

**Sortie :**

```
-Subproject commit a1b2c3d4e5f6
+Subproject commit e7f8g9h0i1j2
```

#### Trouver quel commit du parent utilise quel commit du submodule

```bash
git log --oneline -- lib/awesome-lib
```

---

### Conclusion

Les **submodules** sont un outil puissant pour gérer des dépendances Git dans Git, mais ils ajoutent de la complexité. Utilisez-les quand :

**✅ Utilisez les submodules quand :**
- Vous développez activement plusieurs projets liés
- Vous voulez un contrôle précis des versions
- Vous avez du code partagé entre projets
- Votre équipe est à l'aise avec Git

**❌ Évitez les submodules quand :**
- Vous avez des dépendances stables (utilisez npm, pip, etc.)
- Votre équipe débute avec Git
- Vous voulez un workflow simple
- Les dépendances changent rarement

**Points clés à retenir :**

- Submodule = dépôt Git dans un dépôt Git
- Toujours cloner avec `--recursive`
- Pousser le submodule AVANT le parent
- Les submodules pointent vers un commit spécifique
- Créer une branche avant de modifier un submodule
- Considérez `git subtree` comme alternative plus simple

**Commandes essentielles :**

```bash
# Clone
git clone --recursive <url>

# Mise à jour
git submodule update --init --recursive

# Modification
cd submodule && git checkout -b feature && ...

# Mise à jour vers latest
git submodule update --remote
```

Avec de la pratique et une bonne documentation, les submodules peuvent grandement améliorer l'organisation de vos projets ! 📦

---

**Prochaine étape :** Module 6.9 - Hooks Git (automatisation)

⏭️ [Hooks Git (automatisation)](/module-06-fonctions-avancees/09-hooks-git.md)
