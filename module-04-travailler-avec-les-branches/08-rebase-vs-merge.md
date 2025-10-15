🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 4 : Travailler avec les branches
## 8. Rebase vs Merge : la règle d'or et quand choisir

### Introduction

Maintenant que vous connaissez à la fois le merge et le rebase, la question devient : lequel choisir ? Cette décision peut sembler complexe, mais en suivant quelques règles simples et en comprenant les implications de chaque approche, vous saurez toujours faire le bon choix.

**Analogie :** C'est comme choisir entre prendre des notes manuscrites ou taper sur ordinateur. Chacun a ses avantages selon le contexte : en réunion, le manuscrit est souvent meilleur (merge = préserve tout), mais pour un document final propre, taper est préférable (rebase = historique nettoyé).

---

### La règle d'or absolue

> **RÈGLE D'OR : Ne jamais réécrire l'historique public**

Cette règle est si importante qu'elle mérite d'être répétée :

🚨 **NE JAMAIS REBASER DES COMMITS QUI ONT ÉTÉ POUSSÉS ET PARTAGÉS AVEC D'AUTRES PERSONNES** 🚨

#### Qu'est-ce que l'historique public ?

**Historique public** = Commits qui ont été poussés (`git push`) sur un dépôt distant partagé (GitHub, GitLab, etc.) et que d'autres personnes ont pu récupérer.

```bash
# ✅ Historique privé (OK pour rebase)
git switch feature-login
# Commits locaux uniquement, jamais pushés
git rebase main  # OK !

# ❌ Historique public (DANGER avec rebase)
git switch main  # Branche partagée par toute l'équipe
git rebase feature  # DANGEREUX !
```

#### Pourquoi cette règle ?

Quand vous rebasez, vous **réécrivez l'historique** : les commits changent d'identifiant. Si quelqu'un a basé son travail sur vos anciens commits, son historique devient incompatible avec le vôtre.

**Exemple de catastrophe :**

```
Vous :                  Alice (votre collègue) :
A ← B ← C               A ← B ← C ← D
    ↑                           ↑
  main                        feature-alice
                              (basée sur C)

Vous rebasez main :
A ← B ← C' ← C''
         ↑
       main (NEW)

Alice essaie de pousser :
❌ Conflits massifs !
❌ Historique incompatible !
❌ Git ne comprend plus rien !
```

Alice devra faire un `git pull`, qui créera un merge horrible, et l'historique deviendra un cauchemar.

---

### Principes de décision : Trois questions

Pour choisir entre merge et rebase, posez-vous ces trois questions dans l'ordre :

```
┌─────────────────────────────────────────────────────┐
│ Question 1 : Les commits sont-ils publics ?         │
└─────────────────────────────────────────────────────┘
              │
              ├─ OUI → MERGE uniquement
              │
              └─ NON → Passer à Question 2
                        │
        ┌───────────────────────────────────────────────┐
        │ Question 2 : Voulez-vous un historique        │
        │              linéaire et propre ?              │
        └───────────────────────────────────────────────┘
                        │
                        ├─ OUI → REBASE (si maîtrisé)
                        │
                        └─ NON → Passer à Question 3
                                  │
                ┌─────────────────────────────────────────┐
                │ Question 3 : Quel est le contexte ?     │
                └─────────────────────────────────────────┘
                                  │
                                  ├─ Feature importante → MERGE (traçabilité)
                                  ├─ Travail en équipe sur branche → MERGE (sûreté)
                                  ├─ Petite correction → REBASE (propreté)
                                  └─ En doute → MERGE (sûreté)
```

---

### Règle d'or détaillée : Contextes d'application

#### Contexte 1 : Branches principales (main, develop)

**TOUJOURS merge, JAMAIS rebase**

```bash
# ❌ INTERDIT
git switch main
git rebase feature  # NON !

# ✅ CORRECT
git switch main
git merge feature   # OUI !
```

**Pourquoi ?** Ces branches sont partagées par toute l'équipe. Les rebaser causerait le chaos.

#### Contexte 2 : Branches feature locales (non poussées)

