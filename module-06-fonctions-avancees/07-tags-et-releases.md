🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 7. Tags et releases

### Introduction

Imaginez que vous développez une application et que vous voulez marquer des moments importants : la version 1.0, la version 2.0, etc. Comment faire pour retrouver facilement le code exact de chaque version ? C'est là qu'interviennent les **tags** !

Un **tag** est comme un marque-page permanent dans l'historique de votre projet. Il permet de donner un nom significatif (comme "v1.0.0" ou "release-2024") à un commit spécifique, pour pouvoir y revenir facilement.

Les **releases** sont des tags enrichis avec des notes de version, des fichiers téléchargeables et de la documentation, généralement utilisés sur GitHub, GitLab ou Bitbucket pour distribuer votre logiciel.

**Pourquoi utiliser des tags ?**
- Marquer les versions de production (v1.0, v2.0)
- Retrouver facilement une version déployée
- Créer des releases téléchargeables
- Documenter l'historique des versions
- Faciliter le déploiement et le rollback

---

### Les deux types de tags

Git propose deux types de tags : **légers** et **annotés**.

#### Tags légers (lightweight)

Un tag léger est simplement un pointeur vers un commit, comme une branche qui ne bouge jamais.

**Caractéristiques :**
- Simple référence à un commit
- Pas d'informations supplémentaires
- Rapide à créer
- Idéal pour les marqueurs temporaires ou personnels

#### Tags annotés (annotated)

Un tag annoté est un objet Git complet avec des métadonnées.

**Caractéristiques :**
- Contient le nom du créateur, la date, un message
- Peut être signé cryptographiquement (GPG)
- Recommandé pour les versions officielles
- Stocké comme objet Git complet

**Comparaison :**

| Tag léger | Tag annoté |
|-----------|------------|
| Pointeur simple | Objet Git complet |
| Pas de métadonnées | Auteur, date, message |
| `git tag v1.0` | `git tag -a v1.0 -m "Version 1.0"` |
| Usage personnel | Usage public/production |

**Recommandation :** Utilisez toujours des tags annotés pour les versions officielles.

---

### Créer des tags

#### Créer un tag léger

```bash
git tag v1.0.0
```

Crée un tag nommé "v1.0.0" sur le commit actuel.

#### Créer un tag annoté

```bash
git tag -a v1.0.0 -m "Version 1.0.0 - Initial release"
```

**Options :**
- `-a` : Crée un tag annoté
- `-m` : Message du tag (comme pour un commit)

**Sans l'option -m**, Git ouvre votre éditeur pour saisir un message détaillé :

```bash
git tag -a v1.0.0
```

Vous pouvez alors écrire un message plus long :

```
Version 1.0.0 - Initial Release

Features:
- User authentication
- Payment processing
- Email notifications

This is our first production-ready release.
```

#### Créer un tag sur un ancien commit

```bash
git tag -a v0.9.0 a1b2c3d -m "Version 0.9.0 - Beta release"
```

Crée le tag sur le commit `a1b2c3d` au lieu du commit actuel.

**Exemple pratique :**

```bash
# Voir l'historique pour trouver le bon commit
git log --oneline
# e5f6g7h Fix critical bug
# a1b2c3d Add payment feature
# h7i8j9k Initial commit

# Créer un tag sur le commit qui ajoutait le paiement
git tag -a v0.5.0 a1b2c3d -m "Version 0.5.0 - Payment feature added"
```

---

### Lister les tags

#### Afficher tous les tags

```bash
git tag
```

**Sortie typique :**

```
v0.5.0  
v0.9.0  
v1.0.0  
v1.1.0  
v2.0.0
```

#### Afficher les tags avec un pattern

```bash
git tag -l "v1.*"
```

Affiche uniquement les tags qui correspondent au pattern.

**Exemples :**

```bash
# Tous les tags de la version 1
git tag -l "v1.*"
# v1.0.0
# v1.1.0
# v1.2.0

# Tous les tags beta
git tag -l "*-beta"
# v1.0.0-beta
# v2.0.0-beta

# Tous les tags de 2024
git tag -l "*2024*"
# release-2024-01
# release-2024-02
```

