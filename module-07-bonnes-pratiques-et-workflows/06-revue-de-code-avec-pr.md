🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 6. Revue de code avec Pull Requests

### Introduction

Imaginez que vous écrivez un article pour un journal. Avant publication, plusieurs personnes le relisent : un correcteur vérifie l'orthographe, un rédacteur en chef valide le fond, un expert vérifie les faits. Ce processus garantit la qualité du contenu final.

Dans le développement logiciel, c'est exactement le même principe : la **revue de code** (code review) est le processus par lequel vos collègues examinent votre code avant qu'il ne soit intégré au projet principal.

La **Pull Request** (ou **Merge Request** sur GitLab) est l'outil qui rend ce processus possible. C'est bien plus qu'un simple merge : c'est un espace de collaboration, de discussion et d'amélioration continue.

---

## Qu'est-ce qu'une Pull Request ?

### Définition

Une **Pull Request** (PR) est une demande formelle pour intégrer vos modifications dans une branche du dépôt.

**Techniquement** :
- Vous avez fait des commits sur votre branche
- Vous demandez de "tirer" (pull) vos modifications vers une autre branche (généralement `main` ou `develop`)
- Votre équipe examine votre code avant de l'accepter

**En pratique**, c'est un espace qui contient :
- 📝 Une **description** de ce que fait votre code
- 💻 Le **code** ajouté/modifié/supprimé
- 💬 Des **discussions** sur le code
- ✅ Des **validations** (tests automatiques, approbations)
- 📊 Des **statistiques** (lignes modifiées, fichiers touchés)

### Pourquoi "Pull" Request ?

Le nom peut sembler contre-intuitif :
- Vous **poussez** (push) votre code sur une branche
- Puis vous demandez qu'on **tire** (pull) vos modifications

C'est du point de vue du dépôt principal : vous demandez au projet de "tirer" vos contributions.

**Terminologie** :
- **GitHub** : Pull Request (PR)
- **GitLab** : Merge Request (MR)
- **Bitbucket** : Pull Request (PR)
- **Azure DevOps** : Pull Request (PR)

Le concept est le même, seul le nom change.

---

## Pourquoi faire des Pull Requests ?

### 1. Qualité du code

La revue de code permet de :
- 🐛 **Détecter les bugs** avant qu'ils arrivent en production
- 🎨 **Améliorer la lisibilité** et la maintenabilité
- 📚 **Partager les connaissances** dans l'équipe
- 🔒 **Identifier les failles de sécurité**
- ⚡ **Optimiser les performances**

### 2. Collaboration et apprentissage

- **Feedback constructif** : Apprendre des autres
- **Mentoring** : Les seniors aident les juniors
- **Cohérence** : Tout le monde suit les mêmes standards
- **Visibilité** : Comprendre ce sur quoi travaillent les autres

### 3. Documentation et traçabilité

- **Historique** : Pourquoi tel changement a été fait
- **Discussions** : Les décisions d'architecture sont documentées
- **Références** : Lien avec les issues/tickets
- **Contexte** : Explications détaillées des modifications

### 4. Protection du code

- **Validation** : Au moins 1-2 paires d'yeux avant le merge
- **Tests automatiques** : CI/CD vérifie que tout fonctionne
- **Prévention** : Impossible de casser `main` par accident

---

## Le cycle de vie d'une Pull Request

### Vue d'ensemble

```
1. Créer une branche
    ↓
2. Développer et commiter
    ↓
3. Pousser la branche
    ↓
4. Ouvrir une Pull Request
    ↓
5. Discussion et revue
    ↓
6. Modifications si nécessaire
    ↓
7. Approbation
    ↓
8. Merge
    ↓
9. Suppression de la branche
```

---

## Créer une bonne Pull Request

### Étape 1 : Préparer votre branche

Avant d'ouvrir une PR, assurez-vous que :

```bash
# Votre branche est à jour avec la branche cible
git checkout develop  
git pull origin develop  
git checkout feature/ma-feature  
git merge develop

# Ou avec rebase (préférable)
git rebase develop

# Tous vos tests passent
npm test  # ou votre commande de test

# Le code est propre
npm run lint  # si vous utilisez un linter
```

