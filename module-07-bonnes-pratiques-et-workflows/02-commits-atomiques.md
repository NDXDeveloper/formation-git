🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 7 : Bonnes pratiques et workflows
## 2. Commits atomiques et organisation du travail

### Introduction

Imaginez que vous deviez retrouver un livre dans une bibliothèque. Préférez-vous une bibliothèque où les livres sont classés par thème, auteur et date, ou une bibliothèque où tous les livres sont empilés au hasard ? Votre historique Git, c'est pareil !

Un **commit atomique** est un commit qui représente **une seule modification logique, complète et cohérente**. C'est la pierre angulaire d'un historique Git propre et maintenable.

Dans cette section, nous allons voir comment organiser votre travail pour créer un historique clair et utile.

---

### Qu'est-ce qu'un commit atomique ?

#### Définition

Un commit est **atomique** quand il :
1. **Fait une seule chose** : une fonctionnalité, une correction, un refactoring
2. **Fait cette chose complètement** : le projet fonctionne avant et après le commit
3. **Ne fait que cette chose** : rien d'autre n'est mélangé

Le mot "atomique" vient de "atome" : la plus petite unité indivisible.

#### Analogie

Pensez à une recette de cuisine :

❌ **Mauvais** : Un commit "Prépare le repas"
- Mélange les courses, la préparation des légumes, la cuisson, le dressage

✅ **Bon** : Des commits séparés
- "Achète les ingrédients"
- "Prépare les légumes"
- "Cuit le plat principal"
- "Dresse l'assiette"

Chaque étape est complète et indépendante. Si le dressage ne vous plaît pas, vous pouvez le refaire sans tout recommencer.

---

### Pourquoi les commits atomiques sont importants ?

#### 1. Historique lisible

Un historique atomique raconte une histoire claire :

✅ **Bon** :
```
feat: ajoute le bouton de connexion
feat: implémente la logique d'authentification
feat: ajoute la validation du formulaire
fix: corrige l'erreur de redirection après login
```

Chaque commit a un but clair. On comprend l'évolution du projet.

❌ **Mauvais** :
```
WIP
Update
Fix stuff and add things
Final version (for real this time)
```

Impossible de comprendre ce qui a été fait et pourquoi.

#### 2. Facilite le débogage

Quand un bug apparaît, avec des commits atomiques :
- Vous pouvez identifier **exactement quel commit** a introduit le problème
- Vous pouvez utiliser `git bisect` efficacement
- Vous pouvez annuler (`revert`) seulement le commit problématique

**Exemple** :

Si vous avez un commit géant "Ajoute login + corrige CSS + met à jour API", et que le login ne marche plus, vous devez :
- Analyser tout le commit
- Isoler ce qui pose problème
- Peut-être tout annuler, même les parties qui fonctionnent

Avec des commits séparés, vous annulez seulement le commit du login.

#### 3. Collaboration simplifiée

- **Revue de code** : plus facile de reviewer 10 petits commits qu'un énorme
- **Conflits** : moins de risques de conflits de fusion
- **Cherry-pick** : vous pouvez appliquer des commits spécifiques sur d'autres branches
- **Comprendre le code** : les nouveaux membres comprennent pourquoi le code est ainsi

#### 4. Flexibilité

Avec des commits atomiques, vous pouvez :
- Annuler une fonctionnalité spécifique sans toucher au reste
- Réorganiser l'historique proprement avec `rebase`
- Créer des branches à partir de n'importe quel état cohérent
- Fusionner partiellement du travail

---

### Comment créer des commits atomiques ?

#### Principe de base : une logique = un commit

Posez-vous la question : **"Ce commit fait-il UNE seule chose ?"**

✅ **Exemples de commits atomiques** :

```
feat: ajoute la recherche par nom d'utilisateur
```
→ Une fonctionnalité complète

```
fix: corrige le calcul de TVA pour les produits importés
```
→ Une correction ciblée

```
refactor: extrait la fonction de validation email
```
→ Un refactoring spécifique

```
docs: ajoute les exemples d'utilisation de l'API
```
→ Une amélioration de documentation

```
style: corrige l'indentation dans le fichier auth.js
```
→ Un changement de style isolé

❌ **Exemples de commits NON atomiques** :

```
feat: ajoute recherche, corrige bug pagination, met à jour CSS
```
→ Trois choses différentes = trois commits

```
Update multiple files
```
→ Trop vague, probablement trop de choses

