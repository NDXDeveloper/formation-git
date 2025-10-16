🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 10 : Cas pratiques et projets
## 3. Atelier : Projet en équipe avec Git Flow

Git Flow est une méthodologie de gestion des branches conçue spécifiquement pour le travail en équipe. Ce guide vous accompagne dans la mise en place et l'utilisation de Git Flow pour un projet collaboratif structuré et professionnel.

---

## Qu'est-ce que Git Flow ?

Git Flow est un modèle de branches créé par Vincent Driessen qui définit des règles claires sur comment et quand créer et fusionner des branches. C'est une convention adoptée par de nombreuses équipes de développement.

### Pourquoi utiliser Git Flow ?

**Sans Git Flow :**
- Chacun travaille comme il veut
- Les branches sont créées de manière anarchique
- Difficile de savoir quelle version est en production
- Les conflits sont fréquents
- Le déploiement est compliqué

**Avec Git Flow :**
- ✅ Structure claire et prévisible
- ✅ Tout le monde suit les mêmes règles
- ✅ Facilite le travail en parallèle
- ✅ Simplifie les déploiements
- ✅ Permet de gérer plusieurs versions simultanément

---

## Les branches dans Git Flow

Git Flow utilise plusieurs types de branches avec des rôles bien définis :

### 1. Branches principales (permanentes)

#### **main** (ou master)
- **Rôle** : Code en production
- **Contenu** : Uniquement du code stable et testé
- **Qui y touche ?** : Personne directement ! Seulement via merge
- **Durée de vie** : Permanente

```
main = Ce que voient vos utilisateurs en production
```

#### **develop** (ou dev)
- **Rôle** : Branche d'intégration pour le développement
- **Contenu** : Dernières fonctionnalités développées
- **Qui y touche ?** : Personne directement ! Seulement via merge
- **Durée de vie** : Permanente

```
develop = La prochaine version en préparation
```

### 2. Branches de support (temporaires)

#### **feature/** - Branches de fonctionnalités
- **Rôle** : Développer une nouvelle fonctionnalité
- **Part de** : `develop`
- **Fusionne dans** : `develop`
- **Durée de vie** : Temporaire (supprimée après merge)

```
feature/login
feature/user-profile
feature/payment-system
```

#### **release/** - Branches de release
- **Rôle** : Préparer une nouvelle version pour la production
- **Part de** : `develop`
- **Fusionne dans** : `main` ET `develop`
- **Durée de vie** : Temporaire (supprimée après déploiement)

```
release/1.0.0
release/2.3.0
```

#### **hotfix/** - Branches de correction urgente
- **Rôle** : Corriger un bug critique en production
- **Part de** : `main`
- **Fusionne dans** : `main` ET `develop`
- **Durée de vie** : Temporaire (supprimée après correction)

```
hotfix/fix-payment-crash
hotfix/security-patch
```

---

## Schéma de Git Flow

```
main     ──●────────────●──────●──────────────●──
            ↑            ↑      ↑              ↑
            │            │      │              │
            │   release/ │      │   hotfix/    │
            │   ┌───●────┤      └──●───┐       │
            │   │        │         │   │       │
develop  ───●───●────●───●─────●───●───●───●───●──
            ↑        ↑           ↑       ↑
            │ feature/│  feature/│       │
         ●──┴──●      └──●───┐   │       │
         feature/        │   │   │       │
                         └───┴───┘       │
                         features fusionnées
```

**Légende :**
- `●` = Commit
- `↑` = Merge
- Branches horizontales = main et develop (permanentes)
- Branches qui partent/arrivent = feature, release, hotfix (temporaires)

---

## Workflow Git Flow : Cas pratique complet

Imaginons que vous développez une application de gestion de tâches avec votre équipe.

### Étape 0 : Initialisation du projet

**Le chef de projet ou lead developer commence :**

```bash
# Créer le dépôt et la première structure
git init projet-todo-app
cd projet-todo-app

# Créer un fichier initial
echo "# Todo App" > README.md
git add README.md
git commit -m "Initial commit"

# Créer la branche develop
git branch develop
git checkout develop

# Pousser les deux branches
git push -u origin main
git push -u origin develop
```

