🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 2 : Concepts fondamentaux

## 5. Visualiser l'historique : git log --graph et la notion de graphe

### Introduction : L'historique Git est un graphe

Jusqu'à présent, nous avons vu l'historique de Git comme une **liste linéaire** de commits : commit 1, commit 2, commit 3, etc. C'est vrai pour un projet simple avec une seule branche, mais la réalité est souvent plus complexe.

En fait, l'historique de Git est un **graphe** (dans le sens mathématique) : une structure où les commits sont des nœuds reliés entre eux. Comprendre cette notion de graphe est essentiel pour maîtriser Git, surtout quand vous commencerez à travailler avec plusieurs branches.

Dans cette section, nous allons :
- Comprendre ce qu'est un graphe dans Git
- Apprendre à visualiser cet historique
- Interpréter les représentations graphiques
- Comprendre les différents types de structures

---

## Qu'est-ce qu'un graphe dans Git ?

### Définition simple

Un **graphe** est une structure composée de :
- **Nœuds** (ou sommets) : Dans Git, ce sont les **commits**
- **Arêtes** (ou liens) : Les connexions entre les commits (relations parent-enfant)

### Analogie : L'arbre généalogique

Pensez à un **arbre généalogique familial** :
- Chaque personne est un nœud (comme un commit)
- Les liens parent-enfant connectent les générations
- On peut avoir des branches (différentes lignes familiales)
- Parfois, des branches fusionnent (mariages entre familles)

C'est exactement comme cela que fonctionne Git !

### Structure linéaire simple

Commençons par le cas le plus simple : un historique linéaire.

```
A ← B ← C ← D ← E (HEAD)
```

Chaque commit pointe vers son parent :
- **E** a pour parent **D**
- **D** a pour parent **C**
- **C** a pour parent **B**
- **B** a pour parent **A**
- **A** est le premier commit (pas de parent)

**HEAD** indique où vous êtes actuellement.

### Structure avec branches

Maintenant, imaginons que vous créez une branche au niveau du commit C :

```
        ┌─ F ─ G (feature)
        │
A ─ B ─ C ─ D ─ E (main)
```

Vous avez maintenant **deux branches** :
- **main** : A → B → C → D → E
- **feature** : A → B → C → F → G

Les deux branches partagent l'historique jusqu'à C (leur **ancêtre commun**), puis divergent.

### Structure après fusion (merge)

Quand vous fusionnez la branche feature dans main :

```
        ┌─ F ─ G ─┐
        │         │
A ─ B ─ C ─ D ─ E ─ M (main)
```

Le commit **M** (merge) a **deux parents** :
- **E** (de main)
- **G** (de feature)

C'est ce qu'on appelle un **merge commit**.

---

## git log --graph : Visualiser le graphe

### Commande de base

```bash
git log --graph
```

Cette option ajoute une représentation graphique ASCII sur le côté gauche de la sortie de `git log`.

Résultat typique (avec historique linéaire) :

```
* commit a1b2c3d
| Author: Marie Dupont <marie@email.com>
| Date:   Wed Oct 15 14:30:00 2025
|
|     Ajout de la fonctionnalité de recherche
|
* commit d5e6f7g
| Author: Jean Martin <jean@email.com>
| Date:   Wed Oct 15 10:15:00 2025
|
|     Correction du bug de pagination
```

Les **astérisques (*)** représentent les commits, et la **ligne verticale (|)** montre la continuité de l'historique.

### Combinaison recommandée

La commande la plus utile combine plusieurs options :

```bash
git log --graph --oneline --all --decorate
```

Décortiquons chaque option :
- `--graph` : Affiche le graphe ASCII
- `--oneline` : Format compact (une ligne par commit)
- `--all` : Montre toutes les branches (pas seulement la branche actuelle)
- `--decorate` : Affiche les noms de branches et tags

Résultat :

```
* a1b2c3d (HEAD -> main) Ajout de la fonctionnalité de recherche
* d5e6f7g Correction du bug de pagination
* x5y6z7a Refactorisation du code CSS
* e8f9g0h Mise à jour de la documentation
```

### Créer un alias pour cette commande

Cette commande est tellement utile qu'il est recommandé de créer un alias :

```bash
git config --global alias.lg "log --graph --oneline --all --decorate"
```

Ensuite, vous pourrez simplement taper :

```bash
git lg
```

---

