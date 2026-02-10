🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches

## Introduction au Module 4

Bienvenue dans le Module 4 de la formation Git ! Si les modules précédents vous ont appris les bases et la correction, ce module va transformer votre façon de travailler avec Git. Les branches sont la fonctionnalité qui fait de Git un outil **révolutionnaire** pour le développement logiciel.

### Pourquoi ce module est crucial

Les branches sont au cœur de la puissance de Git. Elles permettent :

- **Le développement parallèle** : Travailler sur plusieurs fonctionnalités simultanément
- **L'isolation du code** : Expérimenter sans casser le code stable
- **La collaboration efficace** : Chaque développeur peut travailler indépendamment
- **L'organisation du travail** : Séparer les features, les corrections, les expérimentations
- **La sécurité** : Le code principal reste toujours fonctionnel

**Sans les branches**, Git serait juste un système de sauvegarde sophistiqué. **Avec les branches**, Git devient un outil de collaboration et d'organisation indispensable.

---

### La révolution des branches dans Git

#### Avant Git : Les branches étaient un cauchemar

Dans les anciens systèmes de contrôle de version (SVN, CVS), créer une branche signifiait :

- **Copier physiquement** tous les fichiers du projet
- **Attendre longtemps** (plusieurs minutes sur gros projets)
- **Consommer beaucoup d'espace disque**
- **Avoir peur** de créer des branches par crainte de la complexité

**Résultat :** Les développeurs évitaient de créer des branches et travaillaient tous sur le même code, causant conflits et bugs.

#### Avec Git : Les branches sont instantanées et légères

Dans Git, une branche est simplement un **pointeur** (41 octets) vers un commit. Cela signifie :

- **Création instantanée** : Moins d'une milliseconde
- **Aucun espace supplémentaire** : Pas de copie de fichiers
- **Changement ultra-rapide** : Passer d'une branche à l'autre en une seconde
- **Aucune limite** : Vous pouvez avoir des centaines de branches sans problème

**Résultat :** Git encourage à créer des branches pour tout !

---

### Philosophie Git des branches

Git a été conçu avec une philosophie spécifique concernant les branches :

> **"Branch early, branch often"**
> (Branchez tôt, branchez souvent)

**Analogie :** Les branches dans Git sont comme des brouillons dans l'écriture. Vous ne rédigez pas votre texte final directement. Vous créez des brouillons, vous expérimentez, vous réorganisez, puis vous intégrez le meilleur dans la version finale.

#### Les quatre principes des branches Git

**Principe 1 : Les branches sont jetables**

```bash
git branch experiment-A  # Créer une branche d'expérimentation
# ... ça ne marche pas ...
git branch -d experiment-A  # La supprimer sans regret
```

Créer et supprimer des branches est normal et encouragé.

**Principe 2 : Les branches isolent le travail**

```
main (stable et déployable)
  │
  ├─ feature-A (développement de la fonctionnalité A)
  ├─ feature-B (développement de la fonctionnalité B)
  └─ hotfix-urgent (correction critique)
```

Chaque tâche a sa propre branche. Si l'une casse, les autres continuent de fonctionner.

**Principe 3 : Les branches facilitent la collaboration**

```
Alice travaille sur feature-login  
Bob travaille sur feature-payment  
Claire travaille sur bugfix-header

Tous en parallèle, sans se gêner !
```

Chacun peut avancer à son rythme, puis intégrer son travail quand c'est prêt.

**Principe 4 : Les branches racontent une histoire**

```
main
  │
  ├─ Version 1.0 (release)
  │   ├─ feature-auth (ajout authentification)
  │   └─ feature-profile (ajout profils)
  │
  └─ Version 2.0 (release)
      ├─ feature-payment (ajout paiements)
      └─ refactor-database (refonte BDD)
```

L'historique des branches montre l'évolution logique du projet.

---

### Vue d'ensemble du module

Ce module est divisé en huit sections qui couvrent tout le cycle de vie des branches :

#### 1. Qu'est-ce qu'une branche ?

Comprendre le concept fondamental : qu'est-ce qu'une branche, comment Git les stocke, pourquoi elles sont si rapides.

**Vous apprendrez :**
- La nature technique d'une branche (un pointeur)
- La différence avec d'autres systèmes de contrôle de version
- Les conventions de nommage
- Branches locales vs branches distantes

#### 2. Créer, lister, supprimer des branches

Les opérations de base pour gérer vos branches.

**Vous apprendrez :**
- `git branch` pour créer et lister
- `git checkout -b` et `git switch -c` pour créer et changer
- Supprimer des branches avec `-d` et `-D`
- Gérer les branches locales et distantes

#### 3. Changer de branche (git checkout et git switch)

Naviguer entre vos différentes lignes de développement.

**Vous apprendrez :**
- Basculer d'une branche à l'autre
- Comprendre HEAD et son déplacement
- Gérer les modifications non commitées
- Éviter les erreurs courantes