**Rebase possible et recommandé**

```bash
# ✅ OK : Feature locale, jamais pushée
git switch feature-login
git rebase main
```

**Pourquoi ?** C'est votre branche privée. Vous pouvez la réorganiser comme vous voulez.

#### Contexte 3 : Branches feature partagées avec l'équipe

**MERGE seulement (ou rebase avec coordination extrême)**

```bash
# Si Alice et Bob travaillent tous deux sur feature-payment
# ❌ Bob ne doit PAS rebaser
git switch feature-payment
git rebase main  # Causerait des problèmes à Alice

# ✅ Utiliser merge
git merge main
```

**Pourquoi ?** Dès que plusieurs personnes travaillent sur une branche, elle devient "publique" pour ce groupe.

#### Contexte 4 : Votre branche feature avant de la partager

**Rebase recommandé pour nettoyer**

```bash
# Vous avez développé localement avec beaucoup de commits "sales"
git switch feature-newsletter
git rebase -i main  # Nettoyer l'historique
git push origin feature-newsletter  # Maintenant propre !
```

**Pourquoi ?** Nettoyer avant de partager est une bonne pratique.

---

### Matrice de décision complète

| Situation | Commits publics ? | Historique important ? | Recommandation | Explication |
|-----------|-------------------|------------------------|----------------|-------------|
| Intégrer feature dans main | Oui | Oui | **MERGE** | Traçabilité + Sécurité |
| Mettre à jour feature avec main | Non (feature locale) | Non | **REBASE** | Garde feature à jour |
| Corriger typo sur feature locale | Non | Non | **REBASE + squash** | Historique propre |
| Synchroniser entre collègues | Oui | Oui | **MERGE** | Plusieurs personnes travaillent |
| Nettoyer avant PR | Non (pas encore pushé) | Non | **REBASE -i** | Présenter un historique propre |
| Hotfix urgent sur main | Oui | Oui | **MERGE --no-ff** | Traçabilité critique |
| Feature longue durée | Dépend | Oui | **MERGE régulier** | Éviter accumulation conflits |

---

### Workflows recommandés

#### Workflow 1 : Feature Branch avec Rebase (Recommandé)

**Pour :** Historique propre, un développeur par branche

```bash
# Jour 1 : Créer feature
git switch -c feature-search
git commit -m "WIP: start search"
git commit -m "Add basic search"

# Jour 2 : Synchroniser avec main
git fetch origin
git rebase origin/main  # Mettre à jour feature

# Continuer le développement
git commit -m "Add filters"
git commit -m "Fix bug"

# Jour 3 : Nettoyer avant de partager
git rebase -i origin/main
# Squash les commits "WIP"

# Pousser la branche propre
git push origin feature-search

# Créer Pull Request
# Après review et approbation :
git switch main
git merge --no-ff feature-search
```

**Résultat :**
- Feature branch a un historique propre
- main garde la trace de l'intégration avec merge commit
- Meilleur des deux mondes

#### Workflow 2 : Merge uniquement (Plus simple, plus sûr)

**Pour :** Équipes débutantes, historique chronologique important

```bash
# Créer feature
git switch -c feature-payment
git commit -m "Add payment form"
git commit -m "Add validation"
git commit -m "Connect to API"

# Synchroniser avec main
git switch feature-payment
git merge main  # Merge au lieu de rebase

# Continuer
git commit -m "Add tests"

# Merger dans main
git switch main
git merge --no-ff feature-payment
```

**Résultat :**
- Historique avec branches visibles
- Chronologie préservée
- Plus simple, moins de risques

#### Workflow 3 : Rebase local, Merge pour intégration

**Pour :** Historique propre localement, traçabilité sur main

```bash
# Développer localement
git switch -c feature-profile
# ... beaucoup de commits locaux ...

# Avant de pousser, nettoyer
git rebase -i main
# Squash, reorder, clean up

# Pousser
git push origin feature-profile

# Sur main, merger avec commit explicite
git switch main
git merge --no-ff feature-profile -m "Add user profile feature

Complete implementation:
- Profile page with avatar
- Edit functionality
- Privacy settings
- Unit tests coverage: 95%"
```

