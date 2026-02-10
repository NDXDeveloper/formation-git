🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 7. Versionnement sémantique (SemVer)

### Introduction

Imaginez que vous achetez un logiciel. Comment savoir si la nouvelle version corrige juste quelques bugs ou change complètement l'interface ? Comment savoir si vous pouvez mettre à jour sans tout casser ?

C'est exactement le problème que résout le **versionnement sémantique** (Semantic Versioning ou SemVer). C'est un système de numérotation standardisé qui **communique l'impact** d'une mise à jour.

Au lieu de versions cryptiques comme :
- Version 2024.10.16
- Version 3.14159
- Version "Édition Hiver"

Le versionnement sémantique utilise un format clair :
```
MAJOR.MINOR.PATCH
```

Par exemple : **2.4.7**

Cette simple série de chiffres raconte une histoire : elle indique **quels types de changements** ont été faits et **quel risque** représente la mise à jour.

---

## Qu'est-ce que le versionnement sémantique ?

### Définition

Le **Semantic Versioning** (SemVer) est une convention de nommage des versions créée par Tom Preston-Werner (co-fondateur de GitHub) en 2010.

**Site officiel** : [semver.org](https://semver.org/)

**Format de base** :
```
MAJOR.MINOR.PATCH
```

Chaque nombre a une signification précise :
- **MAJOR** : Changements incompatibles avec les versions précédentes
- **MINOR** : Nouvelles fonctionnalités compatibles
- **PATCH** : Corrections de bugs compatibles

---

## Anatomie d'un numéro de version

### Le format MAJOR.MINOR.PATCH

```
     2   .   4   .   7
     │       │       │
     │       │       └── PATCH (correctifs)
     │       └────────── MINOR (fonctionnalités)
     └────────────────── MAJOR (ruptures)
```

### 1. MAJOR (version majeure)

**Quand incrémenter** : Quand vous faites des changements **incompatibles** avec les versions précédentes.

**Exemples de changements MAJOR** :
- Suppression d'une fonction publique
- Modification du comportement d'une fonction existante
- Changement du format des données
- Modification de l'API publique qui casse le code existant
- Réécriture complète du projet

**Analogie** : C'est comme changer de voiture. Vous ne pouvez pas utiliser les accessoires de l'ancienne.

**Exemples réels** :
```
1.9.5 → 2.0.0   (Python 2 → Python 3)
2.14.3 → 3.0.0  (Angular 2 → Angular 3)
```

**Impact pour l'utilisateur** :
```
❌ Incompatible - Nécessite des modifications du code
⚠️  Risque élevé de breaking changes
📚 Documentation de migration nécessaire
```

### 2. MINOR (version mineure)

**Quand incrémenter** : Quand vous ajoutez des **nouvelles fonctionnalités** compatibles.

**Exemples de changements MINOR** :
- Ajout d'une nouvelle fonction
- Ajout d'un nouveau endpoint API
- Nouvelle option dans une fonction existante
- Amélioration qui n'affecte pas le code existant
- Dépréciation d'une fonctionnalité (avec avertissement)

**Analogie** : C'est comme ajouter une nouvelle pièce à votre maison. L'existant fonctionne toujours.

**Exemples réels** :
```
2.3.1 → 2.4.0   (Nouvelle fonctionnalité de recherche)
1.8.2 → 1.9.0   (Support d'un nouveau format de fichier)
```

**Impact pour l'utilisateur** :
```
✅ Compatible - Code existant fonctionne
🆕 Nouvelles fonctionnalités disponibles
📖 Mise à jour recommandée
```

### 3. PATCH (version corrective)

**Quand incrémenter** : Quand vous corrigez des **bugs** sans changer l'API.

**Exemples de changements PATCH** :
- Correction d'un bug
- Correction d'une fuite mémoire
- Amélioration de performance (sans changer l'API)
- Correction de documentation
- Correction de typo
- Mise à jour de dépendances (sécurité)

**Analogie** : C'est comme réparer une fuite d'eau. Tout fonctionne comme avant, mais mieux.

**Exemples réels** :
```
2.4.6 → 2.4.7   (Correction bug validation)
1.9.0 → 1.9.1   (Correction faille sécurité)
```

**Impact pour l'utilisateur** :
```
✅ Compatible - Aucun changement d'API
🐛 Bugs corrigés
🔒 Mise à jour fortement recommandée (surtout sécurité)
```

---

## Règles fondamentales de SemVer

### 1. Version initiale : 0.y.z

Avant la première version stable :

```
0.1.0   → Premier prototype fonctionnel
0.2.0   → Ajout de fonctionnalités
0.5.0   → Bêta publique
0.9.0   → Release candidate
```

**⚠️ Important** : En version `0.x.x`, tout peut changer à tout moment. C'est le développement initial.

**Règle** : Tant que MAJOR est à 0, l'API est considérée comme instable.

### 2. Première version stable : 1.0.0

```
0.9.5 → 1.0.0   (Première version stable et publique)
```

**Signification** : L'API publique est définie et stable.

À partir de ce moment, vous devez suivre strictement SemVer.

### 3. Une fois publié, c'est immuable

**Règle d'or** : Une version publiée ne doit **JAMAIS** être modifiée.

❌ **Interdit** :
```
Publier 2.1.0  
Modifier le code  
Republier 2.1.0 (même numéro)
```

✅ **Correct** :
```
Publier 2.1.0  
Trouver un bug  
Publier 2.1.1 (nouveau numéro)
```

### 4. Incrémenter un niveau remet les suivants à zéro

```
1.4.7 → 1.5.0  (MINOR incrémenté, PATCH remis à 0)
1.5.0 → 2.0.0  (MAJOR incrémenté, MINOR et PATCH remis à 0)
```

**Pourquoi** : Chaque niveau englobe les niveaux suivants.

### 5. Ordre de priorité

Les versions sont ordonnées ainsi :

```
1.0.0 < 1.0.1 < 1.1.0 < 2.0.0
```

On compare d'abord MAJOR, puis MINOR, puis PATCH.

---

## Pré-releases et métadonnées de build

### Format complet de SemVer

```
MAJOR.MINOR.PATCH-prerelease+build
```

**Exemple** :
```
2.0.0-alpha.1+20241016.githash
  │       │          │
  │       │          └── Métadonnées de build (ignorées pour l'ordre)
  │       └────────────── Pré-release (affecte l'ordre)
  └────────────────────── Version de base
```

### Pré-releases

Les pré-releases sont des versions **avant** la version stable.

**Format** :
```
MAJOR.MINOR.PATCH-identifiant
```

**Exemples** :
```
1.0.0-alpha
1.0.0-alpha.1
1.0.0-beta
1.0.0-beta.2
1.0.0-rc.1    (release candidate)
1.0.0-rc.2
1.0.0         (version finale)
```

**Ordre** :
```
1.0.0-alpha < 1.0.0-alpha.1 < 1.0.0-beta < 1.0.0-rc.1 < 1.0.0
```

**Conventions courantes** :

| Identifiant | Signification | Stabilité |
|-------------|---------------|-----------|
| `alpha` | Première version test, très instable | ⭐ |
| `beta` | Version de test, fonctionnalités figées | ⭐⭐ |
| `rc` (release candidate) | Candidat pour la release finale | ⭐⭐⭐⭐ |
| (aucun) | Version stable | ⭐⭐⭐⭐⭐ |

**Numérotation des pré-releases** :
```
2.0.0-alpha.1
2.0.0-alpha.2
2.0.0-alpha.3
↓
2.0.0-beta.1
2.0.0-beta.2
↓
2.0.0-rc.1
2.0.0-rc.2
↓
2.0.0
```

### Métadonnées de build

Les métadonnées sont **informatives** et n'affectent pas l'ordre des versions.

**Format** :
```
MAJOR.MINOR.PATCH+metadata
```

**Exemples** :
```
1.0.0+20241016
1.0.0+exp.sha.5114f85
2.1.0+build.123
```

**Comparaison** :
```
1.0.0+build1 == 1.0.0+build2
```

Les métadonnées sont ignorées lors de la comparaison de versions.

**Usage typique** :
- Timestamp du build
- Hash du commit Git
- Numéro de build CI/CD

---

## Exemples de progression de versions

### Scénario 1 : Application web typique

```
0.1.0   → Prototype initial
0.2.0   → Ajout authentification
0.3.0   → Ajout base de données
0.5.0   → Première bêta publique
0.9.0   → Feature complete, tests intensifs
1.0.0   → 🎉 Première version stable publique

1.0.1   → Correction bug de login
1.1.0   → Ajout dark mode
1.1.1   → Correction bug dark mode
1.2.0   → Ajout export PDF
1.2.1   → Correction export PDF
1.3.0   → Ajout notifications push

2.0.0   → 💥 Nouvelle API incompatible
2.0.1   → Correction bugs API
2.1.0   → Ajout recherche avancée
2.2.0   → Ajout support multi-langues
```

### Scénario 2 : Librairie JavaScript

```
1.0.0   → Version initiale stable
        → API : add(), remove(), get()

1.1.0   → Ajout : find()
        → API : add(), remove(), get(), find()

1.2.0   → Ajout : filter()
        → API : add(), remove(), get(), find(), filter()

1.2.1   → Correction bug dans find()
        → API inchangée

2.0.0   → 💥 Changement : add() renommé en create()
        → API : create(), remove(), get(), find(), filter()
        → Breaking change !

2.1.0   → Ajout : update()
        → API : create(), remove(), get(), find(), filter(), update()
```

### Scénario 3 : API REST

```
1.0.0   → API v1 lancée
        → GET /users
        → POST /users

1.1.0   → Ajout endpoint
        → GET /users/:id/posts (nouveau)

1.1.1   → Correction bug pagination GET /users

2.0.0   → 💥 Changement format réponse
        → Avant : { data: [...] }
        → Après : { users: [...], meta: {...} }
        → Breaking change !

2.1.0   → Ajout filtres query
        → GET /users?role=admin (nouveau paramètre)
```

---

## Git et le versionnement sémantique

### Tags Git

Les versions sont généralement marquées avec des **tags Git**.

#### Créer un tag

```bash
# Tag annoté (recommandé)
git tag -a v1.0.0 -m "Release 1.0.0"

# Tag léger (simple pointeur)
git tag v1.0.0
```

**Différence** :
- **Tag annoté** : Contient auteur, date, message
- **Tag léger** : Juste une référence au commit

**Toujours utiliser des tags annotés** pour les releases.

#### Convention de nommage des tags

**Format recommandé** :
```
v1.0.0      (avec 'v' préfixe)
```

**Exemples** :
```
v1.0.0  
v1.0.1  
v2.0.0-alpha.1  
v2.0.0-beta.1  
v2.0.0
```

#### Pousser les tags

```bash
# Pousser un tag spécifique
git push origin v1.0.0

# Pousser tous les tags
git push origin --tags
```

#### Lister les tags

```bash
# Tous les tags
git tag

# Tags avec pattern
git tag -l "v1.*"

# Dernier tag
git describe --tags --abbrev=0
```

#### Supprimer un tag

```bash
# Localement
git tag -d v1.0.0

# Sur le dépôt distant
git push origin --delete v1.0.0
```

**⚠️ Attention** : Ne supprimez **jamais** un tag de version déjà publié !

### Workflow de release

#### Processus complet

```bash
# 1. S'assurer d'être sur la bonne branche
git checkout main  
git pull origin main

# 2. Mettre à jour le numéro de version dans les fichiers
# (package.json, setup.py, pom.xml, etc.)
# Version : 1.2.3 → 1.3.0

# 3. Mettre à jour le CHANGELOG
# Ajouter les changements de cette version

# 4. Commiter les changements de version
git add package.json CHANGELOG.md  
git commit -m "chore: bump version to 1.3.0"

# 5. Créer le tag
git tag -a v1.3.0 -m "Release 1.3.0

## Nouveautés
- Ajout de la recherche avancée
- Support des filtres

## Corrections
- Correction bug pagination
- Amélioration performances

Closes #234, #235, #236"

# 6. Pousser
git push origin main  
git push origin v1.3.0

# 7. Créer la release sur GitHub/GitLab (optionnel)
# Via l'interface web ou CLI
```

### Structure des fichiers de version

#### package.json (Node.js)

```json
{
  "name": "mon-projet",
  "version": "1.3.0",
  "description": "Mon super projet"
}
```

#### setup.py (Python)

```python
setup(
    name='mon-projet',
    version='1.3.0',
    description='Mon super projet'
)
```

#### pom.xml (Java/Maven)

```xml
<project>
  <groupId>com.exemple</groupId>
  <artifactId>mon-projet</artifactId>
  <version>1.3.0</version>
</project>
```

#### Cargo.toml (Rust)

```toml
[package]
name = "mon-projet"  
version = "1.3.0"
```

---

## Le fichier CHANGELOG

### Qu'est-ce qu'un CHANGELOG ?

Le **CHANGELOG** (journal des modifications) liste **toutes** les modifications notables pour chaque version.

**Format** : `CHANGELOG.md` à la racine du projet

**Standard** : [Keep a Changelog](https://keepachangelog.com/)

### Structure d'un CHANGELOG

```markdown
# Changelog

Toutes les modifications notables de ce projet seront documentées dans ce fichier.

Le format est basé sur [Keep a Changelog](https://keepachangelog.com/),  
et ce projet adhère au [Semantic Versioning](https://semver.org/).

## [Non publié]

### Ajouté
- Fonctionnalités en cours de développement

## [2.1.0] - 2024-10-15

### Ajouté
- Nouvelle fonctionnalité de recherche avancée (#234)
- Support du thème sombre (#245)
- Export des données en CSV (#256)

### Modifié
- Amélioration des performances de la page d'accueil (50% plus rapide)
- Interface de configuration refactorisée

### Déprécié
- L'ancienne API `/v1/users` sera supprimée dans la version 3.0.0

### Corrigé
- Correction du bug de pagination (#267)
- Correction fuite mémoire dans le module de cache (#271)

### Sécurité
- Correction faille XSS dans les commentaires (#289)

## [2.0.0] - 2024-09-01

### Ajouté
- Nouvelle API REST v2
- Support de l'authentification OAuth2

### Modifié
- **BREAKING**: Format de réponse API changé (voir guide de migration)
- **BREAKING**: Base de données migrée de MongoDB vers PostgreSQL

### Supprimé
- **BREAKING**: Ancienne API v1 complètement retirée
- Support d'Internet Explorer

## [1.2.1] - 2024-08-15

### Corrigé
- Correction bug validation email (#234)

## [1.2.0] - 2024-08-01

### Ajouté
- Ajout notifications par email
- Support multi-langues (EN, FR, ES)

### Corrigé
- Amélioration gestion des erreurs

## [1.1.0] - 2024-07-15

### Ajouté
- Page de profil utilisateur
- Upload d'avatar

## [1.0.0] - 2024-07-01

### Ajouté
- 🎉 Première version stable
- Authentification utilisateur
- CRUD articles
- Interface d'administration

[Non publié]: https://github.com/user/projet/compare/v2.1.0...HEAD
[2.1.0]: https://github.com/user/projet/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/user/projet/compare/v1.2.1...v2.0.0
[1.2.1]: https://github.com/user/projet/compare/v1.2.0...v1.2.1
[1.2.0]: https://github.com/user/projet/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/user/projet/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/user/projet/releases/tag/v1.0.0
```

### Catégories du CHANGELOG

**Catégories standard** :

- **Ajouté** (Added) : Nouvelles fonctionnalités
- **Modifié** (Changed) : Changements de fonctionnalités existantes
- **Déprécié** (Deprecated) : Fonctionnalités bientôt supprimées
- **Supprimé** (Removed) : Fonctionnalités supprimées
- **Corrigé** (Fixed) : Corrections de bugs
- **Sécurité** (Security) : Corrections de vulnérabilités

**Marquer les breaking changes** :
```markdown
### Modifié
- **BREAKING**: Le format de la config a changé
```

---

## Automatisation du versionnement

### Outils populaires

#### 1. semantic-release (JavaScript/Node.js)

Automatise complètement le processus de release.

```bash
npm install --save-dev semantic-release
```

**Configuration** `.releaserc.json` :

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/npm",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

**Analyse les commits** :
```
feat: → MINOR  
fix: → PATCH  
BREAKING CHANGE: → MAJOR
```

#### 2. standard-version

Outil plus simple pour gérer versions et CHANGELOG.

```bash
npm install --save-dev standard-version
```

**Usage** :

```bash
# Release automatique
npm run release

# Release spécifique
npm run release -- --release-as minor  
npm run release -- --release-as 2.0.0

# Première release
npm run release -- --first-release
```

#### 3. commitizen

Aide à écrire des commits conventionnels.

```bash
npm install -g commitizen  
commitizen init cz-conventional-changelog --save-dev --save-exact
```

**Usage** :

```bash
git add .  
git cz    # Au lieu de git commit
```

Interface interactive pour créer des commits formatés.

#### 4. bump2version (Python)

```bash
pip install bump2version
```

```bash
# Incrémenter la version
bump2version patch  # 1.2.3 → 1.2.4  
bump2version minor  # 1.2.4 → 1.3.0  
bump2version major  # 1.3.0 → 2.0.0
```

#### 5. Maven versions plugin (Java)

```bash
# Définir nouvelle version
mvn versions:set -DnewVersion=2.0.0

# Valider
mvn versions:commit
```

### GitHub Actions pour releases automatiques

**.github/workflows/release.yml** :

```yaml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release
```

---

## Bonnes pratiques

### 1. Commencez à 0.1.0

```
0.1.0   → Première version fonctionnelle
```

Pas `0.0.1`, car `0.0.x` implique quelque chose de vraiment minimal.

### 2. Passez à 1.0.0 quand vous êtes prêts

**Critères pour 1.0.0** :
- ✅ API publique définie et stable
- ✅ Utilisé en production par au moins quelques utilisateurs
- ✅ Tests solides
- ✅ Documentation complète
- ✅ Prêt à supporter la compatibilité

### 3. Soyez strict sur les breaking changes

**Tout changement qui peut casser le code existant est un breaking change** :

```javascript
// Version 1.x.x
function calculate(a, b) {
  return a + b;
}

// Version 2.0.0 - Breaking change
function calculate(options) {  // Signature changée
  return options.a + options.b;
}
```

**Les utilisateurs doivent modifier leur code** → MAJOR

### 4. Documentez les breaking changes

Créez un guide de migration :

```markdown
# Guide de migration 1.x → 2.0

## Changements incompatibles

### API calculate()

**Avant (1.x)** :
```javascript
const result = calculate(5, 3);
\```

**Après (2.0)** :
```javascript
const result = calculate({ a: 5, b: 3 });
\```

## Étapes de migration

1. Remplacer tous les appels à calculate()
2. Tester l'application
3. Déployer
```

### 5. Dépréciez avant de supprimer

**Version 1.5.0** : Dépréciation
```javascript
/**
 * @deprecated Utilisez newFunction() à la place.
 * Sera supprimé dans la version 2.0.0
 */
function oldFunction() {
  console.warn('oldFunction() est dépréciée, utilisez newFunction()');
  return newFunction();
}
```

**Version 2.0.0** : Suppression
```javascript
// oldFunction n'existe plus
```

**Donnez du temps** : Au moins une version MINOR entre dépréciation et suppression.

### 6. Respectez les dépendances

Dans `package.json` :

```json
{
  "dependencies": {
    "express": "^4.18.0",     // MINOR et PATCH OK
    "lodash": "~4.17.21",     // PATCH seulement
    "react": "18.2.0"         // Version exacte
  }
}
```

**Symboles** :
- `^` (caret) : Accepte MINOR et PATCH (`^1.2.3` → `1.x.x`)
- `~` (tilde) : Accepte PATCH seulement (`~1.2.3` → `1.2.x`)
- Pas de symbole : Version exacte

### 7. Testez avant de publier

```bash
# Tests complets
npm test

# Build
npm run build

# Test du package
npm pack
# Installe le .tgz dans un projet test
```

### 8. Ne republiez jamais une version

Si vous avez fait une erreur :

❌ **Mauvais** :
```bash
npm publish    # 1.0.0
# Oups, bug
npm publish    # 1.0.0 (même version)
```

✅ **Bon** :
```bash
npm publish    # 1.0.0
# Oups, bug
# Correction
npm publish    # 1.0.1
```

### 9. Communiquez clairement

**Annonce de version** :
```markdown
# Version 2.0.0 - Changements majeurs

⚠️  Cette version contient des breaking changes

## 💥 Breaking Changes
- L'API a changé de format
- Node.js 14+ requis

## ✨ Nouveautés
- Support TypeScript natif
- Amélioration performances 50%

## 📚 Documentation
[Guide de migration 1.x → 2.0](link)

## 🗓️ Timeline
- Beta : 2024-10-01
- RC : 2024-10-15
- Stable : 2024-11-01
```

### 10. Maintenez plusieurs versions majeures

Pour les grosses librairies :

```
main (v3.x)
├── v3.1.0 (active)
├── v3.0.0
│
v2.x (LTS - maintenance)
├── v2.8.5 (sécurité seulement)
├── v2.8.4
│
v1.x (end of life)
└── v1.15.3 (plus de support)
```

---

## Cas particuliers

### Monorepo

Avec plusieurs packages dans un repo :

**Option 1 : Version unifiée**
```
monorepo/
├── packages/
│   ├── package-a/  (v2.1.0)
│   ├── package-b/  (v2.1.0)
│   └── package-c/  (v2.1.0)
└── package.json    (v2.1.0)
```

Tous les packages partagent la même version.

**Option 2 : Versions indépendantes**
```
monorepo/
├── packages/
│   ├── package-a/  (v1.5.0)
│   ├── package-b/  (v3.2.1)
│   └── package-c/  (v2.0.0)
```

Chaque package a sa propre version.

**Outil** : [Lerna](https://lerna.js.org/)

### Versions des APIs

**Option 1 : Version dans l'URL**
```
/api/v1/users
/api/v2/users
```

**Option 2 : Version dans le header**
```
GET /api/users  
Accept: application/vnd.api+json; version=2
```

**Stratégie de dépréciation** :
```
v1 → v2 lancée  
v1 → Dépréciée (6 mois)  
v1 → End of life
```

### Applications mobiles

Les stores ajoutent leurs contraintes :

**iOS App Store** :
- Version marketing : `2.1.0`
- Build number : `214` (incrémental)

**Google Play** :
- versionName : `2.1.0` (SemVer)
- versionCode : `214` (entier incrémental)

---

## Outils et ressources

### Validateurs

**semver.org** : Calculateur de versions
```
1.0.0 + feature = 1.1.0
1.1.0 + bugfix = 1.1.1
1.1.1 + breaking = 2.0.0
```

**npm semver calculator** :
```bash
npm install -g semver

# Comparer versions
semver 1.2.3 -r "^1.0.0"  # true

# Incrémenter
semver 1.2.3 -i minor  # 1.3.0
```

### Badges pour README

```markdown
![Version](https://img.shields.io/github/v/release/user/repo)
![Version npm](https://img.shields.io/npm/v/package)
```

Affiche : ![Version](https://img.shields.io/badge/version-2.1.0-blue)

### Intégrations CI/CD

**GitLab CI** :

```yaml
release:
  stage: deploy
  only:
    - main
  script:
    - npm run semantic-release
```

**CircleCI** :

```yaml
version: 2.1  
workflows:
  release:
    jobs:
      - release:
          filters:
            branches:
              only: main
```

---

## Checklist de release

### Avant la release

- [ ] Tous les tests passent
- [ ] La documentation est à jour
- [ ] Le CHANGELOG est complet
- [ ] Les breaking changes sont documentés
- [ ] La branche est à jour avec main
- [ ] Revue de code effectuée
- [ ] Tests d'intégration passent
- [ ] Build réussit

### Pendant la release

- [ ] Incrémenter le numéro de version
- [ ] Mettre à jour le CHANGELOG
- [ ] Commiter les changements de version
- [ ] Créer le tag Git
- [ ] Pousser le tag
- [ ] Créer la release GitHub/GitLab
- [ ] Publier sur npm/PyPI/Maven Central

### Après la release

- [ ] Annoncer la release (blog, Twitter, Discord)
- [ ] Mettre à jour la documentation en ligne
- [ ] Monitorer les issues pour les bugs
- [ ] Préparer hotfix si nécessaire

---

## Exemples du monde réel

### Node.js

```
v18.0.0  → Nouvelle version majeure  
v18.1.0  → Nouvelles fonctionnalités  
v18.2.0  → Encore plus de features  
v18.12.1 → Correction de sécurité  
v19.0.0  → Nouvelle version majeure
```

### React

```
18.0.0   → React 18
18.1.0   → Nouvelles fonctionnalités
18.2.0   → Améliorations
19.0.0   → React 19 (à venir)
```

### Vue.js

```
3.0.0    → Vue 3
3.1.0    → Script setup
3.2.0    → Composition API améliorée
3.3.0    → TypeScript amélioré
```

---

## Erreurs courantes

### 1. Oublier de mettre à jour PATCH

❌ **Mauvais** :
```
1.2.0 → Correction bug → 1.3.0
```

✅ **Bon** :
```
1.2.0 → Correction bug → 1.2.1
```

### 2. Sous-estimer un breaking change

❌ **Mauvais** :
```
1.5.0 → Changement signature fonction → 1.6.0
```

✅ **Bon** :
```
1.5.0 → Changement signature fonction → 2.0.0
```

### 3. Sauter des versions

❌ **Mauvais** :
```
1.2.0 → 1.4.0  (où est 1.3.0 ?)
```

✅ **Bon** :
```
1.2.0 → 1.3.0 → 1.4.0
```

### 4. Mélanger les types de changements

❌ **Mauvais** :
```
2.1.0 → Bug fix + breaking change → 2.1.1
```

✅ **Bon** :
```
2.1.0 → Bug fix → 2.1.1
2.1.1 → Breaking change → 3.0.0
```

---

## Conclusion

Le versionnement sémantique est un **langage commun** entre développeurs. Il transforme un simple numéro en une **communication claire** sur :
- Ce qui a changé
- L'impact de la mise à jour
- Les risques associés

**Les 3 règles d'or** :

1. **MAJOR** → Changements incompatibles
2. **MINOR** → Nouvelles fonctionnalités compatibles
3. **PATCH** → Corrections de bugs compatibles

**En pratique** :
- Utilisez Git tags pour marquer les versions
- Maintenez un CHANGELOG à jour
- Automatisez quand possible
- Communiquez clairement les breaking changes
- Soyez cohérent et prévisible

**Rappelez-vous** : Le versionnement sémantique n'est pas qu'une convention technique, c'est un **contrat de confiance** avec vos utilisateurs. Respectez-le, et ils vous feront confiance pour mettre à jour !

**Ressources** :
- [semver.org](https://semver.org/) - Spécification officielle
- [keepachangelog.com](https://keepachangelog.com/) - Guide du CHANGELOG
- [conventionalcommits.org](https://www.conventionalcommits.org/) - Commits conventionnels

**Prochaine étape** : Vous maîtrisez maintenant le versionnement ! Il est temps d'explorer les outils et interfaces graphiques qui faciliteront votre travail quotidien avec Git.

⏭️ [Module 8 : Dépannage et résolution de problèmes](/module-08-depannage/README.md)