✅ **Résultat :** Votre projet a maintenant deux branches : `main` (vide mais prête) et `develop` (base de travail)

---

### Étape 1 : Développer une nouvelle fonctionnalité

**Développeur 1 : Créer le système de login**

```bash
# 1. Se mettre à jour avec develop
git checkout develop
git pull origin develop

# 2. Créer une branche de fonctionnalité
git checkout -b feature/login

# 3. Développer la fonctionnalité
# ... écrire le code ...

# 4. Commiter régulièrement
git add src/auth/login.js
git commit -m "Add: Implémentation de la page de login"

git add src/auth/validation.js
git commit -m "Add: Validation des identifiants"

git add tests/auth/login.test.js
git commit -m "Add: Tests unitaires pour le login"

# 5. Pousser la branche
git push -u origin feature/login
```

**Développeur 2 : En parallèle, créer le profil utilisateur**

```bash
# Même processus, mais branche différente
git checkout develop
git pull origin develop
git checkout -b feature/user-profile

# ... développement ...

git add src/user/profile.js
git commit -m "Add: Page de profil utilisateur"

git push -u origin feature/user-profile
```

**💡 Point important :** Les deux développeurs travaillent en parallèle sans se gêner !

---

### Étape 2 : Code Review et fusion des features

**Développeur 1 : Créer une Pull Request**

Sur GitHub/GitLab/Bitbucket :
1. Allez sur votre dépôt
2. Cliquez sur "New Pull Request"
3. **Base** : `develop` ← **Compare** : `feature/login`
4. Titre : "Feature: Système de login"
5. Description détaillée de la fonctionnalité
6. Demandez une revue à un collègue

**Développeur 3 : Faire la revue de code**

```markdown
# Commentaires sur la PR
✅ Le code est clair et bien structuré
✅ Les tests passent
💬 Petite suggestion : ajouter une vérification email
❌ Attention : mot de passe pas assez sécurisé
```

**Développeur 1 : Corriger selon les retours**

```bash
# Toujours sur la branche feature/login
git add src/auth/login.js
git commit -m "Fix: Amélioration de la sécurité des mots de passe"

git push origin feature/login
# La PR se met automatiquement à jour !
```

**Développeur 3 : Approuver et merger**

Sur la plateforme :
1. Cliquez sur "Approve"
2. Cliquez sur "Merge Pull Request"
3. Choisissez "Squash and merge" (pour un historique propre)
4. Supprimez la branche `feature/login` après merge

**Développeur 1 : Nettoyer localement**

```bash
git checkout develop
git pull origin develop
git branch -d feature/login
```

---

### Étape 3 : Préparer une release

Après plusieurs features mergées, l'équipe décide de sortir la version 1.0.0.

**Lead Developer : Créer la branche de release**

```bash
# Partir de develop (qui contient toutes les features prêtes)
git checkout develop
git pull origin develop

# Créer la branche de release
git checkout -b release/1.0.0

# Mettre à jour le numéro de version
echo "1.0.0" > VERSION
git add VERSION
git commit -m "Bump version to 1.0.0"

# Pousser la branche
git push -u origin release/1.0.0
```

**L'équipe : Tester et corriger uniquement les bugs**

```bash
# Sur la branche release, on ne fait QUE des corrections
git checkout release/1.0.0

# Exemple : correction d'un bug trouvé en test
git add src/auth/login.js
git commit -m "Fix: Correction du bug de double-clic sur login"

git push origin release/1.0.0
```

**⚠️ Important :** Sur une branche release :
- ✅ Corriger des bugs
- ✅ Mettre à jour la documentation
- ✅ Préparer les notes de version
- ❌ PAS de nouvelles fonctionnalités !

---

### Étape 4 : Déployer en production

**Lead Developer : Merger dans main et develop**

