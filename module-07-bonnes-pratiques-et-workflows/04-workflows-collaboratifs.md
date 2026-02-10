🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 4. Workflows collaboratifs (Git Flow, GitHub Flow, Trunk-Based)

### Introduction

Quand plusieurs personnes travaillent sur le même projet, il est essentiel d'avoir des **règles communes** pour organiser le travail. C'est ce qu'on appelle un **workflow Git**.

Un workflow définit :
- Comment organiser les **branches**
- Quand créer une **nouvelle branche**
- Comment **fusionner** le code
- Quand **déployer** en production
- Comment gérer les **versions** et les **releases**

Il n'existe pas de workflow parfait. Chacun a ses avantages et inconvénients. Le choix dépend de :
- La taille de l'équipe
- La fréquence des déploiements
- La complexité du projet
- Les contraintes de production

Dans cette section, nous allons explorer les **trois workflows les plus populaires** :

1. **Git Flow** : structuré et robuste, idéal pour les versions multiples
2. **GitHub Flow** : simple et moderne, parfait pour le déploiement continu
3. **Trunk-Based Development** : minimaliste et rapide, pour les équipes matures

---

## 1. Git Flow

### Présentation

**Git Flow** est un workflow créé par Vincent Driessen en 2010. C'est le workflow le plus structuré et le plus complet.

**Philosophie** : Séparer clairement les différents types de travail avec des branches dédiées.

**Idéal pour** :
- Projets avec des **releases planifiées**
- Applications avec **plusieurs versions en production** simultanément
- Équipes qui ont besoin de **stabilité**
- Projets open source avec des contributeurs multiples

**Moins adapté pour** :
- Déploiement continu (CD)
- Petites équipes qui veulent aller vite
- Applications web modernes avec une seule version en production

---

### Les branches de Git Flow

Git Flow utilise **5 types de branches** :

#### 1. `main` (ou `master`)

- Branche **principale** et **stable**
- Contient **uniquement** le code en production
- Chaque commit sur `main` correspond à une **release**
- **Protégée** : pas de commit direct, seulement des merges
- Taguée avec les numéros de version (v1.0.0, v1.1.0, etc.)

#### 2. `develop`

- Branche de **développement** principal
- Contient les dernières fonctionnalités en cours d'intégration
- Point de départ pour les nouvelles features
- Plus **volatile** que `main`, peut contenir des bugs
- Sera mergée dans `main` lors de la prochaine release

#### 3. `feature/*`

- Branches pour développer les **nouvelles fonctionnalités**
- Créées depuis `develop`
- Une branche par fonctionnalité
- Nommées `feature/nom-de-la-fonctionnalite`
- Mergées dans `develop` quand la feature est terminée
- Supprimées après le merge

**Exemples** :
- `feature/user-authentication`
- `feature/payment-system`
- `feature/dark-mode`

#### 4. `release/*`

- Branches pour **préparer une release**
- Créées depuis `develop` quand on est prêt à déployer
- Permettent de finaliser la version (tests finaux, corrections mineures, mise à jour de version)
- Nommées `release/1.0.0`, `release/2.1.0`, etc.
- Mergées dans **`main` ET `develop`**
- Supprimées après le merge

#### 5. `hotfix/*`

- Branches pour **corriger des bugs critiques** en production
- Créées depuis `main` (production)
- Permettent de corriger rapidement sans attendre la prochaine release
- Nommées `hotfix/nom-du-bug`
- Mergées dans **`main` ET `develop`** (pour garder la correction)
- Supprimées après le merge

---

### Le cycle de vie d'une fonctionnalité

Voici le processus complet pour ajouter une fonctionnalité :

#### Étape 1 : Créer une branche feature

```bash
# Se placer sur develop
git checkout develop

# Récupérer les dernières modifications
git pull origin develop

# Créer la branche feature
git checkout -b feature/user-login

# Travailler sur la fonctionnalité
# ... commits ...
```

#### Étape 2 : Développer et commiter

```bash
# Faire vos modifications
git add .
git commit -m "feat: ajoute le formulaire de login"

git add .
git commit -m "feat: implémente l'authentification JWT"

# Pousser régulièrement
git push origin feature/user-login
```

#### Étape 3 : Merger dans develop