### Étape 2 : Pousser votre branche

```bash
# Première fois
git push -u origin feature/ma-feature

# Mises à jour suivantes
git push origin feature/ma-feature
```

### Étape 3 : Ouvrir la Pull Request

Sur **GitHub** :
1. Allez sur votre dépôt
2. Cliquez sur "Pull requests" → "New pull request"
3. Sélectionnez les branches :
   - **Base** : `develop` (ou `main`)
   - **Compare** : `feature/ma-feature`
4. Cliquez sur "Create pull request"

---

## Anatomie d'une bonne Pull Request

### 1. Le titre

**Format recommandé** : Suivez les mêmes conventions que les commits

```
feat: ajoute l'authentification Google OAuth  
fix: corrige le bug de pagination  
refactor: simplifie le module de paiement  
docs: met à jour le guide d'installation
```

**Règles** :
- ✅ Clair et concis (50-70 caractères)
- ✅ Commence par un verbe d'action
- ✅ Décrit CE QUE fait la PR
- ❌ Pas de "WIP", "Update", "Fix stuff"

### 2. La description

Une bonne description contient :

#### a) Résumé du changement

```markdown
## Description

Cette PR ajoute l'authentification via Google OAuth pour permettre  
aux utilisateurs de se connecter avec leur compte Google.
```

#### b) Motivation et contexte

```markdown
## Pourquoi ?

Les utilisateurs demandent depuis longtemps une option de connexion  
simplifiée. L'authentification Google réduit les frictions et améliore  
l'expérience utilisateur.

Closes #234  
Related to #189
```

#### c) Type de changement

```markdown
## Type de changement

- [x] Nouvelle fonctionnalité (non-breaking change)
- [ ] Correction de bug (non-breaking change)
- [ ] Breaking change (modification qui casse la compatibilité)
- [ ] Documentation uniquement
```

#### d) Changements apportés

```markdown
## Changements

- Ajout du bouton "Se connecter avec Google"
- Implémentation du flux OAuth 2.0
- Création du service `GoogleAuthService`
- Ajout des tests unitaires et d'intégration
- Mise à jour de la documentation utilisateur
```

#### e) Captures d'écran (si pertinent)

```markdown
## Captures d'écran

### Avant
![Ancien écran de login](before.png)

### Après
![Nouvel écran avec Google OAuth](after.png)
```

#### f) Comment tester

```markdown
## Comment tester

1. Démarrer l'application : `npm start`
2. Aller sur `/login`
3. Cliquer sur "Se connecter avec Google"
4. S'authentifier avec un compte Google de test
5. Vérifier que la connexion fonctionne
```

#### g) Checklist

```markdown
## Checklist

- [x] Mon code suit les conventions du projet
- [x] J'ai effectué une revue de mon propre code
- [x] J'ai commenté les parties complexes
- [x] J'ai mis à jour la documentation
- [x] Mes modifications ne génèrent pas de nouveaux warnings
- [x] J'ai ajouté des tests qui prouvent que ma correction fonctionne
- [x] Les tests unitaires passent localement
- [x] Les tests d'intégration passent localement
```

### Template de Pull Request complet

**`.github/PULL_REQUEST_TEMPLATE.md`** :

