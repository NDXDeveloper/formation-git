🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 8. Fork et Pull Request (PR)

### Introduction

Vous avez trouvé un bug dans votre bibliothèque JavaScript préférée. Ou vous voulez ajouter une fonctionnalité à un projet open source. Comment contribuer au code de quelqu'un d'autre ?

C'est là qu'interviennent les **forks** et les **Pull Requests** (ou Merge Requests sur GitLab). Ce sont les mécanismes qui permettent à des millions de développeurs de collaborer sur des projets open source dans le monde entier.

### Qu'est-ce qu'un Fork ?

Un **fork** est une copie complète d'un dépôt Git sous votre propre compte GitHub/GitLab/Bitbucket. C'est comme photocopier un livre entier pour pouvoir écrire vos propres annotations dessus.

**Analogie :**

Imaginez une recette de cuisine populaire sur Internet :
- **Dépôt original** : La recette publiée par le chef
- **Fork** : Votre copie personnelle de la recette
- **Modifications** : Vos ajustements (plus de sel, moins de sucre)
- **Pull Request** : Proposer vos améliorations au chef original

### Pourquoi forker ?

**Vous n'avez pas de droits d'écriture**
Vous ne pouvez pas pousser directement vers le dépôt d'origine. Le fork crée votre propre espace où vous avez tous les droits.

**Expérimentation sûre**
Vous pouvez modifier le code sans craindre de casser le projet original. Votre fork est complètement indépendant.

**Contribution au projet**
Après vos modifications, vous pouvez proposer vos changements au projet original via une Pull Request.

**Personnalisation**
Vous pouvez adapter un projet à vos besoins spécifiques sans affecter l'original.

**Apprentissage**
Forker un projet est un excellent moyen d'apprendre en étudiant et modifiant du code réel.

### Fork vs Clone vs Branch

Ces trois concepts sont souvent confondus. Voici les différences :

**Clone**
Copie un dépôt distant vers votre machine locale. Reste lié au dépôt original.
```bash
git clone https://github.com/auteur/projet.git
```

**Branch**
Crée une nouvelle ligne de développement dans le même dépôt. Tout reste dans le même projet.
```bash
git checkout -b ma-branche
```

**Fork**
Copie complète du dépôt sous votre compte sur la plateforme (GitHub/GitLab). Crée un nouveau dépôt indépendant.
- Se fait via l'interface web (bouton "Fork")

**Tableau récapitulatif :**

| Action | Clone | Branch | Fork |
|--------|-------|--------|------|
| **Où ?** | Local → Local | Même dépôt | Serveur → Serveur |
| **Indépendant ?** | Non, lié | Non, lié | Oui, séparé |
| **Droits nécessaires** | Lecture | Lecture/Écriture | Aucun (public) |
| **Usage** | Travail local | Fonctionnalités isolées | Contribution externe |

### Comment forker un projet

#### Sur GitHub

1. **Trouvez le projet** que vous voulez forker
2. **Cliquez sur "Fork"** en haut à droite de la page
3. **Choisissez votre compte** (si vous avez plusieurs comptes ou organisations)
4. **Attendez quelques secondes** pendant la copie

Vous êtes maintenant sur votre fork : `github.com/votre-nom/projet`

#### Sur GitLab

1. Trouvez le projet
2. Cliquez sur "Fork" en haut de la page
3. Sélectionnez votre namespace (compte personnel ou groupe)
4. Le fork est créé

#### Sur Bitbucket

1. Trouvez le projet
2. Cliquez sur le menu "..." puis "Fork"
3. Configurez le nom et la visibilité
4. Cliquez sur "Fork repository"

### Workflow complet de contribution

Voici le processus complet pour contribuer à un projet open source :

#### Étape 1 : Forker le projet

Sur GitHub, cliquez sur "Fork". Vous avez maintenant :
- **Dépôt original** : `github.com/auteur-original/projet`
- **Votre fork** : `github.com/votre-nom/projet`

#### Étape 2 : Cloner votre fork localement

```bash
git clone https://github.com/votre-nom/projet.git
cd projet
```

#### Étape 3 : Ajouter le dépôt original comme remote "upstream"

Cela vous permettra de récupérer les mises à jour du projet original :

```bash
git remote add upstream https://github.com/auteur-original/projet.git
```

Vérifiez vos remotes :
```bash
git remote -v
```

