🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 3 : Corriger et modifier

## Introduction au Module 3

Bienvenue dans le Module 3 de la formation Git ! Après avoir appris les concepts fondamentaux et les opérations de base, vous allez maintenant découvrir comment corriger vos erreurs et modifier votre travail dans Git.

### Pourquoi ce module est essentiel

En tant que développeur, vous allez faire des erreurs. C'est inévitable et complètement normal ! Vous allez :

- Faire des commits trop tôt ou trop tard
- Oublier d'ajouter des fichiers à un commit
- Écrire des messages de commit peu clairs
- Commiter du code qui ne fonctionne pas
- Supprimer accidentellement des fichiers importants
- Vouloir annuler des modifications

**La bonne nouvelle ?** Git a été conçu avec l'idée que les développeurs font des erreurs. Il offre des outils puissants pour corriger pratiquement n'importe quelle situation. Ce module vous apprendra à utiliser ces outils en toute confiance.

---

### Philosophie de Git : L'histoire est flexible (jusqu'à un certain point)

Git vous permet de réécrire l'histoire de votre projet, mais avec une règle d'or importante :

> **Règle d'or de Git :** Vous pouvez modifier l'historique **local** tant que vous voulez, mais une fois que vous avez partagé vos commits avec d'autres (via `git push`), vous devez être beaucoup plus prudent.

**Analogie :** C'est comme écrire un brouillon vs publier un livre. Vous pouvez modifier votre brouillon autant que vous le souhaitez, mais une fois le livre publié et entre les mains des lecteurs, vous ne pouvez plus le modifier (ou seulement avec beaucoup de précautions).

---

### Les trois zones de Git : Un rappel essentiel

Pour bien comprendre les commandes de correction, il est crucial de se rappeler les trois zones de Git :

```
┌─────────────────────────────────────────┐
│     Working Directory                   │
│     (Répertoire de travail)             │
│                                         │
│     Vos fichiers tels que vous les      │
│     voyez dans votre éditeur            │
└─────────────────────────────────────────┘
                    ↕ git add
┌─────────────────────────────────────────┐
│     Staging Area (Index)                │
│                                         │
│     Zone de préparation des fichiers    │
│     pour le prochain commit             │
└─────────────────────────────────────────┘
                    ↕ git commit
┌─────────────────────────────────────────┐
│     Repository (.git)                   │
│                                         │
│     Historique permanent des commits    │
│     HEAD → pointe vers le commit actuel │
└─────────────────────────────────────────┘
```

**Chaque commande de correction agit différemment sur ces trois zones.** Comprendre sur quelle zone une commande agit vous aidera à choisir la bonne commande pour chaque situation.

---

### Vue d'ensemble du module

Ce module couvre cinq aspects essentiels de la correction et de la modification dans Git :

#### 1. Modifier le dernier commit (`git commit --amend`)
- Corriger le message du dernier commit
- Ajouter des fichiers oubliés au dernier commit
- Modifier le contenu du dernier commit

**Quand l'utiliser :** Vous venez de faire un commit et vous réalisez immédiatement une erreur mineure.

#### 2. Annuler des modifications (`git restore`, `git checkout`)
- Annuler des modifications dans le Working Directory
- Retirer des fichiers de la Staging Area
- Restaurer des fichiers à leur état du dernier commit

**Quand l'utiliser :** Vous avez fait des modifications que vous voulez abandonner, avant ou après avoir fait `git add`.

#### 3. Comprendre `git reset` (`--soft`, `--mixed`, `--hard`)
- Déplacer la branche actuelle vers un commit antérieur
- Comprendre les trois modes et leur impact sur chaque zone
- Savoir quand utiliser chaque option

**Quand l'utiliser :** Vous voulez "revenir en arrière" dans l'historique des commits (uniquement pour les commits locaux).

#### 4. Annuler un commit publié (`git revert`)
- Créer un nouveau commit qui annule un commit précédent
- Annuler en toute sécurité sans réécrire l'historique
- Gérer les conflits lors d'un revert

**Quand l'utiliser :** Vous devez annuler un commit qui a déjà été partagé avec l'équipe.

#### 5. Récupérer des fichiers d'anciennes versions
- Explorer l'historique pour trouver une version spécifique
- Récupérer des fichiers ou du code supprimé
- Comparer différentes versions

**Quand l'utiliser :** Vous avez besoin de récupérer quelque chose d'une version antérieure sans affecter le reste du projet.

---

### Différence entre correction "sûre" et "dangereuse"

#### Commandes sûres (n'affectent pas l'historique public)
- `git revert` : crée un nouveau commit d'annulation
- `git restore` : ne touche que les fichiers non commités

**Ces commandes peuvent être utilisées à tout moment**, même après avoir partagé vos commits.

#### Commandes qui réécrivent l'historique (dangereuses si public)
- `git commit --amend` : modifie le dernier commit
- `git reset` : déplace HEAD et peut supprimer des commits

**Ces commandes ne doivent être utilisées que sur des commits locaux** qui n'ont pas encore été partagés.

---

### Matrice de décision rapide

Voici un guide rapide pour choisir la bonne commande selon votre situation :

| Situation | Le commit est local ? | Commande à utiliser |
|-----------|----------------------|---------------------|
| Corriger le dernier commit | ✅ Oui | `git commit --amend` |
| Corriger le dernier commit | ❌ Non (déjà pushé) | `git revert HEAD` |
| Annuler modifications non commitées | N/A | `git restore` |
| Revenir plusieurs commits en arrière | ✅ Oui | `git reset` |
| Revenir plusieurs commits en arrière | ❌ Non (déjà pushé) | `git revert` (plusieurs fois) |
| Récupérer un fichier ancien | N/A | `git restore --source` |
| Retirer de la staging area | N/A | `git restore --staged` |

