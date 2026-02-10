🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 9 : Outils et intégration

## 5. Déploiement automatisé avec Git

### Introduction

Imaginez : vous faites un `git push`, et quelques minutes plus tard, votre site web est mis à jour et accessible à vos utilisateurs. Pas de commandes compliquées, pas de connexion SSH, pas de manipulation de fichiers sur un serveur. C'est ça, le **déploiement automatisé** !

Le déploiement automatisé transforme le processus fastidieux et risqué de mise en production en quelque chose de simple, répétable et sûr. Dans cette section, nous allons découvrir :

- Comment fonctionne le déploiement automatisé avec Git
- Les différentes stratégies de déploiement
- Les plateformes qui simplifient le déploiement
- Comment configurer vos propres déploiements automatisés
- Les bonnes pratiques pour déployer en toute sécurité

**Note pour les débutants :** Le déploiement peut sembler intimidant, mais les outils modernes le rendent très accessible. Nous allons partir des bases et construire progressivement votre compréhension.

---

### Qu'est-ce que le déploiement ?

#### Définition simple

**Déployer** signifie rendre votre application accessible aux utilisateurs. C'est le processus qui fait passer votre code de votre ordinateur à un serveur accessible sur Internet.

**Exemple concret :**
- Vous développez un site web sur votre ordinateur (localhost)
- Vos amis ne peuvent pas y accéder car c'est sur votre machine
- Vous déployez → Le site est maintenant sur un serveur avec une vraie URL
- Tout le monde peut maintenant visiter votre site !

#### Le déploiement manuel (l'ancienne façon)

Avant l'automatisation :

1. **Vous développez** localement
2. **Vous testez** localement
3. **Vous vous connectez au serveur** (via FTP ou SSH)
4. **Vous transférez les fichiers** un par un
5. **Vous redémarrez** le serveur web
6. **Vous priez** que tout fonctionne
7. **Si ça casse**, panique et corrections en urgence

**Problèmes :**
- Risque d'erreurs humaines (mauvais fichier transféré, fichier oublié)
- Lent et fastidieux
- Pas de traçabilité (quelle version est déployée ?)
- Difficile de revenir en arrière
- Stressant (surtout à 23h un vendredi soir !)

#### Le déploiement automatisé (la façon moderne)

Avec l'automatisation :

1. **Vous développez** localement
2. **Vous testez** localement
3. **Vous faites `git push`**
4. **Le système fait tout** automatiquement :
   - Récupère le code
   - Exécute les tests
   - Construit l'application
   - Déploie sur le serveur
   - Vérifie que tout fonctionne
5. **Vous recevez une notification** : ✅ Déploiement réussi !
6. **Si problème**, retour automatique à la version précédente

**Avantages :**
- Rapide et fiable
- Répétable (même processus à chaque fois)
- Traçable (vous savez exactement quelle version Git est déployée)
- Facile de revenir en arrière (rollback)
- Moins de stress !

---

### Le lien entre Git et le déploiement

Git est **au cœur** du déploiement automatisé moderne. Voici pourquoi :

#### Git comme source de vérité

Votre dépôt Git contient :
- Le code source
- L'historique complet des changements
- Les différentes versions (tags)
- Les branches pour différents environnements

**Principe fondamental :** Ce qui est dans Git, c'est ce qui est déployé. Toujours.

#### Déclencheurs basés sur Git

Les déploiements sont déclenchés par des actions Git :

**Push sur une branche**
```bash
git push origin main
```
→ Déploiement automatique en production

**Création d'un tag**
```bash
git tag v1.5.0  
git push origin v1.5.0
```
→ Création d'une release et déploiement

**Merge d'une Pull Request**
```
PR mergée dans main
```
→ Déploiement automatique

#### Identification de version

Chaque commit Git a un identifiant unique (hash). Cela permet de savoir exactement quelle version du code est déployée :

```bash
# Version déployée en production
Production: commit abc123def456 (v2.1.0)

# Version en staging
Staging: commit def789abc012 (feature/new-dashboard)
```