**Résultat :**
- Feature branch propre (rebase local)
- main avec traçabilité (merge commit)
- Historique lisible à tous les niveaux

#### Workflow 4 : Git Flow avec Merge exclusif

**Pour :** Projets complexes, releases multiples

```bash
# Développement
git switch develop
git switch -c feature/new-dashboard

# Merge dans develop (jamais rebase)
git switch develop
git merge --no-ff feature/new-dashboard

# Release
git switch -c release/v2.0 develop
# ... préparation release ...
git switch main
git merge --no-ff release/v2.0

# Hotfix
git switch -c hotfix/security main
# ... fix ...
git switch main
git merge --no-ff hotfix/security
git switch develop
git merge --no-ff hotfix/security
```

**Résultat :**
- Historique complet et traçable
- Adapté aux exigences réglementaires
- Clair pour audits

---

### Cas d'usage spécifiques

#### Cas 1 : Développeur solo sur projet personnel

```bash
# Vous êtes seul, faites ce que vous voulez !

# Option A : Rebase partout (historique ultra-propre)
git rebase main
git rebase -i main
git push --force-with-lease  # Vous êtes seul, pas de danger

# Option B : Merge partout (plus simple)
git merge main
```

**Recommandation :** Utilisez l'occasion pour pratiquer le rebase sans risque !

#### Cas 2 : Équipe de 10+ développeurs

```bash
# JAMAIS de rebase sur branches partagées

# Toujours merge
git switch main
git merge --no-ff feature-X

# Politique stricte : merge uniquement
git config branch.main.rebase false
```

**Recommandation :** Simplicité et sécurité avant tout. Merge exclusif.

#### Cas 3 : Projet open source avec contributions externes

```bash
# Les contributeurs externes ne maîtrisent pas forcément Git

# Accepter les PR avec squash merge
# (GitHub/GitLab option)
git merge --squash feature-contribution
git commit -m "Add feature X (PR #123)

Contributed by @external-dev
- Feature description
- Tests added"
```

**Recommandation :** Squash merge pour garder main propre.

#### Cas 4 : Feature de longue durée (plusieurs semaines)

```bash
# Synchroniser régulièrement pour éviter les conflits massifs

# Option A : Merge régulier (recommandé pour équipe)
git switch feature-refactor
git merge main  # Chaque semaine

# Option B : Rebase régulier (si solo)
git switch feature-refactor
git rebase main  # Chaque jour
```

**Recommandation :** Merge si travail en équipe sur la feature, rebase si solo.

#### Cas 5 : Correction urgente en production

```bash
# Hotfix doit être tracé clairement

git switch -c hotfix/critical-bug main
# ... correction ...
git switch main
git merge --no-ff hotfix/critical-bug -m "HOTFIX: Critical security vulnerability

CVE-2024-XXXX: SQL injection in login
Fixed by sanitizing user input
Tested in production environment
Deploy immediately"
```

**Recommandation :** Toujours merge avec message détaillé pour traçabilité.

---

### Comparaison philosophique

#### Philosophie du Merge

**"L'historique doit refléter ce qui s'est vraiment passé"**

```
Avantages :
✅ Vérité historique préservée
✅ On voit quand les décisions ont été prises
✅ Traçabilité complète
✅ Sûr pour tout le monde

Inconvénients :
❌ Historique complexe visuellement
❌ Beaucoup de commits de merge
❌ Difficile de suivre une feature
```

**Quand adopter :** Projets réglementés, équipes larges, débutants Git.

#### Philosophie du Rebase

**"L'historique doit raconter l'histoire logique du projet"**

```
Avantages :
✅ Historique linéaire et clair
✅ Facile à comprendre
✅ Chaque commit est propre
✅ Facilite la recherche de bugs

Inconvénients :
❌ Perd l'information temporelle
❌ Dangereux si mal utilisé
❌ Demande plus de maîtrise
❌ Ne montre pas le vrai processus
```

**Quand adopter :** Projets solos, équipes expérimentées, historique propre prioritaire.

---