```bash
# Se placer sur develop
git checkout develop

# Récupérer les dernières modifications
git pull origin develop

# Merger la feature
git merge --no-ff feature/user-login

# Pousser develop
git push origin develop

# Supprimer la branche locale
git branch -d feature/user-login

# Supprimer la branche distante
git push origin --delete feature/user-login
```

**Note** : `--no-ff` (no fast-forward) crée toujours un commit de merge, même si possible en fast-forward. Cela préserve l'historique des features.

---

### Le cycle d'une release

#### Étape 1 : Créer la branche release

```bash
# Quand develop est prêt pour la production
git checkout develop
git pull origin develop

# Créer la branche release
git checkout -b release/1.0.0
```

#### Étape 2 : Finaliser la release

```bash
# Corriger les derniers bugs
git commit -am "fix: corrige le bug de validation"

# Mettre à jour le numéro de version
# Éditer package.json, version.txt, etc.
git commit -am "chore: bump version to 1.0.0"

# Tests finaux, QA...
```

#### Étape 3 : Merger dans main et develop

```bash
# Merger dans main (production)
git checkout main
git pull origin main
git merge --no-ff release/1.0.0

# Taguer la version
git tag -a v1.0.0 -m "Version 1.0.0"
git push origin main --tags

# Merger aussi dans develop (pour garder les corrections)
git checkout develop
git pull origin develop
git merge --no-ff release/1.0.0
git push origin develop

# Supprimer la branche release
git branch -d release/1.0.0
git push origin --delete release/1.0.0
```

---

### Le cycle d'un hotfix

Un bug critique en production nécessite une correction immédiate :

#### Étape 1 : Créer la branche hotfix

```bash
# Partir de main (production)
git checkout main
git pull origin main

# Créer la branche hotfix
git checkout -b hotfix/security-vulnerability
```

#### Étape 2 : Corriger et tester

```bash
# Faire la correction
git commit -am "fix: corrige la faille de sécurité CVE-2024-1234"

# Tests...
```

#### Étape 3 : Merger dans main et develop

```bash
# Merger dans main
git checkout main
git merge --no-ff hotfix/security-vulnerability

# Taguer la version patch
git tag -a v1.0.1 -m "Hotfix 1.0.1 - Security patch"
git push origin main --tags

# Merger dans develop
git checkout develop
git merge --no-ff hotfix/security-vulnerability
git push origin develop

# Supprimer la branche hotfix
git branch -d hotfix/security-vulnerability
git push origin --delete hotfix/security-vulnerability
```

---

### Schéma visuel de Git Flow

```
main        o---o---o-------o-----------o---o
                     \     /           /   /
release               \   /           /   /
                       o-o           /   /
                        \           /   /
develop     o---o---o---o---o---o---o---o---o
             \     /     \     /
feature       o---o       o---o
                    \
hotfix               \----------o
                                 \
```

---

### Outils pour Git Flow

**Extension Git Flow** : Simpllifie les commandes Git Flow

```bash
# Installation (macOS)
brew install git-flow

# Installation (Linux)
apt-get install git-flow

# Initialiser Git Flow dans un projet
git flow init
```

**Commandes simplifiées** :

```bash
# Créer une feature
git flow feature start user-login

# Terminer une feature
git flow feature finish user-login

# Créer une release
git flow release start 1.0.0

# Terminer une release
git flow release finish 1.0.0

# Créer un hotfix
git flow hotfix start security-fix

# Terminer un hotfix
git flow hotfix finish security-fix
```

---

### Avantages de Git Flow

✅ **Structure claire** : Chaque type de travail a sa branche  
✅ **Historique propre** : Facile de voir les features, releases, hotfixes  
✅ **Multiversioning** : Plusieurs versions en production simultanément  
✅ **Stabilité** : `main` reste toujours stable  
✅ **Processus rigoureux** : Idéal pour les grosses équipes

### Inconvénients de Git Flow

❌ **Complexité** : Beaucoup de branches à gérer  
❌ **Lenteur** : Processus long pour déployer  
❌ **Pas adapté au CD** : Trop lourd pour le déploiement continu  
❌ **Risque de conflits** : Les branches longues créent des conflits  
❌ **Overhead** : Trop pour les petites équipes

---

## 2. GitHub Flow

