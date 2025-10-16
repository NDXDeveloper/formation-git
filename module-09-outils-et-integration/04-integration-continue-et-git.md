🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 9 : Outils et intégration

## 4. Intégration continue (CI/CD) et Git

### Introduction

Imaginez que chaque fois que vous poussez du code sur Git, une série de vérifications automatiques se déclenchent : vos tests s'exécutent, votre code est analysé, et si tout va bien, votre application est même déployée automatiquement ! C'est exactement ce que permet l'**intégration continue** (CI - Continuous Integration) et le **déploiement continu** (CD - Continuous Deployment).

Dans cette section, nous allons découvrir :
- Ce qu'est vraiment le CI/CD (sans le jargon technique)
- Pourquoi c'est devenu indispensable en développement moderne
- Comment Git est au cœur de ces processus
- Les principales plateformes CI/CD
- Comment créer vos premiers pipelines automatisés

**Note importante :** Le CI/CD peut sembler complexe au premier abord, mais ne vous inquiétez pas ! Nous allons démystifier tout cela étape par étape. À la fin de cette section, vous comprendrez les concepts fondamentaux et saurez créer des automatisations simples.

---

### Qu'est-ce que l'intégration continue (CI) ?

#### Le problème

Imaginons que vous travaillez sur un projet avec 5 autres développeurs. Chacun code de son côté pendant une semaine, puis tout le monde essaie de fusionner son code en même temps. Que se passe-t-il ?

- Des conflits de merge partout
- Du code qui ne compile plus
- Des fonctionnalités qui se cassent mutuellement
- Des tests qui échouent
- Une journée (ou plus !) perdue à tout réparer

C'est ce qu'on appelle "l'enfer de l'intégration" (integration hell).

#### La solution : l'intégration continue

L'**intégration continue** est une pratique qui consiste à :
1. Intégrer son code fréquemment (plusieurs fois par jour)
2. Vérifier automatiquement que tout fonctionne après chaque intégration
3. Détecter les problèmes immédiatement (pas une semaine plus tard !)

**Concrètement, comment ça marche ?**

Quand vous faites un `git push` :
1. Un serveur détecte votre push
2. Il récupère votre code
3. Il exécute automatiquement :
   - La compilation/construction du projet
   - Les tests unitaires
   - Les tests d'intégration
   - L'analyse de code (qualité, sécurité)
4. Il vous informe du résultat (✅ succès ou ❌ échec)

Si quelque chose ne va pas, vous le savez immédiatement et vous pouvez corriger avant que d'autres développeurs ne soient impactés.

---

### Qu'est-ce que le déploiement continu (CD) ?

Le **déploiement continu** va plus loin : si tous les tests passent, le code est automatiquement déployé en production !

**Les deux significations de CD :**

**Continuous Delivery (Livraison Continue)**
Le code est automatiquement préparé pour être déployé, mais un humain appuie sur le bouton final pour déployer en production.

**Continuous Deployment (Déploiement Continu)**
Le code est automatiquement déployé en production sans intervention humaine, dès que tous les tests passent.

**Exemple concret :**

Sans CD :
1. Vous développez une fonctionnalité
2. Vous la testez localement
3. Vous faites un commit et push
4. Vous prévenez l'équipe ops
5. L'équipe ops prépare le déploiement
6. Déploiement manuel (souvent le week-end)
7. Croisez les doigts pour que ça marche !

Avec CD :
1. Vous développez une fonctionnalité
2. Vous la testez localement
3. Vous faites un commit et push
4. Les tests automatiques s'exécutent
5. Si tout est vert, déploiement automatique
6. Vos utilisateurs ont la nouvelle fonctionnalité en quelques minutes !

---

### Pourquoi le CI/CD est devenu indispensable

#### Pour les développeurs

**Détection rapide des bugs**
Un bug détecté 5 minutes après avoir écrit le code est facile à corriger. Un bug découvert 2 semaines plus tard est un cauchemar.

**Moins de stress**
Plus besoin de "journées de déploiement" stressantes où tout peut mal tourner. Les déploiements deviennent routiniers et sûrs.

**Confiance dans le code**
Si les tests automatiques passent, vous savez que votre code fonctionne. Vous pouvez refactorer sans peur.

**Feedback immédiat**
Vous savez en quelques minutes si votre code casse quelque chose, pas quelques jours plus tard.