```markdown
## Description

Décrivez clairement les modifications apportées.

## Type de changement

- [ ] 🐛 Bug fix (correction qui ne casse pas l'existant)
- [ ] ✨ Nouvelle fonctionnalité (ajout qui ne casse pas l'existant)
- [ ] 💥 Breaking change (modification qui casse la compatibilité)
- [ ] 📝 Documentation uniquement
- [ ] 🎨 Style (formatage, renommage de variables)
- [ ] ♻️ Refactoring (ni bug fix ni nouvelle fonctionnalité)
- [ ] ⚡️ Performance
- [ ] ✅ Tests

## Motivation et contexte

Pourquoi ce changement est-il nécessaire ? Quel problème résout-il ?

Fixes #(issue)

## Comment a-t-il été testé ?

Décrivez les tests que vous avez effectués.

- [ ] Test A
- [ ] Test B

**Configuration de test** :
- OS :
- Navigateur :
- Version :

## Captures d'écran (si applicable)

Ajoutez des captures d'écran pour aider à expliquer les changements visuels.

## Checklist

- [ ] Mon code suit les conventions de style du projet
- [ ] J'ai effectué une auto-revue de mon code
- [ ] J'ai commenté mon code, particulièrement les parties complexes
- [ ] J'ai mis à jour la documentation correspondante
- [ ] Mes modifications ne génèrent pas de nouveaux warnings
- [ ] J'ai ajouté des tests qui prouvent que ma correction fonctionne
- [ ] Les tests nouveaux et existants passent localement
- [ ] Toutes les dépendances ajoutées sont documentées

## Notes additionnelles

Toute information supplémentaire pertinente pour les reviewers.
```

---

## Faire une revue de code efficace

### Les rôles dans une revue

**L'auteur** (celui qui a créé la PR) :
- Explique clairement son code
- Répond aux questions
- Prend en compte les retours
- Ne prend pas les critiques personnellement

**Le reviewer** (celui qui revoit le code) :
- Examine le code attentivement
- Pose des questions constructives
- Suggère des améliorations
- Approuve ou demande des changements

### Que vérifier dans une revue de code ?

#### 1. La fonctionnalité

✅ **Questions à se poser** :
- Le code fait-il ce qu'il est censé faire ?
- Les cas limites sont-ils gérés ?
- Y a-t-il des bugs évidents ?
- La logique est-elle correcte ?

**Exemple de commentaire** :
```markdown
❓ Que se passe-t-il si `email` est `null` ou `undefined` ?
Il faudrait peut-être ajouter une validation.
```

#### 2. La lisibilité

✅ **Questions à se poser** :
- Le code est-il facile à comprendre ?
- Les noms de variables sont-ils explicites ?
- Y a-t-il des commentaires pour les parties complexes ?
- La structure est-elle logique ?

**Exemple de commentaire** :
```markdown
💡 Suggestion : `getUserById` serait plus explicite que `getUser`
```

#### 3. Les tests

✅ **Questions à se poser** :
- Y a-t-il des tests pour les nouvelles fonctionnalités ?
- Les tests couvrent-ils les cas limites ?
- Les tests sont-ils clairs et maintenables ?
- Tous les tests passent-ils ?

**Exemple de commentaire** :
```markdown
📋 Pourrais-tu ajouter un test pour le cas où l'utilisateur n'existe pas ?
```

#### 4. Les performances

✅ **Questions à se poser** :
- Y a-t-il des opérations coûteuses inutiles ?
- Les boucles sont-elles optimisées ?
- Y a-t-il des requêtes N+1 ?
- La mémoire est-elle bien gérée ?

**Exemple de commentaire** :
```markdown
⚡ Cette boucle pourrait être optimisée avec un `Map` au lieu d'un `Array.find()`
```

#### 5. La sécurité

✅ **Questions à se poser** :
- Les entrées utilisateur sont-elles validées ?
- Y a-t-il des failles XSS, SQL injection, etc. ?
- Les secrets sont-ils correctement protégés ?
- Les permissions sont-elles vérifiées ?

**Exemple de commentaire** :
```markdown
🔒 Attention : ce endpoint devrait vérifier que l'utilisateur est authentifié
```

#### 6. La maintenabilité

✅ **Questions à se poser** :
- Le code respecte-t-il les conventions du projet ?
- Y a-t-il de la duplication ?
- Le code est-il modulaire ?
- La dette technique est-elle minimale ?

**Exemple de commentaire** :
```markdown
♻️ Cette logique existe déjà dans `utils/validation.js`, on pourrait la réutiliser
```

---

## Comment commenter efficacement

### Les types de commentaires

#### 1. Questions

Quand quelque chose n'est pas clair :

```markdown
❓ Pourquoi utilises-tu `setTimeout` ici ? Est-ce vraiment nécessaire ?
```

#### 2. Suggestions