### Présentation

**GitHub Flow** est un workflow créé par GitHub. C'est une version **simplifiée et moderne** de Git Flow.

**Philosophie** : Restez simple. Déployez souvent.

**Règle d'or** : Tout ce qui est sur `main` est **déployable en production**.

**Idéal pour** :
- **Déploiement continu** (Continuous Deployment)
- Applications web modernes
- Petites et moyennes équipes
- Projets open source sur GitHub
- Startups et environnements agiles

**Moins adapté pour** :
- Applications avec plusieurs versions en production
- Releases planifiées et structurées
- Environnements très réglementés

---

### Les branches de GitHub Flow

GitHub Flow n'utilise que **2 types de branches** :

#### 1. `main`

- Branche **principale** et **unique** de référence
- Toujours **déployable** en production
- Protégée : aucun commit direct
- Tout arrive via des **Pull Requests**

#### 2. Branches de fonctionnalités

- Une branche par feature, bug fix, ou amélioration
- Créées depuis `main`
- Nommées de façon descriptive
- Courte durée de vie (quelques jours maximum)
- Mergées dans `main` via Pull Request
- Supprimées après le merge

**Exemples de noms** :
- `add-user-authentication`
- `fix-login-bug`
- `update-readme`
- `improve-performance`

---

### Le workflow GitHub Flow en 6 étapes

#### Étape 1 : Créer une branche

```bash
# Se placer sur main
git checkout main

# Récupérer les dernières modifications
git pull origin main

# Créer une nouvelle branche
git checkout -b add-search-feature
```

**Règle** : Utilisez des noms **descriptifs** et **explicites**.

#### Étape 2 : Faire des commits

```bash
# Développer la fonctionnalité
git add .
git commit -m "feat: ajoute la barre de recherche"

git add .
git commit -m "feat: implémente la logique de recherche"

git add .
git commit -m "test: ajoute les tests de recherche"
```

**Bonnes pratiques** :
- Commits **atomiques** et **fréquents**
- Messages de commit **clairs**
- Pousser régulièrement sur le dépôt distant

#### Étape 3 : Pousser régulièrement

```bash
# Pousser la branche
git push origin add-search-feature
```

**Avantages** :
- Sauvegarde dans le cloud
- Visibilité pour l'équipe
- Possibilité de collaboration

#### Étape 4 : Ouvrir une Pull Request (PR)

Sur GitHub (ou GitLab, Bitbucket) :
1. Cliquez sur "New Pull Request"
2. Sélectionnez votre branche
3. Donnez un **titre clair**
4. Décrivez ce que fait votre code
5. Mentionnez les issues concernées (`Fixes #123`)
6. Demandez des reviewers

**Exemple de description de PR** :

```markdown
## Description
Ajoute une fonctionnalité de recherche dans la barre de navigation.

## Changements
- Nouvelle barre de recherche dans le header
- API endpoint `/search`
- Tests unitaires et d'intégration

## Captures d'écran
[Image de la nouvelle fonctionnalité]

## Checklist
- [x] Tests passent
- [x] Documentation mise à jour
- [x] Code reviewé

Fixes #234
```

**💡 Astuce** : Vous pouvez ouvrir une PR **même si le code n'est pas terminé** en la marquant comme "Draft". Cela permet de recevoir des retours tôt.

#### Étape 5 : Discussion et revue de code

- L'équipe **review** le code
- Discussions dans les commentaires
- Demandes de modifications si nécessaire
- Vous faites les corrections et poussez de nouveaux commits

```bash
# Faire les corrections demandées
git add .
git commit -m "fix: corrige les remarques de la review"
git push origin add-search-feature
```

La PR se met à jour automatiquement.

#### Étape 6 : Merger et déployer

Une fois la PR approuvée :

```bash
# Via l'interface GitHub : cliquer sur "Merge pull request"
```

**Options de merge** :
- **Merge commit** : Garde tous les commits + un commit de merge
- **Squash and merge** : Combine tous les commits en un seul (recommandé)
- **Rebase and merge** : Rejoue les commits un par un

Après le merge :

```bash
# Supprimer la branche (automatique sur GitHub)
# Mettre à jour main localement
git checkout main
git pull origin main

# Supprimer la branche locale
git branch -d add-search-feature
```

