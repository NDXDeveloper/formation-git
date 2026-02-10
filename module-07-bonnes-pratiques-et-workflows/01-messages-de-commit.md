🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 1. Écrire de bons messages de commit

### Introduction

Un message de commit est bien plus qu'une simple formalité. C'est la documentation de l'évolution de votre projet. Dans quelques mois, quand vous ou vos collègues essaieront de comprendre pourquoi tel changement a été fait, le message de commit sera votre meilleur allié.

Un bon message de commit répond à trois questions :
- **Quoi ?** Quel changement a été effectué ?
- **Pourquoi ?** Quelle est la raison de ce changement ?
- **Comment ?** (optionnel) Comment le problème a-t-il été résolu ?

---

### Anatomie d'un bon message de commit

Un message de commit se compose généralement de deux parties :

```
Titre court et descriptif (max 50 caractères)

Corps du message plus détaillé qui explique le contexte,  
les raisons du changement, et éventuellement les impacts.  
Chaque ligne ne devrait pas dépasser 72 caractères.
```

#### Le titre (ligne de sujet)

**Règles d'or** :
1. **Maximum 50 caractères** : soyez concis et précis
2. **Commencez par une majuscule**
3. **Pas de point final**
4. **Utilisez l'impératif présent** : "Ajoute" plutôt que "Ajouté" ou "Ajoute"
5. **Décrivez CE QUE fait le commit**, pas comment vous l'avez fait

**Exemples** :

✅ **Bon** :
```
Ajoute la validation des emails dans le formulaire
```

❌ **Mauvais** :
```
ajout de code pour les emails (trop vague, pas de majuscule)  
Ajouté la validation des emails dans le formulaire d'inscription utilisateur avec regex (trop long, passé)  
bug fix (pas informatif)
```

#### Le corps du message

Le corps est optionnel pour les petits changements évidents, mais **fortement recommandé** pour :
- Les changements complexes
- Les corrections de bugs
- Les nouvelles fonctionnalités
- Les changements qui affectent le comportement

**Structure du corps** :
1. **Laissez une ligne vide** après le titre
2. **Expliquez le POURQUOI**, pas le comment (le code montre le comment)
3. **Donnez du contexte** : quel problème résout ce commit ?
4. **Mentionnez les effets secondaires** ou impacts éventuels
5. **Utilisez des listes à puces** si nécessaire
6. **Limitez à 72 caractères par ligne** pour une bonne lisibilité

**Exemple complet** :

```
Corrige le bug d'affichage des dates en timezone UTC

Les dates s'affichaient en UTC au lieu de la timezone locale  
de l'utilisateur, causant de la confusion sur les horaires  
d'événements.

Solution : conversion automatique de toutes les dates vers  
la timezone du navigateur avant affichage.

Impacte tous les composants affichant des dates.  
Fixes #342
```

---

### Convention d'écriture : l'impératif présent

Pourquoi utiliser l'impératif ? Git lui-même utilise cette convention. Pensez à votre commit comme une instruction :

**"Si appliqué, ce commit va..."** + votre message

✅ **Exemples corrects** :
- Ajoute la fonctionnalité de recherche
- Corrige l'erreur 404 sur la page d'accueil
- Supprime le code obsolète de l'ancien module
- Met à jour les dépendances de sécurité
- Refactorise la fonction de calcul des prix

❌ **À éviter** :
- Ajouté... (passé composé)
- Ajout de... (nom)
- J'ai ajouté... (première personne)

---

### Les préfixes conventionnels

Beaucoup d'équipes utilisent des **préfixes** pour catégoriser les commits. C'est la convention "Conventional Commits".

**Préfixes courants** :

- `feat:` Nouvelle fonctionnalité
- `fix:` Correction de bug
- `docs:` Documentation uniquement
- `style:` Formatage, points-virgules manquants, etc. (pas de changement de code)
- `refactor:` Refactoring du code (ni bug ni nouvelle fonctionnalité)
- `perf:` Amélioration des performances
- `test:` Ajout ou correction de tests
- `chore:` Maintenance (mise à jour de dépendances, configuration, etc.)
- `ci:` Changements dans la configuration CI/CD
- `build:` Changements affectant le système de build

**Exemples** :

```
feat: ajoute l'export PDF des rapports

fix: corrige le crash au démarrage sur iOS 14

docs: met à jour le README avec les instructions d'installation

refactor: simplifie la logique de validation des formulaires

perf: améliore le temps de chargement de la page d'accueil

test: ajoute les tests unitaires pour le module de paiement
```

**Portée optionnelle** (scope) :

Vous pouvez ajouter une portée entre parenthèses pour plus de précision :