## Interpréter les représentations graphiques

### Historique linéaire

Le cas le plus simple : une seule branche, commits successifs.

```bash
git log --graph --oneline
```

```
* a1b2c3d (HEAD -> main) Dernier commit
* d5e6f7g Commit précédent
* x5y6z7a Encore avant
* e8f9g0h Premier commit
```

**Interprétation** :
- Une seule ligne verticale = une seule branche
- Progression de bas en haut (du plus ancien au plus récent)
- HEAD pointe sur main, qui pointe sur le commit le plus récent

### Branche divergente

Quand vous créez une branche et ajoutez des commits :

```bash
git log --graph --oneline --all
```

```
* g1h2i3j (HEAD -> feature) Travail sur feature
* f4e5d6c Suite du développement
| * c7b8a9d (main) Correctif sur main
| * b1a2c3d Autre commit sur main
|/
* e8f9g0h Point de divergence
* i1j2k3l Commit initial
```

**Interprétation** :
- Deux branches apparaissent : `feature` et `main`
- Le symbole `|/` montre où les branches ont divergé
- Au commit `e8f9g0h`, les branches se séparent
- Chaque branche a ensuite son propre historique

**Lecture** :
- La branche de gauche (avec `*`) est la branche courante (feature)
- La branche de droite (avec `|`) est une autre branche (main)

### Fusion de branches (merge)

Après avoir fusionné deux branches :

```bash
git log --graph --oneline --all
```

```
*   m5n6o7p (HEAD -> main) Merge branch 'feature'
|\
| * g1h2i3j (feature) Dernier commit de feature
| * f4e5d6c Développement feature
* | c7b8a9d Commit sur main pendant ce temps
* | b1a2c3d Autre commit sur main
|/
* e8f9g0h Point de divergence
* i1j2k3l Commit initial
```

**Interprétation** :
- Le commit `m5n6o7p` est un **merge commit**
- Le symbole `|\` montre que deux branches fusionnent
- Les deux branches remontent jusqu'au merge commit
- Après le merge, l'historique redevient linéaire

**Structure du merge commit** :
- Il a **deux parents** : un sur chaque branche
- C'est le point où les deux histoires se rejoignent

### Plusieurs branches actives

Dans un projet avec plusieurs développeurs :

```bash
git log --graph --oneline --all
```

```
* p1q2r3s (HEAD -> main) Merge feature-x
|\
| * o4p5q6r (feature-x) Fin de feature-x
* | n7m8l9k Hotfix urgent sur main
| * m4n5o6p Suite feature-x
| * l1m2n3o Début feature-x
* | k7l8m9n Commit sur main
|/
| * j4k5l6m (feature-y) Travail sur feature-y
| * i1j2k3l Début feature-y
|/
* h7i8j9k Point commun
* g1h2i3j Commit initial
```

**Interprétation** :
- Trois branches visibles : `main`, `feature-x`, `feature-y`
- `feature-x` a été fusionnée dans `main`
- `feature-y` est toujours en cours (divergente)
- On voit l'historique complet des trois branches

### Fast-forward merge

Un cas particulier de fusion sans commit de merge :

**Avant le merge** :

```
* d1e2f3g (feature) Commit sur feature
* c4b5a6d Autre commit feature
* b7c8d9e (HEAD -> main) Dernier commit main
* a1b2c3d Commit initial
```

**Après fast-forward merge** :

```
* d1e2f3g (HEAD -> main, feature) Commit sur feature
* c4b5a6d Autre commit feature
* b7c8d9e Dernier commit main
* a1b2c3d Commit initial
```

**Interprétation** :
- Pas de commit de merge créé
- Le pointeur `main` avance simplement jusqu'à `feature`
- L'historique reste linéaire
- Cela arrive quand `main` n'a pas avancé pendant le développement de `feature`

---

## Symboles utilisés dans --graph

Comprendre les symboles ASCII du graphe :

| Symbole | Signification |
|---------|---------------|
| `*` | Un commit |
| `\|` | Ligne verticale (continuité d'une branche) |
| `\|/` | Divergence (création de branche) |
| `\|\` | Convergence (fusion de branches) |
| `/` | Ligne diagonale (connexion entre branches) |
| `\` | Ligne diagonale inverse |

### Exemple annotés

```
*   Merge commit (deux parents)
|\  ← Fusion commence ici
| * Commit sur branche 2
| * Autre commit branche 2
* | Commit sur branche 1
* | Autre commit branche 1
|/  ← Divergence (branches se séparent)
*   Commit commun (ancêtre)
```

---

## Options avancées de git log --graph

### Limiter le nombre de commits

```bash
# Voir seulement les 10 derniers commits
git log --graph --oneline --all -10
```

### Ajouter des dates

```bash
git log --graph --oneline --all --date=short --pretty=format:'%h - %s (%ad) <%an>'
```

Résultat :

```
* a1b2c3d - Ajout feature (2025-10-15) <Marie Dupont>
* d5e6f7g - Correction bug (2025-10-14) <Jean Martin>
```

### Coloriser

```bash
git log --graph --oneline --all --decorate --color
```

Les couleurs sont généralement activées par défaut et aident à distinguer les branches.

### Simplifier l'historique

```bash
# Simplifier en fusionnant les commits linéaires
git log --graph --oneline --all --simplify-by-decoration
```

Montre seulement les commits importants (ceux avec des branches ou tags).

### Afficher l'historique d'une branche spécifique

```bash
# Seulement la branche main
git log --graph --oneline main

