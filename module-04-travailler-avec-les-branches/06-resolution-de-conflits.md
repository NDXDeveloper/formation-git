🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 6. Résolution de conflits

### Introduction

Les conflits sont une partie normale et inévitable du travail collaboratif avec Git. Ils surviennent quand Git ne peut pas fusionner automatiquement deux versions d'un même fichier. Bien que cela puisse sembler intimidant au début, la résolution de conflits est un processus méthodique et prévisible.

**Analogie :** Imaginez deux éditeurs qui corrigent la même phrase d'un livre différemment. L'un écrit "Le chat *noir* dort", l'autre écrit "Le chat *blanc* dort". Un troisième éditeur doit décider quelle version garder, ou trouver une solution alternative. C'est exactement ce que vous faites lors de la résolution de conflits Git.

---

### Qu'est-ce qu'un conflit ?

Un **conflit** se produit quand Git essaie de fusionner deux branches, mais trouve des modifications incompatibles sur les mêmes lignes d'un fichier (ou quand un fichier a été modifié d'un côté et supprimé de l'autre).

**Important :** Un conflit n'est **pas une erreur**. C'est Git qui vous demande de l'aide pour décider comment combiner deux versions différentes.

---

### Pourquoi les conflits se produisent-ils ?

#### Cause principale : Modifications simultanées

```
Branche main :              Branche feature :  
ligne 1                     ligne 1  
ligne 2 → "version A"       ligne 2 → "version B"  ← CONFLIT !  
ligne 3                     ligne 3
```

Deux développeurs (ou vous-même sur deux branches) ont modifié la même ligne différemment.

#### Situations courantes de conflit

**Situation 1 : Modifications concurrentes**

```bash
# Alice sur main
echo "Prix: 100€" > prix.txt  
git commit -am "Fixe prix à 100"

# Bob sur feature-promotion
echo "Prix: 80€" > prix.txt  
git commit -am "Prix promotionnel"

# Lors du merge → CONFLIT
```

**Situation 2 : Fichier modifié vs supprimé**

```bash
# Sur main : modification
echo "Nouveau contenu" >> fichier.txt  
git commit -am "Update fichier"

# Sur feature : suppression
git rm fichier.txt  
git commit -m "Delete fichier"

# Lors du merge → CONFLIT
```

**Situation 3 : Modifications proches (parfois)**

```bash
# Même si les lignes ne sont pas exactement les mêmes,
# Git peut parfois avoir du mal à fusionner automatiquement
```

---

### Détecter un conflit

#### Message de Git lors d'un merge

```bash
git merge feature-payment
# Auto-merging payment.js
# CONFLICT (content): Merge conflict in payment.js
# Automatic merge failed; fix conflicts and then commit the result.
```

Git vous indique clairement :
- Qu'un conflit existe
- Dans quel(s) fichier(s)
- Que vous devez le résoudre avant de continuer

#### Vérifier l'état avec git status

```bash
git status
# On branch main
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#   (use "git merge --abort" to abort the merge)
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#       both modified:   payment.js
#       both modified:   config.json
#
# no changes added to commit
```

**Interprétation :**
- `both modified` : Les deux branches ont modifié ce fichier
- `You have unmerged paths` : Des conflits attendent d'être résolus
- Vous êtes "au milieu" d'un merge

---

### Anatomie d'un conflit

Quand Git rencontre un conflit, il marque le fichier avec des **marqueurs de conflit** spéciaux.

#### Exemple de fichier en conflit

```javascript
function calculatePrice(item) {
  const basePrice = item.price;

<<<<<<< HEAD
  // Version de la branche actuelle (main)
  const discount = 0.10; // 10% de réduction
  return basePrice * (1 - discount);
=======
  // Version de la branche à merger (feature-promotion)
  const discount = 0.20; // 20% de réduction
  const tax = 0.05;      // 5% de taxe
  return basePrice * (1 - discount) * (1 + tax);
>>>>>>> feature-promotion

  console.log("Prix calculé");
}
```