#### Pour les équipes

**Intégration fluide**
Tout le monde intègre son code plusieurs fois par jour. Plus d'enfer de l'intégration.

**Meilleure collaboration**
Les problèmes d'intégration sont détectés et résolus rapidement, avant qu'ils n'affectent toute l'équipe.

**Documentation vivante**
Les pipelines CI/CD documentent comment construire, tester et déployer le projet. Un nouveau développeur peut comprendre le processus facilement.

#### Pour le produit

**Livraisons plus fréquentes**
Au lieu de livrer tous les 3 mois, vous pouvez livrer plusieurs fois par jour.

**Meilleure qualité**
Les tests automatiques garantissent une qualité constante.

**Réactivité**
Un bug en production ? Corrigez-le et déployez en quelques minutes au lieu d'attendre la prochaine release.

---

### Le rôle central de Git dans CI/CD

Git est le **déclencheur** de tous les processus CI/CD. Voici comment :

#### Déclencheurs basés sur Git (triggers)

**Push sur une branche**
```
git push origin feature/nouvelle-fonctionnalite
```
→ Déclenche : tests automatiques, build, analyse de code

**Création d'une Pull Request**
```
Vous créez une PR sur GitHub/GitLab
```
→ Déclenche : tests, vérifications de qualité, prévisualisation de l'application

**Merge dans main**
```
git merge feature/nouvelle-fonctionnalite
git push origin main
```
→ Déclenche : tests complets, build de production, déploiement automatique

**Tags**
```
git tag v1.2.0
git push origin v1.2.0
```
→ Déclenche : création d'une release, déploiement en production

**Actions planifiées**
```
Tous les jours à minuit
```
→ Déclenche : tests de régression, sauvegardes, rapports

#### Branches et environnements

En CI/CD, différentes branches correspondent souvent à différents environnements :

- `main` / `master` → Production (ce que vos utilisateurs voient)
- `develop` → Staging/Pré-production (environnement de test avant la prod)
- `feature/*` → Environnements de développement temporaires
- Tags (`v1.0.0`) → Versions spécifiques déployées

**Exemple de workflow :**

1. Vous travaillez sur la branche `feature/login`
2. À chaque push, le CI :
   - Exécute les tests
   - Construit une prévisualisation de l'application
3. Vous créez une Pull Request vers `develop`
4. Le CI exécute des tests supplémentaires
5. Après review, merge dans `develop`
6. Le CD déploie automatiquement sur l'environnement de staging
7. Après validation, merge de `develop` dans `main`
8. Le CD déploie automatiquement en production
9. Un tag `v1.2.0` est créé pour marquer cette version

---

### Les principales plateformes CI/CD

Il existe de nombreuses plateformes CI/CD. Voici les plus populaires et leurs particularités.

#### GitHub Actions ⭐

**C'est quoi ?**
Le système CI/CD intégré directement dans GitHub. Si votre code est sur GitHub, vous avez déjà accès à GitHub Actions !

**Avantages :**
- Complètement intégré à GitHub (pas de configuration externe)
- Gratuit pour les dépôts publics (et généreux pour les privés)
- Marketplace avec des milliers d'actions réutilisables
- Facile à configurer avec des fichiers YAML
- Excellente documentation

**Inconvénients :**
- Uniquement pour GitHub (pas GitLab, Bitbucket, etc.)
- Peut devenir coûteux pour les gros projets privés

**Utilisation typique :**
- Projets hébergés sur GitHub
- Projets open-source
- Startups et petites équipes

**Configuration :**
Fichier `.github/workflows/ci.yml` dans votre dépôt

**Niveau de difficulté :** Débutant - très accessible

---

#### GitLab CI/CD ⭐

**C'est quoi ?**
Le système CI/CD intégré à GitLab. Comme GitHub Actions mais pour GitLab.

**Avantages :**
- Intégré nativement dans GitLab
- Très puissant et mature
- Gratuit pour tous les dépôts (publics et privés)
- Auto DevOps : configuration automatique pour les cas courants
- Peut être auto-hébergé (sur vos propres serveurs)

**Inconvénients :**
- Uniquement pour GitLab
- Courbe d'apprentissage un peu plus raide que GitHub Actions

