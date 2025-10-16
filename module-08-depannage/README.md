🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 8 : Dépannage et résolution de problèmes

## Introduction au module

Bienvenue dans le Module 8, consacré au **dépannage et à la résolution de problèmes** avec Git. Si vous êtes arrivé jusqu'ici, vous avez déjà acquis une solide compréhension de Git et de ses fonctionnalités. Mais la réalité est que, tôt ou tard, tout utilisateur de Git rencontrera des situations problématiques.

Ce module est votre **guide de premiers secours Git**. Il vous donnera les outils et les connaissances nécessaires pour sortir des situations difficiles et récupérer votre travail, même quand tout semble perdu.

### Pourquoi ce module est essentiel

Git est un outil puissant, mais cette puissance vient avec une certaine complexité. Vous allez inévitablement :
- Faire des erreurs de manipulation
- Vous retrouver dans des états que vous ne comprenez pas
- Perdre temporairement du travail
- Rencontrer des messages d'erreur cryptiques
- Devoir nettoyer un dépôt devenu trop volumineux

**La bonne nouvelle ?** La plupart des problèmes Git ont une solution, et souvent plusieurs ! Git est conçu pour être très résilient : il conserve l'historique et offre de nombreux mécanismes de récupération.

### Ce que vous allez apprendre

Dans ce module, nous allons couvrir les situations problématiques les plus courantes :

1. **Résoudre le "detached HEAD"** : Comprendre cet état particulier et savoir en sortir
2. **Annuler un merge ou un rebase** : Revenir en arrière quand une fusion se passe mal
3. **Récupérer du travail perdu** : Utiliser reflog et reset pour retrouver des commits "perdus"
4. **Gérer les gros fichiers** : Nettoyer un dépôt et éviter les problèmes de performance
5. **Erreurs fréquentes et solutions** : Un catalogue des erreurs courantes avec leurs solutions

### L'état d'esprit du dépannage Git

Avant de plonger dans les solutions techniques, adoptons le bon état d'esprit :

#### 1. Ne paniquez pas

**Git conserve presque tout pendant au moins 30 à 90 jours.** Même si vous pensez avoir tout perdu, il y a de très bonnes chances que vos données soient encore récupérables. Le système de reflog de Git est comme une machine à remonter le temps qui enregistre tous vos mouvements.

#### 2. Lisez les messages d'erreur

Git est généralement très explicite dans ses messages d'erreur. Prenez le temps de **lire attentivement** ce que Git vous dit. Souvent, le message contient directement la solution ou un indice sur ce qui ne va pas.

Exemple :
```
error: Your local changes to the following files would be overwritten by checkout:
    fichier.txt
Please commit your changes or stash them before you switch branches.
```

Ce message vous dit exactement quoi faire : commiter ou utiliser stash avant de changer de branche.

#### 3. Utilisez `git status` constamment

La commande `git status` est votre **boussole** dans Git. Quand vous êtes perdu, commencez toujours par :

```bash
git status
```

Cette commande vous indique :
- Sur quelle branche vous êtes
- Si vous avez des modifications non commitées
- Si vous êtes en plein milieu d'un merge ou d'un rebase
- Ce que vous devez faire ensuite

#### 4. Créez des sauvegardes avant les manipulations risquées

Avant d'effectuer une opération que vous ne maîtrisez pas complètement, **créez une branche de sauvegarde** :

```bash
# Créer une sauvegarde de votre état actuel
git branch sauvegarde-avant-manipulation
```

Si quelque chose tourne mal, vous pourrez toujours revenir à cette branche :

```bash
git checkout sauvegarde-avant-manipulation
```

C'est comme créer un point de sauvegarde dans un jeu vidéo !

#### 5. Expérimentez sans crainte

Git est conçu pour être **expérimental**. N'ayez pas peur d'essayer des commandes ou de faire des erreurs. Avec les bonnes connaissances (que vous allez acquérir dans ce module), vous pourrez toujours revenir en arrière.