#### Explication des marqueurs

```
<<<<<<< HEAD
  Votre version (branche actuelle)
=======
  Leur version (branche à merger)
>>>>>>> nom-de-la-branche
```

**Détail des marqueurs :**

- **`<<<<<<< HEAD`** : Début du conflit, début de votre version
- **`=======`** : Séparation entre les deux versions
- **`>>>>>>> feature-promotion`** : Fin du conflit, fin de leur version

**Important :** Ces marqueurs ne sont **PAS** du code valide. Vous devez les supprimer après résolution !

---

### Les trois étapes de résolution

La résolution de conflits suit toujours ce processus en trois étapes :

#### Étape 1 : Identifier les fichiers en conflit

```bash
git status
```

Ou utiliser un outil visuel pour voir la liste des fichiers.

#### Étape 2 : Résoudre chaque conflit

Pour chaque fichier en conflit :

1. Ouvrir le fichier
2. Chercher les marqueurs `<<<<<<<`
3. Décider quelle version garder (ou créer une nouvelle version)
4. Supprimer les marqueurs de conflit
5. Sauvegarder le fichier

#### Étape 3 : Marquer comme résolu et commiter

```bash
# Marquer chaque fichier résolu
git add fichier-resolu.js

# Vérifier que tout est résolu
git status

# Finaliser le merge
git commit
```

---

### Résoudre un conflit : Guide pas à pas

#### Exemple complet de résolution

**Situation initiale : Conflit dans `index.html`**

```bash
git merge feature-redesign
# CONFLICT (content): Merge conflict in index.html
```

**1. Vérifier l'état**

```bash
git status
# Unmerged paths:
#   both modified:   index.html
```

**2. Ouvrir le fichier `index.html`**

```html
<!DOCTYPE html>
<html>
<head>
<<<<<<< HEAD
  <title>Mon Site - Version Stable</title>
  <link rel="stylesheet" href="style.css">
=======
  <title>Mon Site - Nouveau Design</title>
  <link rel="stylesheet" href="new-style.css">
  <link rel="stylesheet" href="animations.css">
>>>>>>> feature-redesign
</head>
<body>
  <h1>Bienvenue</h1>
</body>
</html>
```

**3. Analyser les deux versions**

- **Version HEAD (main)** : Titre ancien, un seul CSS
- **Version feature-redesign** : Nouveau titre, deux CSS

**4. Décider de la résolution**

Vous avez plusieurs options :

**Option A : Garder la version de HEAD (main)**

```html
<head>
  <title>Mon Site - Version Stable</title>
  <link rel="stylesheet" href="style.css">
</head>
```

**Option B : Garder la version de feature-redesign**

```html
<head>
  <title>Mon Site - Nouveau Design</title>
  <link rel="stylesheet" href="new-style.css">
  <link rel="stylesheet" href="animations.css">
</head>
```

**Option C : Combiner les deux (recommandé ici)**

```html
<head>
  <title>Mon Site - Nouveau Design</title>
  <link rel="stylesheet" href="new-style.css">
  <link rel="stylesheet" href="animations.css">
  <link rel="stylesheet" href="style.css">
</head>
```

**Option D : Créer quelque chose de nouveau**

```html
<head>
  <title>Mon Site - Édition Premium</title>
  <link rel="stylesheet" href="combined-style.css">
</head>
```

**5. Éditer le fichier avec la résolution choisie**

```html
<!DOCTYPE html>
<html>
<head>
  <title>Mon Site - Nouveau Design</title>
  <link rel="stylesheet" href="new-style.css">
  <link rel="stylesheet" href="animations.css">
</head>
<body>
  <h1>Bienvenue</h1>
</body>
</html>
```

**Vérifiez bien :** Les marqueurs `<<<<<<<`, `=======`, `>>>>>>>` sont supprimés !