**Utilisation typique :**
- Projets hébergés sur GitLab
- Entreprises qui auto-hébergent leur infrastructure
- Projets nécessitant un contrôle total

**Configuration :**
Fichier `.gitlab-ci.yml` à la racine de votre dépôt

**Niveau de difficulté :** Intermédiaire

---

#### Jenkins

**C'est quoi ?**
Un serveur d'automatisation open-source très ancien et très populaire. Le "grand-père" du CI/CD !

**Avantages :**
- Très flexible et personnalisable
- Énorme écosystème de plugins (des milliers !)
- Fonctionne avec n'importe quelle plateforme Git
- Gratuit et open-source
- Très utilisé en entreprise

**Inconvénients :**
- Configuration complexe (interface vieillissante)
- Nécessite un serveur dédié (maintenance)
- Courbe d'apprentissage importante
- Configuration souvent par interface graphique (moins reproductible)

**Utilisation typique :**
- Grandes entreprises avec infrastructure existante
- Projets avec des besoins très spécifiques
- Migrations d'anciens systèmes

**Niveau de difficulté :** Avancé

---

#### CircleCI

**C'est quoi ?**
Une plateforme CI/CD cloud, indépendante de la plateforme Git que vous utilisez.

**Avantages :**
- Fonctionne avec GitHub, GitLab, Bitbucket
- Interface moderne et agréable
- Très rapide (bonne optimisation)
- Gratuit pour les petits projets

**Inconvénients :**
- Configuration externe à votre plateforme Git
- Peut devenir coûteux

**Configuration :**
Fichier `.circleci/config.yml`

**Niveau de difficulté :** Intermédiaire

---

#### Travis CI

**C'est quoi ?**
Une des premières plateformes CI/CD cloud, très populaire dans la communauté open-source.

**Avantages :**
- Simple à configurer
- Gratuit pour l'open-source
- Bonne intégration avec GitHub

**Inconvénients :**
- Moins populaire qu'avant (beaucoup migrent vers GitHub Actions)
- L'offre gratuite a été réduite

**Configuration :**
Fichier `.travis.yml`

**Niveau de difficulté :** Débutant

---

#### Comparaison rapide

| Plateforme | Gratuit ? | Hébergement | Difficulté | Meilleur pour |
|------------|-----------|-------------|------------|---------------|
| **GitHub Actions** | Oui (limité) | Cloud | Débutant | Projets sur GitHub |
| **GitLab CI** | Oui | Cloud ou self-hosted | Intermédiaire | Projets sur GitLab |
| **Jenkins** | Oui | Self-hosted | Avancé | Entreprises, besoins spécifiques |
| **CircleCI** | Oui (limité) | Cloud | Intermédiaire | Multi-plateformes |
| **Travis CI** | Oui (open-source) | Cloud | Débutant | Open-source sur GitHub |

---

### Concepts fondamentaux du CI/CD

Avant de créer notre premier pipeline, comprenons le vocabulaire.

#### Pipeline

Un **pipeline** est l'ensemble du processus automatisé, de la récupération du code jusqu'au déploiement.

Imaginez une chaîne de montage :
```
Code → Build → Test → Deploy
```

Chaque étape dépend de la précédente. Si une étape échoue, le pipeline s'arrête.

#### Job

Un **job** est une unité de travail dans le pipeline. C'est une série de commandes à exécuter.

Exemple de jobs :
- Job "Test" : exécuter tous les tests
- Job "Build" : compiler l'application
- Job "Deploy" : déployer en production

#### Stage

Un **stage** (étape) regroupe plusieurs jobs qui peuvent s'exécuter en parallèle.

Exemple :
```
Stage 1: Build
  - Job: Build Frontend
  - Job: Build Backend

Stage 2: Test
  - Job: Tests Frontend
  - Job: Tests Backend
  - Job: Tests E2E

Stage 3: Deploy
  - Job: Deploy to production
```

#### Runner / Agent

Un **runner** (ou agent) est la machine (virtuelle ou physique) qui exécute vos jobs. C'est "l'ouvrier" qui fait le travail.

Les plateformes cloud fournissent des runners automatiquement. Pour Jenkins, vous devez configurer vos propres runners.

#### Artifacts