# Comparer deux branches
git log --graph --oneline main feature
```

---

## Comprendre les concepts de graphe

### Commit parent et enfant

Chaque commit (sauf le premier) a au moins un **parent** :

```
A (parent) ← B (enfant)
```

**B** connaît **A** (son parent), mais **A** ne connaît pas **B**.

C'est une relation **unidirectionnelle** : du plus récent vers le plus ancien.

### Ancêtre commun

Quand deux branches divergent, elles partagent un **ancêtre commun** :

```
        ┌─ D ─ E (branche1)
        │
A ─ B ─ C
        │
        └─ F ─ G (branche2)
```

**C** est l'ancêtre commun de `branche1` et `branche2`.

Git utilise cette notion pour :
- Fusionner les branches intelligemment
- Détecter les conflits
- Optimiser les opérations

### HEAD : votre position actuelle

**HEAD** est un pointeur spécial qui indique **où vous êtes** dans le graphe.

```
* a1b2c3d (HEAD -> main) ← Vous êtes ici
* d5e6f7g
* x5y6z7a
```

Quand vous faites un commit, HEAD avance automatiquement.

Quand vous changez de branche (`git switch feature`), HEAD bouge :

```
* g1h2i3j (HEAD -> feature) ← Vous êtes maintenant ici
* f4e5d6c
| * c7b8a9d (main)
|/
* e8f9g0h
```

### Branches : des pointeurs mobiles

Une **branche** n'est qu'un **pointeur** vers un commit.

```
* a1b2c3d (main) ← Pointeur "main"
* d5e6f7g
* x5y6z7a
```

Quand vous ajoutez un commit sur `main`, le pointeur avance :

```
* z1x2y3v (main) ← Le pointeur a bougé ici
* a1b2c3d
* d5e6f7g
```

**Implication importante** : Créer une branche est **instantané** (c'est juste créer un nouveau pointeur).

---

## Patterns courants dans les graphes Git

### Pattern 1 : Développement linéaire

```
A ─ B ─ C ─ D ─ E (main)
```

Le plus simple. Typique d'un projet avec un seul développeur ou sans branches.

**Commandes** :

```bash
git log --graph --oneline
```

```
* e1f2g3h (HEAD -> main) E
* d4e5f6g D
* c7d8e9f C
* b1c2d3e B
* a4b5c6d A
```

### Pattern 2 : Feature branch

```
        ┌─ D ─ E (feature)
        │
A ─ B ─ C ─────────── (main)
```

Une branche pour développer une fonctionnalité isolée.

**Commandes** :

```bash
git log --graph --oneline --all
```

```
* e1f2g3h (feature) E
* d4e5f6g D
| * c7d8e9f (HEAD -> main) C
|/
* b1c2d3e B
* a4b5c6d A
```

### Pattern 3 : Feature branch fusionnée

```
        ┌─ D ─ E ─┐
        │         │
A ─ B ─ C ───────── M (main)
```

Après fusion de la feature.

**Commandes** :

```bash
git log --graph --oneline
```

```
*   m1n2o3p (HEAD -> main) M: Merge feature
|\
| * e1f2g3h E
| * d4e5f6g D
|/
* c7d8e9f C
* b1c2d3e B
* a4b5c6d A
```

### Pattern 4 : Développement parallèle

```
        ┌─ D ─ E ─┐
        │         │