Pour proposer une amélioration :

```markdown
💡 Suggestion : Tu pourrais utiliser la syntaxe destructuring ici :
```javascript
// Au lieu de
const name = user.name;  
const email = user.email;

// Utilise
const { name, email } = user;
```

#### 3. Problèmes bloquants

Quand quelque chose doit absolument être corrigé :

```markdown
🚫 Bloquant : Cette condition provoquera une erreur si `data` est `null`.
Il faut ajouter une vérification.
```

#### 4. Nits (détails mineurs)

Pour des détails non bloquants :

```markdown
🔍 Nit : Petite typo dans le commentaire (ligne 42) : "recieve" → "receive"
```

#### 5. Compliments

N'oubliez pas de souligner ce qui est bien fait !

```markdown
✨ Excellente gestion des erreurs ! C'est très clair.  
👍 Bonne idée d'extraire cette logique dans une fonction séparée.  
🎉 Les tests sont vraiment complets, bravo !
```

### Principes d'une bonne revue

#### 1. Soyez constructif et bienveillant

❌ **Mauvais** :
```
Ce code est nul. Tu ne sais pas coder ?
```

✅ **Bon** :
```
Cette approche pourrait être améliorée. As-tu considéré utiliser  
un design pattern Observer ? Cela rendrait le code plus maintenable.  
Voici un exemple : [lien vers doc]
```

#### 2. Expliquez le "pourquoi"

❌ **Mauvais** :
```
Change ça.
```

✅ **Bon** :
```
Je suggère d'utiliser `const` plutôt que `let` ici car la variable  
n'est jamais réassignée. Cela rend l'intention du code plus claire  
et évite les bugs potentiels.
```

#### 3. Posez des questions plutôt que d'imposer

❌ **Mauvais** :
```
Tu dois utiliser async/await ici.
```

✅ **Bon** :
```
As-tu considéré utiliser async/await plutôt que .then() ?  
Cela pourrait améliorer la lisibilité. Qu'en penses-tu ?
```

#### 4. Distinguez les opinions des faits

✅ **Opinion** (acceptable) :
```
💭 Avis personnel : Je préférerais utiliser une Guard Clause ici,
mais ton approche fonctionne aussi.
```

✅ **Fait** (objectif) :
```
⚠️ Ce code a une complexité cyclomatique de 15, ce qui dépasse
notre limite de 10. Il devrait être refactorisé.
```

#### 5. Référencez la documentation

✅ **Bon** :
```
📚 Selon notre guide de style (section 3.2), les fonctions async
doivent toujours gérer les erreurs avec try/catch.
[Lien vers le guide]
```

#### 6. Soyez spécifique

❌ **Vague** :
```
Il y a un problème de performance ici.
```

✅ **Spécifique** :
```
⚡ Cette requête dans une boucle crée un problème N+1. Avec 1000 utilisateurs,
cela génère 1000 requêtes au lieu d'une seule. Solution suggérée :  
utiliser un JOIN ou un bulk query.
```

---

## Répondre aux commentaires de revue

### En tant qu'auteur

#### 1. Restez professionnel et ouvert

Les revues sont pour **améliorer le code**, pas pour critiquer la personne.

❌ **Mauvais** :
```
Je sais ce que je fais, laisse-moi tranquille.
```

✅ **Bon** :
```
Merci pour ton retour ! Je comprends ton point. J'ai choisi  
cette approche parce que [raison]. Es-tu d'accord ou préfères-tu  
que je modifie ?
```

#### 2. Répondez à tous les commentaires

Chaque commentaire mérite une réponse, même un simple "Corrigé ✓"

**Types de réponses** :
- **✓ Corrigé** : Vous avez fait le changement
- **❓ Question** : Vous demandez des clarifications
- **💭 Explication** : Vous expliquez votre choix
- **👍 D'accord** : Vous allez le faire dans une prochaine PR

#### 3. Marquez les conversations résolues

Après avoir répondu et/ou corrigé, marquez la conversation comme résolue.

#### 4. Repoussez avec les nouvelles modifications

```bash
# Faire les corrections
git add .  
git commit -m "fix: corrige les remarques de la review"