#### 4. Fusion de branches (git merge)

Combiner le travail de plusieurs branches.

**Vous apprendrez :**
- Le principe du merge
- Les différents types de merge (fast-forward, merge commit)
- Créer et personnaliser des commits de merge
- Quand et comment fusionner

#### 5. Stratégies de merge (fast-forward, merge commit, squash)

Choisir la bonne approche pour fusionner selon le contexte.

**Vous apprendrez :**
- Fast-forward : l'avance rapide
- Merge commit : préserver l'historique complet
- Squash : nettoyer l'historique
- Quand utiliser chaque stratégie

#### 6. Résolution de conflits

Gérer les situations où Git ne peut pas fusionner automatiquement.

**Vous apprendrez :**
- Comprendre pourquoi les conflits surviennent
- Lire et interpréter les marqueurs de conflit
- Résoudre méthodiquement les conflits
- Utiliser des outils pour faciliter la résolution
- Prévenir les conflits

#### 7. Rebasage : le principe de git rebase

Une alternative au merge pour maintenir un historique linéaire.

**Vous apprendrez :**
- Le concept du rebase (rejouer des commits)
- Rebase vs merge : différences visuelles
- Le rebase interactif pour nettoyer l'historique
- Les dangers du rebase

#### 8. Rebase vs Merge : la règle d'or et quand choisir

Maîtriser la décision critique entre ces deux approches.

**Vous apprendrez :**
- La règle d'or du rebase
- Matrices de décision
- Workflows recommandés
- Quand utiliser quoi selon le contexte

---

### Concepts clés à maîtriser

Avant de plonger dans les sections techniques, familiarisez-vous avec ces concepts :

#### HEAD : Votre position actuelle

`HEAD` est un pointeur spécial qui indique **où vous êtes** dans l'historique Git.

```
A ← B ← C ← D
        ↑   ↑
      main  feature
        ↑
      HEAD (vous êtes ici)
```

Quand vous changez de branche, HEAD se déplace.

#### Branches locales vs distantes

**Branches locales** : Sur votre ordinateur uniquement

```bash
git branch
# * main
#   feature-login
#   bugfix-header
```

**Branches distantes** : Sur le serveur (GitHub, GitLab, etc.)

```bash
git branch -r
# origin/main
# origin/develop
# origin/feature-payment
```

#### Fusion (Merge) vs Rebasage (Rebase)

Deux façons de combiner des branches :

**Merge** : Crée un commit de fusion, préserve l'historique complet

```
        ┌─ F ← G ─┐
        │         │
A ← B ← C ← D ← E ← M (merge commit)
```

**Rebase** : Rejoue les commits, crée un historique linéaire

```
A ← B ← C ← D ← E ← F' ← G' (commits rejoués)
```

#### Conflits

Quand deux branches modifient les mêmes lignes différemment, Git ne peut pas choisir automatiquement. C'est un **conflit**, et vous devez le résoudre manuellement.

```javascript
<<<<<<< HEAD
const price = 100;
=======
const price = 80;
>>>>>>> feature-discount
```

---

### Workflow typique avec les branches

Voici comment vous utiliserez les branches au quotidien :

```
1. Partir d'une branche stable (main)
   ↓
2. Créer une branche pour votre tâche
   ↓
3. Développer sur cette branche (commits multiples)
   ↓
4. Synchroniser avec les changements de main (merge ou rebase)
   ↓
5. Tester votre travail
   ↓
6. Fusionner dans main
   ↓
7. Supprimer la branche de travail
   ↓
8. Recommencer pour la prochaine tâche
```

**Exemple concret :**

```bash
# 1. Partir de main
git switch main  
git pull

# 2. Créer une branche
git switch -c feature-dark-mode

# 3. Développer
git commit -m "Ajout variables CSS"  
git commit -m "Ajout toggle"  
git commit -m "Ajout sauvegarde préférence"

# 4. Synchroniser (si main a avancé)
git merge main

# 5. Tester
npm test

# 6. Fusionner dans main
git switch main  
git merge feature-dark-mode

# 7. Supprimer la branche
git branch -d feature-dark-mode

# 8. Pousser
git push origin main
```

---

### Vocabulaire essentiel

Pour bien suivre ce module, familiarisez-vous avec ces termes :

**Branche (Branch)** : Une ligne de développement indépendante, techniquement un pointeur vers un commit.

**HEAD** : Pointeur qui indique sur quelle branche vous êtes actuellement.

**Merge (Fusion)** : Combiner deux branches en créant un commit de merge.

**Rebase (Rebasage)** : Rejouer des commits sur une nouvelle base pour linéariser l'historique.

**Conflit** : Situation où Git ne peut pas fusionner automatiquement et demande votre aide.

