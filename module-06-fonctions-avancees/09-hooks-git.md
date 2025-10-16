🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 9. Hooks Git (automatisation)

### Introduction

Imaginez que vous avez un assistant personnel qui vérifie automatiquement votre code avant chaque commit, qui envoie une notification après chaque push, ou qui lance les tests automatiquement. C'est exactement ce que font les **hooks Git** !

Un **hook** (crochet en français) est un script qui s'exécute automatiquement lorsqu'un événement Git spécifique se produit. C'est comme des "déclencheurs" qui permettent d'automatiser des tâches répétitives et d'appliquer des règles dans votre workflow.

**Exemples d'utilisation :**
- Vérifier le formatage du code avant un commit
- Lancer les tests automatiquement
- Valider les messages de commit
- Empêcher les commits sur certaines branches
- Envoyer des notifications après un push
- Générer de la documentation automatiquement

---

### Qu'est-ce qu'un hook Git ?

Un hook Git est :
- Un **script exécutable** (bash, Python, Node.js, etc.)
- Stocké dans le dossier `.git/hooks/`
- Déclenché automatiquement par des événements Git
- **Local** à votre dépôt (non partagé par défaut)
- Capable de **bloquer** une action si nécessaire

**Analogie :** C'est comme un système d'alarme qui se déclenche automatiquement quand vous ouvrez une porte (événement) et qui peut vous empêcher d'entrer si quelque chose ne va pas.

---

### Types de hooks

Git propose deux catégories de hooks :

#### Hooks côté client (local)

S'exécutent sur votre machine lors d'opérations locales :
- `pre-commit` : Avant un commit
- `prepare-commit-msg` : Avant l'édition du message de commit
- `commit-msg` : Après l'édition du message de commit
- `post-commit` : Après un commit
- `pre-push` : Avant un push
- `post-checkout` : Après un checkout
- `post-merge` : Après un merge

#### Hooks côté serveur (distant)

S'exécutent sur le serveur Git lors d'opérations réseau :
- `pre-receive` : Avant de recevoir un push
- `update` : Avant de mettre à jour une référence
- `post-receive` : Après avoir reçu un push

**Note :** Nous nous concentrerons sur les hooks côté client car ce sont les plus utilisés.

---

### Où sont les hooks ?

Les hooks se trouvent dans le dossier `.git/hooks/` de votre dépôt.

```bash
ls .git/hooks/
```

**Contenu typique :**

```
applypatch-msg.sample
commit-msg.sample
post-update.sample
pre-applypatch.sample
pre-commit.sample
pre-push.sample
pre-rebase.sample
prepare-commit-msg.sample
update.sample
```

Ces fichiers `.sample` sont des exemples fournis par Git. Pour les activer, il faut :
1. Retirer l'extension `.sample`
2. Les rendre exécutables (`chmod +x`)

---

### Créer votre premier hook

#### Exemple 1 : Hook pre-commit simple

Créons un hook qui affiche un message avant chaque commit.

**Étapes :**

```bash
# 1. Créer le fichier hook
touch .git/hooks/pre-commit

# 2. Le rendre exécutable
chmod +x .git/hooks/pre-commit

# 3. Éditer le fichier
nano .git/hooks/pre-commit
```

**Contenu du fichier :**

```bash
#!/bin/bash

echo "🔍 Vérification avant commit..."
echo "✅ Tout est OK !"
```

**Test :**

```bash
git add .
git commit -m "Test du hook"
# 🔍 Vérification avant commit...
# ✅ Tout est OK !
# [main abc123] Test du hook
```

Le message s'affiche automatiquement avant le commit ! 🎉

---

### Les hooks les plus utiles

#### 1. pre-commit : Vérifier avant de commiter

Le hook `pre-commit` s'exécute juste avant la création du commit. Si le script retourne un code d'erreur (≠0), le commit est **annulé**.

**Cas d'usage :**
- Vérifier le formatage du code
- Lancer un linter
- Détecter des secrets (mots de passe, clés API)
- Empêcher les commits de fichiers trop gros

**Exemple : Vérifier qu'il n'y a pas de `console.log` :**

```bash
#!/bin/bash

# Chercher console.log dans les fichiers JavaScript modifiés
if git diff --cached --name-only | grep '\.js$' | xargs grep -l 'console.log' > /dev/null; then
    echo "❌ Erreur : Des console.log ont été trouvés !"
    echo "Supprimez-les avant de commiter."
    exit 1
fi

echo "✅ Pas de console.log trouvé"
exit 0
```