#### Afficher les détails d'un tag annoté

```bash
git show v1.0.0
```

**Sortie typique :**

```
tag v1.0.0  
Tagger: Alice Martin <alice@example.com>  
Date:   Mon Oct 15 14:30:00 2024 +0200

Version 1.0.0 - Initial Release

Features:
- User authentication
- Payment processing

commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6  
Author: Alice Martin <alice@example.com>  
Date:   Mon Oct 15 14:00:00 2024 +0200

    Final touches for v1.0

diff --git a/README.md b/README.md
...
```

---

### Partager des tags avec le dépôt distant

**Important :** Par défaut, `git push` ne pousse **pas** les tags ! Il faut le faire explicitement.

#### Pousser un tag spécifique

```bash
git push origin v1.0.0
```

Envoie uniquement le tag v1.0.0 vers le dépôt distant.

#### Pousser tous les tags

```bash
git push origin --tags
```

Envoie tous les tags locaux vers le dépôt distant.

**Attention :** Cette commande pousse aussi bien les tags légers qu'annotés.

#### Pousser uniquement les tags annotés

```bash
git push origin --follow-tags
```

Recommandé : pousse uniquement les tags annotés.

**Configuration pour toujours pousser les tags :**

```bash
git config --global push.followTags true
```

Avec cette configuration, `git push` poussera automatiquement les tags annotés.

---

### Supprimer des tags

#### Supprimer un tag local

```bash
git tag -d v1.0.0
```

Supprime le tag localement.

#### Supprimer un tag distant

```bash
git push origin --delete v1.0.0
```

ou (syntaxe alternative) :

```bash
git push origin :refs/tags/v1.0.0
```

**Pour supprimer complètement un tag (local + distant) :**

```bash
# 1. Supprimer localement
git tag -d v1.0.0

# 2. Supprimer sur le distant
git push origin --delete v1.0.0
```

---

### Checkout vers un tag

#### Voir le code à un tag spécifique

```bash
git checkout v1.0.0
```

**Attention :** Cela vous met en mode "detached HEAD" - vous n'êtes sur aucune branche.

**Sortie typique :**

```
Note: switching to 'v1.0.0'.

You are in 'detached HEAD' state. You can look around, make experimental  
changes and commit them, and you can discard any commits you make in this  
state without impacting any branches.
```

#### Créer une branche à partir d'un tag

```bash
git checkout -b fix-v1.0 v1.0.0
```

Crée une nouvelle branche `fix-v1.0` basée sur le tag `v1.0.0`. Utile pour faire des correctifs sur une ancienne version.

---

### Versionnement sémantique (Semantic Versioning)

Le **versionnement sémantique** (SemVer) est une convention de nommage des versions très utilisée.

#### Format : MAJOR.MINOR.PATCH

```
v2.4.1
│ │ │
│ │ └─ PATCH : Corrections de bugs
│ └─── MINOR : Nouvelles fonctionnalités (rétrocompatibles)
└───── MAJOR : Changements incompatibles
```

**Exemples :**

| Version | Type de changement | Exemple |
|---------|-------------------|---------|
| v1.0.0 → v1.0.1 | PATCH (bug fix) | Correction d'un bug |
| v1.0.1 → v1.1.0 | MINOR (feature) | Ajout d'une nouvelle fonctionnalité |
| v1.1.0 → v2.0.0 | MAJOR (breaking) | Changement d'API incompatible |

#### Règles de versionnement sémantique

**Version 0.x.x** : Développement initial, API instable

```
v0.1.0 → Première version expérimentale  
v0.2.0 → Ajout de fonctionnalités  
v0.9.0 → Version presque stable
```

**Version 1.0.0** : Première version stable publique

```
v1.0.0 → API stable, prête pour la production
```

**PATCH (x.x.1)** : Corrections de bugs rétrocompatibles

```
v1.0.0 → v1.0.1 : Fix bug dans le login  
v1.0.1 → v1.0.2 : Fix typo dans les emails
```