A ─ B ─ C ─ F ─ G ─ M (main)
```

Les deux branches ont avancé en parallèle avant la fusion.

**Commandes** :

```bash
git log --graph --oneline
```

```
*   m1n2o3p (HEAD -> main) M: Merge feature
|\
| * e1f2g3h E
| * d4e5f6g D
* | g7h8i9j G
* | f4g5h6i F
|/
* c7d8e9f C
* b1c2d3e B
* a4b5c6d A
```

### Pattern 5 : Plusieurs features en parallèle

```
    ┌─ D ─ E (feature-x)
    │
A ─ C ─ F ─ G (main)
    │
    └─ H ─ I (feature-y)
```

Plusieurs branches de développement actives.

**Commandes** :

```bash
git log --graph --oneline --all
```

```
* e1f2g3h (feature-x) E
* d4e5f6g D
| * g7h8i9j (HEAD -> main) G
| * f4g5h6i F
|/
| * i1j2k3l (feature-y) I
| * h4i5j6k H
|/
* c7d8e9f C
* a4b5c6d A
```

### Pattern 6 : Git Flow (complexe)

```
           ┌─ feature1 ─┐
           │            │
main ─ C ─────────────── M ─ release
           │
           └─ feature2 (en cours)
```

Workflow plus complexe avec plusieurs types de branches (main, develop, features, releases, hotfix).

---

## Visualiser avec plus de détails

### Ajouter les auteurs

```bash
git log --graph --oneline --all --pretty=format:'%C(yellow)%h%C(reset) - %s %C(green)(%cr)%C(reset) %C(blue)<%an>%C(reset)'
```

Résultat coloré :

```
* a1b2c3d - Ajout feature (2 hours ago) <Marie Dupont>
* d5e6f7g - Fix bug (5 hours ago) <Jean Martin>
```

### Ajouter les dates

```bash
git log --graph --oneline --all --date=short
```

### Afficher les statistiques

```bash
git log --graph --stat --oneline
```

Combine le graphe avec les statistiques de modifications.

---

## Outils graphiques

Parfois, un outil visuel est plus pratique que la ligne de commande.

### Outils en ligne de commande

#### gitk

```bash
gitk --all
```

Outil graphique simple intégré à Git, qui affiche un graphe interactif.

#### tig

```bash
tig
```

Interface en terminal (type ncurses), très puissante pour naviguer dans l'historique.

### Outils avec interface graphique

- **GitKraken** : Interface moderne, graphe très visuel
- **SourceTree** : Gratuit, excellent pour visualiser les branches
- **GitHub Desktop** : Simple, intègre bien avec GitHub
- **Fork** : Rapide, graphe clair

### Extensions d'éditeur

Dans **VS Code** :
- **Git Graph** : Extension qui affiche un graphe interactif
- **GitLens** : Informations Git enrichies dans l'éditeur

Dans **IntelliJ / PyCharm** :
- Git intégré avec vue graphique dans "Git" → "Log"

---

## Cas pratiques

### Cas 1 : Comprendre où en sont les branches

```bash
git log --graph --oneline --all
```

Vous voyez immédiatement :
- Quelles branches existent
- Où elles ont divergé
- Combien de commits d'avance/retard

### Cas 2 : Vérifier avant un merge

```bash
# Voir ce qui sera fusionné
git log --graph --oneline main..feature

# Ou voir les deux côtés
git log --graph --oneline main...feature
```

### Cas 3 : Débugger un historique complexe

Quand vous avez des problèmes de merge ou un historique confus :

```bash
git log --graph --oneline --all --decorate
```

Le graphe vous montre visuellement ce qui s'est passé.

### Cas 4 : Comparer avec le distant

```bash
# Après un fetch
git log --graph --oneline --all

# Vous verrez origin/main et main
```

Cela montre si vous êtes en avance, en retard, ou si les branches ont divergé.

---

## Comprendre les termes liés au graphe

### Upstream (en amont)

Les commits **avant** un commit donné (ses ancêtres).

```
A ─ B ─ C ─ D
        ↑
    B et A sont upstream de C
```

### Downstream (en aval)

Les commits **après** un commit donné (ses descendants).

```
A ─ B ─ C ─ D
    ↑