**Test :**

```bash
# Ajouter un console.log dans un fichier
echo "console.log('debug');" >> app.js
git add app.js
git commit -m "Test"
# ❌ Erreur : Des console.log ont été trouvés !
# Le commit est annulé !
```

**Exemple : Lancer les tests :**

```bash
#!/bin/bash

echo "🧪 Lancement des tests..."

npm test

if [ $? -ne 0 ]; then
    echo "❌ Les tests échouent. Commit annulé."
    exit 1
fi

echo "✅ Tous les tests passent !"
exit 0
```

**Exemple : Vérifier le formatage avec Prettier :**

```bash
#!/bin/bash

echo "🎨 Vérification du formatage..."

npm run lint

if [ $? -ne 0 ]; then
    echo "❌ Erreurs de formatage détectées."
    echo "Lancez 'npm run lint:fix' pour corriger."
    exit 1
fi

echo "✅ Le code est bien formaté !"
exit 0
```

#### 2. commit-msg : Valider le message de commit

Le hook `commit-msg` reçoit le message de commit en argument et peut le valider ou le rejeter.

**Cas d'usage :**
- Appliquer une convention de messages (Conventional Commits)
- S'assurer qu'un numéro de ticket est présent
- Vérifier la longueur du message

**Exemple : Forcer la convention Conventional Commits :**

```bash
#!/bin/bash

commit_msg=$(cat "$1")

# Pattern pour Conventional Commits : type(scope): message
pattern="^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+"

if ! echo "$commit_msg" | grep -qE "$pattern"; then
    echo "❌ Message de commit invalide !"
    echo ""
    echo "Format attendu : type(scope): message"
    echo "Types valides : feat, fix, docs, style, refactor, test, chore"
    echo ""
    echo "Exemples :"
    echo "  feat(auth): Add login feature"
    echo "  fix(api): Correct timeout issue"
    echo "  docs: Update README"
    exit 1
fi

echo "✅ Message de commit valide"
exit 0
```

**Test :**

```bash
git commit -m "Added a feature"
# ❌ Message de commit invalide !

git commit -m "feat: Add new feature"
# ✅ Message de commit valide
```

**Exemple : Exiger un numéro de ticket :**

```bash
#!/bin/bash

commit_msg=$(cat "$1")

# Vérifier la présence d'un numéro de ticket type JIRA-123
if ! echo "$commit_msg" | grep -qE "[A-Z]+-[0-9]+"; then
    echo "❌ Le message doit contenir un numéro de ticket (ex: JIRA-123)"
    exit 1
fi

echo "✅ Numéro de ticket présent"
exit 0
```

#### 3. pre-push : Vérifier avant de pousser

Le hook `pre-push` s'exécute avant un `git push`. Utile pour des vérifications plus longues.

**Cas d'usage :**
- Lancer la suite de tests complète
- Empêcher le push sur certaines branches
- Vérifier la qualité du code

**Exemple : Empêcher le push direct sur main :**

```bash
#!/bin/bash

current_branch=$(git symbolic-ref --short HEAD)

if [ "$current_branch" = "main" ]; then
    echo "❌ Push direct sur 'main' interdit !"
    echo "Créez une Pull Request à la place."
    exit 1
fi

echo "✅ Push autorisé sur la branche $current_branch"
exit 0
```

**Exemple : Lancer les tests avant push :**

```bash
#!/bin/bash

echo "🧪 Lancement de tous les tests avant push..."

npm run test:all

if [ $? -ne 0 ]; then
    echo "❌ Les tests échouent. Push annulé."
    echo "Corrigez les tests avant de pousser."
    exit 1
fi

echo "✅ Tous les tests passent, push autorisé"
exit 0
```

#### 4. post-commit : Actions après commit

Le hook `post-commit` s'exécute après un commit réussi. Il ne peut pas annuler le commit.

**Cas d'usage :**
- Envoyer une notification
- Mettre à jour la documentation
- Loguer l'activité

**Exemple : Notification simple :**

```bash
#!/bin/bash

commit_msg=$(git log -1 --pretty=%B)
echo "✅ Commit réussi : $commit_msg"

# Afficher une notification système (macOS)
osascript -e "display notification \"Commit réussi !\" with title \"Git\""
```

#### 5. prepare-commit-msg : Préparer le message