```
feat(auth): ajoute la connexion via Google  
fix(api): corrige la gestion des erreurs 500  
docs(readme): ajoute la section troubleshooting
```

---

### Référencer des issues et tickets

Si votre projet utilise un système de suivi (GitHub Issues, Jira, etc.), référencez les tickets dans vos commits :

```
fix: corrige la pagination des résultats de recherche

La pagination ne fonctionnait pas au-delà de la page 10  
en raison d'une erreur de calcul de l'offset.

Fixes #123
```

**Mots-clés GitHub** qui ferment automatiquement les issues :
- `Fixes #123`
- `Closes #123`
- `Resolves #123`

**Pour simplement référencer** sans fermer :
- `Refs #123`
- `See #123`
- `Related to #123`

---

### Ce qu'il faut éviter

❌ **Messages trop vagues** :
```
Update  
Fix bug  
Changes  
WIP  
asdfgh
```

❌ **Messages trop longs dans le titre** :
```
Ajout de la fonctionnalité permettant aux utilisateurs de modifier leur profil avec validation des champs et gestion des erreurs
```

❌ **Description du HOW au lieu du WHY** :
```
Ajoute une boucle for et un if pour vérifier les valeurs
```
→ Le code montre déjà le "comment", expliquez plutôt "pourquoi"

❌ **Tout mettre dans un seul commit géant** :
```
Ajoute login, corrige bugs, met à jour CSS, refactorise API
```
→ Un commit = un changement logique

❌ **Humour ou messages personnels** :
```
YOLO, ça devrait marcher maintenant  
J'en ai marre de ce bug
```
→ Restez professionnel, votre historique est public

---

### Bonnes pratiques avancées

#### 1. Le commit atomique

Chaque commit devrait représenter **un changement logique unique**. Si vous devez utiliser "et" dans votre message, c'est probablement trop gros.

✅ **Bon** : Séparer en plusieurs commits
```
feat: ajoute le formulaire de contact  
fix: corrige l'erreur de validation email  
docs: met à jour le guide utilisateur
```

❌ **Mauvais** : Tout en un
```
Ajoute formulaire de contact et corrige bug et met à jour docs
```

#### 2. Utiliser les co-auteurs

Si vous travaillez en pair programming :

```
feat: implémente la fonctionnalité de chat en temps réel

Co-authored-by: Marie Dupont <marie@example.com>  
Co-authored-by: Pierre Martin <pierre@example.com>
```

#### 3. Breaking changes

Si votre changement casse la compatibilité :

```
feat!: change le format de réponse de l'API

BREAKING CHANGE: L'API renvoie maintenant un objet au lieu  
d'un tableau. Les clients doivent mettre à jour leur code.

Migration : remplacer `data` par `data.items`
```

Le `!` après le type signale un breaking change.

---

### Configurer votre éditeur de commit

Par défaut, Git ouvre un éditeur de texte pour les messages longs. Configurez votre éditeur préféré :

```bash
# Utiliser VS Code
git config --global core.editor "code --wait"

# Utiliser Vim
git config --global core.editor "vim"

# Utiliser Nano
git config --global core.editor "nano"
```

**Template de commit** :

Vous pouvez créer un template pour vous rappeler les bonnes pratiques :

1. Créez un fichier `~/.gitmessage.txt` :

```
# <type>: <sujet> (max 50 caractères)

# Corps du message (72 caractères par ligne)
# Expliquez le POURQUOI, pas le comment

# Footer (optionnel)
# Fixes #
# Co-authored-by:
```

2. Configurez Git pour l'utiliser :

```bash
git config --global commit.template ~/.gitmessage.txt
```

---

### Résumé des règles d'or

1. **Titre court et clair** (≤ 50 caractères)
2. **Impératif présent** : "Ajoute" pas "Ajouté"
3. **Majuscule au début**, pas de point final
4. **Ligne vide** entre le titre et le corps
5. **Corps explicatif** : pourquoi, pas comment
6. **72 caractères max** par ligne dans le corps
7. **Un commit = un changement logique**
8. **Référencez les issues** quand pertinent
9. **Soyez professionnel** : votre historique est votre CV

---

### Pour aller plus loin

**Ressources** :
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Comment écrire un message de commit Git](https://cbea.ms/git-commit/)
- Les messages de commit des grands projets open source (Linux, Git, React)

**Rappelez-vous** : un bon message de commit, c'est un cadeau que vous faites à votre futur vous et à votre équipe. Prenez ces quelques secondes supplémentaires pour bien l'écrire, cela vous fera gagner des heures de recherche plus tard !

⏭️ [Commits atomiques et organisation du travail](/module-07-bonnes-pratiques-et-workflows/02-commits-atomiques.md)