Vous devriez voir :
```
origin    https://github.com/votre-nom/projet.git (fetch)
origin    https://github.com/votre-nom/projet.git (push)
upstream  https://github.com/auteur-original/projet.git (fetch)
upstream  https://github.com/auteur-original/projet.git (push)
```

#### Étape 4 : Créer une branche pour votre contribution

Ne travaillez jamais directement sur `main` ou `master` :

```bash
git checkout -b fix-bug-login
```

Nommez votre branche de manière descriptive : `fix-`, `feature-`, `add-`, `update-`, etc.

#### Étape 5 : Faire vos modifications

Travaillez normalement :

```bash
# Modifier des fichiers
nano src/login.js

# Ajouter et committer
git add src/login.js
git commit -m "Fix: Correction du bug de connexion"

# Continuer si nécessaire
git add .
git commit -m "Add: Tests pour le fix"
```

#### Étape 6 : Pousser votre branche vers votre fork

```bash
git push origin fix-bug-login
```

#### Étape 7 : Créer une Pull Request

**Sur GitHub :**

1. Allez sur votre fork : `github.com/votre-nom/projet`
2. Vous verrez un bandeau jaune : "Compare & pull request"
3. Cliquez dessus
4. Remplissez le formulaire de la Pull Request :
   - **Titre** : Description courte et claire
   - **Description** : Expliquez vos changements, pourquoi, comment
   - Référencez les issues liées si applicable (#123)
5. Cliquez sur "Create pull request"

Félicitations ! Votre contribution est maintenant soumise au projet original.

### Qu'est-ce qu'une Pull Request ?

Une **Pull Request (PR)** est une demande pour que vos modifications soient intégrées dans le projet original. C'est comme dire : "J'ai fait des changements, pouvez-vous les examiner et les inclure ?"

**Sur GitLab**, on parle de **Merge Request (MR)** mais le concept est identique.

**Composants d'une Pull Request :**

**Titre**
Un résumé court et descriptif de vos changements
```
Fix: Correction du bug de déconnexion automatique
```

**Description**
Explication détaillée :
- Quel problème vous résolvez
- Comment vous l'avez résolu
- Quels tests vous avez effectués
- Captures d'écran si pertinent

**Commits**
La liste de tous vos commits dans cette PR

**Fichiers modifiés**
Vue détaillée de toutes les modifications (diff)

**Conversation**
Discussion entre vous et les mainteneurs du projet

### Anatomie d'une bonne Pull Request

#### Titre clair et descriptif

❌ **Mauvais :**
```
Fix bug
Update code
Changes
```

✅ **Bons :**
```
Fix: Correction de la fuite mémoire dans le composant UserList
Add: Ajout de la validation email côté serveur
Update: Migration vers React 18
```

#### Description complète

**Template de description :**

```markdown
## Problème
Le formulaire de connexion ne validait pas les emails correctement.

## Solution
- Ajout d'une regex de validation côté client
- Ajout de validation côté serveur
- Ajout de tests unitaires

## Tests effectués
- ✅ Email valide accepté
- ✅ Email invalide rejeté
- ✅ Tests unitaires passent

## Captures d'écran
[Si applicable]

## Checklist
- [x] Tests ajoutés
- [x] Documentation mise à jour
- [x] Code reviewé en local
```

#### Commits atomiques et clairs

Chaque commit doit être logique et isolé :

❌ **Mauvais :**
```
git commit -m "fix"
git commit -m "wip"
git commit -m "more changes"
```

✅ **Bons :**
```
git commit -m "Fix: Correction validation email"
git commit -m "Test: Ajout tests validation email"
git commit -m "Docs: Mise à jour documentation API"
```

### Cycle de vie d'une Pull Request

**1. Création**
Vous créez la PR avec vos changements.

**2. Review automatique**
Des outils automatisés vérifient :
- Les tests passent
- Le code respecte les conventions
- Pas de problèmes de sécurité évidents

**3. Review humaine**
Les mainteneurs examinent votre code et peuvent :
- **Approuver** : Tout est bon !
- **Demander des changements** : Modifications nécessaires
- **Commenter** : Questions ou suggestions

**4. Échanges et modifications**
Vous répondez aux commentaires et faites des ajustements :

```bash
# Faire les modifications demandées
git add fichier-modifié.js
git commit -m "Review: Prise en compte des commentaires"
git push origin fix-bug-login
```

La PR se met à jour automatiquement !

**5. Approbation**
Une fois que tout est validé, un mainteneur approuve la PR.

**6. Merge**
Le mainteneur fusionne vos changements dans le projet original.

**7. Nettoyage**
Votre branche peut être supprimée (automatiquement ou manuellement).

### Maintenir votre fork à jour

Pendant que vous travaillez sur votre contribution, le projet original continue d'évoluer. Vous devez régulièrement synchroniser votre fork.

**Récupérer les changements du projet original :**

```bash
# Récupérer les nouveautés d'upstream
git fetch upstream

# Se placer sur main
git checkout main

# Fusionner les changements d'upstream
git merge upstream/main

# Pousser vers votre fork
git push origin main
```

**Mettre à jour votre branche de travail :**

```bash
# Se placer sur votre branche
git checkout fix-bug-login

# Rebaser sur main à jour
git rebase main

# Pousser avec force (votre branche a été réécrite)
git push origin fix-bug-login --force-with-lease
```

### Répondre aux reviews

Les mainteneurs peuvent demander des changements. Voici comment bien réagir :

**Lisez attentivement**
Comprenez bien les commentaires avant de modifier.

**Répondez poliment**
```markdown
Merci pour la review ! Vous avez raison, je vais simplifier cette fonction.
```

**Faites les changements**
```bash
# Modifier le code
git add .
git commit -m "Review: Simplification de la fonction validate()"
git push origin fix-bug-login
```

**Marquez les conversations comme résolues**
Une fois que vous avez traité un commentaire, marquez-le comme résolu.

**Soyez patient**
Les mainteneurs sont souvent bénévoles. La review peut prendre du temps.

**Apprenez de la review**
Les commentaires vous aident à progresser. C'est une opportunité d'apprentissage !

### Différences entre plateformes

| Fonctionnalité | GitHub | GitLab | Bitbucket |
|----------------|--------|---------|-----------|
| **Nom** | Pull Request | Merge Request | Pull Request |
| **Draft PR** | Oui | Oui (WIP) | Non |
| **Auto-merge** | Oui | Oui | Oui |
| **Review multiple** | Oui | Oui | Oui |
| **Suggestions de code** | Oui | Oui | Limité |
| **Templates PR** | Oui | Oui | Oui |

Le fonctionnement de base reste identique sur toutes les plateformes.

### Draft Pull Request

Sur GitHub et GitLab, vous pouvez créer une **Draft PR** (brouillon) :

**Quand utiliser un Draft ?**
- Vous voulez du feedback précoce
- Votre travail n'est pas terminé
- Vous voulez montrer votre progression

**Créer un Draft PR :**
- Sur GitHub : Choisir "Create draft pull request" au lieu de "Create pull request"
- Sur GitLab : Préfixer le titre avec "Draft:" ou "WIP:"

**Marquer comme prête :**
Une fois terminé, cliquez sur "Ready for review" pour transformer le draft en PR normale.

### Templates de Pull Request

Les projets peuvent avoir des templates pour standardiser les PR :

**Fichier `.github/pull_request_template.md` :**

```markdown
## Description
Décrivez vos changements ici

## Type de changement
- [ ] Bug fix
- [ ] Nouvelle fonctionnalité
- [ ] Breaking change
- [ ] Documentation

## Checklist
- [ ] Tests ajoutés/mis à jour
- [ ] Documentation mise à jour
- [ ] Code reviewé localement
- [ ] Tous les tests passent
```

Ce template s'affiche automatiquement lors de la création d'une PR.

### Conventions de contribution

La plupart des projets open source ont un fichier `CONTRIBUTING.md` qui explique :
- Comment contribuer
- Les standards de code
- Le processus de review
- Comment soumettre une PR

**Lisez toujours ce fichier avant de contribuer !**

Exemples de conventions courantes :
- **Style de code** : Linter spécifique (ESLint, Prettier)
- **Messages de commit** : Format Conventional Commits
- **Tests** : Couverture minimum requise
- **Documentation** : Mise à jour obligatoire

### Après le merge de votre PR

Une fois votre PR fusionnée :

**1. Synchronisez votre fork**
```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

**2. Supprimez votre branche locale**
```bash
git branch -d fix-bug-login
```

**3. Supprimez votre branche distante**
```bash
git push origin --delete fix-bug-login
```

Ou sur GitHub, cliquez sur "Delete branch" dans la PR fusionnée.

**4. Célébrez !**
Votre contribution fait maintenant partie du projet. Vous êtes officiellement contributeur !

### Gérer les conflits dans une PR

Si le projet original a évolué pendant votre travail, des conflits peuvent survenir.

**Résolution :**

```bash
# Récupérer les dernières modifications
git fetch upstream

# Rebaser votre branche sur upstream/main
git checkout fix-bug-login
git rebase upstream/main

# Résoudre les conflits
# (éditer les fichiers marqués en conflit)

git add fichier-résolu.js
git rebase --continue

# Pousser avec force
git push origin fix-bug-login --force-with-lease
```

La PR se mettra automatiquement à jour sans conflits.

### Fermer une Pull Request

Vous pouvez fermer votre propre PR si :
- Vous avez changé d'avis
- Le problème a été résolu autrement
- Vous voulez recommencer différemment

Sur GitHub/GitLab, cliquez sur "Close pull request".

Les mainteneurs peuvent aussi fermer votre PR si :
- Elle n'est pas alignée avec la vision du projet
- Le code ne répond pas aux standards
- Le problème est résolu autrement

Ce n'est pas un échec ! C'est normal dans l'open source.

### Conseils pour une première contribution

**Commencez petit**
Cherchez des issues marquées "good first issue" ou "beginner-friendly".

**Présentez-vous**
Dans votre première PR, mentionnez que c'est votre première contribution. La communauté sera compréhensive !

**Posez des questions**
Si quelque chose n'est pas clair, demandez. Mieux vaut demander que faire une erreur.

**Soyez patient et respectueux**
Les mainteneurs sont souvent bénévoles. Ils font de leur mieux.

**Acceptez les refus**
Parfois, une PR n'est pas acceptée. Ce n'est pas personnel. Apprenez et continuez !

**Lisez le code existant**
Avant de contribuer, explorez le projet pour comprendre son architecture et ses conventions.

### Exemples de bonnes premières contributions

**Corrections de documentation**
- Typos dans le README
- Amélioration des exemples
- Traductions

**Corrections de bugs simples**
- Problèmes de validation
- Bugs d'interface utilisateur mineurs
- Messages d'erreur améliorés

**Ajout de tests**
- Tests manquants pour des fonctions existantes
- Amélioration de la couverture de tests

**Améliorations mineures**
- Ajout de commentaires
- Refactoring léger
- Amélioration de messages d'erreur

### Outils utiles pour les PR

**GitHub CLI**
Créer des PR depuis le terminal :
```bash
gh pr create --title "Fix bug" --body "Description"
gh pr list
gh pr view 123
```

**Extensions navigateur**
- Refined GitHub : Améliore l'interface GitHub
- Octotree : Navigation en arborescence du code

**Intégrations IDE**
La plupart des IDE ont des extensions Git avancées pour gérer les PR.

### Résumé du workflow Fork + PR

```bash
# 1. Forker sur GitHub (via l'interface web)

# 2. Cloner votre fork
git clone https://github.com/votre-nom/projet.git
cd projet

# 3. Ajouter upstream
git remote add upstream https://github.com/auteur-original/projet.git

# 4. Créer une branche
git checkout -b ma-contribution

# 5. Faire vos modifications
# ... éditer des fichiers ...
git add .
git commit -m "Description des changements"

# 6. Pousser vers votre fork
git push origin ma-contribution

# 7. Créer la Pull Request sur GitHub

# 8. Répondre aux reviews et ajuster
git add .
git commit -m "Review: Ajustements demandés"
git push origin ma-contribution

# 9. Une fois fusionnée, nettoyer
git checkout main
git pull upstream main
git push origin main
git branch -d ma-contribution
git push origin --delete ma-contribution
```

### Ce qu'il faut retenir

Le système Fork + Pull Request est au cœur de la collaboration open source moderne :

- **Fork** : Votre copie personnelle du projet où vous avez tous les droits
- **Pull Request** : Votre proposition de changements vers le projet original

**Le processus est simple :**
1. Fork → Clone → Branch → Commit → Push → Pull Request

**Règles d'or :**
- Toujours créer une branche spécifique pour votre contribution
- Écrire des messages de commit clairs et des descriptions de PR détaillées
- Être patient et respectueux avec les mainteneurs
- Apprendre des reviews et accepter les refus avec grâce

Contribuer à l'open source via des Pull Requests est une expérience enrichissante qui améliore vos compétences, construit votre portfolio, et vous permet de redonner à la communauté. N'ayez pas peur de faire votre première PR - tout le monde a commencé quelque part !

Votre première Pull Request fusionnée sera un moment mémorable dans votre parcours de développeur. Alors lancez-vous, la communauté open source vous attend !

⏭️ [Module 6 : Fonctions avancées](/module-06-fonctions-avancees/README.md)