# Pousser (la PR se met à jour automatiquement)
git push origin feature/ma-feature
```

#### 5. Demandez une nouvelle revue si nécessaire

Si les changements sont importants, demandez au reviewer de revoir.

---

## Le processus d'approbation

### États d'une Pull Request

#### 1. Draft (Brouillon)

```
🚧 Work in Progress
```

- PR non terminée
- Permet d'avoir des retours précoces
- Ne peut pas être mergée

**Quand l'utiliser** :
- Vous voulez des retours tôt sur votre approche
- Vous travaillez sur quelque chose de complexe
- Vous voulez montrer votre progression

#### 2. Awaiting Review (En attente de revue)

```
👀 Ready for Review
```

- PR complète et prête
- En attente de reviewers
- Peut recevoir des commentaires

#### 3. Changes Requested (Changements demandés)

```
🔄 Changes Requested
```

- Le reviewer a demandé des modifications
- L'auteur doit corriger
- Ne peut pas être mergée

#### 4. Approved (Approuvée)

```
✅ Approved
```

- Au moins X reviewers ont approuvé (selon config)
- Les tests CI/CD passent
- Prête à être mergée

#### 5. Merged (Mergée)

```
✓ Merged
```

- Code intégré à la branche cible
- PR fermée
- Branche peut être supprimée

---

## Les options de merge

### 1. Merge Commit

```
     A---B---C feature
    /         \
---D---E---F---G---H main
```

**Commande** : `git merge --no-ff`

**Caractéristiques** :
- Crée un **commit de merge**
- Préserve **tout l'historique** des commits de la branche
- L'historique montre clairement quand la feature a été intégrée

**Avantages** :
- ✅ Historique complet et transparent
- ✅ Facile de voir quels commits appartiennent à quelle feature
- ✅ Facile de revert toute la feature d'un coup

**Inconvénients** :
- ❌ Historique peut devenir complexe
- ❌ Beaucoup de commits de merge

**Quand l'utiliser** :
- Workflows Git Flow ou GitHub Flow
- Vous voulez garder l'historique complet
- Vous avez des commits atomiques bien organisés

### 2. Squash and Merge

```
     A---B---C feature
    /
---D---E---F---G main
               (A+B+C squashés)
```

**Commande** : `git merge --squash`

**Caractéristiques** :
- **Combine tous les commits** de la PR en un seul
- Historique linéaire sur `main`
- Les commits intermédiaires disparaissent

**Avantages** :
- ✅ Historique propre et linéaire
- ✅ Un commit = une fonctionnalité
- ✅ Facile de comprendre l'évolution du projet

**Inconvénients** :
- ❌ Perte de l'historique détaillé
- ❌ Impossible de voir les étapes intermédiaires

**Quand l'utiliser** :
- Vous voulez un historique très propre
- Vos branches de feature ont beaucoup de petits commits
- Vous voulez simplifier l'historique de `main`

**Recommandé pour** : GitHub Flow avec commits fréquents

### 3. Rebase and Merge

```
               A'--B'--C' (rebasés)
              /
---D---E---F main
```

**Commande** : `git rebase` puis `git merge --ff-only`

**Caractéristiques** :
- **Rejoue** les commits de la feature sur la branche cible
- Historique complètement **linéaire**
- Pas de commit de merge

**Avantages** :
- ✅ Historique parfaitement linéaire
- ✅ Garde tous les commits individuels
- ✅ Facile à lire avec `git log`

**Inconvénients** :
- ❌ Réécrit l'historique (change les hash des commits)
- ❌ Peut créer des conflits à résoudre

**Quand l'utiliser** :
- Trunk-Based Development
- Vous voulez un historique linéaire mais détaillé
- Votre équipe est à l'aise avec rebase

---

## Comparaison des stratégies de merge

| Critère | Merge Commit | Squash | Rebase |
|---------|--------------|--------|--------|
| **Historique** | Complet | Simplifié | Linéaire |
| **Commits de feature** | Préservés | Écrasés | Préservés |
| **Commit de merge** | Oui | Oui | Non |
| **Complexité** | Moyenne | Simple | Élevée |
| **Réversibilité** | Facile | Facile | Moyenne |
| **Lisibilité** | Moyenne | Excellente | Excellente |

**Recommandation générale** :
- **Squash and Merge** pour la plupart des équipes
- **Merge Commit** si vous voulez garder l'historique complet
- **Rebase and Merge** pour les équipes avancées

---

## Configuration des Pull Requests

### Protection de branche sur GitHub

**Settings** → **Branches** → **Branch protection rules**

#### Règles recommandées

**Pour `main`** :

```yaml
✅ Require a pull request before merging  
   ✅ Require approvals: 2  
   ✅ Dismiss stale reviews when new commits are pushed  
   ✅ Require review from Code Owners

