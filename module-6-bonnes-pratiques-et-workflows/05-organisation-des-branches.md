# 6.5. Organisation des branches (main, dev, feature/, hotfix/)

## Pourquoi organiser ses branches ?

Imaginez un projet Git comme un grand arbre avec différentes branches qui poussent dans toutes les directions. Sans organisation, cela devient rapidement un enchevêtrement difficile à gérer. Une bonne organisation des branches vous permet de :

- **Travailler en parallèle** sans vous gêner mutuellement
- **Isoler les nouvelles fonctionnalités** du code stable
- **Faciliter le suivi des modifications** en cours
- **Gérer les versions** de votre logiciel
- **Maintenir un historique** clair et compréhensible

Voyons comment structurer efficacement vos branches pour des projets de toute taille.

## Les différents types de branches

### Vue d'ensemble

Voici une structure typique utilisée dans de nombreux projets :

```
main (ou master)
├── develop
│   ├── feature/login
│   ├── feature/newsletter
│   └── feature/search
└── hotfix/security-patch
```

Chaque type de branche a un rôle spécifique dans l'organisation de votre projet.

## Branches principales

### Branche `main` (ou `master`)

La branche `main` (anciennement appelée `master` par défaut) est la branche principale de votre projet.

**Caractéristiques :**
- Contient le code **stable** et **prêt pour la production**
- Chaque commit représente idéalement une **version publiée**
- Souvent **protégée** contre les modifications directes
- Ne devrait jamais contenir de code cassé

**Quand l'utiliser :**
- Pour déployer en production
- Pour créer des tags de version (v1.0.0, v1.1.0, etc.)
- Pour générer des builds officiels

**Exemple :**
```bash
# Créer un tag de version sur main
git checkout main
git tag -a v1.0.0 -m "Version 1.0.0"
git push origin v1.0.0
```

### Branche `develop`

Dans les projets qui suivent Git Flow, une branche `develop` sert d'intégration continue.

**Caractéristiques :**
- Contient les dernières fonctionnalités **développées** mais pas encore publiées
- **Plus active** que `main`
- Sert de base pour les branches de fonctionnalités
- Peut être déployée dans des environnements de **test** ou **staging**

**Quand l'utiliser :**
- Pour intégrer les nouvelles fonctionnalités
- Pour préparer la prochaine version
- Pour les tests d'intégration

**Exemple :**
```bash
# Créer la branche develop à partir de main
git checkout main
git checkout -b develop
git push origin develop
```

## Branches temporaires

### Branches `feature/`

Les branches de fonctionnalités (`feature/`) servent à développer de nouvelles fonctionnalités isolément.

**Caractéristiques :**
- Créées à partir de `develop` (ou de `main` dans GitHub Flow)
- Nommées avec un préfixe `feature/` suivi d'un nom descriptif
- **Temporaires** : supprimées après fusion
- Une branche par fonctionnalité ou sous-fonctionnalité

**Convention de nommage :**
- `feature/login-system`
- `feature/user-profile`
- `feature/payment-integration`

**Exemple :**
```bash
# Créer une branche de fonctionnalité
git checkout develop
git checkout -b feature/login-system

# Une fois terminée, la fusionner dans develop
git checkout develop
git merge --no-ff feature/login-system
git push origin develop

# Supprimer la branche locale et distante
git branch -d feature/login-system
git push origin --delete feature/login-system
```

### Branches `bugfix/` ou `fix/`

Pour corriger des bugs dans le code en développement.

**Caractéristiques :**
- Similaires aux branches `feature/` mais dédiées aux corrections
- Généralement créées à partir de `develop`
- Nommées avec un préfixe `bugfix/` ou `fix/`

**Convention de nommage :**
- `bugfix/login-validation`
- `fix/broken-links`

**Exemple :**
```bash
git checkout develop
git checkout -b bugfix/login-validation
# Correction du bug
git commit -m "fix: corriger la validation du formulaire de login"
```

### Branches `hotfix/`

Les branches `hotfix/` servent à corriger rapidement des bugs critiques en production.

**Caractéristiques :**
- Créées à partir de `main`
- Fusionnées à la fois dans `main` ET dans `develop`
- Généralement suivies d'une nouvelle version mineure
- Utilisées uniquement pour les bugs **urgents** en production

**Convention de nommage :**
- `hotfix/security-vulnerability`
- `hotfix/critical-performance`

**Exemple :**
```bash
# Créer une branche hotfix
git checkout main
git checkout -b hotfix/security-vulnerability

# Corriger le bug et commiter
git commit -m "fix: corriger la faille de sécurité XSS"

# Fusionner dans main
git checkout main
git merge --no-ff hotfix/security-vulnerability
git tag -a v1.0.1 -m "Version 1.0.1 - Correctif de sécurité"

# Fusionner aussi dans develop
git checkout develop
git merge --no-ff hotfix/security-vulnerability

# Supprimer la branche
git branch -d hotfix/security-vulnerability
```

### Branches `release/`

Les branches `release/` préparent une nouvelle version pour la production.