```bash
# 1. Merger dans main (production)
git checkout main
git pull origin main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Version 1.0.0 - Premier déploiement"
git push origin main --tags

# 2. Merger aussi dans develop (pour ne pas perdre les corrections)
git checkout develop
git pull origin develop
git merge --no-ff release/1.0.0
git push origin develop

# 3. Supprimer la branche de release
git branch -d release/1.0.0
git push origin --delete release/1.0.0
```

**🎉 Résultat :** La version 1.0.0 est maintenant en production !

---

### Étape 5 : Hotfix - Corriger un bug critique

**Situation :** Le lendemain, un utilisateur signale que l'application crash au login !

**Développeur en astreinte : Correction urgente**

```bash
# 1. Partir de main (production)
git checkout main
git pull origin main

# 2. Créer une branche hotfix
git checkout -b hotfix/login-crash

# 3. Corriger le bug rapidement
git add src/auth/login.js
git commit -m "Hotfix: Correction du crash au login avec username vide"

# 4. Pousser
git push -u origin hotfix/login-crash
```

**Lead Developer : Déployer le hotfix**

```bash
# 1. Merger dans main
git checkout main
git merge --no-ff hotfix/login-crash
git tag -a v1.0.1 -m "Hotfix: Correction crash login"
git push origin main --tags

# 2. Merger aussi dans develop
git checkout develop
git merge --no-ff hotfix/login-crash
git push origin develop

# 3. Supprimer la branche hotfix
git branch -d hotfix/login-crash
git push origin --delete hotfix/login-crash
```

**⚡ Résultat :** Le bug est corrigé en production ET dans develop !

---

## Règles d'or de Git Flow

### 1. Ne jamais commiter directement sur main ou develop

```bash
# ❌ MAUVAIS
git checkout main
git add fichier.js
git commit -m "Fix rapide"
git push

# ✅ BON
git checkout develop
git checkout -b feature/correction-rapide
git add fichier.js
git commit -m "Fix: Correction du bug"
git push -u origin feature/correction-rapide
# Puis créer une PR
```

### 2. Toujours partir de la bonne branche

- **Feature** → part de `develop`
- **Release** → part de `develop`
- **Hotfix** → part de `main`

### 3. Toujours merger au bon endroit

- **Feature** → merge dans `develop`
- **Release** → merge dans `main` ET `develop`
- **Hotfix** → merge dans `main` ET `develop`

### 4. Garder les branches à jour

```bash
# Avant de commencer à travailler chaque jour
git checkout develop
git pull origin develop

# Avant de merger une feature
git checkout feature/ma-feature
git merge develop
# Résoudre les conflits si nécessaire
```

### 5. Nommer correctement les branches

**Convention de nommage :**

```bash
feature/nom-descriptif          # feature/add-dark-mode
feature/numero-issue            # feature/issue-42
feature/prenom-fonctionnalite   # feature/pierre-notification

release/version                 # release/2.0.0

hotfix/description-bug          # hotfix/fix-payment-error
hotfix/numero-issue             # hotfix/issue-789
```

---

## Commandes Git Flow résumées

### Installation de Git Flow (optionnel)

Git Flow dispose d'extensions qui automatisent certaines commandes :

```bash
# Installation (selon votre système)
# macOS
brew install git-flow

# Linux
sudo apt-get install git-flow

# Windows (Git Bash)
# Télécharger depuis https://github.com/nvie/gitflow
```

### Avec Git Flow extension

```bash
# Initialiser Git Flow
git flow init

# Créer une feature
git flow feature start login

# Terminer une feature (merge dans develop)
git flow feature finish login

# Créer une release
git flow release start 1.0.0

# Terminer une release (merge dans main et develop)
git flow release finish 1.0.0

# Créer un hotfix
git flow hotfix start fix-login

# Terminer un hotfix (merge dans main et develop)
git flow hotfix finish fix-login
```

### Sans Git Flow extension (commandes Git standards)

