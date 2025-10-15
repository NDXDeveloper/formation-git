🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 5. Stratégies de merge (fast-forward, merge commit, squash)

### Introduction

Dans la section précédente, nous avons vu comment fusionner des branches avec `git merge`. Maintenant, nous allons approfondir les différentes **stratégies de merge** que vous pouvez utiliser selon vos besoins. Chaque stratégie a ses avantages et inconvénients, et le choix dépend de votre workflow et de vos priorités.

**Analogie :** Imaginez trois façons de combiner deux documents :
1. **Fast-forward** : Simplement ajouter les nouvelles pages à la fin du document existant
2. **Merge commit** : Créer une page de transition qui explique comment les deux documents ont été combinés
3. **Squash** : Résumer toutes les nouvelles pages en une seule avant de l'ajouter

---

### Vue d'ensemble des trois stratégies

| Stratégie | Crée un commit de merge ? | Conserve l'historique détaillé ? | Historique linéaire ? |
|-----------|---------------------------|----------------------------------|----------------------|
| **Fast-Forward** | ❌ Non | ✅ Oui | ✅ Oui (si pas de divergence) |
| **Merge Commit** | ✅ Oui | ✅ Oui | ❌ Non (graphe avec branches) |
| **Squash** | ❌ Non (un seul commit) | ❌ Non | ✅ Oui |

---

## 1. Fast-Forward (Avance rapide)

### Concept

Le fast-forward est la stratégie la plus simple. Git déplace simplement le pointeur de la branche cible vers le dernier commit de la branche source, **sans créer de commit de merge**.

**Condition nécessaire :** La branche cible (main) ne doit pas avoir divergé depuis la création de la branche source (feature).

### Visualisation

#### Avant le merge

```
A ← B ← C ← D ← E
        ↑       ↑
      main    feature (HEAD)
```

La branche `main` est restée sur `C`, tandis que `feature` a avancé avec les commits `D` et `E`.

#### Après git merge feature (fast-forward)

```
A ← B ← C ← D ← E
                ↑
            main, feature
```

Git a simplement déplacé le pointeur `main` vers `E`. Aucun nouveau commit créé.

### Commande

```bash
# Fast-forward se produit automatiquement si possible
git switch main
git merge feature-login
# Updating c3d4e5f..e5f6g7h
# Fast-forward
#  login.html | 50 ++++++++++
#  auth.js    | 120 ++++++++++++++++++
#  2 files changed, 170 insertions(+)
```

### Avantages du fast-forward

✅ **Historique linéaire et propre** : Pas de commits de merge superflus

✅ **Simple à comprendre** : L'historique se lit comme une ligne droite

✅ **Léger** : Aucun commit supplémentaire n'est créé

✅ **Rapide** : Opération instantanée

### Inconvénients du fast-forward

❌ **Perte de contexte** : Impossible de savoir qu'une branche feature a existé

❌ **Pas de point de revert** : Difficile d'annuler toute une feature d'un coup

❌ **Pas toujours possible** : Nécessite que main n'ait pas avancé

### Quand utiliser fast-forward ?

**✅ Utilisez fast-forward pour :**
- Petites modifications rapides
- Corrections de bugs mineurs
- Mises à jour de documentation
- Projets personnels avec historique simple
- Quand vous travaillez seul sur une branche

**❌ Évitez fast-forward pour :**
- Grandes fonctionnalités qu'on pourrait vouloir annuler
- Projets en équipe où la traçabilité est importante
- Quand vous voulez voir clairement l'intégration des features

### Exemple pratique : Fast-forward

```bash
# Situation initiale
git switch main
git log --oneline
# c3d4e5f (HEAD -> main) Update README
# b2c3d4e Initial commit

# Créer une branche pour corriger une faute de frappe
git switch -c fix-typo

# Faire la correction
sed -i 's/teh/the/g' README.md
git commit -am "Fix typo in README"

# Ajouter une autre petite correction
sed -i 's/wiht/with/g' README.md
git commit -am "Fix another typo"

# Retour sur main
git switch main

# Main n'a pas bougé, fast-forward sera automatique
git merge fix-typo
# Updating c3d4e5f..e5f6g7h
# Fast-forward
#  README.md | 2 +-
#  1 file changed, 1 insertion(+), 1 deletion(-)

# Vérifier l'historique
git log --oneline
# e5f6g7h (HEAD -> main, fix-typo) Fix another typo
# d4e5f6g Fix typo in README
# c3d4e5f Update README
# b2c3d4e Initial commit

# Historique linéaire, pas de commit de merge
```

