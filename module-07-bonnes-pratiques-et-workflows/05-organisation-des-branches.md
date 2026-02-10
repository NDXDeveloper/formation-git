🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 5. Organisation des branches (main, dev, feature/, hotfix/)

### Introduction

Une bonne organisation des branches est comme un système de classement dans une bibliothèque. Si chaque livre est rangé n'importe où, impossible de retrouver quoi que ce soit. Mais avec des rayons clairement identifiés et une logique de classement, tout devient facile.

Dans Git, une **convention de nommage** claire permet à toute l'équipe de :
- **Comprendre immédiatement** l'objectif d'une branche
- **Retrouver facilement** les branches
- **Automatiser** certaines tâches (CI/CD, déploiements)
- **Collaborer efficacement** sans confusion

Cette section vous guide pour mettre en place une organisation de branches professionnelle et cohérente.

---

## Les branches principales

### 1. La branche `main` (ou `master`)

**Rôle** : Branche de **production stable**.

**Caractéristiques** :
- Contient **uniquement** du code testé et validé
- Représente ce qui est **en production** (ou déployable)
- **Protégée** : pas de commit direct
- **Permanente** : ne se supprime jamais
- **Taguée** : chaque version reçoit un tag (v1.0.0, v1.1.0, etc.)

**Historique** : Anciennement appelée `master`, elle est maintenant généralement nommée `main` pour des raisons d'inclusivité.

**Configuration recommandée** :

```bash
# Renommer master en main (si besoin)
git branch -m master main  
git push -u origin main

# Définir main comme branche par défaut
git config --global init.defaultBranch main
```

**Règles strictes** :
- ❌ Jamais de `git push` direct
- ✅ Seulement via Pull Request/Merge Request
- ✅ Revue de code obligatoire
- ✅ Tests CI/CD passants requis
- ✅ Au moins 1-2 approbations

**Quand merger dans `main` ?**
- Après validation complète d'une fonctionnalité
- Après tests d'intégration réussis
- Quand le code est prêt pour la production
- Après approbation de l'équipe

---

### 2. La branche `develop` (ou `dev`)

**Rôle** : Branche de **développement** et **intégration**.

**Caractéristiques** :
- Point d'intégration pour toutes les nouvelles fonctionnalités
- Contient les derniers développements validés
- Plus **volatile** que `main`, peut contenir des bugs mineurs
- **Semi-protégée** : commits via PR de préférence
- **Permanente** : ne se supprime jamais

**Quand l'utiliser ?**
- Dans un workflow **Git Flow**
- Pour des projets avec des **releases planifiées**
- Quand vous voulez **séparer développement et production**

**Quand ne PAS l'utiliser ?**
- Workflow **GitHub Flow** ou **Trunk-Based** (pas de `develop`)
- Déploiement continu où `main` suffit
- Petites équipes qui veulent rester simples

**Flux typique** :

```bash
# Créer develop depuis main
git checkout main  
git checkout -b develop  
git push -u origin develop

# Les features mergent dans develop
git checkout develop  
git merge --no-ff feature/nouvelle-fonctionnalite

# Quand prêt, develop merge dans main
git checkout main  
git merge --no-ff develop  
git tag -a v1.0.0 -m "Release 1.0.0"
```

---

### Comparaison main vs develop

| Aspect | `main` | `develop` |
|--------|--------|-----------|
| **Stabilité** | ✅✅ Maximale | ✅ Élevée |
| **Contenu** | Production | Intégration |
| **Tests** | Tous passent | Peuvent échouer |
| **Déploiement** | Production | Staging/Dev |
| **Protection** | Très protégée | Moyennement protégée |
| **Fréquence de mise à jour** | Faible | Élevée |

---

## Les branches temporaires

Contrairement aux branches principales qui sont permanentes, les branches temporaires ont un **cycle de vie limité** : elles sont créées pour une tâche spécifique, puis supprimées après merge.

---

### 1. Branches `feature/`