✅ Require status checks to pass before merging  
   ✅ Require branches to be up to date before merging  
   ☑️ Status checks: tests, lint, build

✅ Require conversation resolution before merging

✅ Require signed commits (optionnel mais recommandé)

✅ Require linear history (si vous utilisez rebase/squash)

✅ Include administrators
```

**Pour `develop`** :

```yaml
✅ Require a pull request before merging  
   ✅ Require approvals: 1

✅ Require status checks to pass before merging
```

### Fichier CODEOWNERS

Définissez qui doit reviewer quels fichiers :

**`.github/CODEOWNERS`** :

```
# Global owners
*       @team-leads

# Frontend
/frontend/          @frontend-team
*.css               @frontend-team
*.jsx               @frontend-team

# Backend
/backend/           @backend-team
*.py                @backend-team

# Infrastructure
/docker/            @devops-team
/.github/           @devops-team
/k8s/               @devops-team

# Documentation
*.md                @tech-writers
/docs/              @tech-writers

# Security-sensitive files
/auth/              @security-team
/payment/           @security-team

# Database
/migrations/        @database-team @backend-team
```

**Effet** : Quand un fichier est modifié, les propriétaires sont automatiquement ajoutés comme reviewers obligatoires.

---

## Automatisation avec les Pull Requests

### Intégration Continue (CI)

**.github/workflows/pr-checks.yml** :

```yaml
name: PR Checks

on:
  pull_request:
    branches: [main, develop]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run linter
        run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: npm install
      - name: Run tests
        run: npm test
      - name: Upload coverage
        uses: codecov/codecov-action@v3

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build
        run: npm run build

  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run security audit
        run: npm audit
```

### Bots utiles

#### 1. Dependabot

Crée automatiquement des PR pour mettre à jour les dépendances.

#### 2. CodeCov

Commente sur les PR avec la couverture de code.

#### 3. SonarCloud

Analyse la qualité du code et commente sur les PR.

#### 4. Prettier/ESLint bots

Formatent automatiquement le code ou créent des commentaires.

### Labels automatiques

**.github/workflows/pr-labeler.yml** :

```yaml
name: PR Labeler

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  label:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/labeler@v4
        with:
          repo-token: "${{ secrets.GITHUB_TOKEN }}"
```

**.github/labeler.yml** :

```yaml
'frontend':
  - 'frontend/**/*'
  - '**/*.jsx'
  - '**/*.css'

'backend':
  - 'backend/**/*'
  - '**/*.py'

'documentation':
  - '**/*.md'
  - 'docs/**/*'

'tests':
  - '**/*.test.*'
  - '**/*.spec.*'
```

---

## Bonnes pratiques

### Pour l'auteur de la PR

#### 1. Gardez les PR petites

✅ **Idéal** : 200-400 lignes de code modifiées
⚠️ **Acceptable** : 400-800 lignes
❌ **Trop grand** : > 800 lignes

**Pourquoi** :
- Plus facile à reviewer
- Moins de bugs passent inaperçus
- Plus rapide à merger
- Moins de conflits

**Comment** :
- Découpez les grosses features en plusieurs PR
- Une PR = une fonctionnalité logique
- Utilisez des PR empilées si nécessaire

#### 2. Faites votre propre revue d'abord

Avant de demander une revue :

```bash
# Relisez vos changements
git diff develop