Le hook `prepare-commit-msg` modifie le message avant que l'éditeur s'ouvre.

**Cas d'usage :**
- Ajouter automatiquement un numéro de ticket depuis le nom de branche
- Générer un template de message

**Exemple : Ajouter le numéro de ticket depuis la branche :**

```bash
#!/bin/bash

commit_msg_file="$1"
branch=$(git symbolic-ref --short HEAD)

# Extraire le numéro de ticket du nom de branche
# Ex : feature/JIRA-123-login -> JIRA-123
ticket=$(echo "$branch" | grep -oE '[A-Z]+-[0-9]+')

if [ -n "$ticket" ]; then
    # Ajouter le ticket au début du message
    echo "[$ticket] $(cat $commit_msg_file)" > "$commit_msg_file"
fi
```

**Résultat :**

```bash
# Sur la branche feature/JIRA-123-login
git commit
# L'éditeur s'ouvre avec : [JIRA-123]
```

---

### Bypasser les hooks

Parfois, vous devez ignorer les hooks (avec précaution !) :

```bash
git commit --no-verify -m "Emergency fix"
# ou
git commit -n -m "Emergency fix"
```

**Attention :** N'utilisez ceci que pour des cas exceptionnels !

---

### Partager les hooks avec l'équipe

**Problème :** Les hooks dans `.git/hooks/` ne sont **pas versionnés** par Git (car `.git/` n'est pas dans le dépôt).

**Solutions :**

#### Solution 1 : Dossier hooks/ versionné avec script d'installation

```bash
# Structure du projet
projet/
├── .git/
├── hooks/          ← Dossier versionné
│   ├── pre-commit
│   └── commit-msg
└── install-hooks.sh
```

**Script d'installation :**

```bash
#!/bin/bash
# install-hooks.sh

echo "Installation des hooks Git..."

# Copier les hooks
cp hooks/* .git/hooks/

# Les rendre exécutables
chmod +x .git/hooks/*

echo "✅ Hooks installés avec succès !"
```

**Usage :**

```bash
git clone <url>
./install-hooks.sh
```

#### Solution 2 : Utiliser Husky (Node.js)

**Husky** est un outil très populaire qui gère automatiquement les hooks Git.

**Installation :**

```bash
npm install --save-dev husky
npx husky init
```

**Configuration dans package.json :**

```json
{
  "scripts": {
    "prepare": "husky"
  }
}
```

**Créer un hook :**

```bash
npx husky add .husky/pre-commit "npm test"
```

**Contenu de `.husky/pre-commit` :**

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm test
```

**Avantages de Husky :**
- Hooks versionnés avec le projet
- Installation automatique pour tous les développeurs
- Simple à configurer
- Compatible avec lint-staged

#### Solution 3 : Git config core.hooksPath

```bash
# Pointer vers un dossier de hooks custom
git config core.hooksPath .githooks

# Maintenant Git utilise .githooks/ au lieu de .git/hooks/
```

---

### Hooks avancés avec des outils modernes

#### Lint-staged : Linter uniquement les fichiers modifiés

**Problème :** Lancer le linter sur tout le projet est lent.

**Solution :** `lint-staged` lint uniquement les fichiers stagés.

**Installation :**

```bash
npm install --save-dev husky lint-staged
```

**Configuration dans package.json :**

```json
{
  "lint-staged": {
    "*.js": ["eslint --fix", "prettier --write"],
    "*.css": ["stylelint --fix", "prettier --write"],
    "*.md": ["prettier --write"]
  }
}
```

**Hook pre-commit avec Husky :**

```bash
npx husky add .husky/pre-commit "npx lint-staged"
```

**Résultat :** Seuls les fichiers modifiés sont vérifiés, c'est rapide ! ⚡

#### Commitlint : Valider les messages de commit

**Installation :**

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

**Configuration :**

```bash
# commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

**Hook commit-msg :**

```bash
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

**Maintenant, les commits doivent suivre Conventional Commits automatiquement !**

---

### Exemples de hooks complets

#### Exemple complet : Pre-commit avec plusieurs vérifications

```bash
#!/bin/bash

echo "🔍 Vérifications pre-commit..."

# 1. Vérifier qu'il n'y a pas de fichiers trop gros
echo "📦 Vérification de la taille des fichiers..."
max_size=5000000  # 5 MB
for file in $(git diff --cached --name-only); do
    if [ -f "$file" ]; then
        size=$(wc -c < "$file")
        if [ $size -gt $max_size ]; then
            echo "❌ Erreur : $file est trop gros ($(($size / 1000000)) MB)"
            exit 1
        fi
    fi
done

# 2. Vérifier qu'il n'y a pas de secrets
echo "🔐 Recherche de secrets..."
if git diff --cached | grep -iE '(api_key|password|secret|token).*=.*["\047][^"\047]{20,}' > /dev/null; then
    echo "❌ Attention : Possible secret détecté !"
    echo "Vérifiez que vous ne commitez pas de clés ou mots de passe."
    exit 1
fi

# 3. Lancer les tests
echo "🧪 Lancement des tests..."
npm test
if [ $? -ne 0 ]; then
    echo "❌ Les tests échouent"
    exit 1
fi

# 4. Vérifier le formatage
echo "🎨 Vérification du formatage..."
npm run lint
if [ $? -ne 0 ]; then
    echo "❌ Erreurs de formatage"
    exit 1
fi

echo "✅ Toutes les vérifications sont passées !"
exit 0
```

#### Exemple complet : Post-commit avec statistiques

```bash
#!/bin/bash

# Afficher des stats après chaque commit
commit_hash=$(git rev-parse --short HEAD)
commit_msg=$(git log -1 --pretty=%B)
files_changed=$(git show --pretty="" --name-only | wc -l)

echo ""
echo "📊 Statistiques du commit $commit_hash:"
echo "   Message : $commit_msg"
echo "   Fichiers modifiés : $files_changed"
echo "   Total de commits : $(git rev-list --count HEAD)"
echo ""

# Envoyer une notification (optionnel)
# curl -X POST https://hooks.slack.com/... -d "New commit: $commit_msg"
```

---

### Cas d'usage pratiques

#### Cas 1 : Projet professionnel avec CI/CD

```bash
# .husky/pre-commit
npm run lint
npm run test:unit

# .husky/pre-push
npm run test:integration
npm run build

# .husky/commit-msg
npx commitlint --edit "$1"
```

#### Cas 2 : Projet open source

```bash
# Vérifier que les contributeurs ont signé le CLA
# Vérifier le formatage du code
# Lancer les tests
# Vérifier la couverture de code
```

#### Cas 3 : Projet personnel

```bash
# Simple pre-commit pour éviter les erreurs de base
#!/bin/bash
npm run lint
echo "✅ Commit autorisé"
```

---

### Déboguer les hooks

#### Activer le mode debug

```bash
# Afficher les commandes exécutées
set -x

# Dans votre hook
#!/bin/bash
set -x  # Active le debug

echo "Test"
# + echo Test
# Test
```

#### Voir les erreurs

```bash
# Rediriger les erreurs vers un fichier
#!/bin/bash

exec 2>> /tmp/git-hook-errors.log

echo "Hook pre-commit exécuté" >> /tmp/git-hook-errors.log
# Vos commandes...
```

#### Tester un hook manuellement

```bash
# Tester sans faire de commit
.git/hooks/pre-commit

# Ou avec plus de détails
bash -x .git/hooks/pre-commit
```

---

### Bonnes pratiques

#### ✅ Faites des hooks rapides

Les hooks ralentissent le workflow. Privilégiez la rapidité :

```bash
# ❌ Mauvais : lancer tous les tests (lent)
npm test

# ✅ Bon : lancer seulement les tests unitaires rapides
npm run test:unit
```

#### ✅ Documentez vos hooks

Créez un fichier `HOOKS.md` expliquant les hooks du projet :

```markdown
# Hooks Git du projet

## pre-commit
- Vérifie le formatage avec ESLint
- Lance les tests unitaires
- Durée : ~15 secondes

## commit-msg
- Valide le format Conventional Commits

## pre-push
- Lance tous les tests
- Durée : ~2 minutes
```

#### ✅ Permettez de bypasser en cas d'urgence

Documentez comment bypasser :

```bash
# En cas d'urgence
git commit --no-verify -m "Hotfix"
```

#### ✅ Versionnez vos hooks

Utilisez Husky ou un script d'installation pour que toute l'équipe utilise les mêmes hooks.

#### ✅ Messages clairs

```bash
# ❌ Mauvais
echo "Error"
exit 1

# ✅ Bon
echo "❌ Le formatage du code ne respecte pas les standards."
echo "Lancez 'npm run lint:fix' pour corriger automatiquement."
exit 1
```

#### ❌ N'abusez pas des hooks

Trop de vérifications = workflow trop lent = développeurs frustrés

#### ❌ Ne faites pas confiance uniquement aux hooks

Les hooks peuvent être bypassés. Utilisez aussi la CI/CD.

---

### Hooks et CI/CD

Les hooks côté client et la CI/CD sont complémentaires :

| Hooks locaux | CI/CD |
|--------------|-------|
| Feedback rapide | Vérifications exhaustives |
| Peuvent être bypassés | Incontournables |
| Environnement du développeur | Environnement standardisé |
| Vérifications légères | Tests complets |

**Stratégie recommandée :**

```bash
# Pre-commit : Vérifications rapides (< 30s)
- lint
- tests unitaires

# CI/CD : Vérifications complètes
- tous les tests
- build
- analyse de sécurité
- déploiement
```

---

### Récapitulatif des hooks principaux

| Hook | Moment | Usage typique | Peut bloquer ? |
|------|--------|---------------|----------------|
| `pre-commit` | Avant commit | Lint, tests rapides | Oui |
| `prepare-commit-msg` | Avant édition message | Template, ticket auto | Non |
| `commit-msg` | Après édition message | Valider format | Oui |
| `post-commit` | Après commit | Notifications, logs | Non |
| `pre-push` | Avant push | Tests complets, build | Oui |
| `post-checkout` | Après checkout | Installer dépendances | Non |
| `post-merge` | Après merge | Installer dépendances | Non |

---

### Outils et ressources

#### Outils populaires

- **Husky** : Gestionnaire de hooks Git moderne
- **lint-staged** : Lint uniquement les fichiers modifiés
- **commitlint** : Valider les messages de commit
- **pre-commit** (Python) : Framework de hooks multi-langages
- **lefthook** : Alternative à Husky, plus rapide

#### Templates de hooks

GitHub regorge de templates de hooks prêts à l'emploi :
- Recherchez "git hooks templates"
- Recherchez "pre-commit hooks examples"

---

### Configuration complète d'un projet moderne

Voici un setup complet avec Husky + lint-staged + commitlint :

**package.json :**

```json
{
  "scripts": {
    "prepare": "husky",
    "lint": "eslint .",
    "test": "jest"
  },
  "devDependencies": {
    "husky": "^9.0.0",
    "lint-staged": "^15.0.0",
    "@commitlint/cli": "^18.0.0",
    "@commitlint/config-conventional": "^18.0.0",
    "eslint": "^8.0.0",
    "prettier": "^3.0.0"
  },
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{json,md,css}": [
      "prettier --write"
    ]
  }
}
```

**commitlint.config.js :**

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

**Hooks Husky :**

```bash
# Installation
npm install
npx husky init