### Empêcher le fast-forward

Si vous voulez créer un commit de merge même quand un fast-forward est possible :

```bash
git merge --no-ff feature-login
```

Nous verrons cela en détail dans la section suivante.

---

## 2. Merge Commit (Commit de fusion)

### Concept

Un merge commit est créé quand les deux branches ont divergé. Ce commit spécial a **deux parents** : un de chaque branche. Il représente explicitement le point d'intégration des deux lignes de développement.

### Visualisation

#### Avant le merge

```
        ┌─ F ← G
        │
A ← B ← C ← D ← E
            ↑
          main (HEAD)
```

Les deux branches ont divergé : `main` a les commits `D` et `E`, `feature` a les commits `F` et `G`.

#### Après git merge feature (merge commit)

```
        ┌─ F ← G ─┐
        │         │
A ← B ← C ← D ← E ← M
                    ↑
                  main (HEAD)
```

Le commit `M` est un merge commit avec deux parents : `E` (de main) et `G` (de feature).

### Commande

```bash
git switch main
git merge feature-payment
# Merge made by the 'ort' strategy.
#  payment.js | 85 ++++++++++++++++++++++++++++++
#  1 file changed, 85 insertions(+)
```

Git ouvre votre éditeur pour le message de merge commit. Le message par défaut est :

```
Merge branch 'feature-payment' into main
```

Vous pouvez le personnaliser pour être plus descriptif.

### Forcer un merge commit (--no-ff)

Même quand un fast-forward est possible, vous pouvez forcer la création d'un merge commit :

```bash
git merge --no-ff feature-login
```

**Visualisation de la différence :**

```
Avec fast-forward (défaut) :
A ← B ← C ← D ← E
                ↑
              main

Avec --no-ff (forcé) :
        ┌─ D ← E ─┐
        │         │
A ← B ← C ← ──── ─M
                  ↑
                main
```

### Avantages du merge commit

✅ **Traçabilité complète** : On voit clairement qu'une feature a été intégrée

✅ **Historique préservé** : Tous les commits de la branche sont conservés

✅ **Point de revert** : Facile d'annuler toute la feature en revertant le merge

✅ **Information temporelle** : On sait quand la feature a été intégrée

✅ **Contexte riche** : Le message de merge peut expliquer pourquoi et comment

### Inconvénients du merge commit

❌ **Historique complexe** : Le graphe peut devenir difficile à lire

❌ **"Pollution"** : Beaucoup de merge commits peuvent encombrer l'historique

❌ **Complexité visuelle** : Les outils de visualisation montrent un graphe ramifié

### Quand utiliser merge commit ?

**✅ Utilisez merge commit pour :**
- Grandes fonctionnalités importantes
- Développement en équipe
- Quand la traçabilité est cruciale
- Features qu'on pourrait vouloir revert entièrement
- Projets avec workflow Git Flow ou GitHub Flow

**❌ Évitez merge commit pour :**
- Petites corrections triviales
- Quand vous voulez un historique parfaitement linéaire
- Synchronisation fréquente avec main (créerait trop de merge commits)

### Exemple pratique : Merge commit