C et D sont downstream de B
```

### Reachable (accessible)

Un commit est **accessible** depuis un autre si on peut l'atteindre en remontant l'historique.

```
A ─ B ─ C ─ D

D est accessible depuis C, B, A  
C est accessible depuis B, A
```

### Merge base (base de fusion)

L'**ancêtre commun** le plus récent entre deux branches.

```
        ┌─ D ─ E (branch1)
        │
A ─ B ─ C
        │
        └─ F ─ G (branch2)

C est le merge base de branch1 et branch2
```

Git utilise cette notion pour les merges et les rebases.

---

## Bonnes pratiques

### 1. Utilisez régulièrement --graph

Visualiser l'historique vous aide à comprendre la structure de votre projet.

```bash
# Créer un alias
git config --global alias.lg "log --graph --oneline --all --decorate"

# L'utiliser souvent
git lg
```

### 2. Avant un merge, visualisez

```bash
git log --graph --oneline main..feature
```

Voyez ce qui va être fusionné.

### 3. Gardez un historique propre

Un graphe simple est plus facile à comprendre :
- Utilisez des branches pour séparer le travail
- Fusionnez régulièrement
- Évitez les branches qui traînent trop longtemps

### 4. Comprendre avant d'agir

Si l'historique vous semble confus, prenez le temps de le visualiser avec `--graph` avant de faire des opérations complexes.

### 5. Documenter les merges

Quand vous fusionnez, écrivez un message clair :

```bash
git merge feature -m "Merge feature-x: ajout de la recherche avancée"
```

Cela aide à comprendre le graphe plus tard.

---

## Exercices de lecture de graphe

### Exemple 1

```
*   m1n2o3p (HEAD -> main) Merge feature
|\
| * e1f2g3h (feature) Commit C
| * d4e5f6g Commit B
* | c7d8e9f Commit A
|/
* b1c2d3e Initial commit
```

**Questions** :
- Combien de branches ? **Deux** (main et feature)
- Quel est l'ancêtre commun ? **b1c2d3e**
- Combien de commits ont été faits sur chaque branche ? **1 sur main, 2 sur feature**
- Quel type de fusion ? **Merge avec commit de merge**

### Exemple 2

```
* g1h2i3j (HEAD -> main, feature) Dernier commit
* f4g5h6i Commit précédent
* e7f8g9h Commit initial
```

**Questions** :
- Combien de branches ? **Une** (main et feature pointent au même endroit)
- C'est quel type de fusion ? **Fast-forward** (ou pas encore de divergence)
- L'historique est-il linéaire ? **Oui**

### Exemple 3

```
* i1j2k3l (feature-y) Commit Y2
| * h4i5j6k (feature-x) Commit X2
|/
* g7h8i9j (HEAD -> main) Commit commun
* f1g2h3i Initial
```

**Questions** :
- Combien de branches actives ? **Trois** (main, feature-x, feature-y)
- Quelle est l'ancêtre commun des features ? **g7h8i9j** (où est HEAD)
- Les features ont-elles été fusionnées ? **Non**, elles sont toujours divergentes

---

## En résumé

La visualisation de l'historique Git avec `--graph` est essentielle pour :

1. **Comprendre la structure** de votre projet
   - Voir les branches et leurs relations
   - Identifier les merges et divergences

2. **Naviguer efficacement** dans l'historique
   - Savoir où vous êtes (HEAD)
   - Comprendre où sont les autres branches

3. **Prendre des décisions éclairées**
   - Avant un merge, voir ce qui va être fusionné
   - Détecter les problèmes potentiels

4. **Communiquer avec l'équipe**
   - Partager une vision commune de l'historique
   - Comprendre le travail des autres

**Commande à retenir** :

```bash
git log --graph --oneline --all --decorate
```

Ou créez l'alias :

```bash
git config --global alias.lg "log --graph --oneline --all --decorate"
```

**Concept clé** : L'historique Git n'est pas une ligne droite, c'est un **graphe** où les commits sont connectés par des relations parent-enfant, permettant des branches parallèles et des fusions.

Maîtriser la lecture de ce graphe vous donne une compréhension profonde de ce qui se passe dans votre dépôt.

---

*Dans la section suivante, nous verrons comment utiliser .gitignore pour ignorer certains fichiers.*

⏭️ [.gitignore : ignorer des fichiers](/module-02-concepts-fondamentaux/06-gitignore.md)