# Vérifiez ce qui sera dans la PR
git log develop..feature/ma-feature --oneline
```

**Checklist personnelle** :
- [ ] Ai-je supprimé les `console.log` de debug ?
- [ ] Ai-je commenté les parties complexes ?
- [ ] Les noms de variables sont-ils clairs ?
- [ ] Ai-je ajouté des tests ?
- [ ] La documentation est-elle à jour ?
- [ ] Ai-je vérifié qu'il n'y a pas de code mort ?

#### 3. Écrivez une description détaillée

Une bonne description fait gagner du temps à tout le monde.

#### 4. Ajoutez des captures d'écran

Pour les changements UI/UX, toujours ajouter des screenshots ou GIFs.

**Outils** :
- **macOS** : Cmd + Shift + 5
- **Windows** : Win + Shift + S
- **GIF** : [Kap](https://getkap.co/), [LICEcap](https://www.cockos.com/licecap/)

#### 5. Liez les issues

```markdown
Closes #123  
Fixes #456  
Resolves #789  
Related to #101
```

#### 6. Demandez des reviewers spécifiques

- Quelqu'un qui connaît bien cette partie du code
- Quelqu'un qui a travaillé sur des features similaires
- Un senior pour du mentoring

#### 7. Répondez rapidement aux commentaires

Ne laissez pas une PR stagner. Répondez dans les 24h.

### Pour le reviewer

#### 1. Reviewez rapidement

**Objectif** : Répondre dans les 24h maximum

Les PR qui traînent :
- Bloquent l'auteur
- Créent des conflits
- Démotivent l'équipe

#### 2. Commencez par le positif

```markdown
✨ J'aime beaucoup comment tu as structuré ces tests !

Quelques suggestions mineures ci-dessous...
```

#### 3. Soyez pédagogique

```markdown
❓ Cette approche fonctionne, mais as-tu considéré utiliser
un WeakMap ici ? Cela éviterait les fuites mémoire.

📚 Référence : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap
```

#### 4. Distinguez les nits des blockers

```markdown
🔍 Nit : Typo dans le commentaire (non bloquant)

🚫 Blocker : Cette fonction n'a pas de gestion d'erreur,
elle peut crasher l'app
```

#### 5. Suggérez du code

Utilisez les suggestions GitHub :

```markdown
```suggestion
const result = items.filter(item => item.isActive);
\```
```

L'auteur peut accepter la suggestion en un clic.

#### 6. Ne bloquez pas sur des détails

Si c'est juste une préférence personnelle, laissez passer ou marquez comme "nit".

#### 7. Approuvez avec des conditions si nécessaire

```markdown
✅ LGTM (Looks Good To Me) avec des nits mineurs.

Tu peux merger après avoir corrigé :
- La typo ligne 45
- Le nom de variable ligne 89

Pas besoin d'une nouvelle revue pour ça.
```

### Pour l'équipe

#### 1. Établissez un SLA (Service Level Agreement)

Exemples de SLA :
- Première revue : 4h ouvrées
- Revue complète : 24h
- Réponse aux commentaires : 24h

#### 2. Rotation des reviewers

Ne surchargez pas toujours les mêmes personnes.

#### 3. Revue en pair programming

Pour les PR complexes, faites une session de revue en direct.

#### 4. Rétrospectives sur les PR

Régulièrement, discutez :
- Qu'est-ce qui fonctionne bien ?
- Qu'est-ce qui pourrait être amélioré ?
- Les PR sont-elles trop grandes ?
- Les revues sont-elles trop lentes ?

---

## Cas pratiques

### Scénario 1 : PR simple (nouvelle feature)

```markdown
# Title
feat: ajoute la fonctionnalité de favoris

# Description
## Description
Permet aux utilisateurs de marquer des articles en favoris.

## Changements
- Nouveau bouton "Ajouter aux favoris"
- API endpoint POST /favorites
- Page "Mes favoris"
- Tests unitaires et d'intégration

## Comment tester
1. Lancer l'app : `npm start`
2. Se connecter
3. Cliquer sur ⭐ sur un article
4. Aller sur "Mes favoris"
5. Vérifier que l'article apparaît

## Checklist
- [x] Code suit les conventions
- [x] Tests ajoutés
- [x] Documentation mise à jour

Closes #234
```