```bash
# === FEATURE ===
git checkout develop
git checkout -b feature/login
# ... travail ...
git checkout develop
git merge --no-ff feature/login
git branch -d feature/login

# === RELEASE ===
git checkout develop
git checkout -b release/1.0.0
# ... corrections ...
git checkout main
git merge --no-ff release/1.0.0
git tag v1.0.0
git checkout develop
git merge --no-ff release/1.0.0
git branch -d release/1.0.0

# === HOTFIX ===
git checkout main
git checkout -b hotfix/fix-bug
# ... correction ...
git checkout main
git merge --no-ff hotfix/fix-bug
git tag v1.0.1
git checkout develop
git merge --no-ff hotfix/fix-bug
git branch -d hotfix/fix-bug
```

---

## Scénario complet : Semaine type en équipe

### Lundi matin

**Équipe : Réunion de sprint planning**
- Fonctionnalités à développer cette semaine :
  - Système de notifications
  - Export PDF des tâches
  - Amélioration du design

**Répartition :**
- Alice → `feature/notifications`
- Bob → `feature/pdf-export`
- Charlie → `feature/design-improvements`

### Lundi - Mercredi : Développement

```bash
# Alice
git checkout develop
git pull origin develop
git checkout -b feature/notifications
# ... code code code ...
git add .
git commit -m "Add: Système de notifications en temps réel"
git push -u origin feature/notifications

# Bob fait de même sur sa feature
# Charlie fait de même sur sa feature
```

### Jeudi : Code Review

- Alice termine sa feature → crée une PR → Bob fait la revue
- Bob termine sa feature → crée une PR → Charlie fait la revue
- Charlie termine sa feature → crée une PR → Alice fait la revue

```bash
# Après approbation, on merge dans develop
# (via l'interface web ou en ligne de commande)
```

### Vendredi : Préparation du déploiement

```bash
# Lead Developer
git checkout develop
git pull origin develop
git checkout -b release/1.1.0

# Équipe : Tests finaux sur release/1.1.0
# Correction de bugs mineurs si besoin

# Fin de journée : Déploiement
git checkout main
git merge --no-ff release/1.1.0
git tag v1.1.0
git push origin main --tags

git checkout develop
git merge --no-ff release/1.1.0
git push origin develop

git branch -d release/1.1.0
```

### Weekend : Hotfix si nécessaire

```bash
# Si un bug critique est détecté
git checkout main
git checkout -b hotfix/critical-bug
# ... correction rapide ...
git checkout main
git merge --no-ff hotfix/critical-bug
git tag v1.1.1
git push origin main --tags

git checkout develop
git merge --no-ff hotfix/critical-bug
git push origin develop
```

---

## Protection des branches

Pour éviter les erreurs, configurez les branches protégées sur GitHub/GitLab :

### Sur GitHub

1. **Settings** → **Branches** → **Add rule**
2. Branch name pattern : `main`
3. ✅ Require pull request reviews before merging
4. ✅ Require status checks to pass
5. ✅ Include administrators

Répétez pour `develop`.

**Résultat :** Impossible de pousser directement sur main ou develop !

---

## Intégration Continue avec Git Flow

### Configuration CI/CD

```yaml
# .github/workflows/ci.yml (exemple GitHub Actions)

name: CI

on:
  push:
    branches: [ develop, main ]
  pull_request:
    branches: [ develop, main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: npm test

  deploy-staging:
    needs: test
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to staging
        run: ./deploy-staging.sh

  deploy-production:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: ./deploy-production.sh
```

**Comportement :**
- Push sur `develop` → tests + déploiement automatique sur l'environnement de staging
- Push sur `main` → tests + déploiement automatique en production
- Pull Request → tests uniquement

---

## Versionnement sémantique (SemVer)

Avec Git Flow, utilisez le versionnement sémantique : **MAJOR.MINOR.PATCH**

```
1.2.3
│ │ │
│ │ └─ PATCH : Corrections de bugs (hotfix)
│ └─── MINOR : Nouvelles fonctionnalités (release)
└───── MAJOR : Changements incompatibles (breaking changes)
```

**Exemples :**

- `1.0.0` → Première version stable
- `1.0.1` → Hotfix (correction bug)
- `1.1.0` → Release (nouvelle fonctionnalité)
- `2.0.0` → Breaking change (modification incompatible)