**Déploiement** : Avec le CI/CD configuré, le déploiement se fait automatiquement après le merge dans `main`.

---

### Les Pull Requests : le cœur de GitHub Flow

La **Pull Request** (PR) n'est pas qu'un simple merge. C'est un **outil de collaboration** :

#### Fonctionnalités clés

1. **Revue de code**
   - Commentaires ligne par ligne
   - Suggestions de modifications
   - Approbation obligatoire

2. **Discussions**
   - Questions et réponses
   - Décisions d'architecture
   - Historique des débats

3. **Tests automatiques**
   - CI/CD intégré
   - Tests obligatoires avant merge
   - Déploiement preview

4. **Documentation**
   - Traçabilité des changements
   - Lien avec les issues
   - Historique des décisions

---

### Protection de la branche main

Pour que GitHub Flow fonctionne bien, `main` doit être **protégée** :

**Paramètres recommandés** (sur GitHub) :
- ✅ Require pull request reviews (1-2 approbations)
- ✅ Require status checks to pass (tests CI/CD)
- ✅ Require branches to be up to date
- ✅ Require linear history (optionnel)
- ✅ Include administrators (même les admins suivent les règles)

**Configuration** :
Settings → Branches → Branch protection rules → Add rule

---

### Schéma visuel de GitHub Flow

```
main    o---o---o-------o-------o-------o
             \         /       /       /
feature       o---o---o       /       /
                       \     /       /
feature                 o---o       /
                                   /
feature                       o---o
```

---

### Variantes de GitHub Flow

#### GitLab Flow

GitLab a créé une variante avec des branches d'environnement :

```
main → staging → production
```

- `main` : développement
- `staging` : pré-production
- `production` : production

Chaque merge déclenche un déploiement dans l'environnement correspondant.

#### GitHub Flow + Release branches

Pour des releases planifiées :

```
main    o---o---o---o
             \
release/1.0   o---o (déployé quand prêt)
```

---

### Avantages de GitHub Flow

✅ **Simplicité** : Une seule branche principale  
✅ **Rapidité** : Déploiement continu possible  
✅ **Collaboration** : Les PR facilitent la revue  
✅ **Traçabilité** : Tout est documenté dans les PR  
✅ **Flexibilité** : S'adapte aux petites et moyennes équipes  
✅ **Moderne** : Conçu pour le web moderne

### Inconvénients de GitHub Flow

❌ **Une seule version** : Pas adapté au multiversioning  
❌ **Dépendance CI/CD** : Nécessite une bonne automatisation  
❌ **Releases complexes** : Pas de processus formel de release  
❌ **Rollback difficile** : Nécessite un hotfix si problème en prod

---

## 3. Trunk-Based Development

### Présentation

**Trunk-Based Development** (TBD) est le workflow le plus **minimaliste** et **rapide**.

**Philosophie** : Tout le monde travaille sur la même branche (le "trunk" = `main`). Les branches de feature sont très courtes (quelques heures) ou n'existent pas.

**Idéal pour** :
- Équipes **très matures** avec forte culture DevOps
- **Déploiement continu** intensif (plusieurs fois par jour)
- Projets avec **tests automatisés solides**
- Équipes qui pratiquent le **pair/mob programming**

**Moins adapté pour** :
- Équipes débutantes
- Projets sans bonne couverture de tests
- Équipes distribuées géographiquement
- Projets avec releases complexes

---

### Les principes de TBD

#### 1. Une seule branche principale : `main`

Tout le monde commit directement sur `main` ou via des branches très courtes (< 1 jour).

#### 2. Branches de feature ultra-courtes

Si vous utilisez des branches :
- Durée de vie : **quelques heures maximum**
- Petites modifications seulement
- Mergées **plusieurs fois par jour**

#### 3. Feature Flags (Feature Toggles)

Pour développer de grosses fonctionnalités sans branches longues :

```javascript
if (featureFlags.isEnabled('newSearchAlgorithm')) {
  // Nouveau code
  return newSearch(query);
} else {
  // Ancien code
  return oldSearch(query);
}
```

**Avantages** :
- Code toujours sur `main`
- Activation/désactivation sans redéploiement
- Tests en production sur un % d'utilisateurs
- Rollback instantané