**6. Marquer comme résolu**

```bash
git add index.html
```

**7. Tester le code**

```bash
# Ouvrir le fichier dans le navigateur
# Vérifier que tout fonctionne
npm test  # Lancer les tests
```

**8. Finaliser le merge**

```bash
git commit
```

Git ouvre un éditeur avec un message pré-rempli :

```
Merge branch 'feature-redesign'

# Conflicts:
#       index.html
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.
```

Vous pouvez personnaliser ce message :

```
Merge branch 'feature-redesign'

Résolution du conflit dans index.html :
- Conservé le nouveau titre
- Conservé tous les CSS (ancien et nouveaux)
- Testé sur Chrome, Firefox, Safari

Closes #123
```

**9. Vérifier le résultat**

```bash
git log --oneline --graph -5
# *   a1b2c3d (HEAD -> main) Merge branch 'feature-redesign'
# |\
# | * e5f6g7h Nouveau design
# * | d4e5f6g Update stable
# |/
# * c3d4e5f Base
```

---

### Stratégies de résolution

#### Stratégie 1 : Garder une version complète

**Quand l'utiliser :** Une des deux versions est clairement meilleure.

```bash
# Garder uniquement la version de HEAD (main)
git checkout --ours fichier.txt  
git add fichier.txt

# Garder uniquement la version de l'autre branche
git checkout --theirs fichier.txt  
git add fichier.txt
```

**Exemple :**

```bash
# Conflit dans config.json
# La version de feature est complètement fausse

git checkout --ours config.json  
git add config.json  
git commit
```

#### Stratégie 2 : Combiner intelligemment

**Quand l'utiliser :** Les deux versions apportent quelque chose.

**Exemple :**

```javascript
// Version main
function process(data) {
  validate(data);
  return transform(data);
}

// Version feature
function process(data) {
  sanitize(data);
  return transform(data);
}

// Résolution combinée
function process(data) {
  sanitize(data);  // De feature
  validate(data);  // De main
  return transform(data);
}
```

#### Stratégie 3 : Réécrire complètement

**Quand l'utiliser :** Aucune des deux versions n'est satisfaisante.

**Exemple :**

```python
# Au lieu de choisir entre deux mauvaises implémentations
# Écrire une nouvelle version meilleure

# Après avoir supprimé les marqueurs de conflit
def calculate_total(items):
    """Nouvelle implémentation optimisée"""
    return sum(item.price * item.quantity for item in items)
```

#### Stratégie 4 : Demander de l'aide

**Quand l'utiliser :** Vous ne comprenez pas le code ou les implications.

```bash
# Annuler temporairement le merge
git merge --abort

# Discuter avec l'équipe
# Comprendre les deux versions
# Recommencer avec plus d'informations
```

---

### Conflits de types spéciaux

#### Type 1 : Fichier modifié vs supprimé

```bash
git merge feature-cleanup
# CONFLICT (modify/delete): fichier.txt deleted in feature-cleanup
# and modified in HEAD.
```

**Résolution :**

```bash
# Option A : Garder le fichier (avec vos modifications)
git add fichier.txt

# Option B : Accepter la suppression
git rm fichier.txt

# Puis commiter
git commit
```

#### Type 2 : Fichier renommé avec modifications

```bash
git merge feature-refactor
# CONFLICT (rename/modify): old-name.txt renamed to new-name.txt
# but modified on both branches.
```

**Résolution :**

```bash
# Éditer le fichier avec le nouveau nom
code new-name.txt
# Résoudre les conflits
git add new-name.txt  
git commit
```

#### Type 3 : Les deux branches ont créé le même fichier

```bash
git merge feature-new-file
# CONFLICT (add/add): Merge conflict in new-file.txt
```

**Résolution :**

Éditer le fichier et combiner les deux versions, puis :

```bash
git add new-file.txt  
git commit
```

---

### Outils de résolution de conflits