### Questions fréquentes

#### Q1 : "Je travaille seul, merge ou rebase ?"

**Réponse :** Les deux fonctionnent ! C'est l'occasion idéale de pratiquer le rebase.

```bash
# Vous pouvez rebaser autant que vous voulez
git rebase main
git rebase -i HEAD~10
git push --force origin feature
```

Mais si vous débutez, restez sur merge pour éviter les erreurs.

#### Q2 : "Mon équipe utilise merge, puis-je rebaser ma branche locale ?"

**Réponse :** OUI, si elle n'est pas partagée !

```bash
# ✅ OK : Rebaser votre branche locale avant de la pousser
git switch ma-feature  # Jamais poussée
git rebase main
git push origin ma-feature

# ❌ NON : Rebaser une fois qu'elle est poussée et que d'autres travaillent dessus
```

#### Q3 : "J'ai rebasé par erreur une branche publique, que faire ?"

**Réponse :** Réparer immédiatement et communiquer !

```bash
# 1. Annuler le rebase avec reflog
git reflog
git reset --hard HEAD@{X}  # Avant le rebase

# 2. Informer l'équipe
# "N'avez pas pull la branche X, j'ai fait une erreur"

# 3. Force push l'ancienne version
git push --force origin feature-X

# Si c'est trop tard, discuter avec l'équipe pour coordonner
```

#### Q4 : "Quand utiliser `git pull --rebase` ?"

**Réponse :** Sur vos branches personnelles uniquement.

```bash
# ✅ Sur votre feature branch
git switch ma-feature
git pull --rebase origin ma-feature

# ❌ Sur main ou branches partagées
git switch main
git pull --rebase  # Potentiellement dangereux
```

#### Q5 : "Merge ou rebase pour résoudre des conflits avec main ?"

**Réponse :** Dépend si la branche est partagée.

```bash
# Branche personnelle : rebase (plus propre)
git switch feature-mine
git rebase main

# Branche partagée : merge (plus sûr)
git switch feature-shared-with-alice
git merge main
```

---

### Configurations recommandées par profil

#### Profil : Débutant Git

```bash
# Désactiver rebase par défaut (sécurité)
git config --global pull.rebase false

# Toujours créer merge commit
git config --global merge.ff false

# Rester sur merge partout
```

**Philosophie :** Maîtriser merge d'abord, rebase plus tard.

#### Profil : Développeur intermédiaire

```bash
# Pull avec rebase sur branches locales
git config --global pull.rebase true

# Mais merge.ff true (laisser Git décider)
git config --global merge.ff true

# Utiliser rebase pour nettoyer, merge pour intégrer
```

**Philosophie :** Combiner les deux selon le contexte.

#### Profil : Expert Git

```bash
# Rebase par défaut
git config --global pull.rebase true

# Autostash pour fluidité
git config --global rebase.autoStash true

# Maîtrise complète des deux outils
```

**Philosophie :** Choisir l'outil optimal selon la situation.

#### Profil : Équipe (configuration du projet)

```bash
# Dans le repo, documenter la politique
# .github/CONTRIBUTING.md ou docs/git-workflow.md

# Exemples :
# "Ce projet utilise merge uniquement"
# "Rebase autorisé sur branches feature, merge sur main"
# "Squash merge pour toutes les PR"
```

**Philosophie :** Cohérence d'équipe avant préférences personnelles.

---

### Signaux d'alerte : Quand vous faites fausse route

#### 🚨 Signal 1 : Force push sur branche partagée

```bash
git push --force origin main
# ❌ STOP ! Jamais sur branche partagée !
```

**Solution :** Annuler immédiatement et utiliser merge à la place.

#### 🚨 Signal 2 : Conflits répétitifs lors d'un rebase

```bash
git rebase main
# CONFLICT in file1.js
git rebase --continue
# CONFLICT in file2.js
git rebase --continue
# CONFLICT in file3.js
# ... 10 fois ...
```

**Solution :** Abandonner le rebase et utiliser merge.

```bash
git rebase --abort
git merge main  # Plus simple
```

