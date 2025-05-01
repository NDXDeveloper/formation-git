# 7.4. Déploiement automatisé avec Git

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

Le déploiement automatisé (ou déploiement continu) est une extension naturelle de l'intégration continue. Il permet de publier automatiquement votre application sur un serveur ou un environnement de production après validation des tests. Ce processus, souvent appelé CD (Continuous Deployment/Delivery), simplifie grandement la mise en ligne de vos projets.

## Qu'est-ce que le déploiement automatisé ?

Le déploiement automatisé consiste à :

- **Automatiser** la publication de votre application
- **Éliminer** les erreurs humaines lors du déploiement
- **Accélérer** la mise en production des nouvelles fonctionnalités
- **Standardiser** le processus de déploiement

![Workflow CD](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuU8goIp9ILLmJyrBBKfAJ5L8paaiBbR8ICt9oUToICrB0Ke10000)

## Comment fonctionne le déploiement automatisé avec Git ?

1. Vous **poussez** du code vers une branche spécifique (ex: `main` ou `production`)
2. Le système CI exécute les tests et vérifications
3. Si tous les tests **réussissent**, le système de déploiement prend le relais
4. Votre code est automatiquement **déployé** sur l'environnement cible
5. Vous recevez une **notification** de succès ou d'échec

## Les principales solutions de déploiement avec Git

### GitHub Pages

GitHub Pages est une solution simple pour héberger des sites statiques directement depuis votre dépôt GitHub.