**Comment incrémenter :**

```bash
# Hotfix : 1.0.0 → 1.0.1
git tag v1.0.1

# Release avec features : 1.0.1 → 1.1.0
git tag v1.1.0

# Breaking change : 1.1.0 → 2.0.0
git tag v2.0.0
```

---

## Alternatives et variantes de Git Flow

### GitHub Flow (plus simple)

Si Git Flow semble trop complexe pour votre équipe :

```
main ──●──────●──────●──────●──
        ↑      ↑      ↑
        │      │      │
     ●──┴──●   │   ●──┴──●
     feature   │   feature
               │
            ●──┴──●
            hotfix
```

**Règles :**
- Une seule branche principale : `main`
- Toutes les branches partent de `main`
- Toutes les branches mergent dans `main`
- Deploy automatique après merge dans `main`

**Quand l'utiliser ?** Petites équipes, déploiement continu.

### Trunk-Based Development (encore plus simple)

```
main ──●──●──●──●──●──●──●──
```

**Règles :**
- Tout le monde commit sur `main`
- Petits commits très fréquents
- Feature flags pour désactiver le code incomplet
- Déploiement continu

**Quand l'utiliser ?** Équipes très expérimentées, forte automatisation.

---

## Conseils pour réussir avec Git Flow

### Communication

1. **Réunions quotidiennes** : Qui travaille sur quoi ?
2. **Documentation** : Maintenez un CHANGELOG.md
3. **Slack/Discord** : Canal dédié aux merges et releases

### Organisation

1. **Board Kanban** : GitHub Projects, Jira, Trello
2. **Issues liées** : Chaque feature → une issue
3. **Milestones** : Regroupez les issues par release

### Qualité

1. **Tests automatisés** : Lancez-les sur chaque PR
2. **Linter** : Vérifiez le style de code
3. **Code review obligatoire** : Minimum 1 approbation

### Déploiement

1. **Environnements** : Development → Staging → Production
2. **Rollback** : Prévoyez un plan de retour en arrière
3. **Monitoring** : Surveillez les erreurs après déploiement

---

## Checklist : Mettre en place Git Flow

- [ ] Créer les branches `main` et `develop`
- [ ] Protéger `main` et `develop` (aucun push direct)
- [ ] Documenter le workflow dans le README
- [ ] Former l'équipe aux conventions
- [ ] Mettre en place la CI/CD
- [ ] Définir la convention de nommage des branches
- [ ] Créer un template de Pull Request
- [ ] Établir un processus de code review
- [ ] Planifier les releases (ex: tous les 15 jours)
- [ ] Prévoir un process pour les hotfix

---

## Résumé : Quand utiliser quoi ?

| Type de changement | Branche à utiliser | Part de | Merge dans |
|-------------------|-------------------|---------|------------|
| Nouvelle fonctionnalité | `feature/nom` | `develop` | `develop` |
| Correction bug (dev) | `feature/fix-nom` | `develop` | `develop` |
| Préparation release | `release/x.y.z` | `develop` | `main` + `develop` |
| Bug critique (prod) | `hotfix/nom` | `main` | `main` + `develop` |

---

## Conclusion

Git Flow peut sembler complexe au début, mais c'est comme apprendre à conduire : une fois que vous avez les automatismes, cela devient naturel !

**Les avantages :**
- ✅ Travail en équipe fluide
- ✅ Code en production toujours stable
- ✅ Possibilité de travailler sur plusieurs versions
- ✅ Historique Git propre et lisible
- ✅ Déploiements prévisibles et sécurisés

**Commencez petit :**
1. Mettez en place les branches `main` et `develop`
2. Utilisez les `feature/` branches systématiquement
3. Ajoutez les `release/` et `hotfix/` au fur et à mesure

Avec Git Flow, votre équipe travaillera de manière professionnelle et organisée ! 🚀

⏭️ [Projet personnel guidé](/module-10-cas-pratiques/04-projet-personnel-guide.md)
