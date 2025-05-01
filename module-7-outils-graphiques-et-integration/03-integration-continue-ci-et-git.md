# 7.3. Intégration continue (CI) et Git

L'intégration continue (CI) est une pratique de développement qui consiste à automatiser les tests et les vérifications de code chaque fois que des modifications sont envoyées à votre dépôt Git. Cette section vous introduira aux concepts de base de la CI et à son intégration avec Git.

## Qu'est-ce que l'intégration continue ?

L'intégration continue est une méthode qui permet de :

- **Vérifier automatiquement** la qualité du code à chaque changement
- **Détecter les problèmes tôt** dans le cycle de développement
- **Réduire les risques** lors de l'intégration de nouvelles fonctionnalités
- **Maintenir un code de qualité** grâce à des tests automatisés

![Workflow CI](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuU8goIp9ILLmJyrBBKfAJ5L8paaiBbR8ICt9oUToICrB0Ke10000)

## Comment fonctionne la CI avec Git ?

1. Un développeur **pousse (push)** des modifications vers le dépôt Git
2. Le système de CI **détecte automatiquement** ce changement
3. Le système de CI **récupère le code** et lance des **pipelines**
4. Les pipelines exécutent des tests, analyses et vérifications
5. Le système de CI **rapporte les résultats** (succès ou échec)
6. Les développeurs sont **notifiés** en cas de problème

## Principaux outils de CI qui s'intègrent avec Git

### GitHub Actions

GitHub Actions est l'outil d'intégration continue intégré à GitHub.

![GitHub Actions](https://docs.github.com/assets/cb-25535/mw-1440/images/help/actions/learn-github-actions-workflow.webp)

**Points forts :**
- Intégration native avec GitHub
- Configuration par fichiers YAML dans le dépôt
- Large marketplace d'actions prédéfinies
- Facile à démarrer

**Configuration de base :**

1. Créez un dossier `.github/workflows` dans votre dépôt
2. Ajoutez un fichier YAML (par exemple `ci.yml`)
3. Définissez votre workflow :

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up environment
      uses: actions/setup-node@v3
      with:
        node-version: '16'
    - name: Install dependencies
      run: npm install
    - name: Run tests
      run: npm test
```

### GitLab CI/CD

GitLab propose une solution CI/CD complète intégrée à sa plateforme.

![GitLab CI](https://docs.gitlab.com/ee/ci/img/pipelines_graph_white_28_May_2019.png)

**Points forts :**
- Intégration complète avec GitLab
- Configuration par fichier `.gitlab-ci.yml`
- Runners auto-hébergés ou gérés par GitLab
- Pipeline visuellement riche

**Configuration de base :**

1. Ajoutez un fichier `.gitlab-ci.yml` à la racine de votre dépôt
2. Définissez vos stages et jobs :

```yaml
stages:
  - build
  - test

build-job:
  stage: build
  script:
    - echo "Compilation du projet..."
    - npm install

test-job:
  stage: test
  script:
    - echo "Exécution des tests..."
    - npm test
```

### Jenkins

Jenkins est une solution open-source très flexible pour l'intégration continue.

![Jenkins](https://www.jenkins.io/images/jenkins-ci-web-logo.png)

**Points forts :**
- Hautement personnalisable
- Grande communauté et nombreux plugins
- Peut être auto-hébergé
- Supporte de nombreux types de projets

**Configuration de base :**

1. Installez Jenkins sur un serveur
2. Créez un "Jenkinsfile" à la racine de votre dépôt :

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
    }
}
```

## Avantages de la CI pour les débutants

Même si la CI peut sembler complexe, elle offre plusieurs avantages pour les débutants :

1. **Retour rapide** : soyez informé immédiatement si votre code casse quelque chose
2. **Filet de sécurité** : les tests automatisés vous rassurent sur vos modifications
3. **Bonnes pratiques** : encourage à écrire des tests et du code de qualité
4. **Apprentissage** : comprenez comment fonctionne un workflow professionnel

## Mise en place d'une CI simple pour débutants

Voici comment configurer une CI basique avec GitHub Actions :

### 1. Préparez votre projet

Assurez-vous que votre projet contient :
- Un système de test (par exemple, Jest pour JavaScript)
- Des tests simples qui peuvent être lancés via une commande (ex: `npm test`)

### 2. Créez votre workflow CI

1. Dans votre dépôt GitHub, créez un dossier `.github/workflows`
2. Créez un fichier `simple-ci.yml` avec ce contenu :

```yaml
name: Simple CI

on: [push]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Installer les dépendances
      run: npm install
    - name: Vérifier le code
      run: npm run lint
    - name: Exécuter les tests
      run: npm test
```

### 3. Poussez vos changements

```bash
git add .github/workflows/simple-ci.yml
git commit -m "Ajouter la configuration CI"
git push
```

### 4. Vérifiez les résultats

1. Allez sur votre dépôt GitHub
2. Cliquez sur l'onglet "Actions"
3. Vous verrez votre workflow en cours d'exécution ou terminé

## Bonnes pratiques CI avec Git

1. **Commit souvent** : privilégiez des commits plus petits et fréquents
2. **Branches courtes** : évitez les longues branches de fonctionnalités
3. **Pull avant Push** : intégrez les changements des autres avant de pousser les vôtres
4. **Vérifiez les échecs** : corrigez immédiatement les problèmes détectés par la CI
5. **Utilisez des badges** : ajoutez des badges de statut CI dans votre README

## Exercice pratique

1. Choisissez un projet simple existant ou créez-en un nouveau
2. Ajoutez au moins un test automatisé
3. Configurez GitHub Actions ou GitLab CI selon la plateforme que vous utilisez
4. Faites une modification qui fait échouer le test
5. Observez comment la CI vous alerte du problème
6. Corrigez le problème et vérifiez que la CI passe au vert

---

L'intégration continue peut sembler intimidante au début, mais elle devient rapidement un allié précieux dans votre processus de développement. Commencez avec une configuration simple et ajoutez progressivement des fonctionnalités à mesure que vous vous familiarisez avec le concept. Avec le temps, vous ne pourrez plus vous passer de ce filet de sécurité automatisé!