#### 4. Integration continue stricte

- Tests automatiques **obligatoires**
- Builds **très rapides** (< 10 minutes)
- Feedback **immédiat**
- Tout échec bloque l'équipe

#### 5. Commits fréquents

Équipes TBD commutent souvent **10-20 fois par jour** :
- Petits changements
- Testés
- Intégrés immédiatement

---

### Le workflow TBD

#### Option 1 : Commit direct sur main (rare)

```bash
# Se placer sur main
git checkout main
git pull origin main

# Faire un petit changement
git add .
git commit -m "fix: corrige le typo dans le footer"

# Pousser immédiatement
git push origin main
```

**Condition** : Nécessite une **grande confiance** et des **tests solides**.

#### Option 2 : Branches ultra-courtes (plus courant)

```bash
# Créer une branche pour quelques heures
git checkout -b fix-typo

# Faire le changement
git add .
git commit -m "fix: corrige le typo"

# Pousser et ouvrir une PR
git push origin fix-typo

# Merger rapidement (< 2h)
# via PR ou directement

# Supprimer la branche
git branch -d fix-typo
```

#### Option 3 : Feature Flags pour grandes features

```bash
# Commit 1 : Ajouter le flag
git commit -m "feat: ajoute le feature flag newPaymentSystem"

# Commit 2 : Commencer l'implémentation (désactivée)
git commit -m "feat(payment): ajoute la structure (behind flag)"

# Commit 3 : Suite de l'implémentation
git commit -m "feat(payment): implémente la logique (behind flag)"

# Commit 4 : Activer pour 10% des users
git commit -m "feat(payment): active pour 10% des utilisateurs"

# Commit 5 : Activer pour tous
git commit -m "feat(payment): active pour 100%"

# Commit 6 : Nettoyer le flag
git commit -m "chore(payment): supprime le feature flag"
```

---

### Schéma visuel de Trunk-Based Development

```
main    o-o-o-o-o-o-o-o-o-o-o-o-o-o
         \-/ \-/   \-/   \-/
        (branches de quelques heures)
```

---

### Les Feature Flags en détail

Les feature flags sont **essentiels** à TBD.

#### Implémentation basique

```javascript
// config/features.js
const featureFlags = {
  newDashboard: process.env.FEATURE_NEW_DASHBOARD === 'true',
  betaSearch: process.env.FEATURE_BETA_SEARCH === 'true',
  darkMode: true, // Activé pour tous
};

// Utilisation
if (featureFlags.newDashboard) {
  return <NewDashboard />;
} else {
  return <OldDashboard />;
}
```

#### Services de Feature Flags

Pour des besoins avancés, utilisez des outils dédiés :
- **LaunchDarkly**
- **Unleash**
- **ConfigCat**
- **Split.io**

**Fonctionnalités avancées** :
- Activation par % d'utilisateurs
- Activation par segments (bêta-testeurs, VIP, région)
- A/B testing
- Rollback instantané
- Analytics

---

### Branch by Abstraction

Technique pour refactorer sans branches longues :

#### Étape 1 : Créer une abstraction

```javascript
// Abstraction qui utilise l'ancien code
function search(query) {
  return oldSearchImplementation(query);
}
```

#### Étape 2 : Remplacer tous les appels

```javascript
// Avant
const results = oldSearchImplementation(query);

// Après
const results = search(query);
```

#### Étape 3 : Ajouter la nouvelle implémentation

```javascript
function search(query) {
  if (useNewSearch) {
    return newSearchImplementation(query);
  } else {
    return oldSearchImplementation(query);
  }
}
```

#### Étape 4 : Basculer progressivement

```javascript
function search(query) {
  return newSearchImplementation(query); // 100% nouveau
}
```

#### Étape 5 : Supprimer l'ancien code

```javascript
function search(query) {
  return newSearchImplementation(query);
}
// Supprimer oldSearchImplementation
```

Chaque étape est un commit sur `main`, le code reste toujours fonctionnel.

---

### Releases dans TBD

#### Option 1 : Déploiement continu

Chaque commit sur `main` déclenche un déploiement automatique.

#### Option 2 : Release branches

Pour les releases planifiées :

