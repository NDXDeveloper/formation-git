🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 1. Qu'est-ce qu'une branche ?

### Introduction

Les branches sont l'une des fonctionnalités les plus puissantes et les plus utilisées de Git. Elles permettent de travailler sur différentes versions de votre projet en parallèle, sans perturber le code principal. Comprendre les branches est essentiel pour collaborer efficacement et organiser votre travail.

**Analogie :** Imaginez que vous écrivez un roman. La branche principale (`main`) contient votre histoire officielle. Mais vous voulez expérimenter une fin alternative sans toucher à l'original. Vous créez alors une "branche" où vous écrivez cette fin alternative. Si elle vous plaît, vous pouvez la fusionner avec l'histoire principale. Sinon, vous la supprimez sans avoir abîmé votre travail original.

---

### Définition simple

Une **branche** dans Git est un pointeur mobile vers un commit. Elle représente une ligne de développement indépendante qui diverge à partir d'un point donné dans l'historique.

**Encore plus simple :** Une branche est comme un signet qui marque où vous en êtes dans votre travail. Vous pouvez avoir plusieurs signets (branches) à différents endroits de votre projet.

---

### Visualisation d'une branche

#### Historique linéaire (une seule branche)

Quand vous démarrez un projet Git, vous avez une seule branche par défaut, généralement appelée `main` (ou `master` dans les anciennes versions).

```
A ← B ← C ← D ← E
                ↑
              main (HEAD)
```

Chaque lettre représente un commit. La branche `main` pointe sur le dernier commit `E`, et `HEAD` indique que c'est votre position actuelle.

#### Création d'une nouvelle branche

Quand vous créez une nouvelle branche, vous créez un nouveau pointeur à partir de votre position actuelle :

```
A ← B ← C ← D ← E
                ↑
              main
              feature (HEAD)
```

Ici, `feature` est une nouvelle branche qui pointe sur le même commit que `main`. `HEAD` pointe maintenant sur `feature`, ce qui signifie que vous êtes sur cette branche.

#### Développement sur deux branches parallèles

Lorsque vous faites des commits sur la branche `feature`, elle avance indépendamment de `main` :

```
        ┌─ F ← G ← H
        │           ↑
A ← B ← C ← D ← E  feature (HEAD)
            ↑
          main
```

Les commits `F`, `G`, et `H` n'existent que sur la branche `feature`. La branche `main` est restée sur le commit `E`.

---

### Pourquoi utiliser des branches ?

#### 1. Isoler les nouvelles fonctionnalités

Vous pouvez développer une nouvelle fonctionnalité sans affecter le code stable de la branche principale.

**Exemple :** Vous travaillez sur un site web fonctionnel sur `main`. Vous créez une branche `nouvelle-page-contact` pour ajouter un formulaire de contact. Si vous cassez quelque chose pendant le développement, ça n'affecte pas le site sur `main`.

#### 2. Travailler en parallèle sur plusieurs tâches

Vous pouvez avoir plusieurs branches actives pour différentes tâches :

```
                ┌─ feature-login
                │
A ← B ← C ← D ←─┼─ main
                │
                └─ bug-fix-header
```

Vous pouvez passer d'une branche à l'autre selon les priorités.

#### 3. Expérimenter sans risque

Vous voulez tester une approche radicalement différente ? Créez une branche expérimentale. Si ça ne fonctionne pas, supprimez-la simplement. Le code original reste intact.

#### 4. Collaboration en équipe

Chaque membre de l'équipe peut travailler sur sa propre branche sans interférer avec le travail des autres.

```
                Alice: ┌─ feature-A
                       │
A ← B ← C ← D ← E ← ───┼─ main
                       │
                  Bob: └─ feature-B
```

Alice et Bob travaillent indépendamment, puis fusionnent leurs branches quand le travail est terminé.

---

### La branche par défaut : `main` (ou `master`)

Quand vous initialisez un dépôt Git, une branche par défaut est créée automatiquement :

- **Versions récentes de Git (2020+)** : `main`
- **Anciennes versions** : `master`

Cette branche est considérée comme la branche principale ou "de production" de votre projet. C'est généralement la version stable et fonctionnelle de votre code.

```bash
# Initialiser un nouveau dépôt
git init

# Vérifier la branche actuelle
git branch
# * main
```

**Convention moderne :** La plupart des projets utilisent maintenant `main` comme nom de branche principale.

---