**MINOR (x.1.x)** : Nouvelles fonctionnalités rétrocompatibles

```
v1.0.2 → v1.1.0 : Ajout de l'export PDF  
v1.1.0 → v1.2.0 : Ajout du dark mode
```

**MAJOR (2.x.x)** : Changements incompatibles

```
v1.2.0 → v2.0.0 : Refonte complète de l'API  
v2.0.0 → v3.0.0 : Changement de framework
```

#### Suffixes optionnels

```bash
# Pre-release
v1.0.0-alpha     # Version alpha (très instable)  
v1.0.0-beta      # Version beta (tests)  
v1.0.0-rc.1      # Release candidate 1

# Build metadata
v1.0.0+20241015  # Avec date de build  
v1.0.0+001       # Avec numéro de build
```

**Ordre de versionnement :**

```
v1.0.0-alpha < v1.0.0-beta < v1.0.0-rc.1 < v1.0.0
```

---

### Cas d'usage pratiques

#### Cas 1 : Première release d'un projet

```bash
# Votre projet est prêt pour la production
git checkout main  
git pull

# Créer le tag de la première version stable
git tag -a v1.0.0 -m "Version 1.0.0 - First stable release

Features:
- User authentication
- Dashboard
- API endpoints
- Documentation

This is our first production-ready version."

# Pousser le tag
git push origin v1.0.0

# Créer la release sur GitHub (voir section suivante)
```

#### Cas 2 : Hotfix sur une version en production

```bash
# Un bug critique est découvert en v1.0.0

# 1. Créer une branche de fix depuis le tag
git checkout -b hotfix-1.0.1 v1.0.0

# 2. Corriger le bug
# ... modifications ...
git add .  
git commit -m "fix: Critical security vulnerability"

# 3. Créer le nouveau tag
git tag -a v1.0.1 -m "Version 1.0.1 - Security hotfix

Fixed:
- Critical security vulnerability in auth module"

# 4. Pousser
git push origin hotfix-1.0.1  
git push origin v1.0.1

# 5. Merger dans main si nécessaire
git checkout main  
git merge hotfix-1.0.1  
git push
```

#### Cas 3 : Versions beta avant la release

```bash
# Version beta pour les tests
git tag -a v2.0.0-beta -m "Version 2.0.0-beta - Testing release"  
git push origin v2.0.0-beta

# Après tests, release candidate
git tag -a v2.0.0-rc.1 -m "Version 2.0.0-rc.1 - Release candidate"  
git push origin v2.0.0-rc.1

# Après validation finale
git tag -a v2.0.0 -m "Version 2.0.0 - Major release"  
git push origin v2.0.0
```

#### Cas 4 : Tag pour un déploiement

```bash
# Créer un tag de déploiement avec la date
git tag -a deploy-prod-$(date +%Y%m%d) -m "Production deployment $(date +%Y-%m-%d)"  
git push origin deploy-prod-$(date +%Y%m%d)

# Exemple : deploy-prod-20241015
```

---

### Releases sur GitHub

Les **releases** sont la surcouche que GitHub/GitLab ajoute aux tags Git pour créer des versions téléchargeables.

#### Créer une release sur GitHub

**Méthode 1 : Via l'interface web**

1. Aller sur votre dépôt GitHub
2. Cliquer sur "Releases" dans le menu de droite
3. Cliquer sur "Create a new release"
4. Remplir les informations :
   - **Tag version** : v1.0.0 (créez un nouveau tag ou sélectionnez-en un existant)
   - **Release title** : Version 1.0.0 - Initial Release
   - **Description** : Notes de version détaillées
   - **Binaries** : Téléverser des fichiers compilés (optionnel)
   - **Pre-release** : Cocher si c'est une version beta/alpha
5. Cliquer sur "Publish release"

**Méthode 2 : Via GitHub CLI**

```bash
# Installer GitHub CLI : https://cli.github.com/

# Créer une release
gh release create v1.0.0 \
  --title "Version 1.0.0 - Initial Release" \
  --notes "## Features
- User authentication
- Payment processing

## Bug fixes
- Fixed login issue" \
  dist/*.zip
```