**Rôle** : Développer une **nouvelle fonctionnalité**.

#### Convention de nommage

**Format recommandé** :
```
feature/nom-descriptif-en-kebab-case
```

**Exemples** :
```
feature/user-authentication  
feature/payment-integration  
feature/dark-mode  
feature/search-functionality  
feature/export-pdf  
feature/user-profile-page
```

**Mauvais exemples** :
```
feature/fix         ❌ Trop vague  
feature/update      ❌ Pas descriptif  
feature/JIRA-123    ❌ Référence seule (pas de contexte)  
feature123          ❌ Pas de préfixe clair  
new-feature         ❌ Pas de catégorie
```

#### Variantes de nommage

**Avec identifiant de ticket** :
```
feature/JIRA-123-user-authentication  
feature/GH-456-dark-mode  
feature/TKT-789-export-pdf
```

**Par équipe/personne** (pour grandes équipes) :
```
feature/frontend-dark-mode  
feature/backend-payment-api  
feature/mobile-offline-mode
```

**Par priorité** (peu recommandé) :
```
feature/p0-critical-security-fix  
feature/p1-user-auth
```

#### Cycle de vie d'une feature

```bash
# 1. Créer la branche depuis develop (ou main)
git checkout develop  
git pull origin develop  
git checkout -b feature/user-authentication

# 2. Développer
git add .  
git commit -m "feat: ajoute le formulaire de connexion"
# ... autres commits ...

# 3. Pousser régulièrement
git push origin feature/user-authentication

# 4. Ouvrir une Pull Request
# Via GitHub/GitLab interface

# 5. Après approbation, merger
git checkout develop  
git merge --no-ff feature/user-authentication  
git push origin develop

# 6. Supprimer la branche
git branch -d feature/user-authentication  
git push origin --delete feature/user-authentication
```

#### Bonnes pratiques feature/

✅ **À faire** :
- Noms **descriptifs** et **explicites**
- Une feature = une fonctionnalité unique
- Branches **courtes** (< 1-2 semaines)
- Commits **atomiques** et fréquents
- Tests inclus dans la feature
- Documentation mise à jour

❌ **À éviter** :
- Branches qui vivent trop longtemps (> 2 semaines)
- Mélanger plusieurs fonctionnalités
- Noms génériques ou vagues
- Oublier de supprimer après merge
- Développer sans tests

---

### 2. Branches `bugfix/` ou `fix/`

**Rôle** : Corriger un **bug non critique** trouvé en développement.

#### Convention de nommage

**Format** :
```
bugfix/description-du-bug  
fix/description-du-bug
```

**Exemples** :
```
bugfix/login-validation-error  
bugfix/broken-navbar-on-mobile  
bugfix/email-sending-failure  
fix/pagination-incorrect-count  
fix/memory-leak-in-dashboard
```

**Avec référence** :
```
bugfix/JIRA-567-login-error  
fix/GH-890-memory-leak
```

#### Cycle de vie

```bash
# Créer depuis develop
git checkout develop  
git checkout -b bugfix/login-validation-error

# Corriger et tester
git commit -m "fix: corrige la validation du formulaire de login"

# Merger dans develop
git checkout develop  
git merge --no-ff bugfix/login-validation-error  
git branch -d bugfix/login-validation-error
```

#### bugfix/ vs hotfix/

| Aspect | `bugfix/` | `hotfix/` |
|--------|-----------|-----------|
| **Gravité** | Non critique | Critique |
| **Source** | `develop` | `main` (production) |
| **Urgence** | Normal | Immédiate |
| **Destination** | `develop` | `main` + `develop` |
| **Exemple** | Bug d'affichage | Faille de sécurité |

---

### 3. Branches `hotfix/`

**Rôle** : Corriger un **bug critique** en production.

#### Convention de nommage

**Format** :
```
hotfix/description-critique  
hotfix/version-numero
```