#### Outil 1 : Éditeur de texte simple

Le plus basique : ouvrir le fichier dans n'importe quel éditeur.

**Avantages :**
- Fonctionne partout
- Contrôle total
- Pas de dépendance

**Inconvénients :**
- Manuel
- Risque d'oublier des marqueurs
- Pas de visualisation côte à côte

#### Outil 2 : VS Code

VS Code détecte automatiquement les conflits et offre des boutons interactifs.

```
Fichier en conflit dans VS Code :

[Accept Current Change] [Accept Incoming Change] [Accept Both Changes] [Compare Changes]
<<<<<<< HEAD (Current Change)
  const discount = 0.10;
=======
  const discount = 0.20; (Incoming Change)
>>>>>>> feature-promotion
```

**Fonctionnalités :**
- Boutons cliquables pour résolution rapide
- Coloration syntaxique
- Vue côte à côte
- Extension GitLens pour plus de contexte

#### Outil 3 : git mergetool

Lance un outil de merge graphique configuré.

```bash
git mergetool
```

**Outils supportés :**
- **vimdiff** (Linux/Mac)
- **meld** (Linux/Windows/Mac)
- **kdiff3** (Linux/Windows/Mac)
- **p4merge** (Perforce, multi-plateforme)
- **Beyond Compare** (commercial)

**Configuration :**

```bash
# Configurer meld comme outil par défaut
git config --global merge.tool meld

# Lancer lors d'un conflit
git mergetool
```

**Interface typique (3 panneaux) :**

```
┌─────────────┬─────────────┬─────────────┐
│   LOCAL     │    BASE     │   REMOTE    │
│  (votre)    │  (ancêtre)  │   (leur)    │
├─────────────┴─────────────┴─────────────┤
│           MERGED (résultat)             │
│                                         │
└─────────────────────────────────────────┘
```

#### Outil 4 : IDE intégré

**IntelliJ IDEA / WebStorm / PyCharm :**

```
Menu > VCS > Git > Resolve Conflicts
```

Offre une interface graphique sophistiquée avec :
- Vue trois volets
- Navigation entre conflits
- Application automatique de changements non conflictuels

**Exemple :**

```
┌────────────────────────────────────────┐
│  Your Version    │    Their Version    │
├────────────────────────────────────────┤
│  const x = 1;    │    const x = 2;     │
│                  │                     │
│  [<<] [ X ]      │      [>>] [ X ]     │
└────────────────────────────────────────┘
         ↓                    ↓
┌────────────────────────────────────────┐
│           Result (à éditer)            │
│           const x = ?                  │
└────────────────────────────────────────┘
```

#### Outil 5 : GitHub/GitLab Web Interface

Si le conflit est simple, vous pouvez le résoudre directement dans l'interface web de la Pull/Merge Request.

**Avantages :**
- Pas besoin de cloner
- Interface familière
- Rapide pour petits conflits

**Inconvénients :**
- Limité aux conflits simples
- Pas de tests locaux
- Moins de contrôle

---

### Prévenir les conflits

#### Bonne pratique 1 : Synchroniser régulièrement

```bash
# Mettre à jour votre branche avec main régulièrement
git switch feature-votre-branche  
git merge main

# Ou avec rebase
git rebase main
```

**Pourquoi ?** Résoudre les conflits au fur et à mesure est plus facile que tout d'un coup.

#### Bonne pratique 2 : Commits atomiques

Faire des commits petits et focalisés.

```bash
# ✅ Bon
git commit -m "Ajout fonction validateEmail"  
git commit -m "Ajout fonction validatePassword"

# ❌ Mauvais
git commit -m "Ajout toutes les validations + refactor + fix bugs"
```

**Pourquoi ?** Plus facile de comprendre et résoudre les conflits.

#### Bonne pratique 3 : Communication d'équipe

Coordonner qui travaille sur quoi.

