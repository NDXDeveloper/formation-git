# 6.2. Commits clairs et convention de messages

## Pourquoi les messages de commit sont importants

Un bon message de commit est comme une note explicative que vous laissez à votre futur vous et à vos collègues. Imaginez que vous revenez sur un projet après plusieurs mois ou que vous devez comprendre pourquoi un changement a été fait par un collègue : des messages de commit clairs sont essentiels pour comprendre l'historique du projet.

Les bénéfices de bons messages de commit :

- **Facilitent la maintenance** du code à long terme
- **Accélèrent la revue** de code
- **Améliorent la collaboration** en équipe
- **Permettent de générer des changelogs** automatiquement
- **Aident à identifier l'origine des bugs** plus rapidement

## Les principes d'un bon commit

### 1. Commits atomiques

Un commit devrait représenter **une seule modification logique**. Évitez les commits qui mélangent plusieurs fonctionnalités ou corrections sans rapport entre elles.

❌ Mauvais exemple : "Ajout formulaire de contact + correction bug panier + mise à jour footer"
✅ Bon exemple : Faire trois commits séparés, un pour chaque changement

### 2. Messages descriptifs et concis

Un bon message de commit répond à ces questions :
- **Pourquoi** ce changement a été fait ?
- **Quel impact** a-t-il sur le système ?
- **Quelles limitations** ou effets secondaires doit-on connaître ?

### 3. Structure recommandée

La convention la plus répandue est de structurer vos messages ainsi :

```
<type>(<portée>): <sujet>

<description>

<références>
```

- **Type** : catégorie du changement (feat, fix, docs, style, etc.)
- **Portée** (optionnelle) : module/composant modifié (auth, api, etc.)
- **Sujet** : description courte (moins de 50 caractères)
- **Description** (optionnelle) : explication détaillée
- **Références** (optionnelles) : liens vers issues/tickets (#123)

## Conventions de messages populaires

### Convention Conventional Commits

La convention [Conventional Commits](https://www.conventionalcommits.org/) est aujourd'hui une référence. Elle définit ce format :

```
<type>[portée optionnelle]: <description>

[corps optionnel]

[pied de page optionnel]
```

Les types principaux sont :

- **feat** : nouvelle fonctionnalité
- **fix** : correction de bug
- **docs** : modification de documentation
- **style** : formatage, point-virgules manquants, etc.
- **refactor** : refactorisation du code
- **test** : ajout ou modification de tests
- **chore** : tâches de maintenance, dépendances, etc.

### Exemples de bons messages

```
feat(auth): ajouter l'authentification par Google

Implémente l'authentification OAuth avec Google.
Les utilisateurs peuvent maintenant se connecter avec leur compte Google
en plus de l'authentification par email/mot de passe.

Closes #45
```

```
fix(panier): corriger le calcul du total avec les remises

Le total du panier ne prenait pas en compte les remises lorsque
plusieurs produits étaient ajoutés dans un certain ordre.

Fixes #67
```

```
docs: mettre à jour le README avec les instructions d'installation
```

```
refactor(api): simplifier la gestion des erreurs
```

## Conseils pratiques

### 1. Utilisez l'impératif présent

Rédigez comme si vous donniez un ordre :
- ✅ "Ajouter une fonctionnalité" (et non "Ajout" ou "Ajouté")
- ✅ "Corriger un bug" (et non "Correction" ou "Corrigé")

### 2. Première lettre en minuscule ou majuscule ?

Les deux pratiques existent, l'important est la cohérence :
- "ajouter une fonctionnalité"
- "Ajouter une fonctionnalité"

Choisissez une convention et respectez-la dans tout le projet.

### 3. Pas de point à la fin du sujet

Par convention, on ne met pas de point à la fin de la ligne de sujet.

### 4. Limitez la longueur

- **Sujet** : idéalement moins de 50 caractères
- **Corps** : généralement limité à 72 caractères par ligne

### 5. Utilisez le corps du message pour expliquer "pourquoi" et non "comment"

Le "comment" est visible dans le code lui-même via `git diff`. Le message devrait expliquer pourquoi ce changement était nécessaire.

## Outils pour vous aider

### Hooks Git

Vous pouvez configurer des hooks Git (voir Module 5.6) pour vérifier que vos messages de commit respectent la convention choisie.

### Commitizen

[Commitizen](https://github.com/commitizen/cz-cli) est un outil en ligne de commande qui vous guide dans la création de messages de commit conformes.

Installation :
```bash
npm install -g commitizen
```

Utilisation (au lieu de `git commit`) :
```bash
git cz
```

### Commitlint

[Commitlint](https://github.com/conventional-changelog/commitlint) vérifie que vos messages suivent la convention choisie.

## Exemples à éviter

❌ **Messages trop vagues** :
- "Fix bug"
- "Update code"
- "Changes"
- "WIP" (Work In Progress)

❌ **Messages trop longs** qui dépassent largement les 50 caractères pour le sujet

❌ **Messages qui ne décrivent que les fichiers modifiés** :
- "Mise à jour de index.js et style.css"

## En résumé

- Faites des commits **atomiques** (une seule modification logique)
- Suivez une **convention** cohérente (comme Conventional Commits)
- Utilisez le **type** pour catégoriser vos changements (feat, fix, etc.)
- Écrivez une **description claire et concise** au présent de l'impératif
- Expliquez le **pourquoi** dans le corps du message si nécessaire
- Référencez les **issues** concernées

En suivant ces bonnes pratiques, vous créerez un historique Git propre, exploitable et professionnel qui facilitera le travail de toute l'équipe !