**Caractéristiques :**
- Créées à partir de `develop`
- Permettent de finaliser une version (corrections mineures, documentation)
- Fusionnées dans `main` ET `develop`
- Nommées avec un préfixe `release/` suivi du numéro de version

**Convention de nommage :**
- `release/1.0.0`
- `release/2.3.0`

**Exemple :**
```bash
# Créer une branche de release
git checkout develop
git checkout -b release/1.0.0

# Derniers ajustements et commits
git commit -m "docs: finaliser les notes de version"

# Fusionner dans main
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"

# Fusionner dans develop
git checkout develop
git merge --no-ff release/1.0.0

# Supprimer la branche
git branch -d release/1.0.0
```

## Conventions de nommage

Pour une organisation efficace, suivez ces conventions :

1. **Utilisez des préfixes clairs** pour identifier le type de branche :
   - `feature/`
   - `bugfix/` ou `fix/`
   - `hotfix/`
   - `release/`

2. **Nommez de façon descriptive** après le préfixe :
   - ✅ `feature/user-authentication`
   - ❌ `feature/task123` (pas assez descriptif)

3. **Utilisez des tirets** (`-`) pour séparer les mots :
   - ✅ `feature/shopping-cart`
   - ❌ `feature/shoppingCart` ou `feature/shopping_cart`

4. **Restez concis** tout en étant descriptif (3-5 mots maximum)

5. **Incluez des identifiants de tickets** si vous utilisez un système de suivi :
   - `feature/PROJ-123-user-authentication`

## Organisation selon la taille du projet

### Pour les petits projets personnels

Pour les projets simples, vous pouvez adopter une structure minimaliste :

```
main
└── feature/x
```

- **Branch principale** : `main`
- **Branches de fonctionnalités** : `feature/x` directement depuis `main`

### Pour les projets d'équipe de taille moyenne

```
main
├── develop
│   ├── feature/x
│   └── feature/y
└── hotfix/z
```

- Ajoutez la branche **`develop`** comme intégration
- Utilisez les branches **`feature/`** depuis `develop`
- Ajoutez les branches **`hotfix/`** depuis `main` pour les urgences

### Pour les grands projets

Structure complète Git Flow :

```
main
├── develop
│   ├── feature/x
│   ├── feature/y
│   └── release/1.0.0
└── hotfix/z
```

- Ajoutez les branches **`release/`** pour préparer les versions
- Considérez ajouter des branches d'environnement comme `staging`

## Protection des branches

Pour les projets en équipe, pensez à protéger vos branches principales :

### Sur GitHub

1. Allez dans **Settings > Branches**
2. Sélectionnez **Add branch protection rule**
3. Configurez les règles pour `main` et `develop` :
   - **Require pull request reviews**
   - **Require status checks to pass**
   - **Restrict who can push**

![Protection de branches sur GitHub](https://docs.github.com/assets/cb-32892/mw-1440/images/help/repository/branch-protection-rule-options.webp)

### Sur GitLab

1. Allez dans **Settings > Repository > Protected Branches**
2. Ajoutez `main` et `develop` avec les protections appropriées

## Bonnes pratiques

1. **Gardez vos branches à jour** :
   ```bash
   # Mettre à jour sa branche feature avec develop
   git checkout feature/ma-fonctionnalite
   git pull origin develop
   ```

2. **Nettoyez régulièrement** les branches fusionnées :
   ```bash
   # Lister les branches fusionnées
   git branch --merged

   # Supprimer les branches locales fusionnées
   git branch --merged | grep -v "\*" | grep -v "main" | grep -v "develop" | xargs git branch -d
   ```

3. **Utilisez des branches courtes** : plus une branche vit longtemps, plus les conflits risquent d'être nombreux

4. **Une fonctionnalité = une branche** : évitez de mélanger plusieurs fonctionnalités dans une seule branche

5. **Communiquez** sur les branches en cours pour éviter les doublons

## Visualiser votre structure de branches

Pour mieux comprendre votre organisation, utilisez des outils de visualisation :

```bash
# Afficher un graphe ASCII en ligne de commande
git log --graph --oneline --all --decorate
```

Ou utilisez des outils graphiques comme :
- GitKraken
- Sourcetree
- GitHub Desktop
- Extension Git Graph dans VS Code

![Exemple de visualisation avec GitKraken](https://www.gitkraken.com/img/og-git-client.png)

## Exercice pratique

Essayez cette structure sur un projet simple :

1. Créez un dépôt et une branche `main`
2. Ajoutez une branche `develop`
3. Créez deux branches de fonctionnalités depuis `develop` :
   - `feature/header`
   - `feature/footer`
4. Simulez un bug en production et créez une branche `hotfix/typo`
5. Fusionnez toutes ces branches en respectant le workflow

## Conclusion

Une bonne organisation des branches est le fondement d'une collaboration efficace avec Git. Elle reflète souvent la qualité générale des pratiques de développement d'une équipe.

Commencez simple, puis adaptez progressivement votre structure selon les besoins de votre projet. L'essentiel est que toute l'équipe comprenne et respecte les conventions établies.

N'oubliez pas : les branches sont des outils flexibles, et leur organisation doit servir votre workflow, pas l'inverse. Adaptez ces recommandations à vos besoins spécifiques !