```bash
# Situation avec divergence
git switch main
git commit -m "Update version in package.json"

git switch feature-api
# (plusieurs commits sur feature-api)
git commit -m "Ajout endpoint users"
git commit -m "Ajout validation"
git commit -m "Ajout tests"

# Retour sur main pour merger
git switch main
git merge feature-api
# Merge made by the 'ort' strategy.

# Personnaliser le message de merge
# (éditeur s'ouvre)
# "Merge feature-api: API REST pour gestion utilisateurs
#
# - Ajout endpoint GET/POST/PUT/DELETE /users
# - Validation avec Joi
# - Tests unitaires et intégration
# - Documentation Swagger"

# Vérifier l'historique
git log --oneline --graph
# *   a1b2c3d (HEAD -> main) Merge feature-api
# |\
# | * h7i8j9k Ajout tests
# | * f6g7h8i Ajout validation
# | * e5f6g7h Ajout endpoint users
# * | d4e5f6g Update version in package.json
# |/
# * c3d4e5f Base commit
```

### Convention : Toujours utiliser --no-ff pour les features

Beaucoup d'équipes adoptent cette règle :

```bash
# Configuration globale pour toujours créer un merge commit
git config --global merge.ff false

# Ou configuration par projet
git config merge.ff false
```

**Avantage :** Historique cohérent où chaque feature est clairement identifiable.

```bash
# Sans cette config (fast-forward possible)
git merge feature-small
# Fast-forward...

# Avec cette config (merge commit forcé)
git merge feature-small
# Merge made by the 'ort' strategy.
```

---

## 3. Squash (Fusion écrasée)

### Concept

Le squash combine tous les commits d'une branche en **un seul commit** avant de l'intégrer dans la branche cible. C'est comme créer un résumé de tout le travail de la branche.

**Important :** Ce n'est pas un "vrai" merge. Git ne crée pas de commit avec deux parents. C'est un commit normal qui contient toutes les modifications.

### Visualisation

#### Avant le squash

```
        ┌─ F ← G ← H
        │
A ← B ← C ← D
        ↑
      main (HEAD)
```

La branche `feature` a 3 commits : `F`, `G`, et `H`.

#### Après git merge --squash feature

```
        ┌─ F ← G ← H
        │
A ← B ← C ← D ← E
                ↑
              main (HEAD)
```

Le commit `E` contient toutes les modifications de `F`, `G`, et `H` combinées, mais n'a qu'un seul parent (`D`). Les commits `F`, `G`, `H` ne sont pas dans l'historique de `main`.

### Commande

```bash
git switch main
git merge --squash feature-multiple-commits

# Git prépare les modifications mais NE commit PAS automatiquement
# Squash commit -- not updating HEAD
# Automatic merge went well; stopped before committing as requested

# Vous devez commiter manuellement
git commit -m "Ajout système de notifications complètes"
```

### Différence importante avec merge normal

Avec un merge normal, Git crée automatiquement le commit. Avec `--squash`, vous devez **commiter manuellement**.

```bash
# Après --squash, vérifier l'état
git status
# On branch main
# All conflicts fixed but you are still merging.
#   (use "git commit" to conclude merge)
#
# Changes to be committed:
#   (tous les fichiers modifiés par la branche)

# Créer le commit manuellement
git commit -m "Feature complète : système de notifications

- Notifications push
- Notifications email
- Paramètres utilisateur
- Tests unitaires"
```

### Avantages du squash

✅ **Historique ultra-propre** : Un commit par feature, peu importe le nombre de commits intermédiaires

✅ **Simplicité** : Facile de comprendre ce que fait chaque commit sur main

✅ **Revert simple** : Un seul commit à revert pour annuler la feature

✅ **Pas de bruit** : Les commits "WIP", "fix typo", "oups" ne polluent pas main

✅ **Messages clairs** : Vous pouvez écrire un message de commit parfait qui résume tout

### Inconvénients du squash

❌ **Perte d'historique détaillé** : Impossible de voir les étapes intermédiaires sur main

❌ **Perte d'auteur** : Si plusieurs personnes ont contribué, seul l'auteur du squash apparaît (sauf si configuré)

❌ **Pas de "vrai" merge** : Git ne sait pas que c'est une fusion

❌ **Difficulté à remerger** : Si vous re-mergez la branche plus tard, Git ne saura pas que c'était déjà mergé

❌ **Commits perdus pour bisect** : L'outil `git bisect` ne peut pas naviguer dans les commits squashés

### Quand utiliser squash ?