```bash
# Si Alice et Bob doivent modifier auth.js
# Alice : "Je travaille sur auth.js ce matin"
# Bob : "OK, je le fais cet après-midi"
```

**Pourquoi ?** Éviter les modifications concurrentes.

#### Bonne pratique 4 : Branches courtes

Ne pas laisser une branche vivre trop longtemps.

```bash
# ✅ Bon : Feature développée en 2-3 jours, mergée
# ❌ Mauvais : Feature qui vit 2 mois sans sync avec main
```

**Pourquoi ?** Plus la branche est longue, plus les conflits s'accumulent.

#### Bonne pratique 5 : Architecture modulaire

Organiser le code pour minimiser les modifications concurrentes.

```javascript
// Au lieu d'un gros fichier que tout le monde modifie
// utils.js (500 lignes, modifié par 5 personnes)

// Plusieurs petits fichiers
// email-utils.js (50 lignes)
// date-utils.js (50 lignes)
// string-utils.js (50 lignes)
```

---

### Workflow complet de résolution

Voici un workflow professionnel pour gérer les conflits efficacement :

```bash
# 1. Tenter le merge
git switch main  
git merge feature-payment
# CONFLICT!

# 2. Ne pas paniquer, évaluer la situation
git status
# Voir combien de fichiers sont en conflit

# 3. Pour chaque fichier en conflit
# Option A : Résoudre manuellement
code src/payment.js
# Éditer, résoudre, sauvegarder
git add src/payment.js

# Option B : Utiliser un outil graphique
git mergetool

# Option C : Accepter une version complète
git checkout --ours src/config.json  
git add src/config.json

# 4. Vérifier qu'il ne reste plus de conflits
git status
# All conflicts fixed but you are still merging.

# 5. Chercher les marqueurs oubliés
grep -r "<<<<<<" .  
grep -r ">>>>>>>" .
# (ne devrait rien retourner)

# 6. Tester le code
npm test  
npm run lint  
npm start
# Tester manuellement les fonctionnalités affectées

# 7. Commiter la résolution
git commit

# 8. Vérifier le résultat
git log --oneline --graph -3

# 9. Pousser
git push origin main
```

---

### Cas pratiques détaillés

#### Cas 1 : Conflit simple sur une ligne

**Situation :**

```python
# Sur main
def greet(name):
    return f"Hello {name}!"

# Sur feature
def greet(name):
    return f"Bonjour {name} !"
```

**Conflit :**

```python
def greet(name):
<<<<<<< HEAD
    return f"Hello {name}!"
=======
    return f"Bonjour {name} !"
>>>>>>> feature-french
```

**Résolution : Ajouter un paramètre**

```python
def greet(name, language="en"):
    if language == "fr":
        return f"Bonjour {name} !"
    return f"Hello {name}!"
```

#### Cas 2 : Conflit avec plusieurs sections

**Situation :**

```javascript
// Conflit dans plusieurs endroits du même fichier
class User {
<<<<<<< HEAD
  constructor(name, age) {
    this.name = name;
    this.age = age;
=======
  constructor(name, email) {
    this.name = name;
    this.email = email;
>>>>>>> feature-email
  }

  getInfo() {
<<<<<<< HEAD
    return `${this.name}, ${this.age} ans`;
=======
    return `${this.name} (${this.email})`;
>>>>>>> feature-email
  }
}
```

**Résolution : Combiner les attributs**

```javascript
class User {
  constructor(name, age, email) {
    this.name = name;
    this.age = age;
    this.email = email;
  }

  getInfo() {
    return `${this.name}, ${this.age} ans (${this.email})`;
  }
}
```

#### Cas 3 : Conflit dans un fichier JSON

**Situation :**

```json
{
  "name": "mon-app",
<<<<<<< HEAD
  "version": "1.0.0",
  "description": "Application web"
=======
  "version": "1.1.0",
  "description": "Application web moderne",
  "author": "Jean Dupont"
>>>>>>> feature-update
}
```