```
WIP: travail du jour
```
→ Pas de logique claire

---

### La règle du "et"

**Règle simple** : Si vous utilisez "et" dans votre message de commit, vous devez probablement le séparer.

❌ **Mauvais** :
```
Ajoute le formulaire de contact et corrige le footer
```

✅ **Bon** :
```
Commit 1: feat: ajoute le formulaire de contact
Commit 2: fix: corrige l'alignement du footer
```

**Exception** : "et" est acceptable quand les changements sont intrinsèquement liés :
```
feat: ajoute le modèle User et sa migration
```
→ Le modèle et sa migration vont ensemble

---

### Stratégies pour organiser son travail

#### 1. Planifier avant de coder

Avant de commencer, décomposez votre travail en petites étapes logiques.

**Exemple** : Vous devez créer une page de profil utilisateur

**Planification** :
1. Créer le composant de base (structure HTML)
2. Ajouter les styles CSS
3. Connecter aux données de l'API
4. Ajouter la fonctionnalité d'édition
5. Ajouter la validation des champs
6. Ajouter les tests

→ 6 commits atomiques au lieu d'un gros commit

#### 2. Commiter souvent, commiter tôt

Ne attendez pas d'avoir tout terminé. Commitez dès qu'une petite partie logique est fonctionnelle.

**Workflow recommandé** :
```bash
# Travaillez sur une petite fonctionnalité
# Testez que ça marche
git add <fichiers concernés>
git commit -m "feat: ajoute X"

# Continuez avec la suite
# Testez que ça marche
git add <fichiers concernés>
git commit -m "feat: ajoute Y"
```

**Avantages** :
- Vous ne perdez jamais beaucoup de travail
- Vous pouvez revenir en arrière facilement
- Votre historique documente votre processus de réflexion

#### 3. Utiliser git add -p (patch mode)

Cette commande est votre meilleure amie pour créer des commits atomiques !

```bash
git add -p fichier.js
```

Git vous montre chaque modification une par une et vous demande ce que vous voulez ajouter.