# Créer les hooks
npx husky add .husky/pre-commit "npx lint-staged"
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

**Résultat :** Projet professionnel avec hooks automatiques ! 🎉

---

### Conclusion

Les **hooks Git** sont un outil puissant pour automatiser votre workflow et maintenir la qualité du code. Ils permettent de :

**Points clés à retenir :**

- Les hooks sont des scripts qui s'exécutent automatiquement lors d'événements Git
- Les plus utiles : `pre-commit`, `commit-msg`, `pre-push`
- Les hooks peuvent bloquer une action (commit, push)
- Utilisez Husky pour partager les hooks avec l'équipe
- Combinez avec lint-staged pour de meilleures performances
- Gardez les hooks rapides (< 30 secondes)
- Les hooks complètent la CI/CD, ne la remplacent pas

**Setup recommandé pour démarrer :**

```bash
# 1. Installer Husky
npm install --save-dev husky
npx husky init

# 2. Ajouter un hook pre-commit simple
npx husky add .husky/pre-commit "npm test"

# 3. Tester
git commit -m "test"
```

**Pour aller plus loin :**

- Ajoutez lint-staged pour optimiser
- Configurez commitlint pour des messages standardisés
- Documentez vos hooks pour l'équipe
- Surveillez les performances des hooks

Avec des hooks bien configurés, vous éviterez de nombreuses erreurs et maintiendrez un code de qualité automatiquement ! 🚀

---

**Prochaine étape :** Module 6.10 - Alias Git pour la productivité

⏭️ [Alias Git pour la productivité](/module-06-fonctions-avancees/10-alias-git.md)