### Comment Git stocke les branches ?

Contrairement à d'autres systèmes de contrôle de version, les branches dans Git sont **extrêmement légères**. Une branche n'est qu'un petit fichier (41 octets) contenant l'identifiant (hash) du commit sur lequel elle pointe.

**Ce que cela signifie pour vous :**
- Créer une branche est **instantané** (même sur un projet énorme)
- Avoir des dizaines ou centaines de branches n'a **aucun impact** sur les performances
- Changer de branche est **très rapide**

**Où sont stockées les branches ?**

```
.git/
  └── refs/
      └── heads/
          ├── main        (fichier contenant le hash du commit)
          ├── feature-A   (fichier contenant le hash du commit)
          └── feature-B   (fichier contenant le hash du commit)
```

Chaque branche est juste un pointeur vers un commit dans l'historique.

---

### HEAD : Votre position actuelle

`HEAD` est un pointeur spécial qui indique sur quelle branche vous êtes actuellement. C'est votre "vous êtes ici" sur la carte de votre projet.

```
A ← B ← C ← D ← E
                ↑
              main ← HEAD
```

`HEAD` pointe généralement vers une branche, qui elle-même pointe vers un commit.

**Quand vous changez de branche :**

```bash
git checkout feature
```

```
        ┌─ F
        │   ↑
A ← B ← C  feature ← HEAD
        ↑
      main
```

`HEAD` se déplace pour pointer vers `feature` au lieu de `main`.

---

### Anatomie d'une branche : Exemple concret

Prenons un exemple réel de développement d'une application web :

#### État initial

```
A ← B ← C
        ↑
      main (HEAD)
      "Site fonctionnel v1.0"
```

Vous avez un site web fonctionnel avec 3 commits.

#### Création d'une branche pour une nouvelle fonctionnalité

```bash
git branch feature-newsletter
git checkout feature-newsletter
```

```
A ← B ← C
        ↑
      main
      feature-newsletter (HEAD)
```

Les deux branches pointent sur le même commit. Vous êtes maintenant sur `feature-newsletter`.

#### Développement sur la branche feature

Vous faites deux commits pour implémenter la newsletter :

```bash
git commit -m "Ajout formulaire newsletter"
git commit -m "Connexion API MailChimp"
```

```
        ┌─ D ← E
        │       ↑
A ← B ← C      feature-newsletter (HEAD)
        ↑      "Newsletter complète"
      main
      "Site v1.0"
```

Votre branche `feature-newsletter` a avancé avec les commits `D` et `E`. La branche `main` est restée sur `C`.

#### Pendant ce temps, correction d'un bug sur main

Vous découvrez un bug critique et devez le corriger sur `main` :

```bash
git checkout main
git commit -m "Fix bug sécurité"
```

```
        ┌─ D ← E
        │       ↑
A ← B ← C      feature-newsletter
        └─ F
           ↑
         main (HEAD)
         "Site v1.0 + fix"
```

Les deux branches ont maintenant divergé :
- `main` a le commit `F` (fix du bug)
- `feature-newsletter` a les commits `D` et `E` (newsletter)

#### Fusion finale

Une fois la newsletter terminée et testée, vous la fusionnez dans `main` :

```bash
git checkout main
git merge feature-newsletter
```

```
        ┌─ D ← E ─┐
        │         │
A ← B ← C ← F ← ─ G
                  ↑
                main (HEAD)
                "v1.1 avec newsletter"
```

Un nouveau commit de fusion `G` est créé, combinant le travail des deux branches.

---

### Types de branches : Conventions de nommage

Bien que vous puissiez nommer vos branches comme vous voulez, il existe des conventions courantes :

#### Branches permanentes

- **`main`** (ou `master`) : Branche principale, code de production
- **`develop`** : Branche de développement, prochaine version en préparation

#### Branches temporaires

- **`feature/nom-fonctionnalite`** : Nouvelle fonctionnalité
  - Exemple : `feature/login`, `feature/dark-mode`

- **`bugfix/description`** ou `fix/description` : Correction de bug
  - Exemple : `bugfix/header-responsive`, `fix/memory-leak`

- **`hotfix/description`** : Correction urgente en production
  - Exemple : `hotfix/security-patch`

- **`release/version`** : Préparation d'une nouvelle version
  - Exemple : `release/v2.0.0`

**Exemple d'organisation :**