En cas de problème, vous savez exactement où chercher !

---

### Les environnements de déploiement

La plupart des projets professionnels utilisent plusieurs **environnements** (environments). Chaque environnement correspond généralement à une branche Git.

#### Développement (Development / Dev)

**But :** Où les développeurs testent leurs fonctionnalités individuelles

**Caractéristiques :**
- Change constamment
- Peut être cassé temporairement
- Données de test
- Accès restreint (équipe de développement uniquement)

**Branche Git typique :** `feature/*`, branches personnelles

**Déploiement :** À chaque push sur la branche de feature (optionnel)

#### Staging (Pré-production)

**But :** Environnement qui reproduit la production pour les tests finaux

**Caractéristiques :**
- Copie quasi-identique de la production
- Stable mais pas encore publique
- Données qui ressemblent à la production (mais anonymisées)
- Tests d'acceptance, démonstrations clients

**Branche Git typique :** `develop` ou `staging`

**Déploiement :** À chaque push sur `develop`

#### Production (Prod)

**But :** L'environnement "réel" que vos utilisateurs utilisent

**Caractéristiques :**
- Doit être stable et fiable
- Données réelles
- Accessible au public
- Sauvegardé régulièrement

**Branche Git typique :** `main` / `master`

**Déploiement :** Uniquement après validation sur staging

#### Schéma typique

```
Développeur 1: feature/login → Dev Environment
              ↓
Développeur 2: feature/signup → Dev Environment
              ↓
          [Pull Request]
              ↓
         develop branch → Staging Environment
              ↓
          [Tests OK]
              ↓
          main branch → Production Environment
              ↓
          tag v1.2.0
```

---

### Stratégies de déploiement

Il existe plusieurs façons de déployer une application. Chacune a ses avantages et inconvénients.

#### Déploiement basique (Basic Deployment)

**Comment ça marche :**
L'ancienne version est arrêtée, la nouvelle version est déployée, puis démarrée.

```
[Ancienne version] → [Arrêt] → [Déploiement] → [Nouvelle version]
          ↓
    Downtime (indisponibilité)
```

**Avantages :**
- Simple à mettre en place
- Pas besoin d'infrastructure complexe

**Inconvénients :**
- Site indisponible pendant le déploiement (de quelques secondes à quelques minutes)
- Si la nouvelle version a un bug, tous les utilisateurs sont impactés immédiatement

**Quand l'utiliser :**
- Petits projets personnels
- Sites avec peu de trafic
- Déploiements en dehors des heures de pointe

#### Rolling Deployment (Déploiement progressif)

**Comment ça marche :**
Les serveurs sont mis à jour un par un. Pendant la mise à jour, les autres serveurs continuent de servir l'ancienne version.

```
Serveur 1: V1 → V2  
Serveur 2: V1 → V2  
Serveur 3: V1 → V2  
Serveur 4: V1 → V2
```

**Avantages :**
- Pas de downtime
- Si problème sur un serveur, les autres continuent de fonctionner

**Inconvénients :**
- Deux versions coexistent temporairement (peut causer des bugs si elles ne sont pas compatibles)
- Plus complexe à configurer

**Quand l'utiliser :**
- Applications avec plusieurs serveurs
- Services qui ne peuvent pas se permettre de downtime

#### Blue-Green Deployment

**Comment ça marche :**
Deux environnements identiques (Blue et Green). L'un est actif, l'autre en standby. Vous déployez sur l'environnement inactif, puis basculez le trafic.

```
Blue (V1) ← Trafic utilisateurs  
Green (V2) ← Nouveau déploiement

[Bascule instantanée]

Blue (V1) ← En standby (prêt pour rollback)  
Green (V2) ← Trafic utilisateurs
```

**Avantages :**
- Bascule instantanée
- Rollback immédiat en cas de problème
- Tests complets possibles avant la bascule

**Inconvénients :**
- Nécessite deux fois plus de ressources (deux environnements complets)
- Plus coûteux