**Exemples** :
```
hotfix/security-vulnerability  
hotfix/payment-processing-failure  
hotfix/data-loss-bug  
hotfix/1.2.1  
hotfix/critical-server-crash
```

**Avec CVE (vulnérabilité)** :
```
hotfix/CVE-2024-1234-sql-injection  
hotfix/security-patch-auth-bypass
```

#### Cycle de vie d'un hotfix

```bash
# 1. Créer depuis main (production!)
git checkout main  
git pull origin main  
git checkout -b hotfix/security-vulnerability

# 2. Corriger rapidement
git commit -m "fix: corrige la faille de sécurité CVE-2024-1234"

# 3. Tester intensivement
# Tests automatiques + tests manuels

# 4. Merger dans main
git checkout main  
git merge --no-ff hotfix/security-vulnerability

# 5. Taguer la nouvelle version
git tag -a v1.2.1 -m "Hotfix 1.2.1 - Security patch"  
git push origin main --tags

# 6. Merger AUSSI dans develop (important!)
git checkout develop  
git merge --no-ff hotfix/security-vulnerability  
git push origin develop

# 7. Supprimer la branche
git branch -d hotfix/security-vulnerability  
git push origin --delete hotfix/security-vulnerability

# 8. Déployer immédiatement en production
```

#### ⚠️ Attention avec les hotfix

- **Documenter** : Note détaillée sur ce qui a été corrigé
- **Communiquer** : Alerter l'équipe immédiatement
- **Double merge** : Ne pas oublier de merger dans `develop` aussi
- **Tester** : Tests rigoureux avant le déploiement
- **Post-mortem** : Analyser pourquoi le bug est arrivé en prod

---

### 4. Branches `release/`

**Rôle** : Préparer une **nouvelle version** pour la production.

#### Convention de nommage

**Format** :
```
release/version-numero  
release/YYYY-MM-DD  
release/nom-de-version
```

**Exemples** :
```
release/1.0.0  
release/2.3.1  
release/2024-10  
release/q4-2024  
release/winter-update
```

#### Cycle de vie

```bash
# 1. Créer depuis develop quand prêt pour release
git checkout develop  
git checkout -b release/1.0.0

# 2. Finaliser (corrections mineures seulement)
git commit -m "chore: bump version to 1.0.0"  
git commit -m "fix: corrige typo dans la doc"  
git commit -m "docs: met à jour le CHANGELOG"

# 3. Tests finaux, QA, validation

# 4. Merger dans main
git checkout main  
git merge --no-ff release/1.0.0  
git tag -a v1.0.0 -m "Release 1.0.0"  
git push origin main --tags

# 5. Merger dans develop (garder les corrections)
git checkout develop  
git merge --no-ff release/1.0.0  
git push origin develop

# 6. Supprimer la branche
git branch -d release/1.0.0  
git push origin --delete release/1.0.0
```

#### Que faire dans une branche release ?

✅ **Autorisé** :
- Corrections de bugs mineurs
- Mise à jour de version dans les fichiers
- Mise à jour du CHANGELOG
- Corrections de documentation
- Ajustements de configuration

❌ **Interdit** :
- Nouvelles fonctionnalités
- Refactoring majeur
- Changements de l'API
- Tout ce qui n'est pas lié à la release

---

### 5. Branches `refactor/`

**Rôle** : Refactoriser du code **sans changer le comportement**.

#### Convention de nommage

```
refactor/description-du-refactoring
```

**Exemples** :
```
refactor/simplify-authentication-logic  
refactor/extract-payment-service  
refactor/improve-database-queries  
refactor/modernize-api-endpoints
```

#### Caractéristiques

- **Pas de nouvelles fonctionnalités**
- **Pas de corrections de bugs** (sauf si découverts pendant le refactoring)
- **Tests inchangés** (doivent toujours passer)
- Code plus **propre**, plus **maintenable**

```bash
git checkout develop  
git checkout -b refactor/simplify-auth-logic  
git commit -m "refactor: extrait la logique de validation dans une fonction"  
git commit -m "refactor: renomme les variables pour plus de clarté"
```