**Options** :
- `y` : oui, ajoute ce morceau
- `n` : non, ne l'ajoute pas
- `s` : divise ce morceau en plus petits morceaux
- `e` : édite manuellement ce morceau
- `q` : quitte (n'ajoute plus rien)

**Cas d'usage** :

Vous avez modifié un fichier pour deux raisons différentes :
1. Correction d'un bug
2. Ajout d'une nouvelle fonctionnalité

Avec `git add -p`, vous pouvez créer deux commits séparés à partir du même fichier !

```bash
# Premier commit : le bug
git add -p fichier.js  # Sélectionnez seulement les corrections
git commit -m "fix: corrige l'erreur de calcul"

# Deuxième commit : la fonctionnalité
git add -p fichier.js  # Sélectionnez le reste
git commit -m "feat: ajoute la fonction d'export"
```

#### 4. Staging area comme outil d'organisation

La staging area (zone de transit) vous permet de préparer vos commits.

**Workflow** :
```bash
# Vous avez modifié plusieurs fichiers
git status

# Ajoutez seulement ce qui concerne une logique
git add header.css navbar.js
git commit -m "feat: améliore le menu de navigation"

# Puis le reste
git add footer.css
git commit -m "fix: corrige l'alignement du footer"
```

**Commandes utiles** :
```bash
# Voir ce qui est staged vs non-staged
git status

# Voir les différences
git diff              # Ce qui n'est PAS staged
git diff --staged     # Ce qui EST staged

# Enlever un fichier du staging
git restore --staged fichier.js
```

#### 5. Commandes interactives

**Ajouter interactivement** :
```bash
git add -i
```

Menu interactif avec des options pour :
- Ajouter/retirer des fichiers du staging
- Voir les différences
- Etc.

---

### Organiser les types de commits

#### Séparer les commits par nature

✅ **Bonne pratique** : Un type de changement par commit

**Exemple** : Vous créez une nouvelle page

```bash
# 1. Structure HTML
git add page.html
git commit -m "feat: ajoute la structure de la page profil"

# 2. Styles
git add page.css
git commit -m "style: ajoute les styles de la page profil"

# 3. Fonctionnalités JavaScript
git add page.js
git commit -m "feat: implémente la logique de la page profil"

# 4. Tests
git add page.test.js
git commit -m "test: ajoute les tests pour la page profil"
```

#### Les types courants de commits

**Fonctionnalités** (`feat:`) :
- Chaque nouvelle fonctionnalité = un commit
- Si grande fonctionnalité, la découper en sous-fonctionnalités

**Corrections** (`fix:`) :
- Un bug = un commit
- Même si la correction touche plusieurs fichiers

**Refactoring** (`refactor:`) :
- Séparez le refactoring des ajouts de fonctionnalités
- Refactoring d'abord, puis nouvelles features

**Documentation** (`docs:`) :
- Commits séparés pour la documentation
- Facilite les revues

**Tests** (`test:`) :
- Peuvent être avec la fonctionnalité ou séparés
- Séparez si tests ajoutés après coup

**Style/Formatage** (`style:`) :
- Toujours séparés des changements fonctionnels
- Facilite la revue : on ignore ces commits

---

### Erreurs courantes et comment les éviter

#### Erreur 1 : Le commit fourre-tout

❌ **Symptôme** :
```bash
git commit -am "Modifications diverses"
```

**Pourquoi c'est mauvais** :
- Impossible de comprendre ce qui a changé
- Impossible d'annuler une partie
- Conflits de merge assurés

✅ **Solution** :
- Ne jamais utiliser `git commit -am` sans réfléchir
- Toujours réviser avec `git status` et `git diff`
- Utiliser `git add -p` pour séparer

#### Erreur 2 : Commiter des fichiers non liés

❌ **Exemple** :
```bash
git add .
git commit -m "feat: ajoute la recherche"
```

Mais `git add .` a aussi ajouté :
- Des corrections de bugs non liés
- Des changements de style
- Des fichiers de configuration

✅ **Solution** :
- Ne jamais faire `git add .` sans vérifier
- Ajouter les fichiers explicitement
- Utiliser `git add -p`

#### Erreur 3 : Commits trop petits

Il existe aussi l'extrême inverse : des commits trop granulaires.

❌ **Trop petit** :
```bash
Commit 1: Ajoute la ligne 1 du fichier
Commit 2: Ajoute la ligne 2 du fichier
Commit 3: Ajoute la ligne 3 du fichier
```

✅ **Juste ce qu'il faut** :
```bash
Commit 1: Ajoute la fonction de validation
```

**Règle** : Un commit doit laisser le projet dans un état fonctionnel.

#### Erreur 4 : Mélanger refactoring et nouvelles features

❌ **Mauvais** :
```bash
feat: ajoute la fonctionnalité X et refactorise le module Y
```

**Problème** : Si la nouvelle fonctionnalité a un bug, vous ne pouvez pas l'annuler sans perdre le refactoring.

✅ **Bon** :
```bash
Commit 1: refactor: simplifie le module Y
Commit 2: feat: ajoute la fonctionnalité X
```

---

### Workflow quotidien recommandé

#### Le cycle atomique

```bash
# 1. Vérifier l'état
git status

# 2. Identifier une tâche atomique à compléter
# Exemple : "Corriger le bug de validation email"

# 3. Faire les modifications nécessaires
# Éditer les fichiers...

# 4. Tester que ça fonctionne
# Lancer l'application, les tests...

# 5. Regarder ce qui a changé
git diff

# 6. Ajouter seulement ce qui concerne cette tâche
git add -p fichiers_concernes.js

# 7. Vérifier ce qui est staged
git diff --staged

# 8. Commiter avec un message clair
git commit -m "fix: corrige la validation email"

# 9. Répéter pour la tâche suivante
```

#### Quand vous avez mélangé des changements

Si vous réalisez que vous avez fait plusieurs choses non liées :

**Option 1 : Créer plusieurs commits** (recommandé)
```bash
# Ajouter par morceaux
git add -p fichier1.js  # Sélectionnez les parties liées à la première tâche
git commit -m "feat: ajoute X"

git add -p fichier1.js  # Sélectionnez les parties liées à la deuxième tâche
git add fichier2.js
git commit -m "fix: corrige Y"
```

**Option 2 : Utiliser des branches**
```bash
# Sauvegarder vos modifications
git stash

# Créer une branche pour la première tâche
git checkout -b feature/premiere-tache
git stash pop
# Garder seulement les changements liés, commiter
git add ...
git commit -m "feat: ajoute X"

# Revenir à la branche principale
git checkout main
# Répéter pour les autres tâches
```

---

### Nettoyer l'historique avant de partager

Si votre historique local est brouillon, **nettoyez-le avant de pousser** sur le dépôt distant.

#### Utiliser rebase interactif

```bash
# Modifier les 5 derniers commits
git rebase -i HEAD~5
```

**Actions possibles** :
- `pick` : garder le commit tel quel
- `reword` : modifier le message
- `edit` : modifier le contenu
- `squash` : fusionner avec le commit précédent
- `fixup` : fusionner sans garder le message
- `drop` : supprimer le commit

**Exemple** :

Avant :
```
Commit 5: WIP
Commit 4: Oops, forgot this
Commit 3: Add feature X (part 2)
Commit 2: Add feature X (part 1)
Commit 1: Update config
```

Après rebase interactif :
```
Commit 2: feat: ajoute la fonctionnalité X
Commit 1: chore: met à jour la configuration
```

**⚠️ Attention** : Ne jamais rebase des commits déjà partagés (poussés) !

---

### Résumé des bonnes pratiques

**Les 10 commandements du commit atomique** :

1. **Un commit = une modification logique** complète
2. **Commit souvent**, commit tôt
3. **Test avant de commiter** : le projet doit fonctionner
4. **Utilisez git add -p** pour séparer les changements
5. **Message clair** qui décrit le QUOI et le POURQUOI
6. **Séparez les types** : feat, fix, refactor, docs...
7. **Refactoring séparé** des nouvelles fonctionnalités
8. **Pas de git add .** sans vérification
9. **Nettoyez l'historique** avant de pousser
10. **Revue avant push** : `git log --oneline -10`

---

### Cas pratiques

#### Cas 1 : Ajouter une nouvelle page

❌ **Approche monolithique** :
```bash
# Tout coder
# Tout ajouter d'un coup
git add .
git commit -m "Ajoute la page contact"
```

✅ **Approche atomique** :
```bash
# 1. Structure HTML
git add contact.html
git commit -m "feat(contact): ajoute la structure HTML"

# 2. Formulaire
git add contact-form.html contact-form.js
git commit -m "feat(contact): implémente le formulaire"

# 3. Validation
git add validation.js
git commit -m "feat(contact): ajoute la validation du formulaire"

# 4. Styles
git add contact.css
git commit -m "style(contact): ajoute les styles de la page"

# 5. API
git add api/contact.js
git commit -m "feat(contact): connecte le formulaire à l'API"
```

#### Cas 2 : Corriger plusieurs bugs

Vous découvrez 3 bugs différents. Ne les corrigez pas tous dans un seul commit !

✅ **Bon** :
```bash
# Bug 1
git add auth.js
git commit -m "fix: corrige la redirection après login"

# Bug 2
git add form.js
git commit -m "fix: corrige la validation des emails"

# Bug 3
git add api.js
git commit -m "fix: corrige la gestion des erreurs 500"
```

**Avantage** : Si l'un des bugs réapparaît, vous savez exactement quel commit annuler.

---

### Checklist avant chaque commit

Avant de faire `git commit`, posez-vous ces questions :

- [ ] Mon commit fait-il **une seule chose** ?
- [ ] Le projet **fonctionne-t-il** après ce commit ?
- [ ] Mon message de commit est-il **clair et descriptif** ?
- [ ] Ai-je ajouté **seulement** les fichiers concernés par ce changement ?
- [ ] Y a-t-il des changements non liés que je devrais mettre dans un autre commit ?
- [ ] Ai-je vérifié avec `git diff --staged` ?

Si vous répondez "oui" à tout, vous êtes prêt à commiter !

---

### Conclusion

Les commits atomiques sont une compétence qui s'acquiert avec la pratique. Au début, ça peut sembler fastidieux, mais rapidement cela devient une seconde nature.

**Rappelez-vous** :
- Chaque commit raconte une histoire
- Un bon historique est un investissement pour l'avenir
- Vous (et votre équipe) vous remercierez dans 6 mois

Commencez dès aujourd'hui à appliquer ces principes, même sur vos projets personnels. C'est en forgeant qu'on devient forgeron, et c'est en commitant atomiquement qu'on devient un bon utilisateur de Git !

**Prochaine étape** : Maintenant que vous savez créer de bons commits, apprenons à utiliser `.gitignore` pour garder votre dépôt propre.

⏭️ [.gitignore et .gitattributes avancés](/module-07-bonnes-pratiques-et-workflows/03-gitignore-et-gitattributes-avances.md)