**Quand l'utiliser :**
- Applications critiques où le downtime est inacceptable
- Quand vous avez besoin de tester en conditions réelles avant la bascule

#### Canary Deployment (Déploiement canari)

**Comment ça marche :**
La nouvelle version est déployée progressivement : d'abord 5% du trafic, puis 25%, puis 50%, puis 100%.

```
V1: 100% du trafic  
V2: 0%
    ↓
V1: 95% du trafic  
V2: 5% du trafic (surveillance intensive)
    ↓
V1: 50% du trafic  
V2: 50% du trafic
    ↓
V1: 0%  
V2: 100% du trafic
```

**Avantages :**
- Détection rapide de problèmes (seulement 5% des utilisateurs affectés au début)
- Possibilité de rollback à tout moment
- Excellent pour tester des changements majeurs

**Inconvénients :**
- Nécessite une infrastructure qui supporte le routing du trafic
- Plus complexe à configurer

**Quand l'utiliser :**
- Grosses applications avec beaucoup d'utilisateurs
- Changements majeurs ou risqués
- Quand vous voulez minimiser le risque

#### Feature Flags (Drapeaux de fonctionnalités)

**Comment ça marche :**
Le code de la nouvelle fonctionnalité est déployé, mais désactivé par défaut. Vous activez progressivement la fonctionnalité pour certains utilisateurs.

```javascript
if (featureFlags.newDashboard) {
  // Nouvelle version du dashboard
  return <NewDashboard />
} else {
  // Ancienne version
  return <OldDashboard />
}
```

**Avantages :**
- Déploiement découplé de l'activation de fonctionnalités
- Possibilité d'activer pour des utilisateurs beta seulement
- Rollback instantané (désactiver le flag)

**Inconvénients :**
- Le code devient plus complexe (if/else partout)
- Nécessite de nettoyer les anciens flags

**Quand l'utiliser :**
- Développement de grosses fonctionnalités sur plusieurs semaines
- Tests A/B
- Activation progressive de nouvelles fonctionnalités

---

### Plateformes de déploiement automatisé

Il existe de nombreuses plateformes qui simplifient le déploiement. Découvrons les plus populaires.

#### Netlify ⭐ (Sites statiques et JAMstack)

**Ce que c'est :**
Plateforme de déploiement pour sites statiques et applications JAMstack (JavaScript, APIs, Markup).

**Parfait pour :**
- Sites vitrines
- Blogs
- Applications React / Vue / Angular
- Documentation
- Landing pages

**Comment ça marche :**

1. **Connectez votre dépôt Git** (GitHub, GitLab, Bitbucket)
2. **Configurez la commande de build**
   ```
   Build command: npm run build
   Publish directory: dist
   ```
3. **Chaque push déclenche un déploiement automatique**
4. **Votre site est en ligne** avec une URL netlify.app (ou votre domaine custom)

**Avantages :**
- Gratuit pour les projets personnels
- Déploiement ultra-rapide
- CDN global automatique
- HTTPS automatique
- Prévisualisations des Pull Requests
- Rollback en un clic

**Inconvénients :**
- Uniquement pour sites statiques (pas de backend Node.js, Python, etc.)

**Exemple de configuration (netlify.toml) :**
```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "18"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

**Niveau de difficulté :** Débutant - très facile

---

#### Vercel ⭐ (Next.js et frontend)

**Ce que c'est :**
Plateforme de déploiement optimisée pour Next.js (mais supporte aussi React, Vue, etc.).

**Parfait pour :**
- Applications Next.js
- Sites React / Vue / Svelte
- Applications frontend avec API routes
- Projets nécessitant des fonctionnalités serverless

**Comment ça marche :**

1. **Installez Vercel CLI ou connectez via le site web**
2. **Connectez votre dépôt Git**
3. **Vercel détecte automatiquement votre framework**
4. **Déploiement automatique à chaque push**

**Avantages :**
- Gratuit pour projets personnels
- Optimisé pour Next.js (créé par la même équipe)
- Edge Functions (exécution au plus près des utilisateurs)
- Prévisualisations automatiques des PRs
- Performances excellentes

**Inconvénients :**
- Principalement pour frontend (backend limité aux serverless functions)

**Exemple de déploiement rapide :**
```bash
# Installer Vercel CLI
npm install -g vercel