#### 🚨 Signal 3 : Collègue se plaint de conflits étranges

```
Alice : "Je n'arrive pas à pull, Git dit que l'historique a divergé"
```

**Cause probable :** Vous avez rebasé une branche qu'elle utilisait.

**Solution :** Coordination et potentiellement revenir en arrière.

#### 🚨 Signal 4 : Perte de commits après rebase

```bash
git rebase main
# Où sont passés mes commits ?!
```

**Solution :** Utiliser reflog pour récupérer.

```bash
git reflog
git reset --hard HEAD@{X}
# Puis utiliser merge à la place
```

---

### Checklist de décision rapide

Avant de faire un rebase, vérifiez :

```
☐ Les commits sont-ils seulement locaux (jamais pushés) ?
☐ Personne d'autre ne travaille sur cette branche ?
☐ Ce n'est PAS main ou develop ?
☐ Je sais comment récupérer avec reflog si ça tourne mal ?
☐ J'ai fait un backup (branche ou stash) au cas où ?

Si TOUS les checks sont ✅ → Rebase OK
Si UN SEUL est ❌ → Utiliser MERGE
```

---

### Aide-mémoire visuel

```
┌─────────────────────────────────────────────────┐
│                  MERGE                          │
│                                                 │
│  ✅ Sûr toujours                                │
│  ✅ Préserve l'historique                       │
│  ✅ Bon pour branches publiques                 │
│  ✅ Traçabilité complète                        │
│  ❌ Historique complexe                         │
│                                                 │
│  Utiliser quand : Doute, équipe, main           │
└─────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────┐
│                  REBASE                         │
│                                                 │
│  ✅ Historique linéaire                         │
│  ✅ Commits propres                             │
│  ✅ Bon pour branches locales                   │
│  ❌ Dangereux sur branches publiques            │
│  ❌ Perd l'info temporelle                      │
│                                                 │
│  Utiliser quand : Solo, local, nettoyage        │
└─────────────────────────────────────────────────┘
```

---

### Points clés à retenir

✅ **Règle d'or** : Ne JAMAIS rebaser des commits publics

✅ **Par défaut** : Utiliser merge (plus sûr)

✅ **Rebase** : Réservé aux branches locales privées

✅ **Main/Develop** : Toujours merge, jamais rebase

✅ **Feature partagée** : Merge uniquement

✅ **Feature locale** : Rebase possible pour nettoyer

✅ **En doute** : Choisir merge

✅ **Force push** : Signe que quelque chose ne va pas

---

### Conclusion

Le choix entre merge et rebase n'est pas une question de "meilleur" ou "pire", mais de **contexte approprié**. Les deux outils ont leur place dans votre boîte à outils Git.

**Règle simple pour débuter :**
- **Merge** : Toujours sûr, utilisez-le par défaut
- **Rebase** : Uniquement quand vous êtes sûr que c'est sûr

**Avec l'expérience, vous apprendrez à :**
- Reconnaître les situations où le rebase apporte de la valeur
- Éviter instinctivement les pièges du rebase
- Combiner les deux pour obtenir le meilleur des deux mondes

**Le plus important :**
- Comprendre ce que fait chaque commande
- Communiquer avec votre équipe
- Privilégier la sécurité sur la "beauté" de l'historique
- Ne pas avoir peur du merge

Rappelez-vous : un historique "parfait" obtenu avec rebase ne vaut rien si vous avez cassé le dépôt de toute votre équipe. La sécurité et la collaboration passent avant l'esthétique de l'historique.

**Et surtout :** En cas de doute, mergez ! Vous ne vous tromperez jamais en choisissant merge, tandis qu'un rebase mal placé peut causer des heures de problèmes.

Ce chapitre conclut notre exploration approfondie du travail avec les branches. Vous maîtrisez maintenant les concepts, les commandes, et surtout, vous savez **quand** utiliser chaque outil. Dans le prochain module, nous verrons comment travailler efficacement avec des dépôts distants et collaborer avec une équipe.

⏭️ [Module 5 : Git à plusieurs – Dépôts distants](/module-05-git-a-plusieurs/README.md)