**Méthode 3 : Via API GitHub**

```bash
# Créer un tag localement
git tag -a v1.0.0 -m "Version 1.0.0"  
git push origin v1.0.0

# Créer la release via curl
curl -X POST \
  -H "Authorization: token YOUR_GITHUB_TOKEN" \
  https://api.github.com/repos/user/repo/releases \
  -d '{
    "tag_name": "v1.0.0",
    "name": "Version 1.0.0",
    "body": "## Features\n- New authentication system",
    "draft": false,
    "prerelease": false
  }'
```

#### Contenu d'une bonne release

Une release GitHub devrait contenir :

**1. Titre clair :**
```
Version 2.0.0 - Major Update
```

**2. Catégories de changements :**
```markdown
## ✨ New Features
- Dark mode support
- Export to PDF
- Two-factor authentication

## 🐛 Bug Fixes
- Fixed crash on large files
- Corrected timezone display

## 🔄 Changes
- Updated dependencies
- Improved performance

## ⚠️ Breaking Changes
- API endpoint changed from /api/v1 to /api/v2
- `oldFunction()` replaced by `newFunction()`

## 📦 Assets
- `app-v2.0.0-windows.exe` - Windows installer
- `app-v2.0.0-macos.dmg` - macOS installer
- `app-v2.0.0-linux.tar.gz` - Linux archive
```

**3. Instructions d'installation/mise à jour**

**4. Crédits et remerciements (optionnel)**

---

### Générer automatiquement les notes de version

#### Utiliser git log

```bash
# Générer les notes de version depuis le dernier tag
git log v1.0.0..HEAD --oneline --pretty=format:"- %s"
```

**Sortie :**

```
- feat: Add dark mode
- fix: Correct login bug
- docs: Update README
```

#### Script pour générer un CHANGELOG

```bash
#!/bin/bash
# generate-changelog.sh

LAST_TAG=$(git describe --tags --abbrev=0)  
CURRENT_VERSION=$1

echo "## Version $CURRENT_VERSION"  
echo ""  
echo "### Features"  
git log $LAST_TAG..HEAD --oneline --grep="feat:" --pretty=format:"- %s"  
echo ""  
echo "### Bug Fixes"  
git log $LAST_TAG..HEAD --oneline --grep="fix:" --pretty=format:"- %s"  
echo ""  
echo "### Documentation"  
git log $LAST_TAG..HEAD --oneline --grep="docs:" --pretty=format:"- %s"
```

**Utilisation :**

```bash
chmod +x generate-changelog.sh
./generate-changelog.sh 2.0.0 > CHANGELOG-2.0.0.md
```

#### Outils automatiques

**Conventional Changelog :**

```bash
npm install -g conventional-changelog-cli  
conventional-changelog -p angular -i CHANGELOG.md -s
```

**Standard Version :**

```bash
npm install -g standard-version  
standard-version
# Crée automatiquement le tag et met à jour CHANGELOG.md
```

---

### Bonnes pratiques

#### ✅ Conventions de nommage

**Utilisez un préfixe cohérent :**

```bash
# Bon : préfixe "v"
v1.0.0  
v1.1.0  
v2.0.0

# Éviter : mélange de formats
1.0.0
v1.1.0  
release-2.0.0
```

#### ✅ Utilisez toujours des tags annotés pour les releases

```bash
# Bon : tag annoté avec message
git tag -a v1.0.0 -m "Version 1.0.0 - Initial release"

# Éviter : tag léger sans information
git tag v1.0.0
```

#### ✅ Suivez le versionnement sémantique

```bash
# Progression logique
v1.0.0 → v1.0.1 (patch)  
v1.0.1 → v1.1.0 (minor)  
v1.1.0 → v2.0.0 (major)

# Éviter : sauts illogiques
v1.0.0 → v1.5.0 → v1.2.0 ❌
```

#### ✅ Documentez chaque release

Incluez toujours :
- Nouvelles fonctionnalités
- Corrections de bugs
- Changements cassants (breaking changes)
- Instructions de migration si nécessaire

#### ✅ Testez avant de taguer