Les **artifacts** sont les fichiers produits par un job et conservés pour être utilisés plus tard (par d'autres jobs ou pour téléchargement).

Exemples :
- L'application compilée (fichier .jar, .exe, bundle.js)
- Les rapports de tests
- Les logs
- La documentation générée

#### Environment Variables

Les **variables d'environnement** contiennent des informations utilisées par le pipeline :
- Secrets (clés API, mots de passe)
- Configuration (URLs, noms de serveurs)
- Informations Git (nom de la branche, hash du commit)

---

### Créer votre premier pipeline : GitHub Actions

Créons un pipeline simple qui exécute des tests à chaque push. Nous utiliserons GitHub Actions car c'est le plus accessible pour débuter.

#### Étape 1 : Créer le fichier de configuration

Dans votre dépôt Git, créez la structure suivante :
```
.github/
  workflows/
    ci.yml
```

#### Étape 2 : Configuration de base

Dans `.github/workflows/ci.yml` :

```yaml
# Nom du workflow (affiché sur GitHub)
name: CI

# Quand déclencher ce workflow
on:
  push:
    branches: [ main, develop ]  # À chaque push sur main ou develop
  pull_request:
    branches: [ main ]           # À chaque PR vers main

# Les jobs à exécuter
jobs:
  test:
    name: Tests
    runs-on: ubuntu-latest       # Machine virtuelle Ubuntu

    steps:
      # Étape 1: Récupérer le code
      - name: Checkout code
        uses: actions/checkout@v3

      # Étape 2: Installer Node.js (exemple pour un projet JavaScript)
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      # Étape 3: Installer les dépendances
      - name: Install dependencies
        run: npm install

      # Étape 4: Exécuter les tests
      - name: Run tests
        run: npm test
```

#### Comprendre la configuration

**`on:`** - Les déclencheurs
- `push:` : quand vous faites un push
- `pull_request:` : quand quelqu'un crée une PR

**`jobs:`** - Les tâches à effectuer
- `runs-on:` : le type de machine (ubuntu-latest, windows-latest, macos-latest)

**`steps:`** - Les étapes dans l'ordre
- `uses:` : utilise une action prête à l'emploi du marketplace
- `run:` : exécute une commande shell

#### Étape 3 : Commit et push

```bash
git add .github/workflows/ci.yml
git commit -m "feat: add CI pipeline"
git push origin main
```

#### Étape 4 : Voir le résultat

1. Allez sur GitHub, dans votre dépôt
2. Cliquez sur l'onglet "Actions"
3. Vous voyez votre workflow en cours d'exécution !
4. Cliquez dessus pour voir les logs en temps réel

**Si les tests passent :** ✅ Checkmark verte
**Si les tests échouent :** ❌ Croix rouge + détails de l'erreur

---

### Exemple de pipeline complet : Build, Test, Deploy

Voici un exemple plus complet qui construit une application, exécute des tests, et déploie si tout va bien.

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  # Job 1: Build
  build:
    name: Build Application
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci  # ci est plus rapide et déterministe que install

      - name: Build
        run: npm run build

      # Sauvegarder le build pour les jobs suivants
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build
          path: dist/

  # Job 2: Tests (s'exécute en parallèle avec build)
  test:
    name: Run Tests
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Run unit tests
        run: npm test

      - name: Run linter
        run: npm run lint

  # Job 3: Deploy (ne s'exécute que si build et test réussissent)
  deploy:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: [build, test]  # Attend que build ET test soient OK
    if: github.ref == 'refs/heads/main'  # Uniquement sur la branche main

    steps:
      - uses: actions/checkout@v3

      # Récupérer le build créé précédemment
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: build
          path: dist/

      # Déployer (exemple avec Netlify)
      - name: Deploy to Netlify
        uses: netlify/actions/cli@master
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
        with:
          args: deploy --prod --dir=dist
```

**Points clés de ce pipeline :**

1. **Trois jobs distincts** : build, test, deploy
2. **Parallélisation** : build et test s'exécutent en même temps (plus rapide)
3. **Dépendances** : deploy attend que les deux autres soient OK
4. **Conditionnelle** : deploy ne se fait que sur `main`
5. **Secrets** : les tokens sensibles sont stockés dans GitHub Secrets, pas dans le code
6. **Artifacts** : le build est partagé entre jobs

---

### Exemple avec GitLab CI

Pour GitLab, créez un fichier `.gitlab-ci.yml` à la racine :

```yaml
# Définir les stages (étapes)
stages:
  - build
  - test
  - deploy

# Variables globales
variables:
  NODE_VERSION: "18"

# Job: Build
build-job:
  stage: build
  image: node:${NODE_VERSION}  # Container Docker avec Node.js
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 hour

# Job: Tests unitaires
test-unit:
  stage: test
  image: node:${NODE_VERSION}
  script:
    - npm ci
    - npm test

# Job: Linter
test-lint:
  stage: test
  image: node:${NODE_VERSION}
  script:
    - npm ci
    - npm run lint

# Job: Déploiement
deploy-production:
  stage: deploy
  image: node:${NODE_VERSION}
  script:
    - npm install -g netlify-cli
    - netlify deploy --prod --dir=dist
  only:
    - main  # Uniquement sur la branche main
  when: manual  # Déploiement manuel (nécessite clic sur un bouton)
```

**Similitudes avec GitHub Actions :**
- Structure en stages et jobs
- Utilisation d'artifacts
- Conditions d'exécution

**Différences :**
- Syntaxe légèrement différente
- GitLab utilise des images Docker par défaut
- `only:` pour définir les branches (au lieu de `if:`)

---

### Bonnes pratiques CI/CD avec Git

#### 1. Tester tôt, tester souvent

Exécutez vos tests à chaque push, pas seulement sur `main`. Cela détecte les problèmes immédiatement.

```yaml
on:
  push:
    branches: [ '*' ]  # Toutes les branches
```

#### 2. Garder les pipelines rapides

Un pipeline lent = développeurs frustrés qui contournent les tests.

**Comment accélérer :**
- Exécuter les jobs en parallèle
- Utiliser le cache pour les dépendances
- Ne pas exécuter tous les tests à chaque fois (tests ciblés sur les PRs, tests complets sur main)

```yaml
# Exemple de cache
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

#### 3. Protéger les branches importantes

Empêchez les pushs directs sur `main` et obligez à passer par des PRs avec CI/CD validé.

**Sur GitHub :**
Settings → Branches → Add rule
- Require pull request before merging
- Require status checks to pass before merging

#### 4. Utiliser des secrets pour les données sensibles

**Jamais de mots de passe en dur dans le code !**

```yaml
# ❌ MAL
env:
  API_KEY: "abc123secret"

# ✅ BIEN
env:
  API_KEY: ${{ secrets.API_KEY }}
```

Configurez les secrets dans les paramètres de votre dépôt GitHub/GitLab.

#### 5. Versionner vos actions/images

```yaml
# ❌ MAL (peut casser si l'action change)
uses: actions/checkout@latest

# ✅ BIEN (version spécifique)
uses: actions/checkout@v3
```

#### 6. Avoir des messages de commit clairs

Les pipelines peuvent utiliser les messages de commit pour prendre des décisions.

```yaml
# Exemple: skip CI avec un message spécial
if: "!contains(github.event.head_commit.message, '[skip ci]')"
```

#### 7. Monitorer et notifier

Configurez des notifications pour être alerté en cas d'échec :
- Emails
- Slack/Discord
- SMS pour les déploiements production

#### 8. Déploiements progressifs

Pour la production, déployez progressivement :
1. Déploiement sur 5% du trafic
2. Surveillance
3. Déploiement sur 50%
4. Surveillance
5. Déploiement sur 100%

Cela limite l'impact en cas de problème.

---

### Patterns de workflow Git avec CI/CD

#### Feature Branch Workflow avec CI

```
1. Créer une branche feature
   git checkout -b feature/nouvelle-fonctionnalite

2. Développer et committer
   git commit -am "feat: add new feature"

3. Pousser (déclenche CI sur la branche feature)
   git push origin feature/nouvelle-fonctionnalite
   → CI exécute : tests, linter

4. Créer une Pull Request
   → CI exécute : tests complets, build, analyse de sécurité

5. Après review et CI ✅, merge dans main
   → CD exécute : déploiement en staging

6. Tag pour production
   git tag v1.2.0
   git push origin v1.2.0
   → CD exécute : déploiement en production
```

#### Trunk-Based Development avec CI/CD

```
1. Travailler directement sur main (ou branches très courtes)

2. Chaque commit déclenche CI/CD complet

3. Si tests ✅ → déploiement automatique en production

4. Feature flags pour les fonctionnalités incomplètes
```

Ce workflow nécessite :
- Excellente couverture de tests
- Équipe disciplinée
- Bonne communication

---

### Déboguer un pipeline qui échoue

Votre pipeline est rouge ? Pas de panique ! Voici comment investiguer.

#### 1. Lire les logs

Cliquez sur le job en échec et lisez attentivement les logs. L'erreur est généralement claire :
```
npm ERR! Test failed
Error: Expected 5 to equal 4
```

#### 2. Vérifier localement

Exécutez les mêmes commandes localement :
```bash
npm install
npm test
```

Si ça fonctionne localement mais pas en CI, c'est probablement :
- Une différence d'environnement (version de Node.js, OS)
- Un fichier manquant (oublié dans .gitignore)
- Une dépendance non fixée (version aléatoire installée)

#### 3. Utiliser le mode debug

GitHub Actions :
```yaml
- name: Debug information
  run: |
    echo "Node version: $(node --version)"
    echo "NPM version: $(npm --version)"
    ls -la
    cat package.json
```

#### 4. Accéder au runner (fonctionnalité avancée)

Certaines plateformes permettent de se connecter au runner en SSH pour investiguer :
```yaml
# GitHub Actions avec tmate (SSH access)
- name: Setup tmate session
  uses: mxschmitt/action-tmate@v3
  if: failure()
```

---

### Cas d'usage concrets

#### Projet Python avec tests

```yaml
name: Python CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest pytest-cov

      - name: Run tests with coverage
        run: |
          pytest --cov=. --cov-report=xml

      - name: Upload coverage
        uses: codecov/codecov-action@v3
```

#### Projet Java avec Maven

```yaml
name: Java CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn clean package

      - name: Run tests
        run: mvn test
```

#### Projet Docker

```yaml
name: Docker Build

on:
  push:
    branches: [ main ]

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Build Docker image
        run: docker build -t myapp:latest .

      - name: Run tests in container
        run: docker run myapp:latest npm test

      - name: Push to Docker Hub
        if: github.ref == 'refs/heads/main'
        run: |
          echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
          docker push myapp:latest
```

---

### Ressources pour aller plus loin

#### Documentation officielle

- **GitHub Actions** : docs.github.com/en/actions
- **GitLab CI/CD** : docs.gitlab.com/ee/ci/
- **CircleCI** : circleci.com/docs/

#### Marketplace et exemples

- **GitHub Actions Marketplace** : github.com/marketplace
- **Awesome CI/CD** : github.com/cicdops/awesome-ciandcd
- **Starter Workflows** : github.com/actions/starter-workflows

#### Outils complémentaires

- **Codecov** : couverture de tests (codecov.io)
- **SonarCloud** : qualité de code (sonarcloud.io)
- **Snyk** : analyse de sécurité (snyk.io)
- **Dependabot** : mises à jour automatiques des dépendances (intégré GitHub)

---

### Conclusion

L'intégration continue (CI) et le déploiement continu (CD) ont révolutionné le développement logiciel. En combinant Git avec des plateformes CI/CD, vous pouvez :

- **Détecter les problèmes immédiatement**, pas des jours plus tard
- **Automatiser les tâches répétitives** (tests, builds, déploiements)
- **Augmenter la confiance** dans votre code
- **Livrer plus rapidement** et plus souvent
- **Réduire le stress** des déploiements

**Pour débuter :**
1. Commencez simple : un workflow qui exécute vos tests
2. Ajoutez progressivement : linter, analyse de code, build
3. Enfin, automatisez le déploiement

**Points clés à retenir :**
- Git est le déclencheur de tous les processus CI/CD
- Chaque push peut déclencher des vérifications automatiques
- Les pipelines s'écrivent sous forme de fichiers YAML versionnés avec votre code
- GitHub Actions et GitLab CI sont les solutions les plus accessibles
- Commencez petit et itérez

Le CI/CD peut sembler complexe, mais une fois en place, vous ne pourrez plus vous en passer. C'est un investissement qui se rentabilise rapidement en temps gagné et en bugs évités.

Dans la section suivante, nous verrons comment automatiser le déploiement de votre application avec Git, en approfondissant les stratégies de déploiement.

⏭️ [Déploiement automatisé avec Git](/module-09-outils-et-integration/05-deploiement-automatise.md)