**✅ Utilisez squash pour :**
- Features développées avec beaucoup de petits commits "sales"
- Pull Requests sur GitHub/GitLab où vous voulez un commit propre
- Historique de main parfaitement organisé
- Projets où chaque commit sur main doit être déployable
- Workflow type "feature branch, squash, delete"

**❌ Évitez squash pour :**
- Quand l'historique détaillé est important (audit, conformité)
- Grandes features où les étapes intermédiaires ont de la valeur
- Projets où le crédit individuel des contributeurs compte
- Quand vous pourriez remerger la branche plus tard

### Exemple pratique : Squash

```bash
# Développement d'une feature avec beaucoup de commits "sales"
git switch -c feature-dark-mode

git commit -m "WIP: start dark mode"
git commit -m "add css variables"
git commit -m "oups forgot a file"
git commit -m "fix typo"
git commit -m "more colors"
git commit -m "fix button colors"
git commit -m "add toggle"
git commit -m "fix toggle bug"
git commit -m "clean up"
git commit -m "final touches"

# 10 commits peu intéressants individuellement

# Merger avec squash
git switch main
git merge --squash feature-dark-mode

# Créer UN commit propre qui résume tout
git commit -m "Ajout mode sombre

Implémentation complète du mode sombre avec :
- Variables CSS pour tous les thèmes
- Toggle dans les paramètres utilisateur
- Sauvegarde des préférences
- Transitions douces entre modes
- Tests sur tous les composants

Closes #123"

# Vérifier l'historique
git log --oneline
# a1b2c3d (HEAD -> main) Ajout mode sombre
# c3d4e5f Previous commit
# b2c3d4e Another commit

# Un seul commit propre au lieu de 10 !
```

### Squash avec modification manuelle

Après un squash, vous pouvez ajuster les modifications avant de commiter :

```bash
git merge --squash feature-X

# Modifier encore des fichiers si nécessaire
echo "Dernière modification" >> fichier.txt
git add fichier.txt

# Commiter avec toutes les modifications
git commit -m "Feature X complète avec ajustements finaux"
```

---

## Comparaison détaillée des trois stratégies

### Historique résultant

#### Fast-Forward

```
A ← B ← C ← D ← E ← F
                    ↑
                  main
```

Historique parfaitement linéaire. Impossible de distinguer les commits de feature.

#### Merge Commit

```
        ┌─ D ← E ← F ─┐
        │             │
A ← B ← C ← ────────── M
                       ↑
                     main
```

Historique ramifié. Les commits de feature sont préservés. Le merge est explicite.

#### Squash

```
A ← B ← C ← S
            ↑
          main
```

Historique linéaire. Un seul commit `S` contient tout le travail de la branche feature.

### Impact sur git log --oneline

#### Fast-Forward

```bash
git log --oneline
# f6g7h8i (HEAD -> main) Feature step 3
# e5f6g7h Feature step 2
# d4e5f6g Feature step 1
# c3d4e5f Base
```

Tous les commits apparaissent dans main.

#### Merge Commit

```bash
git log --oneline
# a1b2c3d (HEAD -> main) Merge branch 'feature'
# | h7i8j9k Feature step 3
# | f6g7h8i Feature step 2
# | e5f6g7h Feature step 1
# c3d4e5f Base
```

Le merge et tous les commits de feature sont visibles.

#### Squash

```bash
git log --oneline
# s1q2u3a (HEAD -> main) Complete feature (squashed)
# c3d4e5f Base
```

Un seul commit représente toute la feature.

---

## Choisir la bonne stratégie

### Arbre de décision

```
Question 1 : La branche cible a-t-elle divergé ?
    │
    ├─ NON → Fast-Forward possible
    │         │
    │         Question 2 : Voulez-vous garder trace de la branche ?
    │         │
    │         ├─ NON → Utiliser Fast-Forward (défaut)
    │         └─ OUI → Utiliser --no-ff (merge commit)
    │
    └─ OUI → Divergence présente
              │
              Question 3 : L'historique détaillé est-il important ?
              │
              ├─ OUI → Merge Commit (défaut)
              └─ NON → Squash
```