**Résolution :**

```json
{
  "name": "mon-app",
  "version": "1.1.0",
  "description": "Application web moderne",
  "author": "Jean Dupont"
}
```

**Important :** Vérifier que le JSON est valide après résolution !

```bash
# Valider le JSON
cat package.json | jq .
```

#### Cas 4 : Conflit complexe nécessitant refactoring

**Situation :**

```javascript
// Les deux branches ont réorganisé le code différemment
// Version main : une fonction
// Version feature : deux fonctions séparées
```

**Résolution :**

```bash
# Analyser les deux approches
git show :1:fichier.js > base.js  
git show :2:fichier.js > ours.js  
git show :3:fichier.js > theirs.js

# Comprendre les intentions
# Créer une nouvelle version qui combine les meilleures idées

# Parfois il faut réécrire complètement
```

---

### Erreurs courantes et solutions

#### Erreur 1 : Oublier les marqueurs de conflit

```bash
git add fichier.js  
git commit
# Le code contient toujours <<<<<<<

# Résultat : Application cassée en production !
```

**Prévention :**

```bash
# Avant de commiter, chercher les marqueurs
grep -r "<<<<<<" .  
grep -r ">>>>>>>" .

# Configurer un pre-commit hook
# .git/hooks/pre-commit
if grep -r "<<<<<<" .; then
    echo "Conflit markers found!"
    exit 1
fi
```

#### Erreur 2 : Résoudre sans tester

```bash
# Résoudre le conflit rapidement
git add .  
git commit  
git push

# Plus tard : les tests échouent ou l'app crash
```

**Solution :**

```bash
# Toujours tester après résolution
npm test  
npm run build  
npm start
# Vérifier manuellement
```

#### Erreur 3 : Choisir la mauvaise version

```bash
# Accepter "theirs" par erreur
git checkout --theirs fichier-important.js
# Oups, on voulait "ours" !

git checkout --ours fichier-important.js  
git add fichier-important.js
```

#### Erreur 4 : Perdre du code pendant la résolution

```bash
# Dans la précipitation, on supprime du code important
```

**Prévention :**

```bash
# Avant de résoudre des conflits compliqués
git stash  # Sauvegarder le working directory
# Ou créer une branche de backup
git branch backup-avant-resolution
```

**Récupération :**

```bash
# Voir l'ancien contenu
git show :2:fichier.js  # Version de HEAD  
git show :3:fichier.js  # Version de l'autre branche
```

#### Erreur 5 : Abandonner à moitié

```bash
# Commencer à résoudre les conflits
git add fichier1.js
# S'arrêter au milieu sans finir
# Des heures/jours plus tard...
git status
# You have unmerged paths. (confusion totale)
```

**Solution :**

```bash
# Soit finir la résolution
# Soit abandonner complètement
git merge --abort
```

---

### Commandes utiles pendant la résolution

```bash
# DIAGNOSTIC
git status                           # État du merge  
git diff                             # Voir les conflits  
git diff --name-only --diff-filter=U # Lister fichiers en conflit

# VISUALISATION
git show :1:fichier.js               # Version de base (ancêtre commun)  
git show :2:fichier.js               # Version de HEAD (votre branche)  
git show :3:fichier.js               # Version de l'autre branche

# RÉSOLUTION RAPIDE
git checkout --ours fichier.js       # Garder votre version  
git checkout --theirs fichier.js     # Garder leur version  
git mergetool                        # Lancer outil graphique

# ANNULATION
git merge --abort                    # Annuler le merge  
git reset --hard HEAD                # Réinitialiser tout

# FINALISATION
git add fichier.js                   # Marquer comme résolu  
git commit                           # Finaliser le merge
```

---

### Bonnes pratiques de résolution

#### 1. Comprendre avant de résoudre