```bash
# Créer une branche de release depuis main
git checkout -b release/2024-10
git push origin release/2024-10

# main continue d'évoluer
# La branche release ne reçoit que des hotfixes
```

#### Option 3 : Tags de release

```bash
# Taguer main à un moment donné
git tag -a v1.2.3 -m "Release 1.2.3"
git push origin v1.2.3

# Déployer ce tag en production
```

---

### Avantages de Trunk-Based Development

✅ **Simplicité maximale** : Une seule branche  
✅ **Intégration continue réelle** : Pas de branches longues = pas de conflits  
✅ **Feedback rapide** : Problèmes détectés immédiatement  
✅ **Déploiement très fréquent** : Plusieurs fois par jour  
✅ **Moins de merge conflicts** : Tout le monde travaille au même endroit  
✅ **Culture de qualité** : Les tests deviennent essentiels

### Inconvénients de Trunk-Based Development

❌ **Nécessite maturité** : Équipe expérimentée requise  
❌ **Tests obligatoires** : Impossible sans bonne couverture de tests  
❌ **Complexité des feature flags** : Peut devenir un code spaghetti  
❌ **Pression** : Chaque commit va en prod, stress élevé  
❌ **Courbe d'apprentissage** : Difficile pour les débutants  
❌ **Outillage** : Nécessite infrastructure CI/CD solide

---

## Comparaison des workflows

### Tableau récapitulatif

| Critère | Git Flow | GitHub Flow | Trunk-Based |
|---------|----------|-------------|-------------|
| **Complexité** | ⭐⭐⭐ Élevée | ⭐⭐ Moyenne | ⭐ Faible |
| **Nombre de branches** | Beaucoup (5 types) | Peu (1 main + features) | Très peu (1 main) |
| **Durée des branches** | Longue (jours/semaines) | Moyenne (1-3 jours) | Très courte (heures) |
| **Fréquence de déploiement** | Faible (weeks) | Moyenne (daily) | Élevée (plusieurs/jour) |
| **Courbe d'apprentissage** | Difficile | Facile | Moyenne |
| **Adapté débutants** | ❌ Non | ✅ Oui | ❌ Non |
| **Adapté au CD** | ❌ Non | ✅ Oui | ✅✅ Excellent |
| **Multiversioning** | ✅✅ Excellent | ❌ Non | ⭐ Possible |
| **Stabilité** | ✅✅ Très stable | ✅ Stable | ⭐ Variable |
| **Idéal pour** | Gros projets | Projets web | Équipes matures |

---

### Quand utiliser Git Flow ?

✅ **Utilisez Git Flow si** :
- Vous avez des **releases planifiées** (tous les 3-6 mois)
- Vous devez maintenir **plusieurs versions** en production
- Vous avez une **grosse équipe** (>15 personnes)
- Vous travaillez sur un **produit boxed** (logiciel vendu avec versions)
- Vous êtes dans un **environnement réglementé** (banque, santé)
- Vous avez besoin d'un **processus de release formel**

**Exemples** :
- Applications desktop (Photoshop, Office)
- Systèmes embarqués
- Applications mobiles avec review store
- Produits open source majeurs (Linux)

---

### Quand utiliser GitHub Flow ?

✅ **Utilisez GitHub Flow si** :
- Vous faites du **déploiement continu** (plusieurs fois par semaine)
- Vous avez une **petite ou moyenne équipe** (2-15 personnes)
- Vous travaillez sur une **application web ou SaaS**
- Vous n'avez qu'**une seule version** en production
- Vous utilisez déjà **GitHub/GitLab/Bitbucket**
- Vous voulez un **workflow simple et moderne**

**Exemples** :
- Startups web
- Applications SaaS
- Sites web et APIs
- Projets open source sur GitHub
- Équipes agiles

---

### Quand utiliser Trunk-Based Development ?

✅ **Utilisez TBD si** :
- Vous déployez **plusieurs fois par jour**
- Vous avez une **équipe mature** avec forte culture DevOps
- Vous avez une **excellente couverture de tests** (>80%)
- Vous pratiquez le **pair programming**
- Vous avez un **CI/CD très rapide** (<10 min)
- Vous êtes prêts à utiliser des **feature flags**

**Exemples** :
- Google, Facebook, Amazon (équipes internes)
- Startups avec forte culture technique
- Équipes DevOps avancées
- Produits nécessitant innovation rapide