```
main (production)
  │
  ├─ develop (développement actif)
  │   │
  │   ├─ feature/user-profile
  │   ├─ feature/payment-integration
  │   └─ bugfix/responsive-menu
  │
  └─ hotfix/critical-security-fix
```

---

### Différences entre Git et d'autres systèmes

#### Dans d'autres systèmes de contrôle de version (SVN, CVS)

Les branches sont souvent :
- **Lourdes** : copier physiquement tous les fichiers
- **Lentes** : prend du temps sur un gros projet
- **Coûteuses** : prend beaucoup d'espace disque

→ Résultat : on évite de créer des branches

#### Dans Git

Les branches sont :
- **Légères** : juste un pointeur de 41 octets
- **Rapides** : création instantanée
- **Gratuites** : aucun impact sur l'espace disque

→ Résultat : on crée des branches pour tout !

**Philosophie Git :** Créez des branches librement, fusionnez-les souvent, supprimez-les sans hésiter.

---

### Branches locales vs branches distantes

#### Branches locales

Ce sont les branches sur votre ordinateur. Vous seul pouvez les voir et les modifier.

```bash
# Lister vos branches locales
git branch
# * main
#   feature-A
#   feature-B
```

#### Branches distantes (remote branches)

Ce sont les branches sur le serveur (GitHub, GitLab, etc.). Toute l'équipe peut les voir.

```bash
# Lister toutes les branches (locales + distantes)
git branch -a
# * main
#   feature-A
#   remotes/origin/main
#   remotes/origin/develop
#   remotes/origin/feature-C
```

Les branches distantes sont préfixées par `origin/` (ou le nom de votre remote).

**Relation :**

```
Votre ordinateur          │  Serveur GitHub
                          │
main ──────────────────→  │  origin/main
feature-A                 │
                          │  origin/develop
```

Vous pouvez :
- **Pousser** (push) une branche locale vers le serveur
- **Récupérer** (pull/fetch) une branche distante en local

---

### Concepts importants sur les branches

#### 1. Une branche est juste un pointeur

```
Commit A: a1b2c3d
Commit B: c3d4e5f
Commit C: e5f6g7h

Branch main: pointe vers e5f6g7h
```

Quand vous créez une branche, vous créez simplement un nouveau pointeur. Aucune copie de fichier n'est faite.

#### 2. Les commits appartiennent au graphe, pas aux branches

```
        ┌─ D ← E
        │
A ← B ← C
        └─ F
```

Les commits `A`, `B`, et `C` ne sont "sur aucune branche" en particulier. Ils font partie du graphe de commits accessible depuis plusieurs branches.

#### 3. Changer de branche modifie votre Working Directory

Quand vous changez de branche, Git met à jour vos fichiers pour refléter l'état de cette branche.

```bash
# Sur main
cat fichier.txt
# "Version main"

# Changer vers feature
git checkout feature

cat fichier.txt
# "Version feature"
```

Vos fichiers changent physiquement pour correspondre à la branche.

#### 4. Impossible de changer de branche avec des modifications non commitées

```bash
# Vous avez des modifications non sauvegardées
git checkout autre-branche
# error: Your local changes would be overwritten
```

Git refuse de changer de branche pour ne pas perdre vos modifications. Vous devez soit :
- Commiter vos modifications
- Les mettre de côté avec `git stash`
- Les annuler avec `git restore`

---

### Visualiser les branches

#### Avec `git log --graph`

```bash
git log --oneline --graph --all
```

**Résultat :**

```
*   a1b2c3d (HEAD -> main) Merge feature-A
|\
| * c3d4e5f (feature-A) Ajout feature A
| * e5f6g7h Work in progress
|/
* h7i8j9k Fix bug
* j9k0l1m Initial commit
```

Cette visualisation montre :
- Les branches (lignes qui divergent)
- Les merges (lignes qui convergent)
- Les commits sur chaque branche
- Où se trouve HEAD

#### Avec des outils graphiques

De nombreux outils offrent une visualisation plus claire :
- **GitKraken** : interface graphique complète
- **Sourcetree** : outil visuel de Git
- **GitHub Desktop** : client GitHub
- **Extension VSCode** : Git Graph extension

---

### Workflow typique avec les branches

Voici comment les développeurs utilisent typiquement les branches :

```
1. Partir de main (code stable)
   ↓
2. Créer une branche feature
   ↓
3. Développer la fonctionnalité (plusieurs commits)
   ↓
4. Tester la fonctionnalité
   ↓
5. Fusionner dans main
   ↓
6. Supprimer la branche feature
```