```bash
# Ne pas résoudre machinalement
# Comprendre :
# - Pourquoi le conflit existe
# - Ce que chaque version essaie de faire
# - Les implications de chaque choix

# Utiliser git log pour le contexte
git log --oneline feature-branch  
git log --oneline main
```

#### 2. Documenter la résolution

```bash
# Dans le message de commit
git commit -m "Merge branch 'feature-payment'

Résolution des conflits :
- payment.js : combiné les deux systèmes de validation
- config.json : gardé la version de feature (plus à jour)
- style.css : conservé les deux styles (pas incompatibles)

Tests réussis : npm test (100% pass)  
Testé manuellement : paiements Visa, Mastercard, PayPal"
```

#### 3. Tester exhaustivement

```bash
# Tests automatiques
npm test  
npm run lint  
npm run build

# Tests manuels
# - Scénario A : test du cas nominal
# - Scénario B : test des edge cases
# - Scénario C : test de régression
```

#### 4. Demander une revue

```bash
# Pour les conflits complexes
git push origin main
# Créer une PR/MR pour revue
# Demander à un pair de vérifier la résolution
```

#### 5. Apprendre de chaque conflit

```bash
# Après résolution, se demander :
# - Pourquoi ce conflit est arrivé ?
# - Comment l'éviter la prochaine fois ?
# - Faut-il améliorer l'architecture du code ?
```

---

### Aide-mémoire de résolution

```bash
# DÉTECTER
git merge branche           # Conflit apparaît  
git status                  # Voir les fichiers en conflit

# ANALYSER
code fichier.js            # Ouvrir et voir les marqueurs  
git diff fichier.js        # Voir les différences

# RÉSOUDRE
# Éditer manuellement et supprimer les marqueurs
# OU
git checkout --ours file   # Garder votre version  
git checkout --theirs file # Garder leur version  
git mergetool              # Outil graphique

# VÉRIFIER
grep -r "<<<<<<" .         # Chercher marqueurs oubliés  
npm test                   # Tester le code

# FINALISER
git add fichier.js         # Marquer résolu  
git commit                 # Commiter le merge

# ANNULER (si nécessaire)
git merge --abort          # Tout annuler
```

---

### Points clés à retenir

✅ **Les conflits sont normaux** : Ne pas paniquer, c'est un processus standard

✅ **Trois marqueurs** : `<<<<<<<`, `=======`, `>>>>>>>`

✅ **Toujours supprimer les marqueurs** avant de commiter

✅ **Tester après résolution** : Tests automatiques et manuels

✅ **`git merge --abort`** pour tout annuler si nécessaire

✅ **Plusieurs stratégies** : garder une version, combiner, réécrire

✅ **Outils disponibles** : éditeur simple, VS Code, mergetool, IDE

✅ **Prévenir vaut mieux** : sync régulier, commits atomiques, communication

---

### Conclusion

La résolution de conflits peut sembler difficile au début, mais avec la pratique, elle devient une opération de routine. Les conflits ne sont pas des erreurs, mais des situations où Git vous demande de prendre des décisions que seul un humain peut prendre.

**Mentalité à adopter :**
- Les conflits sont une opportunité de comprendre le code
- Chaque conflit résolu améliore votre compréhension du projet
- C'est un processus méthodique, pas une crise
- Les outils sont là pour vous aider

**Workflow recommandé :**
1. Ne pas paniquer
2. Comprendre pourquoi le conflit existe
3. Choisir la meilleure résolution
4. Tester exhaustivement
5. Documenter la décision

Avec l'expérience, vous développerez une intuition pour résoudre les conflits rapidement et correctement. Et rappelez-vous : même les développeurs seniors rencontrent des conflits tous les jours !

Dans la prochaine section, nous verrons **le rebasage (git rebase)**, une technique avancée pour maintenir un historique propre et linéaire, en alternative au merge traditionnel.

⏭️ [Rebasage : le principe de git rebase](/module-04-travailler-avec-les-branches/07-rebasage-principe.md)