**Règle d'or** : Si vous n'avez pas encore partagé vos modifications avec d'autres (pas fait de `git push`), vous pouvez faire à peu près n'importe quoi sans risque. C'est seulement après avoir partagé votre travail qu'il faut être plus prudent.

### Les outils de récupération de Git

Git possède plusieurs mécanismes de sécurité qui vous protègent contre la perte de données :

#### Le reflog (Reference Log)

Le **reflog** est votre filet de sécurité ultime. Il enregistre tous les mouvements de HEAD dans votre dépôt :
- Chaque commit
- Chaque changement de branche
- Chaque merge, rebase, reset
- Pratiquement toute opération Git

Même si un commit semble "perdu", il est probablement encore dans le reflog et récupérable.

#### Le garbage collection

Git ne supprime pas immédiatement les objets "orphelins" (commits qui ne sont plus référencés par aucune branche). Il les garde pendant une période de grâce (par défaut 30 à 90 jours) avant de les nettoyer.

Cela signifie que vous avez généralement **plusieurs semaines** pour récupérer un travail que vous pensiez perdu.

#### Les branches sont bon marché

Dans Git, créer une branche ne coûte presque rien (quelques octets). N'hésitez donc jamais à créer des branches :
- Pour tester une idée
- Avant une manipulation risquée
- Pour sauvegarder un état particulier

### Concepts clés pour le dépannage

Avant d'aborder les solutions spécifiques, voici quelques concepts essentiels à comprendre :

#### HEAD : Le pointeur actuel

**HEAD** est un pointeur qui indique où vous vous trouvez dans votre dépôt. En temps normal, HEAD pointe vers une branche, qui elle-même pointe vers un commit.

```
HEAD -> main -> commit abc123
```

Comprendre où est HEAD est essentiel pour diagnostiquer et résoudre les problèmes.

#### Les trois états de Git

Rappel des trois zones importantes :
1. **Working Directory** : Vos fichiers tels que vous les voyez
2. **Staging Area** (Index) : Les modifications préparées pour le prochain commit
3. **Repository** : L'historique des commits

Savoir dans quelle zone se trouvent vos modifications vous aide à choisir la bonne commande de récupération.

#### Récrire l'historique vs créer de nouveaux commits

Il existe deux philosophies pour annuler des changements :

**Récrire l'historique** (avec `reset`, `rebase`, etc.) :
- ✅ Donne un historique propre
- ❌ Dangereux si déjà partagé avec d'autres

**Créer de nouveaux commits** (avec `revert`) :
- ✅ Sûr pour l'historique partagé
- ❌ Historique moins propre

**Règle simple** : Si vous avez déjà fait `git push`, préférez `revert`. Sinon, vous pouvez utiliser `reset`.

### Comment utiliser ce module

Ce module est organisé comme un **guide de référence**. Vous n'avez pas besoin de tout mémoriser. L'important est de :

1. **Connaître l'existence des solutions** : Savoir que tel problème peut être résolu
2. **Savoir où chercher** : Revenir à ce module quand vous rencontrez un problème
3. **Comprendre les principes** : Avec les concepts de base, vous pourrez résoudre même des problèmes nouveaux

**Suggestion d'utilisation** :
- Lisez d'abord l'ensemble du module pour avoir une vue d'ensemble
- Gardez-le comme référence pour quand vous rencontrez un problème spécifique
- Pratiquez les commandes dans un dépôt de test pour vous familiariser

### Environnement de test sécurisé

Pour vous entraîner sans risque, créez un dépôt de test :

```bash
# Créer un dossier de test
mkdir git-test-depannage
cd git-test-depannage

# Initialiser Git
git init

# Créer quelques commits de test
echo "Fichier 1" > fichier1.txt
git add fichier1.txt
git commit -m "Premier commit"

echo "Fichier 2" > fichier2.txt
git add fichier2.txt
git commit -m "Deuxième commit"

echo "Fichier 3" > fichier3.txt
git add fichier3.txt
git commit -m "Troisième commit"
```