---

### Concepts importants à comprendre

#### HEAD : Votre position actuelle

`HEAD` est un pointeur qui indique où vous êtes dans l'historique de Git. La plupart du temps, HEAD pointe sur le dernier commit de votre branche actuelle.

```
Commits : A ← B ← C ← D
                      ↑
                    HEAD (main)
```

**Références relatives à HEAD :**
- `HEAD` : le commit actuel
- `HEAD~1` ou `HEAD^` : le commit précédent
- `HEAD~2` : deux commits en arrière
- `HEAD~5` : cinq commits en arrière

#### Identifiants de commit (hash SHA)

Chaque commit a un identifiant unique (hash) qui ressemble à : `a1b2c3d4e5f6...`

```bash
# Voir les identifiants
git log --oneline
# a1b2c3d (HEAD -> main) Dernier commit
# c3d4e5f Avant-dernier commit
# h7i8j9k Encore avant
```

Vous pouvez référencer un commit par :
- Son hash complet : `a1b2c3d4e5f6789...`
- Son hash court : `a1b2c3d` (les 7 premiers caractères suffisent généralement)
- Une référence relative : `HEAD~2`
- Un nom de branche : `main`, `develop`
- Un tag : `v1.0.0`

#### L'importance du reflog

Git conserve un journal de tous les déplacements de HEAD dans le **reflog** (reference log). C'est votre filet de sécurité !

Même si vous faites une erreur grave (comme un `git reset --hard` accidentel), vous pouvez souvent récupérer votre travail grâce au reflog.

```bash
# Voir l'historique des déplacements de HEAD
git reflog
```

Nous verrons comment l'utiliser en détail plus tard dans ce module.

---

### Précautions et bonnes pratiques

#### Avant de modifier quoi que ce soit

1. **Vérifiez votre statut :**
   ```bash
   git status
   ```

2. **Consultez l'historique :**
   ```bash
   git log --oneline -10
   ```

3. **Faites une sauvegarde si nécessaire :**
   ```bash
   git branch sauvegarde-avant-modif
   ```

#### Pendant les modifications

1. **Lisez attentivement les messages d'erreur** - Git explique souvent ce qui ne va pas
2. **Testez après chaque modification** - Assurez-vous que tout fonctionne
3. **Commitez régulièrement** - Un commit = un point de sauvegarde

#### La règle d'or (répétition volontaire)

> **Ne modifiez JAMAIS l'historique qui a été partagé avec d'autres !**

Si vous avez déjà fait `git push`, utilisez des commandes sûres comme `git revert` au lieu de réécrire l'historique avec `git reset` ou `git commit --amend`.

---

### Mentalité à adopter

#### Les erreurs font partie du processus

Git a été créé pour gérer les erreurs. Aucun développeur, même les plus expérimentés, ne travaille parfaitement du premier coup. Ce module vous donne les outils pour :

- Expérimenter sans crainte
- Corriger rapidement vos erreurs
- Maintenir un historique propre et compréhensible
- Collaborer efficacement avec votre équipe

#### Pratiquer dans un environnement sûr

N'ayez pas peur de tester ces commandes ! Le meilleur moyen d'apprendre est de pratiquer. Vous pouvez :

1. Créer un projet de test pour expérimenter
2. Faire des commits "jetables" pour tester
3. Utiliser des branches pour isoler vos expérimentations

```bash
# Créer un dépôt de test
mkdir git-test  
cd git-test  
git init

# Créer quelques fichiers et commits pour expérimenter
echo "Test 1" > fichier1.txt  
git add fichier1.txt  
git commit -m "Commit 1"

echo "Test 2" > fichier2.txt  
git add fichier2.txt  
git commit -m "Commit 2"

# Maintenant vous pouvez tester toutes les commandes !
```

---

### Structure de ce module

Chaque section de ce module suivra cette structure :

1. **Introduction** : Qu'est-ce que cette commande fait ?
2. **Cas d'usage** : Quand l'utiliser ?
3. **Syntaxe et options** : Comment l'utiliser ?
4. **Exemples pratiques** : Des cas concrets du quotidien
5. **Avertissements** : Les pièges à éviter
6. **Bonnes pratiques** : Comment l'utiliser efficacement

---

### Commandes que vous allez maîtriser

À la fin de ce module, vous saurez utiliser avec confiance :

```bash
# Modification du dernier commit
git commit --amend

# Annulation de modifications
git restore <fichier>  
git restore --staged <fichier>  
git checkout -- <fichier>

# Reset (avec précaution)
git reset --soft <commit>  
git reset --mixed <commit>  
git reset --hard <commit>

# Revert (sûr)
git revert <commit>

# Récupération de fichiers
git restore --source=<commit> <fichier>  
git checkout <commit> -- <fichier>  
git show <commit>:<fichier>

# Exploration
git log -- <fichier>  
git log -S "texte"  
git reflog  
git diff <commit1> <commit2>
```

---

### Prêt à commencer ?

Vous avez maintenant une vue d'ensemble de ce que vous allez apprendre dans ce module. Les concepts peuvent sembler nombreux, mais chaque commande résout un problème spécifique et simple.

**Rappelez-vous :**
- Git est votre allié, pas votre ennemi
- Chaque erreur a une solution
- La pratique rend ces commandes naturelles
- Le reflog est votre filet de sécurité

Commençons par la commande la plus simple et la plus utilisée : `git commit --amend`, qui permet de corriger rapidement le dernier commit que vous venez de faire.

---

⏭️ [Modifier le dernier commit (git commit --amend)](/module-03-corriger-et-modifier/01-modifier-le-dernier-commit.md)
