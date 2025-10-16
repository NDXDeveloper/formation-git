🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées

## Introduction au Module 6

Bienvenue dans le **Module 6** de votre formation Git ! Ce module marque une étape importante dans votre apprentissage : vous allez passer du niveau intermédiaire au niveau **avancé** de Git.

Jusqu'à présent, vous avez appris les bases de Git (commits, branches, merge) et découvert le travail collaboratif avec les dépôts distants. Vous êtes maintenant capable de gérer un projet Git au quotidien. Mais Git offre bien plus : des outils puissants qui peuvent transformer votre façon de travailler et résoudre des problèmes complexes avec élégance.

Ce module vous présente les **fonctions avancées** de Git qui font la différence entre un utilisateur compétent et un utilisateur expert. Ces outils sont utilisés quotidiennement par les développeurs professionnels et peuvent considérablement améliorer votre productivité.

---

### À qui s'adresse ce module ?

Ce module est conçu pour vous si :

✅ Vous maîtrisez les commandes Git de base (add, commit, push, pull, merge)

✅ Vous comprenez les concepts de branches et de dépôts distants

✅ Vous travaillez régulièrement avec Git et voulez aller plus loin

✅ Vous cherchez à résoudre des problèmes spécifiques (récupérer des commits perdus, nettoyer l'historique, déboguer)

✅ Vous voulez automatiser votre workflow et gagner du temps

✅ Vous souhaitez acquérir les compétences d'un développeur professionnel

**Prérequis :** Avoir complété les Modules 1 à 5 de cette formation, ou avoir une expérience équivalente avec Git.

---

### Pourquoi apprendre les fonctions avancées ?

Les outils que vous allez découvrir dans ce module ne sont pas des "gadgets" : ce sont des solutions à des problèmes réels que vous rencontrerez (ou avez déjà rencontrés) dans votre travail quotidien.

#### Situations courantes que ce module vous aidera à résoudre :

**Scénario 1 : "J'ai besoin de changer de branche mais mon travail n'est pas fini"**
→ Solution : `git stash` (Section 1)

**Scénario 2 : "J'ai besoin d'une correction qui existe sur une autre branche"**
→ Solution : `git cherry-pick` (Section 2)

**Scénario 3 : "Mon historique est un désordre avant de créer ma Pull Request"**
→ Solution : `git rebase -i` (Section 3)

**Scénario 4 : "J'ai supprimé des commits importants par erreur !"**
→ Solution : `git reflog` (Section 4)

**Scénario 5 : "Un bug est apparu mais je ne sais pas quel commit l'a introduit"**
→ Solution : `git bisect` (Section 5)

**Scénario 6 : "Qui a écrit cette ligne de code et pourquoi ?"**
→ Solution : `git blame` et `git log -S` (Section 6)

**Scénario 7 : "Comment marquer les versions de production ?"**
→ Solution : Tags et releases (Section 7)

**Scénario 8 : "Comment intégrer une bibliothèque externe dans mon projet ?"**
→ Solution : Submodules (Section 8)

**Scénario 9 : "Comment automatiser les vérifications avant chaque commit ?"**
→ Solution : Hooks Git (Section 9)

**Scénario 10 : "Je tape les mêmes commandes Git 50 fois par jour"**
→ Solution : Alias Git (Section 10)

---

### Vue d'ensemble du module

Ce module est organisé en **10 sections** qui couvrent les fonctionnalités avancées les plus utiles de Git. Chaque section est indépendante : vous pouvez les suivre dans l'ordre ou aller directement à celle qui vous intéresse.

#### 1. Mettre de côté temporairement (git stash)

Apprenez à sauvegarder temporairement votre travail en cours sans faire de commit. Indispensable pour changer rapidement de contexte.

**Ce que vous apprendrez :**
- Mettre de côté vos modifications avec `git stash`
- Récupérer votre travail avec `git stash pop`
- Gérer plusieurs stash simultanément
- Créer des branches à partir d'un stash

**Temps estimé :** 20 minutes

---

#### 2. Cherry-pick : appliquer des commits spécifiques

Découvrez comment copier un commit d'une branche vers une autre sans faire de merge complet. Parfait pour les corrections de bugs urgentes.

**Ce que vous apprendrez :**
- Copier un commit spécifique avec `git cherry-pick`
- Appliquer plusieurs commits d'un coup
- Gérer les conflits de cherry-pick
- Savoir quand utiliser cherry-pick vs merge

**Temps estimé :** 25 minutes

---

#### 3. Nettoyer l'historique avant de partager (rebase interactif)

Maîtrisez le rebase interactif pour créer un historique Git propre et professionnel. Réorganisez, fusionnez et nettoyez vos commits avant de les partager.

**Ce que vous apprendrez :**
- Utiliser `git rebase -i` pour modifier l'historique
- Fusionner des commits (squash, fixup)
- Modifier des messages de commit
- Réorganiser et supprimer des commits
- La règle d'or : ne jamais rebase du code partagé

**Temps estimé :** 35 minutes

---

#### 4. Reflog : récupérer des commits perdus

Découvrez le filet de sécurité ultime de Git. Apprenez à récupérer des commits "supprimés", des branches effacées, et à revenir en arrière après une erreur.

**Ce que vous apprendrez :**
- Comprendre le reflog (reference log)
- Récupérer des commits perdus avec `git reflog`
- Annuler un reset accidentel
- Restaurer une branche supprimée
- Utiliser le reflog pour déboguer

**Temps estimé :** 25 minutes

---

#### 5. Déboguer avec git bisect

Utilisez la recherche binaire pour trouver rapidement quel commit a introduit un bug. Un outil puissant qui peut vous faire gagner des heures de débogage.

**Ce que vous apprendrez :**
- Utiliser `git bisect` pour trouver un commit problématique
- Comprendre la recherche binaire
- Automatiser le bisect avec des tests
- Identifier efficacement l'origine d'un bug

**Temps estimé :** 30 minutes

---

#### 6. Tracer les modifications (git blame et git log -S)

Devenez un détective du code ! Apprenez à trouver qui a modifié une ligne, quand, et pourquoi. Recherchez dans l'historique pour comprendre l'évolution du code.

**Ce que vous apprendrez :**
- Utiliser `git blame` pour voir l'origine de chaque ligne
- Rechercher dans l'historique avec `git log -S`
- Tracer l'évolution d'une fonctionnalité
- Comprendre le contexte des modifications

**Temps estimé :** 30 minutes

---

#### 7. Tags et releases

Apprenez à marquer les versions importantes de votre projet et à créer des releases professionnelles. Essentiel pour la gestion de versions en production.

**Ce que vous apprendrez :**
- Créer des tags annotés pour marquer des versions
- Suivre le versionnement sémantique (SemVer)
- Gérer les tags sur les dépôts distants
- Créer des releases sur GitHub/GitLab
- Générer des notes de version (changelog)

**Temps estimé :** 30 minutes

---

#### 8. Submodules et sous-projets

Gérez des dépendances Git dans Git. Apprenez à intégrer des projets externes tout en gardant leurs historiques séparés.

**Ce que vous apprendrez :**
- Ajouter un submodule à votre projet
- Cloner un projet avec des submodules
- Mettre à jour et modifier des submodules
- Comprendre quand utiliser des submodules
- L'alternative : git subtree

**Temps estimé :** 35 minutes

---

#### 9. Hooks Git (automatisation)

Automatisez votre workflow avec des scripts qui s'exécutent automatiquement lors d'événements Git. Vérifiez votre code avant chaque commit, lancez des tests automatiquement, et bien plus.

**Ce que vous apprendrez :**
- Comprendre les hooks Git (pre-commit, commit-msg, pre-push)
- Créer des hooks personnalisés
- Partager les hooks avec votre équipe (Husky)
- Automatiser les vérifications de code
- Valider les messages de commit

**Temps estimé :** 35 minutes

---

#### 10. Alias Git pour la productivité

Créez vos propres raccourcis pour les commandes Git que vous utilisez le plus. Transformez des commandes longues en 2-3 lettres et gagnez des heures par mois.

**Ce que vous apprendrez :**
- Créer des alias Git simples et avancés
- Configuration d'alias professionnels
- Alias pour différents workflows
- Meilleures pratiques pour les alias
- Gagner du temps au quotidien

**Temps estimé :** 25 minutes

---

### Compétences acquises après ce module

À la fin de ce module, vous serez capable de :

✅ **Gérer des situations complexes** : Récupérer du travail perdu, résoudre des problèmes d'historique

✅ **Maintenir un historique propre** : Créer des commits bien organisés et faciles à comprendre

✅ **Déboguer efficacement** : Trouver rapidement l'origine d'un bug dans l'historique

✅ **Automatiser votre workflow** : Gagner du temps avec hooks et alias

✅ **Travailler professionnellement** : Utiliser les outils des développeurs experts

✅ **Gérer des versions** : Créer des releases et gérer des tags correctement

✅ **Collaborer à un niveau avancé** : Maîtriser les outils pour les projets complexes

---

### Comment utiliser ce module ?

#### Approche recommandée : Apprentissage séquentiel

Si vous découvrez les fonctions avancées de Git, nous recommandons de suivre les sections dans l'ordre :

1. **Sections 1-4** : Outils de manipulation de l'historique (stash, cherry-pick, rebase, reflog)
2. **Sections 5-6** : Outils de débogage (bisect, blame)
3. **Sections 7-8** : Gestion de projets (tags, submodules)
4. **Sections 9-10** : Automatisation et productivité (hooks, alias)

**Durée totale estimée :** 4-5 heures (peut être répartie sur plusieurs jours)

#### Approche alternative : Apprentissage par besoin

Si vous avez un besoin spécifique, allez directement à la section correspondante :

| Besoin | Section |
|--------|---------|
| Sauvegarder temporairement du travail | Section 1 (Stash) |
| Copier un commit d'une branche | Section 2 (Cherry-pick) |
| Nettoyer l'historique | Section 3 (Rebase interactif) |
| Récupérer du travail perdu | Section 4 (Reflog) |
| Trouver quel commit a cassé le code | Section 5 (Bisect) |
| Comprendre qui a écrit du code | Section 6 (Blame) |
| Créer des versions | Section 7 (Tags) |
| Intégrer des dépendances Git | Section 8 (Submodules) |
| Automatiser des vérifications | Section 9 (Hooks) |
| Créer des raccourcis | Section 10 (Alias) |

Chaque section est **indépendante** et peut être étudiée séparément.

---

### Conseils pour réussir ce module

#### 1. Pratiquez dans un dépôt de test

Les fonctions avancées modifient souvent l'historique. Créez un dépôt de test pour expérimenter sans risque :

```bash
mkdir git-advanced-practice
cd git-advanced-practice
git init
# Créez quelques commits de test
echo "test" > file.txt
git add .
git commit -m "Test commit 1"
# etc.
```

#### 2. N'ayez pas peur de faire des erreurs

Avec le reflog (Section 4), vous pouvez récupérer presque toutes les erreurs. Expérimentez librement !

#### 3. Prenez des notes

Créez votre propre aide-mémoire des commandes que vous trouvez utiles. Vous y reviendrez souvent.

#### 4. Intégrez progressivement

Ne cherchez pas à tout utiliser immédiatement. Intégrez une nouvelle compétence à la fois dans votre workflow quotidien.

#### 5. Partagez avec votre équipe

Les outils avancés sont encore plus puissants quand toute l'équipe les utilise. Partagez vos découvertes !

---

### Différence avec les modules précédents

Ce module se distingue des précédents par plusieurs aspects :

#### Modules 1-5 : Fondations

- Commandes essentielles utilisées quotidiennement
- Workflow de base pour tous les projets
- Obligatoire pour utiliser Git

#### Module 6 : Outils avancés

- Commandes utilisées pour des situations spécifiques
- Solutions à des problèmes complexes
- Optionnel mais très utile pour un usage professionnel

**Analogie :** Si les modules précédents vous ont appris à conduire une voiture, ce module vous apprend les techniques de conduite avancée, la mécanique, et l'entretien.

---

### Ressources complémentaires

#### Documentation officielle

- [Git Documentation - Advanced](https://git-scm.com/docs)
- [Pro Git Book - Chapitres 7-9](https://git-scm.com/book/en/v2)

#### Outils mentionnés dans ce module

- **Husky** : Gestionnaire de hooks Git (Section 9)
- **lint-staged** : Lint des fichiers modifiés (Section 9)
- **GitHub CLI** : Interface en ligne de commande pour GitHub (Section 7)
- **Oh My Zsh** : Framework de configuration shell avec alias Git (Section 10)

#### Communauté

- Stack Overflow - Tag [git]
- GitHub Community Forums
- r/git sur Reddit

---

### Mot de fin

Ce module représente une étape importante dans votre maîtrise de Git. Les outils que vous allez découvrir sont utilisés quotidiennement par les développeurs professionnels du monde entier. Ils peuvent sembler complexes au début, mais avec la pratique, ils deviendront naturels et indispensables.

**Prenez votre temps.** Il n'est pas nécessaire de tout maîtriser d'un coup. L'important est de comprendre que ces outils existent et de savoir y revenir quand vous en aurez besoin.

**Expérimentez sans crainte.** Grâce au reflog et aux bonnes pratiques que vous avez apprises, Git est un outil très sûr. Vous pouvez expérimenter librement.

**Amusez-vous !** Les fonctions avancées de Git sont puissantes et élégantes. C'est un plaisir de les maîtriser et de les utiliser pour résoudre des problèmes complexes.

---

### Prêt à commencer ?

Vous êtes maintenant prêt à découvrir les fonctions avancées de Git ! Commençons par un outil que vous utiliserez constamment : **git stash**, qui vous permet de mettre de côté temporairement votre travail en cours.

**Bonne formation !** 🚀

---

⏭️ [Mettre de côté temporairement (git stash)](/module-06-fonctions-avancees/01-git-stash.md)