---

## Bonnes pratiques communes

Quel que soit le workflow choisi :

### 1. Protégez la branche principale

```bash
# Sur GitHub, GitLab, Bitbucket :
# Settings → Branches → Protect branch 'main'
```

Règles recommandées :
- Pas de push direct
- Revue de code obligatoire
- Tests CI/CD passants
- Branche à jour avant merge

### 2. Faites des commits atomiques

Un commit = une modification logique complète.

### 3. Écrivez de bons messages de commit

```bash
# Bon
feat: ajoute l'authentification Google

# Mauvais
Update
```

### 4. Revue de code systématique

- Au moins 1-2 reviewers
- Commentaires constructifs
- Vérification des tests
- Respect des standards

### 5. Automatisez tout

- Tests unitaires
- Tests d'intégration
- Linting et formatting
- Builds
- Déploiements

### 6. Gardez les branches courtes

Même avec Git Flow :
- Features courtes (< 1 semaine)
- Synchro régulière avec develop
- Merge dès que possible

### 7. Documentez votre workflow

Créez un `CONTRIBUTING.md` :

```markdown
# Comment contribuer

## Workflow
Nous utilisons GitHub Flow :

1. Créer une branche depuis `main`
2. Faire vos modifications
3. Ouvrir une Pull Request
4. Attendre 1 approbation
5. Merger et déployer

## Standards
- Messages de commit selon Conventional Commits
- Tests obligatoires pour toute nouvelle feature
- Couverture de code minimum : 80%
```

### 8. Communiquez

- Stand-ups quotidiens
- Documentation dans les PR
- Commentaires dans le code
- Discussions sur les décisions

---

## Choisir et adopter un workflow

### Évaluer vos besoins

Posez-vous ces questions :

1. **Quelle est la taille de l'équipe ?**
   - Petite (1-5) → GitHub Flow ou TBD
   - Moyenne (5-15) → GitHub Flow
   - Grande (15+) → Git Flow

2. **À quelle fréquence déployez-vous ?**
   - Plusieurs fois par jour → TBD
   - Quotidien/hebdomadaire → GitHub Flow
   - Mensuel/trimestriel → Git Flow

3. **Quel est le niveau de maturité ?**
   - Débutants → GitHub Flow
   - Intermédiaire → GitHub Flow ou Git Flow
   - Experts → TBD

4. **Avez-vous besoin de plusieurs versions ?**
   - Oui → Git Flow
   - Non → GitHub Flow ou TBD

5. **Quelle est votre couverture de tests ?**
   - < 50% → Git Flow (plus safe)
   - 50-80% → GitHub Flow
   - > 80% → TBD possible

### Transition progressive

Ne changez pas tout d'un coup :

#### Phase 1 : Améliorer l'existant (1-2 mois)
- Protégez `main`
- Instaurez les PR obligatoires
- Ajoutez des tests automatiques

#### Phase 2 : Adopter le nouveau workflow (2-3 mois)
- Documentez le workflow choisi
- Formez l'équipe
- Commencez sur un projet pilote

#### Phase 3 : Optimiser (continu)
- Recueillez les retours
- Ajustez les règles
- Automatisez davantage

---

## Conclusion

Il n'y a pas de workflow parfait. Le meilleur workflow est celui qui :
- **S'adapte à votre équipe** et ses compétences
- **Correspond à vos contraintes** de déploiement
- **Évolue avec le projet** au fil du temps

**Recommandations** :

- **Vous débutez ?** → Commencez avec **GitHub Flow**
- **Vous avez besoin de structure ?** → Essayez **Git Flow**
- **Vous voulez la vitesse ?** → Progressez vers **Trunk-Based**

**L'essentiel** :
1. Choisissez un workflow et **documentez-le**
2. Assurez-vous que **toute l'équipe** le comprend
3. Utilisez des **outils** pour l'automatiser
4. **Itérez** et améliorez continuellement

**Prochaine étape** : Maintenant que vous savez organiser vos branches, voyons comment les structurer avec une convention de nommage claire !

⏭️ [Organisation des branches (main, dev, feature/, hotfix/)](/module-07-bonnes-pratiques-et-workflows/05-organisation-des-branches.md)