### Workflows typiques

#### Workflow 1 : GitHub Flow avec squash

```bash
# Idéal pour : Projets avec beaucoup de petits commits
# Philosophie : Un commit propre par feature sur main

# Sur feature
git switch -c feature-X
# ... beaucoup de commits ...

# Merger sur main
git switch main
git merge --squash feature-X
git commit -m "Feature X complete"
git branch -d feature-X
```

#### Workflow 2 : Git Flow avec merge commits

```bash
# Idéal pour : Projets complexes avec releases
# Philosophie : Historique complet et traçable

# Sur feature
git switch -c feature/payment

# Merger dans develop avec merge commit
git switch develop
git merge --no-ff feature/payment

# Plus tard, merger develop dans main
git switch main
git merge --no-ff develop
```

#### Workflow 3 : Trunk-Based Development avec fast-forward

```bash
# Idéal pour : Équipes qui intègrent très fréquemment
# Philosophie : Main toujours déployable, commits petits et fréquents

# Petites modifications
git switch -c quick-fix
# Un ou deux commits
git switch main
git merge quick-fix  # Fast-forward
```

---

## Configuration Git pour les stratégies

### Configuration globale

```bash
# Toujours créer un merge commit (jamais de fast-forward)
git config --global merge.ff false

# Autoriser fast-forward seulement si possible, sinon merge commit
git config --global merge.ff true

# Fast-forward uniquement, échouer si impossible
git config --global merge.ff only
```

### Configuration par branche

```bash
# Pour la branche main, toujours créer merge commit
git config branch.main.mergeoptions "--no-ff"
```

### Vérifier la configuration actuelle

```bash
git config merge.ff
```

---

## Cas pratiques : Choisir la bonne stratégie

### Cas 1 : Petite correction de bug

```bash
# Situation : Faute de frappe dans le README
# Recommandation : Fast-forward (défaut)

git switch -c fix-readme-typo
sed -i 's/teh/the/g' README.md
git commit -am "Fix typo in README"
git switch main
git merge fix-readme-typo  # Fast-forward automatique
```

**Pourquoi ?** C'est trivial, pas besoin de polluer l'historique avec un merge commit.

### Cas 2 : Grande fonctionnalité critique

```bash
# Situation : Nouveau système de paiement (3 semaines de travail)
# Recommandation : Merge commit avec --no-ff

git switch -c feature/payment-system
# ... 50 commits sur 3 semaines ...
git switch main
git merge --no-ff feature/payment-system -m "Merge payment system

Complete payment integration with Stripe:
- Payment form with validation
- Webhook handling
- Subscription management
- Invoice generation
- Admin dashboard
- Comprehensive tests

Closes #234, #235, #236"
```

**Pourquoi ?** Important de garder trace de cette fonctionnalité majeure pour audit et possibilité de revert.

### Cas 3 : Pull Request avec historique chaotique

```bash
# Situation : PR d'un contributeur externe avec 20 commits "WIP"
# Recommandation : Squash

git switch main
git merge --squash feature/external-contribution
git commit -m "Add user profile customization

Implementation contributed by @external-dev:
- Avatar upload with cropping
- Bio and social links
- Privacy settings
- Profile preview

Co-authored-by: External Dev <external@email.com>"
```

**Pourquoi ?** Les commits intermédiaires n'ont pas de valeur, mais le résultat final oui.

### Cas 4 : Synchronisation régulière avec main

```bash
# Situation : Votre feature branch vit longtemps
# Recommandation : Fast-forward ou rebase (pas squash)

# Sur votre feature branch, incorporer les changements de main
git switch feature/long-running
git merge main  # Ou git rebase main

# Plus tard, quand feature est prête
git switch main
git merge --no-ff feature/long-running
```

**Pourquoi ?** Squash serait problématique car vous avez déjà des merges de main dans votre historique.

---

## Messages de commit pour chaque stratégie

### Fast-Forward : Message simple

```bash
# Pas de merge commit, donc pas de message spécial
# Le message du dernier commit de la branche devient visible
```