# Dans votre projet
vercel

# Déploiement en production
vercel --prod
```

**Niveau de difficulté :** Débutant

---

#### Heroku (Applications avec backend)

**Ce que c'est :**
Plateforme cloud qui supporte de nombreux langages (Node.js, Python, Ruby, Java, etc.).

**Parfait pour :**
- Applications full-stack
- APIs backend
- Applications avec bases de données
- Prototypes et MVPs

**Comment ça marche :**

1. **Créer une app Heroku**
   ```bash
   heroku create mon-application
   ```

2. **Connecter Git**
   ```bash
   git remote add heroku https://git.heroku.com/mon-application.git
   ```

3. **Déployer avec Git**
   ```bash
   git push heroku main
   ```

4. **Heroku détecte le langage et déploie automatiquement**

**Avantages :**
- Support de nombreux langages
- Add-ons (bases de données, email, monitoring, etc.)
- Facile à commencer
- CLI puissant

**Inconvénients :**
- Gratuit très limité (applications "dorment" après inactivité)
- Plus cher que les alternatives pour le long terme

**Exemple de fichier Procfile (configuration Heroku) :**
```
web: npm start  
worker: npm run queue
```

**Niveau de difficulté :** Débutant à intermédiaire

---

#### GitHub Pages (Sites statiques gratuits)

**Ce que c'est :**
Service de hosting gratuit intégré à GitHub pour sites statiques.

**Parfait pour :**
- Pages de projet
- Documentation
- Portfolio personnel
- Blogs simples

**Comment ça marche :**

**Méthode 1 : Branche gh-pages**
```bash
# Construire votre site
npm run build

# Déployer sur la branche gh-pages
git checkout -b gh-pages  
git add dist -f  
git commit -m "Deploy"  
git push origin gh-pages
```

**Méthode 2 : GitHub Actions (recommandée)**
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

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Build
        run: |
          npm ci
          npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

**Avantages :**
- Totalement gratuit
- Intégré à GitHub
- HTTPS automatique
- Domaine custom possible

**Inconvénients :**
- Uniquement sites statiques
- Uniquement pour dépôts publics (ou compte GitHub Pro)

**URL :** `https://votre-username.github.io/votre-repo`

**Niveau de difficulté :** Débutant

---

#### DigitalOcean App Platform

**Ce que c'est :**
Plateforme de déploiement de DigitalOcean, entre Heroku et un VPS complet.

**Parfait pour :**
- Applications nécessitant plus de contrôle qu'Heroku
- Projets avec bases de données
- Microservices

**Comment ça marche :**
1. Connectez votre dépôt Git
2. Configurez l'environnement
3. Déploiement automatique à chaque push

**Avantages :**
- Prix compétitifs
- Plus de flexibilité qu'Heroku
- Infrastructure DigitalOcean (fiable)

**Inconvénients :**
- Moins simple que Netlify/Vercel
- Pas de tier gratuit

**Niveau de difficulté :** Intermédiaire

---

#### Comparaison rapide

| Plateforme | Type d'app | Gratuit ? | Complexité | Meilleur pour |
|------------|-----------|-----------|------------|---------------|
| **Netlify** | Frontend/Statique | Oui | Facile | Sites statiques, JAMstack |
| **Vercel** | Frontend/Next.js | Oui | Facile | Next.js, React, Vue |
| **GitHub Pages** | Statique | Oui | Facile | Pages de projet, documentation |
| **Heroku** | Full-stack | Limité | Moyen | Prototypes, MVPs avec backend |
| **DigitalOcean** | Full-stack | Non | Moyen | Applications avec plus de contrôle |
| **AWS/GCP/Azure** | Tout | Crédits gratuits | Difficile | Production, grandes applications |