**En commandes Git :**

```bash
# 1. S'assurer d'être sur main
git checkout main

# 2. Créer et basculer sur la nouvelle branche
git checkout -b feature-nouvelle-page

# 3. Travailler (plusieurs cycles)
git add .
git commit -m "Ajout structure HTML"
git add .
git commit -m "Ajout styles CSS"

# 4. Tester localement
# ... tests ...

# 5. Fusionner dans main
git checkout main
git merge feature-nouvelle-page

# 6. Supprimer la branche
git branch -d feature-nouvelle-page
```

---

### Avantages des branches dans Git

#### 1. Développement parallèle

```
Alice travaille sur feature-A
Bob travaille sur feature-B
Claire fixe un bug sur bugfix-C

Tous en même temps, sans conflit !
```

#### 2. Isolation du code instable

Votre code sur une branche feature peut être cassé, incomplet, expérimental. `main` reste stable et déployable.

#### 3. Facilite la revue de code

Avant de fusionner une branche, l'équipe peut :
- Examiner tous les changements ensemble
- Tester la fonctionnalité complète
- Demander des modifications

#### 4. Permet de mettre en pause

```bash
# Vous travaillez sur feature-A
# Urgence : bug critique sur main !

git commit -m "WIP: Feature A en cours"
git checkout main
git checkout -b hotfix-urgent
# Corriger le bug
# ...
git checkout feature-A
# Reprendre votre travail
```

#### 5. Historique clair

Chaque fonctionnalité = une branche = commits regroupés logiquement

```
main
  ├─ Merge feature-login (5 commits)
  ├─ Merge feature-payment (8 commits)
  └─ Merge bugfix-security (2 commits)
```

---

### Idées reçues sur les branches

#### ❌ "Les branches sont compliquées"

En réalité, elles simplifient la gestion de votre code. Sans branches, tous les développements se mélangent.

#### ❌ "Les branches prennent beaucoup de place"

Non ! Une branche ne prend que 41 octets. Les commits sont partagés entre branches.

#### ❌ "Je travaille seul, je n'ai pas besoin de branches"

Même seul, les branches sont utiles pour :
- Expérimenter sans risque
- Organiser votre travail par fonctionnalité
- Garder `main` propre et fonctionnel

#### ❌ "Créer une branche duplique tous mes fichiers"

Non ! Git ne duplique rien. C'est juste un pointeur dans l'historique.

---

### Résumé visuel

```
┌──────────────────────────────────────────────────────┐
│  Une branche = Un pointeur vers un commit            │
│                                                      │
│  A ← B ← C ← D                                       │
│          ↑   ↑                                       │
│        main  feature                                 │
│                                                      │
│  Créer une branche = Créer un nouveau pointeur       │
│  Changer de branche = Déplacer HEAD                  │
│  Commiter = Avancer le pointeur de la branche        │
└──────────────────────────────────────────────────────┘
```

---

### Points clés à retenir

✅ Une branche est un **pointeur léger** vers un commit

✅ Les branches permettent le **développement parallèle**

✅ Créer une branche est **instantané et gratuit**

✅ `main` est conventionnellement la branche **stable/production**

✅ `HEAD` indique **où vous êtes** actuellement

✅ Les branches sont **locales** par défaut (jusqu'au push)

✅ Git encourage à créer **beaucoup de branches**

✅ Changer de branche modifie votre **Working Directory**

---

### Conclusion

Les branches sont au cœur de la philosophie Git. Elles transforment la façon dont vous développez en permettant d'isoler, d'expérimenter, et de collaborer efficacement. Ce qui semblait compliqué dans d'autres systèmes devient simple et naturel dans Git.

**Pensez aux branches comme des univers parallèles :** Vous pouvez créer une réalité alternative, expérimenter dedans, et si ça fonctionne, fusionner les deux réalités. Si ça ne fonctionne pas, effacez simplement cet univers alternatif.

Dans les prochaines sections, nous verrons comment :
- Créer, lister et supprimer des branches
- Changer de branche
- Fusionner des branches
- Résoudre des conflits
- Utiliser des stratégies avancées avec les branches

Prêt à maîtriser les branches Git ? C'est parti !

⏭️ [Créer, lister, supprimer des branches](/module-04-travailler-avec-les-branches/02-creer-lister-supprimer-des-branches.md)