```bash
# Mauvaise pratique
git tag -a v1.0.0 -m "Release"
# Oops, il y a un bug !

# Bonne pratique
npm test          # ou votre commande de test  
npm run build     # vérifier que ça compile
# Si tout est OK
git tag -a v1.0.0 -m "Version 1.0.0"
```

#### ✅ Gardez un fichier CHANGELOG

```markdown
# CHANGELOG.md

## [2.0.0] - 2024-10-15

### Added
- Dark mode support
- Export to PDF

### Changed
- Improved performance by 50%

### Fixed
- Login bug on Safari

## [1.0.0] - 2024-09-01

### Added
- Initial release
```

#### ❌ Ne modifiez jamais un tag publié

```bash
# MAUVAIS : ne faites JAMAIS cela
git tag -d v1.0.0  
git tag -a v1.0.0 -m "Updated message"  
git push origin v1.0.0 --force

# BON : créez un nouveau tag
git tag -a v1.0.1 -m "Corrected release"
```

Les tags doivent être **immuables** une fois publiés !

---

### Travailler avec des tags dans le CI/CD

#### Déployer automatiquement lors d'un tag

**GitHub Actions :**

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build
        run: npm run build
      - name: Create Release
        uses: actions/create-release@v1
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
```

**GitLab CI :**

```yaml
# .gitlab-ci.yml
deploy:
  stage: deploy
  only:
    - tags
  script:
    - npm run build
    - npm run deploy
```

---

### Commandes utiles

#### Voir le tag le plus récent

```bash
git describe --tags --abbrev=0
```

#### Voir tous les tags avec leurs dates

```bash
git for-each-ref --sort=creatordate --format '%(refname:short) %(creatordate:short)' refs/tags
```

**Sortie :**

```
v1.0.0 2024-09-01  
v1.1.0 2024-09-15  
v2.0.0 2024-10-15
```

#### Compter les commits depuis un tag

```bash
git rev-list v1.0.0..HEAD --count
```

#### Créer une archive d'un tag

```bash
git archive --format=zip --output=release-v1.0.0.zip v1.0.0
```

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git tag v1.0.0` | Crée un tag léger |
| `git tag -a v1.0.0 -m "msg"` | Crée un tag annoté |
| `git tag` | Liste tous les tags |
| `git tag -l "v1.*"` | Liste avec pattern |
| `git show v1.0.0` | Affiche les détails d'un tag |
| `git push origin v1.0.0` | Pousse un tag spécifique |
| `git push origin --tags` | Pousse tous les tags |
| `git tag -d v1.0.0` | Supprime un tag local |
| `git push origin --delete v1.0.0` | Supprime un tag distant |
| `git checkout v1.0.0` | Se positionne sur un tag |
| `git checkout -b branch v1.0.0` | Crée une branche depuis un tag |

---

### Conclusion

Les **tags** et **releases** sont essentiels pour gérer les versions de votre projet de manière professionnelle. Ils permettent de :

**Points clés à retenir :**

- Utilisez des tags annotés (`-a`) pour les versions officielles
- Suivez le versionnement sémantique (MAJOR.MINOR.PATCH)
- Poussez vos tags explicitement : `git push origin --tags`
- Les tags sont immuables : ne les modifiez jamais après publication
- Créez des releases GitHub avec des notes détaillées
- Maintenez un fichier CHANGELOG.md

**Workflow typique :**

```bash
# 1. Développement terminé
npm test

# 2. Créer le tag
git tag -a v1.0.0 -m "Version 1.0.0 - Initial release"

# 3. Pousser le tag
git push origin v1.0.0

# 4. Créer la release sur GitHub avec notes de version

# 5. Mettre à jour CHANGELOG.md
```

Avec des tags et releases bien gérés, votre projet sera plus professionnel, plus facile à maintenir, et vos utilisateurs sauront toujours quelle version ils utilisent ! 🏷️

---

**Prochaine étape :** Module 6.8 - Submodules et sous-projets

⏭️ [Submodules et sous-projets](/module-06-fonctions-avancees/08-submodules.md)