---

### Configuration d'un déploiement automatisé

Créons ensemble un déploiement automatisé complet. Nous utiliserons Netlify comme exemple car c'est le plus simple.

#### Projet exemple : Site React

**Structure du projet :**
```
mon-site/
  ├── src/
  │   ├── App.jsx
  │   └── main.jsx
  ├── public/
  ├── package.json
  ├── vite.config.js
  └── .gitignore
```

#### Étape 1 : Préparer le projet

**package.json :**
```json
{
  "name": "mon-site",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.0.0",
    "vite": "^4.3.0"
  }
}
```

**Vérifier que le build fonctionne localement :**
```bash
npm run build
# Doit créer un dossier dist/
```

#### Étape 2 : Pousser sur Git

```bash
# Initialiser Git si pas déjà fait
git init

# Ajouter un .gitignore
echo "node_modules/\ndist/\n.env" > .gitignore

# Premier commit
git add .  
git commit -m "Initial commit"

# Créer un dépôt sur GitHub et pousser
git remote add origin https://github.com/username/mon-site.git  
git push -u origin main
```

#### Étape 3 : Configurer Netlify

**Option A : Via l'interface web (recommandé pour débuter)**

1. Allez sur netlify.com
2. Cliquez sur "Add new site" → "Import an existing project"
3. Connectez GitHub
4. Sélectionnez votre dépôt
5. Configuration :
   - **Branch to deploy:** main
   - **Build command:** npm run build
   - **Publish directory:** dist
6. Cliquez sur "Deploy site"

**Option B : Via le fichier de configuration (plus avancé)**

Créez `netlify.toml` à la racine :
```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "18"

# Redirections pour Single Page Apps
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

# Headers de sécurité
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
```

Puis :
```bash
# Installer Netlify CLI
npm install -g netlify-cli

# Se connecter
netlify login

# Initialiser
netlify init

# Déployer
netlify deploy --prod
```

#### Étape 4 : Déploiement automatique configuré !

Maintenant, à chaque fois que vous faites :
```bash
git add .  
git commit -m "Nouvelle fonctionnalité"  
git push origin main
```

Netlify :
1. Détecte le push
2. Récupère le code
3. Exécute `npm run build`
4. Déploie le contenu de `dist/`
5. Votre site est mis à jour ! (généralement en 1-2 minutes)

#### Étape 5 : Vérifier le déploiement

Sur Netlify, vous pouvez voir :
- L'historique des déploiements
- Les logs de build
- Les erreurs éventuelles
- L'URL de votre site

---

### Git Hooks pour le déploiement

Les **Git hooks** sont des scripts qui s'exécutent automatiquement lors de certaines actions Git. Ils peuvent être utilisés pour déclencher des déploiements.

#### Qu'est-ce qu'un Git hook ?

C'est un script qui s'exécute à des moments précis :
- `pre-commit` : avant un commit
- `pre-push` : avant un push
- `post-receive` : après réception d'un push (sur le serveur)

#### Hook post-receive pour déploiement

C'est le hook le plus utilisé pour le déploiement automatique.

**Scénario :** Vous avez un serveur avec Git. Chaque fois que vous poussez du code, le serveur déploie automatiquement.

**Sur le serveur :**

```bash
# Créer un dépôt bare (sans working directory)
cd /var/www/  
git init --bare mon-site.git

# Créer le hook post-receive
cd mon-site.git/hooks/  
nano post-receive
```

**Contenu de post-receive :**
```bash
#!/bin/bash

# Dossier de travail où déployer
WORK_TREE=/var/www/mon-site-live  
GIT_DIR=/var/www/mon-site.git

# Checkout du code
git --work-tree=$WORK_TREE --git-dir=$GIT_DIR checkout -f main

# Aller dans le dossier
cd $WORK_TREE

# Installer les dépendances
npm install

# Build
npm run build

# Redémarrer le service (si application Node.js)
pm2 restart mon-site

echo "Déploiement terminé avec succès !"
```