### Scénario 2 : PR de bug fix

```markdown
# Title
fix: corrige le crash lors de l'upload de fichiers > 10MB

# Description
## Problème
L'application crashe quand on upload des fichiers de plus de 10MB.

## Cause
Le buffer Node.js était limité par défaut à 10MB.

## Solution
- Augmentation de la limite à 50MB
- Ajout d'une validation côté client
- Message d'erreur clair pour l'utilisateur

## Tests
- [x] Upload fichier 5MB : ✓
- [x] Upload fichier 15MB : ✓
- [x] Upload fichier 60MB : ✗ (erreur claire)

Fixes #567
```

### Scénario 3 : PR de refactoring

```markdown
# Title
refactor: extrait la logique de validation dans un module séparé

# Description
## Pourquoi
La logique de validation était dupliquée dans 5 fichiers différents,  
rendant la maintenance difficile.

## Changements
- Nouveau module `validation.js`
- Extraction de toutes les fonctions de validation
- Remplacement des duplications par des imports
- Ajout de tests pour le module de validation

## Impact
- ✅ Réduction de 200 lignes de code dupliqué
- ✅ Maintenabilité améliorée
- ✅ Un seul endroit pour corriger les bugs
- ⚠️ Aucun changement de comportement

## Tests
Tous les tests existants passent, confirmant que le comportement  
est identique.
```

---

## Gérer les situations difficiles

### 1. PR qui reste en attente trop longtemps

**Actions** :
- Pinger les reviewers dans les commentaires
- Demander dans le canal Slack/Teams
- Escalader au lead si nécessaire

### 2. Désaccord sur un commentaire

**Approche** :
1. Discutez calmement dans la PR
2. Si pas de consensus, amenez en réunion d'équipe
3. Documentez la décision finale
4. Suivez la décision de l'équipe

### 3. PR trop grosse

**Solutions** :
- Proposer de la découper en plusieurs PR
- Faire une revue progressive
- Programmer une session de pair review

### 4. Commentaires peu constructifs

**Actions** :
- Demandez des clarifications poliment
- Référencez les guidelines de revue de l'équipe
- Discutez avec la personne en privé si récurrent

---

## Métriques et amélioration continue

### Métriques à suivre

1. **Temps moyen de première revue** : < 4h idéal
2. **Temps moyen pour merger une PR** : < 2 jours
3. **Nombre de commentaires par PR** : 5-10 en moyenne
4. **Taux d'approbation première passe** : 70-80%
5. **Taille moyenne des PR** : < 400 lignes

### Outils de suivi

- **GitHub Insights** : Statistiques intégrées
- **LinearB** : Analytics pour les équipes
- **Code Climate Velocity** : Métriques d'engineering

---

## Conclusion

Les Pull Requests ne sont pas qu'un simple merge. C'est un **outil de collaboration** qui :
- Améliore la qualité du code
- Partage les connaissances
- Documente les décisions
- Protège le projet

**Les clés du succès** :

1. **Pour les auteurs** :
   - PR petites et focalisées
   - Descriptions détaillées
   - Réactivité aux commentaires

2. **Pour les reviewers** :
   - Revues rapides et constructives
   - Pédagogie et bienveillance
   - Équilibre entre qualité et pragmatisme

3. **Pour l'équipe** :
   - Guidelines claires
   - SLA définis
   - Amélioration continue

**Rappelez-vous** : Une bonne revue de code est un investissement, pas une perte de temps. Chaque minute passée en revue évite des heures de debugging plus tard !

**Prochaine étape** : Maintenant que vous maîtrisez les Pull Requests, découvrons comment versionner correctement votre projet avec le versionnement sémantique !

⏭️ [Versionnement sémantique (SemVer)](/module-07-bonnes-pratiques-et-workflows/07-versionnement-semantique.md)