### Merge Commit : Message descriptif

```bash
git merge --no-ff feature-X

# Message dans l'éditeur :
"Merge branch 'feature-X': User authentication system

Complete OAuth2 implementation:
- Google and GitHub providers
- JWT token management
- Session handling
- Password reset flow
- Email verification

Resolves: #123, #124
Tested-by: QA Team
Reviewed-by: @senior-dev"
```

### Squash : Résumé complet

```bash
git merge --squash feature-X
git commit

# Message dans l'éditeur :
"Implement user authentication system

Features:
- OAuth2 with Google and GitHub
- JWT token generation and validation
- Session management with Redis
- Password reset via email
- Email verification workflow
- Rate limiting on auth endpoints

Technical details:
- Using Passport.js for OAuth
- bcrypt for password hashing
- 2FA support prepared for future

Tests:
- Unit tests: 95% coverage
- Integration tests for all flows
- E2E tests for login/logout

Breaking changes: None
Migration needed: Run 'npm run migrate:auth'

Closes #123, #124, #125"
```

---

## Bonnes pratiques par stratégie

### Pour Fast-Forward

✅ Utiliser pour des changements triviaux

✅ Garder les commits propres dès le départ

✅ Tester avant le merge

❌ Ne pas utiliser pour des features importantes

### Pour Merge Commit

✅ Écrire des messages de merge détaillés

✅ Utiliser --no-ff systématiquement pour les features

✅ Référencer les issues/tickets dans le message

❌ Ne pas merger trop fréquemment de petites choses

### Pour Squash

✅ Rédiger un message de commit exemplaire

✅ Mentionner tous les contributeurs (Co-authored-by)

✅ Lister les changements importants

❌ Ne pas squasher si l'historique détaillé a de la valeur

---

## Aide-mémoire des commandes

```bash
# FAST-FORWARD (défaut si possible)
git merge branche                    # Fast-forward automatique si possible

# MERGE COMMIT
git merge --no-ff branche            # Forcer un merge commit
git config merge.ff false            # Toujours créer merge commit

# SQUASH
git merge --squash branche           # Combiner tous les commits
git commit -m "Message"              # Commiter manuellement après squash

# VÉRIFICATION
git log --oneline --graph            # Voir l'historique
git log --merges                     # Voir seulement les merge commits
git log --no-merges                  # Voir seulement les commits normaux

# CONFIGURATION
git config --global merge.ff false   # Global : toujours merge commit
git config merge.ff only             # Autoriser fast-forward uniquement
```

---

## Points clés à retenir

✅ **Fast-forward** : Déplace le pointeur, historique linéaire, pas de trace de branche

✅ **Merge commit** : Crée un commit avec deux parents, préserve l'historique complet

✅ **Squash** : Combine tous les commits en un seul, historique ultra-propre

✅ **--no-ff** force un merge commit même si fast-forward possible

✅ **Fast-forward** pour petites corrections

✅ **Merge commit** pour features importantes

✅ **Squash** pour nettoyer un historique chaotique

✅ **Chaque stratégie a sa place** selon le contexte

---

## Conclusion

Le choix de la stratégie de merge dépend de plusieurs facteurs : la taille de la feature, l'importance de la traçabilité, la qualité de l'historique de la branche, et les conventions de votre équipe. Il n'y a pas de "meilleure" stratégie universelle.

**Recommandations générales :**
- **Par défaut :** Laissez Git choisir (fast-forward si possible, merge commit sinon)
- **Pour features importantes :** Utilisez `--no-ff` systématiquement
- **Pour nettoyer l'historique :** Utilisez `--squash` judicieusement
- **Pour cohérence d'équipe :** Définissez une convention et configurez Git en conséquence

La maîtrise de ces trois stratégies vous permettra d'adapter votre workflow Git à n'importe quel projet et de maintenir un historique propre et compréhensible.

Dans la prochaine section, nous verrons comment **résoudre les conflits de merge** de manière efficace et méthodique.

⏭️ [Résolution de conflits](/module-04-travailler-avec-les-branches/06-resolution-de-conflits.md)