**Rendre le script exécutable :**
```bash
chmod +x post-receive
```

**Sur votre machine locale :**
```bash
# Ajouter le serveur comme remote
git remote add production user@votre-serveur.com:/var/www/mon-site.git

# Déployer en production
git push production main
```

**Résultat :** Chaque `git push production main` déclenche le déploiement automatique !

---

### Variables d'environnement et secrets

Les applications ont souvent besoin de **secrets** (clés API, mots de passe de base de données, etc.). Ces secrets ne doivent JAMAIS être dans Git !

#### Mauvaise pratique ❌

```javascript
// config.js - NE FAITES JAMAIS ÇA !
export const API_KEY = "sk_live_abc123def456"  
export const DATABASE_PASSWORD = "motdepasse123"
```

Si vous committez ce fichier, vos secrets sont publics !

#### Bonne pratique ✅

**1. Utiliser des variables d'environnement**

```javascript
// config.js
export const API_KEY = process.env.API_KEY  
export const DATABASE_PASSWORD = process.env.DATABASE_PASSWORD
```

**2. Créer un fichier .env (local uniquement)**

```bash
# .env - Ce fichier n'est PAS commité
API_KEY=sk_live_abc123def456  
DATABASE_PASSWORD=motdepasse123
```

**3. Ajouter .env au .gitignore**

```bash
# .gitignore
.env
.env.local
.env.production
```

**4. Fournir un exemple**

```bash
# .env.example - Ce fichier est commité
API_KEY=your_api_key_here  
DATABASE_PASSWORD=your_password_here
```

**5. Configurer les secrets sur la plateforme de déploiement**

**Netlify :**
- Site settings → Environment variables
- Ajoutez vos secrets
- Ils seront disponibles lors du build

**Heroku :**
```bash
heroku config:set API_KEY=sk_live_abc123def456  
heroku config:set DATABASE_PASSWORD=motdepasse123
```

**GitHub Actions :**
- Repository settings → Secrets → Actions
- Ajoutez vos secrets
- Utilisez-les dans les workflows :
```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

---

### Rollback : revenir en arrière

Un déploiement qui a mal tourné ? Pas de panique ! Le rollback permet de revenir rapidement à la version précédente.

#### Rollback avec Git

**Si vous utilisez Git pour déployer :**

```bash
# Voir les derniers commits
git log --oneline

# Revenir au commit précédent
git revert HEAD

# Ou forcer le retour (attention, dangereux !)
git reset --hard HEAD~1

# Pousser
git push origin main --force
```

#### Rollback avec les plateformes

**Netlify :**
1. Allez dans Deploys
2. Trouvez le déploiement précédent (celui qui fonctionnait)
3. Cliquez sur "Publish deploy"
4. Votre site revient à cette version en quelques secondes !

**Heroku :**
```bash
# Voir les releases
heroku releases

# Revenir à une release précédente
heroku rollback v42
```

**Avec tags Git :**
```bash
# Déployer un tag spécifique
git checkout v1.2.0  
git push production v1.2.0

# Ou créer un nouveau tag qui pointe vers l'ancien commit
git tag v1.2.1 abc123  # abc123 = hash du bon commit  
git push origin v1.2.1
```

---

### Bonnes pratiques de déploiement

#### 1. Ne jamais déployer directement en production

Utilisez toujours staging d'abord :
```
develop → Staging → Tests → main → Production
```

#### 2. Automatiser les tests avant le déploiement

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run tests
        run: npm test

  deploy:
    needs: test  # Ne déploie que si les tests passent
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Netlify
        # ... étapes de déploiement
```

#### 3. Utiliser des tags pour les releases importantes

```bash
# Créer un tag pour la version
git tag -a v1.2.0 -m "Version 1.2.0 - Ajout du dashboard"  
git push origin v1.2.0
```

Les tags marquent des points importants dans l'historique.

#### 4. Avoir un plan de rollback

Avant chaque déploiement, demandez-vous :
- Comment revenir en arrière si ça casse ?
- Combien de temps ça prendra ?
- Y a-t-il des migrations de base de données irréversibles ?