![GitHub Pages](https://pages.github.com/images/slideshow/css.png)

**Avantages pour les débutants :**
- Gratuit et intégré à GitHub
- Configuration minimale requise
- Parfait pour sites statiques (HTML, CSS, JavaScript)
- Custom domain possible

**Configuration de base :**

1. Créez un dépôt nommé `username.github.io` (où `username` est votre nom d'utilisateur GitHub)
2. Ajoutez vos fichiers HTML/CSS/JS
3. Poussez vers GitHub, et votre site est automatiquement déployé à `https://username.github.io`

Pour un projet existant :
1. Allez dans `Settings` > `Pages`
2. Sélectionnez la branche source (généralement `main`)
3. Cliquez sur `Save`

### Netlify

Netlify est une plateforme populaire pour le déploiement automatisé de sites web statiques et d'applications.

![Netlify](https://www.netlify.com/img/global/meta-image.jpg)

**Avantages pour les débutants :**
- Interface utilisateur intuitive
- Déploiement par simple glisser-déposer ou lié à Git
- Prévisualisation des déploiements
- Fonctionnalités gratuites généreuses

**Configuration avec Git :**

1. Créez un compte sur [Netlify](https://www.netlify.com/)
2. Cliquez sur "New site from Git"
3. Sélectionnez votre fournisseur Git (GitHub, GitLab, Bitbucket)
4. Choisissez votre dépôt
5. Configurez les paramètres de build si nécessaire
6. Cliquez sur "Deploy site"

### Vercel

Vercel est spécialisé dans le déploiement d'applications front-end et de sites JAMstack.

![Vercel](https://assets.vercel.com/image/upload/front/vercel/home-og-image.png)

**Avantages pour les débutants :**
- Optimisé pour les frameworks modernes (Next.js, React, Vue, etc.)
- Déploiements de prévisualisation pour chaque branche
- Interface simple et intuitive
- Plan gratuit généreux

**Configuration avec Git :**

1. Créez un compte sur [Vercel](https://vercel.com/)
2. Cliquez sur "Import Project"
3. Choisissez "From Git Repository"
4. Connectez votre compte GitHub/GitLab/Bitbucket
5. Sélectionnez votre dépôt
6. Configurez le projet et cliquez sur "Deploy"

### Heroku

Heroku est une plateforme cloud qui permet de déployer des applications complètes (backend et frontend).

![Heroku](https://www.herokucdn.com/images/og.png)

**Avantages pour les débutants :**
- Support pour de nombreux langages (Node.js, Ruby, Python, etc.)
- Interface graphique simple
- Intégration Git native
- Addons pour bases de données et autres services

**Configuration avec Git :**

1. Créez un compte sur [Heroku](https://www.heroku.com/)
2. Installez la CLI Heroku : `npm install -g heroku`
3. Connectez-vous : `heroku login`
4. Dans votre projet local :
   ```bash
   # Créez une application Heroku
   heroku create mon-app

   # Ajoutez Heroku comme remote
   git remote add heroku https://git.heroku.com/mon-app.git

   # Déployez avec Git
   git push heroku main
   ```

Pour un déploiement automatique :
1. Allez dans le dashboard Heroku
2. Sélectionnez votre application
3. Allez dans l'onglet "Deploy"
4. Connectez GitHub et activez "Automatic Deploys"

## Déploiement automatisé par étapes (pour débutants)

Voici un exemple simple de déploiement automatisé avec GitHub Actions et GitHub Pages :

### 1. Préparez votre projet

Assurez-vous que votre projet est prêt à être déployé :
- Pour un site statique, vous avez besoin de fichiers HTML/CSS/JS
- Pour une application React, vous avez besoin d'un script de build (ex: `npm run build`)

### 2. Créez votre workflow de déploiement

1. Créez un dossier `.github/workflows` dans votre dépôt
2. Ajoutez un fichier `deploy.yml` avec ce contenu pour un site statique :

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3

    # Si vous avez besoin de compiler (ex: projet React)
    # - name: Install dependencies
    #   run: npm install
    # - name: Build
    #   run: npm run build

    - name: Deploy to GitHub Pages
      uses: JamesIves/github-pages-deploy-action@v4
      with:
        branch: gh-pages  # La branche où le site sera déployé
        folder: .         # Le dossier à déployer (ou 'build' pour React)
```

### 3. Activez GitHub Pages

1. Dans votre dépôt GitHub, allez dans `Settings` > `Pages`
2. Pour "Source", sélectionnez la branche `gh-pages`
3. Cliquez sur `Save`

### 4. Poussez vos changements

```bash
git add .github/workflows/deploy.yml
git commit -m "Ajouter le workflow de déploiement automatisé"
git push
```

### 5. Vérifiez le déploiement

1. Allez dans l'onglet "Actions" de votre dépôt GitHub
2. Vous devriez voir votre workflow en cours d'exécution
3. Une fois terminé, votre site sera accessible à `https://username.github.io/repository-name`

## Stratégies de déploiement courants

### 1. Déploiement basé sur les branches

- **Branche `main`/`master`** → environnement de production
- **Branche `staging`** → environnement de préproduction
- **Branches `feature/*`** → environnements de test/prévisualisation

### 2. Déploiement basé sur les tags

Création d'un tag Git pour déclencher un déploiement :

```bash
# Création d'un tag de version
git tag v1.0.0

# Pousser le tag
git push origin v1.0.0
```

### 3. Déploiement manuel après validation automatique

Le code passe par les tests automatiques, mais le déploiement nécessite une validation manuelle (utile pour les débutants).

## Bonnes pratiques pour le déploiement automatisé

1. **Commencez petit** : déployez d'abord sur un environnement de test
2. **Ajoutez des vérifications** : tests, linting, audit de sécurité avant déploiement
3. **Utilisez des variables d'environnement** : ne stockez jamais les secrets dans le code
4. **Prévoyez une stratégie de rollback** : comment revenir en arrière en cas de problème
5. **Documentez le processus** : assurez-vous que toute l'équipe comprend le déploiement

## Exercice pratique

1. Créez un site web simple (un fichier HTML suffit)
2. Créez un dépôt GitHub et poussez votre code
3. Configurez GitHub Pages pour déployer automatiquement votre site
4. Modifiez votre site, poussez les changements et observez le déploiement automatique
5. (Bonus) Ajoutez une étape de validation dans le workflow (par exemple, vérifier que le HTML est valide)

---

Le déploiement automatisé avec Git peut sembler complexe au début, mais il simplifie énormément le processus de mise en ligne de vos projets. Commencez avec des outils simples comme GitHub Pages avant de passer à des solutions plus avancées. Avec le temps, vous apprécierez de ne plus avoir à vous soucier du processus de déploiement et de pouvoir vous concentrer sur le développement de votre application.

⏭️ [Module 8 : Cas pratiques et exercices](/module-8-cas-pratiques-et-exercices/README.md)