Dans ce dépôt, vous pouvez **expérimenter librement** toutes les techniques de dépannage sans risque de perdre un vrai projet.

### Quand demander de l'aide

Certaines situations nécessitent de demander de l'aide :
- Quand vous avez déjà partagé des modifications (après `git push`) et devez récrire l'historique
- Quand le dépôt est corrompu (`git fsck` signale des erreurs)
- Quand vous travaillez sur un projet d'équipe et n'êtes pas sûr de l'impact de vos actions

**Ressources d'aide** :
- Votre équipe ou vos collègues
- Stack Overflow avec le tag "git"
- La documentation officielle Git : https://git-scm.com/doc
- Les communautés Git sur Discord ou Reddit

N'ayez pas honte de demander de l'aide ! Même les développeurs expérimentés consultent régulièrement la documentation et posent des questions.

### Commandes de diagnostic essentielles

Avant d'aborder les solutions spécifiques, voici les commandes que vous utiliserez constamment pour diagnostiquer les problèmes :

```bash
# Voir l'état actuel (commande la plus importante !)
git status

# Voir l'historique récent
git log --oneline -10

# Voir l'historique de tous vos mouvements
git reflog

# Voir les différences non commitées
git diff

# Voir ce qui est dans le staging area
git diff --cached

# Lister toutes les branches
git branch -a

# Voir les dépôts distants
git remote -v

# Vérifier l'intégrité du dépôt
git fsck
```

Mémorisez au moins `git status` et `git reflog` : ces deux commandes vous sauveront dans la majorité des situations.

### Structure du module

Chaque section de ce module suit la même structure :
1. **Description du problème** : Qu'est-ce qui ne va pas ?
2. **Causes** : Pourquoi ce problème se produit-il ?
3. **Symptômes** : Comment reconnaître ce problème ?
4. **Solutions** : Comment le résoudre, étape par étape
5. **Prévention** : Comment éviter ce problème à l'avenir

Cette approche vous permet de comprendre non seulement **comment** résoudre un problème, mais aussi **pourquoi** il se produit et comment l'éviter.

### Philosophie de Git : "Rien n'est jamais vraiment perdu"

Git a été conçu par Linus Torvalds pour gérer le développement du noyau Linux, un projet critique impliquant des milliers de développeurs. La robustesse et la récupérabilité étaient donc des priorités absolues.

Résultat : Git est **incroyablement résilient**. Il conserve presque tout, maintient plusieurs couches de sécurité, et offre de nombreux mécanismes de récupération.

**Citation de Linus Torvalds** :
> "Les seuls vrais problèmes sont les choses que vous n'avez jamais commitées. Tout ce qui est commité peut être récupéré."

Cette philosophie devrait vous rassurer : tant que vous avez fait au moins un `git add` ou un `git commit`, vos données sont probablement récupérables.

### À retenir avant de commencer

- **Git est votre ami** : Il essaie de protéger vos données
- **Les erreurs sont normales** : Elles font partie de l'apprentissage
- **Presque tout est récupérable** : Grâce au reflog et au garbage collection
- **Lisez les messages** : Git communique clairement ce qui ne va pas
- **Expérimentez** : C'est la meilleure façon d'apprendre
- **Créez des sauvegardes** : Une branche ne coûte rien
- **Ne paniquez pas** : Respirez, consultez ce guide, et résolvez le problème étape par étape

### Prêt à devenir un expert du dépannage Git ?

Maintenant que vous avez le bon état d'esprit et comprenez les principes fondamentaux, vous êtes prêt à affronter n'importe quel problème Git !

Les sections suivantes vous guideront à travers les situations problématiques les plus courantes, avec des solutions détaillées et testées. Rappelez-vous : chaque problème que vous résolvez vous rend plus compétent et plus confiant avec Git.

Commençons par le premier problème courant : le mystérieux "detached HEAD"...

---

⏭️ [Résoudre "detached HEAD"](/module-08-depannage/01-resoudre-detached-head.md)