#### 5. Monitorer après le déploiement

Après un déploiement :
- Surveillez les logs d'erreurs
- Vérifiez les métriques (temps de réponse, taux d'erreur)
- Testez les fonctionnalités critiques manuellement

Outils de monitoring :
- Sentry (erreurs)
- Google Analytics (usage)
- Uptime Robot (disponibilité)

#### 6. Déployer aux heures creuses

Si possible, déployez quand il y a peu d'utilisateurs :
- Tôt le matin
- Tard le soir
- Pendant le week-end (selon votre audience)

Évitez absolument :
- Le vendredi soir (si ça casse, votre week-end est fichu)
- Avant les vacances
- Pendant les pics de trafic

#### 7. Documenter les déploiements

Tenez un changelog :
```markdown
# Changelog

## v1.2.0 - 2025-10-16
### Ajouté
- Nouveau dashboard utilisateur
- Export CSV des données

### Corrigé
- Bug sur la connexion mobile
- Performance de la recherche

### Déployé par
Alice - 14h30 UTC
```

#### 8. Tester localement avant de pousser

```bash
# Toujours tester le build localement
npm run build  
npm run preview  # ou servir le dossier dist/

# Vérifier que tout fonctionne avant de pousser
```

#### 9. Utiliser des branches de protection

Sur GitHub/GitLab, protégez la branche `main` :
- Interdire les pushs directs
- Obliger à passer par des Pull Requests
- Exiger que les tests passent avant le merge

#### 10. Communiquer

Prévenez votre équipe :
- "Je vais déployer dans 10 minutes"
- "Déploiement en cours, peut prendre 5 minutes"
- "Déploiement terminé, tout est OK !"

Utilisez Slack, Discord, ou votre outil de communication d'équipe.

---

### Déploiement de différents types d'applications

#### Site statique (HTML/CSS/JS)

**Le plus simple !**

```bash
# Build (si nécessaire)
npm run build

# Déployer
# Sur Netlify : juste pousser sur Git
git push origin main
```

**Plateforme recommandée :** Netlify, Vercel, GitHub Pages

#### Application React / Vue / Angular

**Très similaire au site statique**

```bash
# Build crée un bundle optimisé
npm run build

# Le dossier build/dist contient tout le nécessaire
```

**Points d'attention :**
- Gérer le routing (SPA routing)
- Variables d'environnement pour les APIs
- Optimisation (code splitting, lazy loading)

**Plateforme recommandée :** Vercel, Netlify

#### Application Next.js

**Optimisé pour Vercel (créé par la même équipe)**

```bash
# Vercel détecte automatiquement Next.js
git push origin main
```

**Fonctionnalités automatiques :**
- Server-Side Rendering (SSR)
- Static Site Generation (SSG)
- API Routes
- Image optimization

**Plateforme recommandée :** Vercel

#### Application Node.js / Express

**Nécessite un serveur qui reste actif**

```bash
# Sur Heroku
git push heroku main

# Heroku démarre automatiquement avec
npm start
```

**Fichier nécessaire : Procfile**
```
web: node server.js
```

**Plateforme recommandée :** Heroku, DigitalOcean App Platform, Railway

#### Application Python (Flask / Django)

**Configuration similaire à Node.js**

```python
# requirements.txt
Flask==2.3.0  
gunicorn==20.1.0
```

```
# Procfile
web: gunicorn app:app
```

**Plateforme recommandée :** Heroku, Railway, PythonAnywhere

#### Application avec base de données

**Deux approches :**

**1. Base de données gérée (recommandé)**
- Heroku Postgres
- MongoDB Atlas
- PlanetScale (MySQL)
- Supabase

**2. Base de données auto-hébergée**
- DigitalOcean Droplet + PostgreSQL
- AWS RDS

**Important :**
- Sauvegardes automatiques
- Migrations de base de données (attention au rollback !)
- Variables d'environnement pour les credentials

---

### Dépannage des déploiements

#### Le build échoue

**Symptômes :** Le déploiement s'arrête pendant la phase de build

**Solutions :**
1. Vérifier les logs de build
2. Reproduire localement : `npm run build`
3. Vérifier les versions (Node.js, dépendances)
4. Erreurs communes :
   - Dépendance manquante dans `package.json`
   - Erreur TypeScript/ESLint
   - Fichier manquant

#### L'application ne démarre pas

**Symptômes :** Build OK mais l'app crash au démarrage

**Solutions :**
1. Vérifier les logs d'exécution
2. Variables d'environnement manquantes ?
3. Port correct configuré ?
4. Base de données accessible ?

```javascript
// Astuce : logger les variables d'env au démarrage
console.log('NODE_ENV:', process.env.NODE_ENV)  
console.log('Database connected:', dbConnected)
```

#### Erreurs 404 sur une SPA

**Symptômes :** La page d'accueil fonctionne mais les routes /about, /contact donnent 404

**Solution :** Configurer les redirects pour SPAs

```toml
# netlify.toml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

#### Variables d'environnement non disponibles

**Symptômes :** `undefined` au lieu de la valeur attendue

**Solutions :**
1. Vérifier que la variable est configurée sur la plateforme
2. Redéployer après avoir ajouté la variable
3. Préfixe correct ? (ex: `VITE_` pour Vite, `NEXT_PUBLIC_` pour Next.js)

```javascript
// Avec Vite, seules les variables préfixées sont exposées
// ❌ process.env.API_KEY
// ✅ import.meta.env.VITE_API_KEY
```

---

### Ressources et outils complémentaires

#### Documentation officielle

- **Netlify Docs :** docs.netlify.com
- **Vercel Docs :** vercel.com/docs
- **Heroku Dev Center :** devcenter.heroku.com
- **GitHub Pages :** docs.github.com/pages

#### Outils de monitoring

- **Sentry :** Tracking d'erreurs (sentry.io)
- **LogRocket :** Enregistrement de sessions (logrocket.com)
- **Datadog :** Monitoring infrastructure (datadoghq.com)

#### Outils de performance

- **Lighthouse :** Audit de performance (intégré Chrome)
- **WebPageTest :** Test de performance (webpagetest.org)
- **Bundle Analyzer :** Analyse de la taille des bundles

#### Communautés

- **r/webdev** (Reddit)
- **Dev.to** (Articles et tutoriels)
- **Discord servers** (React, Vue, etc.)

---

### Conclusion

Le déploiement automatisé avec Git a transformé la façon dont nous livrons des applications. Ce qui prenait des heures de travail manuel et de stress peut maintenant se faire en quelques minutes avec un simple `git push`.

**Points clés à retenir :**

✅ **Git est au cœur du déploiement moderne** - Chaque push peut déclencher un déploiement

✅ **Plusieurs environnements** - Dev, Staging, Production pour tester avant de livrer

✅ **Différentes stratégies** - Basic, Rolling, Blue-Green, Canary selon vos besoins

✅ **Plateformes simplifiées** - Netlify, Vercel, Heroku rendent le déploiement accessible

✅ **Sécurité** - Variables d'environnement pour protéger vos secrets

✅ **Rollback** - Possibilité de revenir en arrière rapidement

✅ **Bonnes pratiques** - Tests automatisés, staging, monitoring, communication

**Pour débuter :**
1. Commencez avec un projet simple sur Netlify ou GitHub Pages
2. Configurez le déploiement automatique
3. Faites un changement et poussez
4. Voyez la magie opérer !

Le déploiement automatisé supprime une barrière majeure entre vous et vos utilisateurs. Vos idées peuvent être en ligne en quelques minutes, pas en quelques jours. C'est ça, la puissance du déploiement automatisé avec Git !

Dans la section suivante, nous explorerons Git LFS (Large File Storage) pour gérer les gros fichiers dans Git.

⏭️ [Git LFS pour les gros fichiers](/module-09-outils-et-integration/06-git-lfs.md)