---

### 6. Branches `docs/`

**Rôle** : Modifications de **documentation uniquement**.

#### Convention de nommage

```
docs/description-de-la-doc
```

**Exemples** :
```
docs/update-readme  
docs/add-api-documentation  
docs/fix-installation-guide  
docs/improve-contributing-guidelines
```

**Avantage** : Permet de réviser la documentation sans se mélanger avec le code.

```bash
git checkout main  # ou develop  
git checkout -b docs/update-readme  
git commit -m "docs: ajoute les instructions d'installation macOS"
```

---

### 7. Branches `test/`

**Rôle** : Ajouter ou améliorer les **tests**.

```
test/add-user-service-tests  
test/improve-integration-tests  
test/fix-flaky-tests
```

**Usage** :
```bash
git checkout develop  
git checkout -b test/add-payment-tests  
git commit -m "test: ajoute les tests unitaires pour le service de paiement"
```

---

### 8. Branches `chore/`

**Rôle** : Tâches de **maintenance** diverses.

**Exemples** :
```
chore/update-dependencies  
chore/configure-ci-pipeline  
chore/cleanup-unused-files  
chore/upgrade-nodejs-version
```

**Ce qui entre dans chore/** :
- Mise à jour de dépendances
- Configuration d'outils
- Nettoyage de code mort
- Modifications du build
- Changements de configuration CI/CD

---

### 9. Branches `experiment/` ou `spike/`

**Rôle** : Expérimentations et **prototypes**.

```
experiment/new-architecture  
experiment/performance-optimization  
spike/graphql-migration  
spike/new-ui-framework
```

**Caractéristiques** :
- Peut être **jeté** après l'expérimentation
- Pas forcément mergé
- Sert à tester une idée
- Peut être très brouillon

```bash
git checkout -b experiment/try-new-database
# ... code expérimental ...
# Si ça marche : créer une feature/ propre
# Si ça ne marche pas : supprimer la branche
```

---

## Convention de nommage complète

### Format général recommandé

```
<type>/<numéro-ticket?>-<description-kebab-case>
```

**Composants** :
1. **Type** : `feature`, `bugfix`, `hotfix`, `release`, etc.
2. **Séparateur** : `/` (slash)
3. **Numéro de ticket** (optionnel) : `JIRA-123`, `GH-456`
4. **Séparateur** : `-` (tiret)
5. **Description** : mots séparés par des tirets (kebab-case)

### Exemples complets

```
feature/user-authentication  
feature/JIRA-234-payment-integration  
bugfix/login-form-validation  
bugfix/GH-567-memory-leak  
hotfix/security-vulnerability  
hotfix/1.2.1-critical-patch  
release/2.0.0  
release/2024-Q4  
refactor/simplify-api-endpoints  
docs/update-installation-guide  
test/add-unit-tests-auth  
chore/update-dependencies  
experiment/try-microservices
```

---

## Règles de nommage

### ✅ Bonnes pratiques

1. **Utilisez le kebab-case** (mots-séparés-par-tirets)
   ```
   ✅ feature/user-authentication
   ❌ feature/user_authentication  (snake_case)
   ❌ feature/UserAuthentication   (PascalCase)
   ❌ feature/userAuthentication   (camelCase)
   ```

2. **Soyez descriptifs**
   ```
   ✅ feature/add-payment-with-stripe
   ❌ feature/payment
   ❌ feature/new-feature
   ```

3. **Restez concis mais clair**
   ```
   ✅ feature/dark-mode
   ❌ feature/implement-a-dark-mode-theme-for-the-entire-application
   ```

4. **Utilisez l'anglais** (pour les projets internationaux)
   ```
   ✅ feature/user-registration
   ❌ feature/inscription-utilisateur
   ```

5. **Préfixez avec le type**
   ```
   ✅ feature/search
   ❌ search (pas de préfixe)
   ```

6. **Incluez le ticket si pertinent**
   ```
   ✅ feature/JIRA-123-user-auth
   ✅ feature/user-auth  (si pas de ticket)
   ❌ feature/JIRA-123   (ticket seul sans contexte)
   ```

### ❌ À éviter

1. **Espaces** : `feature/user auth` ❌
2. **Underscores seuls** : `feature/user_auth` ❌ (préférez tirets)
3. **Caractères spéciaux** : `feature/user@auth` ❌
4. **Majuscules** : `feature/UserAuth` ❌
5. **Noms vagues** : `feature/update`, `fix/bug` ❌
6. **Chiffres seuls** : `feature/123` ❌
7. **Mots-clés Git** : `feature/HEAD`, `feature/master` ❌

---

## Organisation par dossiers

Pour les grandes équipes ou projets complexes, organisez par **catégories**.

### Structure recommandée

```
main  
develop

feature/
├── feature/frontend-dark-mode
├── feature/backend-api-v2
└── feature/mobile-offline-sync

bugfix/
├── bugfix/navbar-responsive
└── bugfix/email-validation

hotfix/
└── hotfix/security-patch-1.2.1

release/
└── release/2.0.0

refactor/
└── refactor/database-layer

docs/
└── docs/api-documentation

test/
└── test/integration-tests

chore/
└── chore/update-webpack
```

### Avantages

- **Lisibilité** : Facile de voir tous les types
- **Filtrage** : `git branch --list "feature/*"`
- **Automatisation** : Scripts CI/CD basés sur les préfixes
- **Organisation** : Séparation claire des responsabilités

---

## Commandes utiles pour gérer les branches

### Lister les branches par type

```bash
# Toutes les features
git branch --list "feature/*"

# Toutes les branches locales
git branch

# Toutes les branches distantes
git branch -r

# Toutes les branches (locales + distantes)
git branch -a
```

### Créer et naviguer

```bash
# Créer et basculer en une commande
git checkout -b feature/new-feature

# Ou avec switch (plus moderne)
git switch -c feature/new-feature

# Basculer sur une branche existante
git checkout feature/existing-feature  
git switch feature/existing-feature
```

### Renommer une branche

```bash
# Renommer la branche courante
git branch -m nouveau-nom

# Renommer une autre branche
git branch -m ancien-nom nouveau-nom

# Pousser la branche renommée
git push origin nouveau-nom

# Supprimer l'ancienne sur le distant
git push origin --delete ancien-nom
```

### Nettoyer les branches

```bash
# Supprimer une branche locale (si mergée)
git branch -d feature/completed-feature

# Forcer la suppression (même si pas mergée)
git branch -D feature/abandoned-feature

# Supprimer une branche distante
git push origin --delete feature/completed-feature

# Nettoyer les références locales aux branches distantes supprimées
git fetch --prune
# ou
git remote prune origin
```

### Afficher les branches avec informations

```bash
# Branches avec dernier commit
git branch -v

# Branches mergées dans la branche courante
git branch --merged

# Branches non mergées
git branch --no-merged

# Branches avec tracking (upstream)
git branch -vv
```

---

## Stratégies de protection des branches

### Configuration de protection sur GitHub

**Settings** → **Branches** → **Branch protection rules**

**Pour `main`** :
- ✅ Require pull request reviews (2 approbations)
- ✅ Dismiss stale reviews
- ✅ Require status checks to pass
- ✅ Require branches to be up to date
- ✅ Require conversation resolution
- ✅ Include administrators
- ✅ Restrict push access
- ✅ Require linear history (optionnel)

**Pour `develop`** :
- ✅ Require pull request reviews (1 approbation)
- ✅ Require status checks to pass
- ✅ Require branches to be up to date

---

## Automatisation avec les branches

### Hooks Git basés sur les branches

**`.git/hooks/pre-push`** :

```bash
#!/bin/bash

# Empêcher le push direct sur main
branch=$(git rev-parse --abbrev-ref HEAD)

if [ "$branch" = "main" ]; then
  echo "❌ Push direct sur main interdit!"
  echo "→ Utilisez une Pull Request"
  exit 1
fi
```

### CI/CD basé sur les branches

**`.github/workflows/ci.yml`** :

```yaml
name: CI/CD

on:
  push:
    branches:
      - main
      - develop
      - 'feature/**'
      - 'bugfix/**'
      - 'hotfix/**'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: npm test

  deploy-staging:
    if: github.ref == 'refs/heads/develop'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to staging
        run: ./deploy-staging.sh

  deploy-production:
    if: github.ref == 'refs/heads/main'
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: ./deploy-production.sh
```

### Scripts pour créer des branches

**`create-feature.sh`** :

```bash
#!/bin/bash

if [ -z "$1" ]; then
  echo "Usage: ./create-feature.sh nom-de-la-feature"
  exit 1
fi

FEATURE_NAME=$1  
BRANCH_NAME="feature/$FEATURE_NAME"

git checkout develop  
git pull origin develop  
git checkout -b "$BRANCH_NAME"  
git push -u origin "$BRANCH_NAME"

echo "✅ Branche $BRANCH_NAME créée et poussée"
```

**Utilisation** :

```bash
./create-feature.sh user-authentication
# Crée feature/user-authentication depuis develop
```

---

## Documenter votre organisation

### Fichier BRANCHING.md

Créez un fichier à la racine du projet :

```markdown
# Convention de branches

## Branches principales

- `main` : Production (protégée)
- `develop` : Développement (semi-protégée)

## Branches temporaires

### Features
- Format : `feature/nom-descriptif`
- Depuis : `develop`
- Merge dans : `develop`
- Exemple : `feature/user-authentication`

### Bugfixes
- Format : `bugfix/nom-du-bug`
- Depuis : `develop`
- Merge dans : `develop`
- Exemple : `bugfix/login-validation`

### Hotfixes
- Format : `hotfix/description`
- Depuis : `main`
- Merge dans : `main` + `develop`
- Exemple : `hotfix/security-patch`

### Releases
- Format : `release/x.y.z`
- Depuis : `develop`
- Merge dans : `main` + `develop`
- Exemple : `release/1.0.0`

## Règles

1. Jamais de push direct sur `main`
2. Pull Request obligatoire
3. 2 approbations pour `main`, 1 pour `develop`
4. Tests CI/CD doivent passer
5. Supprimer les branches après merge

## Commandes rapides

```bash
# Créer une feature
git checkout develop  
git pull origin develop  
git checkout -b feature/ma-feature

# Créer un hotfix
git checkout main  
git pull origin main  
git checkout -b hotfix/mon-fix
```
```

---

## Cas pratiques

### Scénario 1 : Nouvelle fonctionnalité

```bash
# 1. Partir de develop
git checkout develop  
git pull origin develop

# 2. Créer la branche
git checkout -b feature/payment-integration

# 3. Développer
git commit -m "feat: ajoute l'interface de paiement"  
git commit -m "feat: intègre l'API Stripe"  
git commit -m "test: ajoute les tests du module paiement"

# 4. Pousser
git push origin feature/payment-integration

# 5. Ouvrir une PR vers develop
# 6. Après merge, supprimer
git checkout develop  
git pull origin develop  
git branch -d feature/payment-integration
```

### Scénario 2 : Bug en développement

```bash
# 1. Partir de develop
git checkout develop  
git pull origin develop

# 2. Créer la branche bugfix
git checkout -b bugfix/navbar-broken-mobile

# 3. Corriger
git commit -m "fix: corrige le menu sur mobile"

# 4. Merger rapidement dans develop
git checkout develop  
git merge --no-ff bugfix/navbar-broken-mobile  
git push origin develop

# 5. Supprimer
git branch -d bugfix/navbar-broken-mobile
```

### Scénario 3 : Bug critique en production

```bash
# 1. Partir de main (PRODUCTION!)
git checkout main  
git pull origin main

# 2. Créer le hotfix
git checkout -b hotfix/critical-security-fix

# 3. Corriger urgemment
git commit -m "fix: corrige la faille de sécurité XSS"

# 4. Merger dans main
git checkout main  
git merge --no-ff hotfix/critical-security-fix  
git tag -a v1.2.1 -m "Hotfix v1.2.1"  
git push origin main --tags

# 5. Merger AUSSI dans develop (important!)
git checkout develop  
git merge --no-ff hotfix/critical-security-fix  
git push origin develop

# 6. Supprimer
git branch -d hotfix/critical-security-fix

# 7. Déployer immédiatement
```

### Scénario 4 : Préparer une release

```bash
# 1. Partir de develop quand prêt
git checkout develop  
git pull origin develop

# 2. Créer la release
git checkout -b release/2.0.0

# 3. Finaliser
git commit -m "chore: bump version to 2.0.0"  
git commit -m "docs: update CHANGELOG"

# 4. Merger dans main
git checkout main  
git merge --no-ff release/2.0.0  
git tag -a v2.0.0 -m "Release 2.0.0"  
git push origin main --tags

# 5. Merger dans develop
git checkout develop  
git merge --no-ff release/2.0.0  
git push origin develop

# 6. Supprimer
git branch -d release/2.0.0
```

---

## Résumé visuel

```
main (production)
  │
  ├─── hotfix/security-fix ────┐
  │                             │
  ├─── release/1.0.0 ───────────┤
  │                             │
develop (intégration)           │
  │                             │
  ├─── feature/user-auth ───────┤
  ├─── feature/dark-mode ───────┤
  ├─── bugfix/navbar-fix ───────┤
  └─── refactor/api-cleanup ────┘
```

---

## Checklist d'organisation

### ✅ Configuration initiale

- [ ] Créer `main` et la protéger
- [ ] Créer `develop` si nécessaire
- [ ] Configurer les protections de branches
- [ ] Documenter dans BRANCHING.md ou CONTRIBUTING.md
- [ ] Former l'équipe aux conventions

### ✅ Pour chaque branche

- [ ] Nom suit la convention `type/description`
- [ ] Créée depuis la bonne branche source
- [ ] Poussée régulièrement
- [ ] PR ouverte quand prête
- [ ] Tests passent
- [ ] Revue de code effectuée
- [ ] Mergée dans la bonne destination
- [ ] Supprimée après merge

### ✅ Maintenance régulière

- [ ] Nettoyer les branches mergées
- [ ] Supprimer les branches abandonnées
- [ ] Réviser les protections de branches
- [ ] Mettre à jour la documentation
- [ ] Faire des rétrospectives sur le workflow

---

## Conclusion

Une bonne organisation des branches, c'est comme avoir un bureau bien rangé : ça prend un peu de discipline au départ, mais ça fait gagner énormément de temps par la suite.

**Points clés à retenir** :

1. **Conventions claires** : Tout le monde suit les mêmes règles
2. **Préfixes explicites** : `feature/`, `bugfix/`, `hotfix/`, etc.
3. **Noms descriptifs** : En un coup d'œil, on sait de quoi il s'agit
4. **Branches courtes** : Merge fréquent pour éviter les conflits
5. **Nettoyage régulier** : Supprimer les branches après usage
6. **Documentation** : Écrire les règles pour les nouveaux arrivants

**Adaptez selon vos besoins** :
- Petite équipe → Convention simple
- Grande équipe → Convention détaillée + préfixes multiples
- Projet open source → Suivre les conventions de la communauté

**Prochaine étape** : Maintenant que vos branches sont bien organisées, découvrons comment effectuer des revues de code efficaces avec les Pull Requests !

⏭️ [Revue de code avec Pull Requests](/module-07-bonnes-pratiques-et-workflows/06-revue-de-code-avec-pr.md)