**Fast-forward** : Type de merge où le pointeur avance simplement sans créer de commit de merge.

**Remote (Distant)** : Dépôt hébergé sur un serveur (GitHub, GitLab, etc.).

**Upstream** : La branche distante que votre branche locale suit.

**Pull Request (PR) / Merge Request (MR)** : Demande de fusion de code sur GitHub/GitLab.

---

### Mindset pour ce module

#### Les branches ne sont pas dangereuses

Beaucoup de débutants ont peur de créer des branches. **N'ayez pas peur !**

- Créer une branche ne modifie rien au code existant
- Vous pouvez toujours supprimer une branche si vous changez d'avis
- Git garde une trace de tout, même des branches supprimées (reflog)
- Expérimenter sur une branche est sûr : main reste intact

#### Vous allez faire des erreurs, et c'est normal

Ce module couvre des concepts plus avancés. Vous allez :

- Créer des branches au mauvais endroit
- Fusionner dans la mauvaise direction
- Rencontrer des conflits déroutants
- Vous perdre entre les branches

**C'est parfaitement normal !** Chaque développeur est passé par là. L'important est d'apprendre de chaque erreur.

#### La pratique rend parfait

La gestion des branches devient naturelle avec la pratique. Au début, réfléchissez à chaque commande. Après quelques semaines, vous le ferez instinctivement.

**Conseil :** Créez un projet de test pour expérimenter sans risque :

```bash
mkdir git-branch-practice  
cd git-branch-practice  
git init

# Créez des fichiers, des commits, des branches
# Expérimentez sans crainte !
```

---

### Prérequis pour ce module

Avant de commencer, assurez-vous de maîtriser :

✅ **Les commandes de base** : `git add`, `git commit`, `git status`, `git log`

✅ **Les concepts fondamentaux** : Working Directory, Staging Area, Repository

✅ **La correction d'erreurs** : `git restore`, `git reset` (Module 3)

Si ces concepts ne sont pas clairs, prenez le temps de réviser les modules précédents. Une bonne compréhension des bases est essentielle pour maîtriser les branches.

---

### Organisation du module

Ce module suit une progression pédagogique :

**Sections 1-3 : Les bases**
- Comprendre et manipuler les branches
- Opérations quotidiennes de base
- Devenir à l'aise avec la navigation

**Sections 4-6 : L'intégration**
- Combiner le travail de plusieurs branches
- Gérer les conflits inévitables
- Collaborer efficacement

**Sections 7-8 : Techniques avancées**
- Maîtriser le rebase
- Prendre des décisions éclairées
- Adopter les bonnes pratiques

Chaque section s'appuie sur les précédentes. Ne sautez pas d'étapes !

---

### Objectifs d'apprentissage

À la fin de ce module, vous serez capable de :

🎯 Créer, lister et supprimer des branches avec confiance

🎯 Naviguer entre branches sans perdre votre travail

🎯 Fusionner des branches et choisir la bonne stratégie

🎯 Résoudre des conflits méthodiquement

🎯 Utiliser le rebase pour nettoyer l'historique (prudemment)

🎯 Décider entre merge et rebase selon le contexte

🎯 Collaborer efficacement avec une équipe via les branches

🎯 Maintenir un historique Git propre et compréhensible

---

### Conseils pour réussir ce module

#### 1. Visualisez constamment

Utilisez `git log --oneline --graph --all` régulièrement pour voir où vous en êtes.

#### 2. Prenez votre temps

Les branches sont un concept plus abstrait. C'est normal de devoir relire certaines sections.

#### 3. Pratiquez sur un projet de test

Ne pratiquez pas sur votre projet principal tant que vous n'êtes pas à l'aise.

#### 4. Utilisez des outils visuels

GitKraken, Sourcetree, ou l'extension Git Graph pour VS Code peuvent vous aider à visualiser les branches.

#### 5. N'ayez pas peur de demander

La gestion des branches est le sujet où les développeurs ont le plus de questions. C'est normal !

---

### Prêt à démarrer ?

Vous êtes maintenant prêt à découvrir la puissance des branches dans Git. Ce module va transformer votre façon de travailler, en vous donnant la liberté d'expérimenter, de collaborer et d'organiser votre code comme jamais auparavant.

**Rappelez-vous :**
- Les branches sont légères et rapides
- Créer des branches est encouragé, pas dangereux
- Chaque erreur est une opportunité d'apprentissage
- La pratique rend le tout naturel

**Changement de mentalité :**

Avant ce module, vous pensiez peut-être à Git comme un outil de sauvegarde. Après ce module, vous le verrez comme un outil de **collaboration** et d'**organisation** qui structure votre façon de développer.

Commençons par la question fondamentale : **Qu'est-ce qu'une branche ?**

---

⏭️ [Qu'est-ce qu'une branche ?](/module-04-travailler-avec-les-branches/01-quest-ce-quune-branche.md)
