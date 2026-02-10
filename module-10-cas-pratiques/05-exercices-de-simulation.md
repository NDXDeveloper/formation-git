🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 10 : Cas pratiques et projets
## 5. Exercices de simulation (conflits, historique complexe)

Dans ce chapitre, nous allons créer volontairement des situations complexes que vous rencontrerez dans la vie réelle : conflits de merge, historique enchevêtré, branches divergentes, etc. L'objectif est de vous entraîner à résoudre ces problèmes dans un environnement contrôlé, sans risque pour vos vrais projets.

---

## Pourquoi simuler des situations complexes ?

**En conditions réelles, ces problèmes arrivent :**
- Quand plusieurs personnes modifient le même fichier
- Quand des branches restent longtemps sans être synchronisées
- Quand on fait des erreurs de manipulation
- Quand l'historique devient difficile à suivre

**Avantages de la simulation :**
- ✅ Vous apprenez sans risque
- ✅ Vous comprenez les causes des problèmes
- ✅ Vous développez des réflexes pour les résoudre
- ✅ Vous gagnez en confiance avec Git

---

## Environnement de simulation

Pour ces simulations, nous allons créer un dépôt d'entraînement dédié.

### Préparation

```bash
# Créer un dossier pour les simulations
mkdir git-simulations  
cd git-simulations

# Initialiser Git
git init

# Configurer (si nécessaire)
git config user.name "Votre Nom"  
git config user.email "votre.email@example.com"
```

**💡 Astuce :** Vous pouvez réinitialiser complètement à tout moment avec :
```bash
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init
```

---

## Simulation 1 : Conflit de merge simple

**Objectif :** Comprendre et résoudre un conflit de base.

### Scénario

Deux développeurs modifient la même ligne dans le même fichier.

### Étape par étape

**1. Créer le fichier initial :**

```bash
# Créer un fichier
echo "Version originale du projet" > fichier.txt  
git add fichier.txt  
git commit -m "Initial: Version originale"
```

**2. Créer deux branches divergentes :**

```bash
# Créer une branche feature-a
git checkout -b feature-a

# Modifier le fichier
echo "Version modifiée par développeur A" > fichier.txt  
git add fichier.txt  
git commit -m "Feature A: Modification du fichier"

# Retourner sur main
git checkout main

# Créer une autre branche
git checkout -b feature-b

# Modifier la MÊME ligne différemment
echo "Version modifiée par développeur B" > fichier.txt  
git add fichier.txt  
git commit -m "Feature B: Modification du fichier"
```

**3. Tenter de merger - Le conflit apparaît :**

```bash
# Essayer de merger feature-a dans feature-b
git merge feature-a

# Vous verrez :
# Auto-merging fichier.txt
# CONFLICT (content): Merge conflict in fichier.txt
# Automatic merge failed; fix conflicts and then commit the result.
```

**4. Comprendre le conflit :**

```bash
# Voir l'état
git status
# Le fichier est marqué "both modified"

# Voir le contenu du fichier
cat fichier.txt
```

Le fichier contient maintenant :
```
<<<<<<< HEAD
Version modifiée par développeur B
=======
Version modifiée par développeur A
>>>>>>> feature-a
```

**Explication des marqueurs :**
- `<<<<<<< HEAD` : Début de votre version actuelle (feature-b)
- `=======` : Séparateur entre les deux versions
- `>>>>>>> feature-a` : Fin de la version à merger

**5. Résoudre le conflit :**

Ouvrez `fichier.txt` et choisissez une solution :

**Option 1 - Garder la version A :**
```
Version modifiée par développeur A
```

**Option 2 - Garder la version B :**
```
Version modifiée par développeur B
```

**Option 3 - Combiner les deux :**
```
Version modifiée par développeur A et B
```

**6. Finaliser la résolution :**

```bash
# Après avoir édité le fichier manuellement
git add fichier.txt

# Vérifier l'état
git status
# Le conflit est marqué comme résolu

# Finaliser le merge
git commit -m "Merge feature-a: Conflit résolu"

# Voir l'historique
git log --oneline --graph --all
```

**📊 Résultat visuel :**
```
*   Merge feature-a: Conflit résolu (HEAD -> feature-b)
|\
| * Feature A: Modification (feature-a)
* | Feature B: Modification
|/
* Initial: Version originale (main)
```

---

## Simulation 2 : Conflit de merge multiple

**Objectif :** Gérer plusieurs fichiers en conflit simultanément.

### Scénario

Trois fichiers modifiés différemment sur deux branches.

### Étapes

```bash
# Réinitialiser pour cette simulation
git checkout main

# Créer trois fichiers
echo "Config A: valeur1" > config.txt  
echo "Code A: fonction1" > code.txt  
echo "Data A: données1" > data.txt  
git add .  
git commit -m "Initial: Trois fichiers"

# Branche developer-1
git checkout -b developer-1  
echo "Config B: valeur2" > config.txt  
echo "Code B: fonction2" > code.txt  
echo "Data B: données2" > data.txt  
git add .  
git commit -m "Dev1: Modifications globales"

# Branche developer-2 (depuis main)
git checkout main  
git checkout -b developer-2  
echo "Config C: valeur3" > config.txt  
echo "Code C: fonction3" > code.txt  
echo "Data C: données3" > data.txt  
git add .  
git commit -m "Dev2: Modifications globales"

# Merger developer-1 dans developer-2
git merge developer-1
```

**Résultat :** Trois fichiers en conflit !

```bash
# Voir tous les conflits
git status

# Vous avez 3 fichiers "both modified"
```

**Résolution méthodique :**

```bash
# 1. Résoudre config.txt
nano config.txt  # ou votre éditeur
# Choisissez la bonne version, supprimez les marqueurs
git add config.txt

# 2. Résoudre code.txt
nano code.txt  
git add code.txt

# 3. Résoudre data.txt
nano data.txt  
git add data.txt

# 4. Vérifier qu'il ne reste aucun conflit
git status

# 5. Finaliser
git commit -m "Merge developer-1: Résolution de 3 conflits"
```

**💡 Conseil :** Résolvez les conflits un par un, ne vous précipitez pas.

---

## Simulation 3 : Conflit lors d'un rebase

**Objectif :** Comprendre la différence entre conflit de merge et conflit de rebase.

### Scénario

Rebaser une branche qui a divergé.

### Étapes

```bash
# Partir d'une base propre
git checkout main  
echo "Ligne 1" > rebase-test.txt  
git add rebase-test.txt  
git commit -m "Initial: Ligne 1"

# Créer une feature
git checkout -b feature-rebase  
echo "Ligne 2 (feature)" >> rebase-test.txt  
git add rebase-test.txt  
git commit -m "Feature: Ajout ligne 2"

echo "Ligne 3 (feature)" >> rebase-test.txt  
git add rebase-test.txt  
git commit -m "Feature: Ajout ligne 3"

# Pendant ce temps, main avance
git checkout main  
echo "Ligne 2 (main)" >> rebase-test.txt  
git add rebase-test.txt  
git commit -m "Main: Ajout ligne 2 différente"

# Maintenant, rebaser feature sur main
git checkout feature-rebase  
git rebase main
```

**Résultat :** Conflit pendant le rebase !

```
CONFLICT (content): Merge conflict in rebase-test.txt  
error: could not apply [commit]... Feature: Ajout ligne 2
```

**Différence avec merge :**
- Avec `merge` : 1 conflit à résoudre, puis c'est fini
- Avec `rebase` : Peut avoir plusieurs conflits (un par commit rebasé)

**Résolution d'un conflit de rebase :**

```bash
# 1. Résoudre le conflit dans le fichier
nano rebase-test.txt
# Éditez et supprimez les marqueurs

# 2. Ajouter le fichier résolu
git add rebase-test.txt

# 3. Continuer le rebase (PAS de commit !)
git rebase --continue

# Si d'autres conflits : répétez 1-3
# Si vous voulez abandonner : git rebase --abort
```

**📝 Note importante :**
- Avec `merge` → `git commit` pour finaliser
- Avec `rebase` → `git rebase --continue` pour finaliser

---

## Simulation 4 : Historique complexe avec branches enchevêtrées

**Objectif :** Naviguer et comprendre un historique non-linéaire.

### Créer un historique complexe

```bash
# Réinitialiser
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Commit initial
echo "A" > file.txt  
git add file.txt  
git commit -m "A: Initial commit"

# Branche feature-1
git checkout -b feature-1  
echo "B" >> file.txt  
git add file.txt  
git commit -m "B: Feature 1 step 1"

echo "C" >> file.txt  
git add file.txt  
git commit -m "C: Feature 1 step 2"

# Retour sur main, nouvelle branche
git checkout main  
git checkout -b feature-2  
echo "D" >> file.txt  
git add file.txt  
git commit -m "D: Feature 2 step 1"

# Merger feature-1 dans feature-2
git merge feature-1
# Résoudre le conflit si nécessaire
git add file.txt  
git commit -m "E: Merge feature-1 into feature-2"

# Continuer sur feature-2
echo "F" >> file.txt  
git add file.txt  
git commit -m "F: Feature 2 step 2"

# Retour sur main
git checkout main  
echo "G" >> file.txt  
git add file.txt  
git commit -m "G: Hotfix on main"

# Nouvelle branche depuis main
git checkout -b feature-3  
echo "H" >> file.txt  
git add file.txt  
git commit -m "H: Feature 3"

# Merger feature-2 dans feature-3
git merge feature-2  
git add file.txt  
git commit -m "I: Merge feature-2 into feature-3"
```

**Visualiser l'historique :**

```bash
git log --oneline --graph --all --decorate
```

**Vous verrez quelque chose comme :**

```
*   I: Merge feature-2 into feature-3 (HEAD -> feature-3)
|\
| *   F: Feature 2 step 2 (feature-2)
| *   E: Merge feature-1 into feature-2
| |\
| | * C: Feature 1 step 2 (feature-1)
| | * B: Feature 1 step 1
| |/
| * D: Feature 2 step 1
* | H: Feature 3
* | G: Hotfix on main (main)
|/
* A: Initial commit
```

**Exercices de navigation :**

```bash
# 1. Revenir au commit "B"
git checkout [hash-du-commit-B]  
cat file.txt  # Voir le contenu à ce moment

# 2. Revenir sur feature-3
git checkout feature-3

# 3. Voir la différence entre deux branches
git diff main feature-3

# 4. Trouver le commit où feature-1 a divergé de main
git merge-base main feature-1

# 5. Voir tous les commits d'une branche
git log feature-2

# 6. Voir qui a modifié une ligne spécifique
git blame file.txt
```

---

## Simulation 5 : Conflit de suppression de fichier

**Objectif :** Gérer un conflit quand un fichier est supprimé d'un côté et modifié de l'autre.

### Scénario

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Fichier initial
echo "Important data" > data.txt  
git add data.txt  
git commit -m "Add data file"

# Branche 1 : Supprime le fichier
git checkout -b branch-delete  
git rm data.txt  
git commit -m "Delete data.txt (obsolete)"

# Branche 2 : Modifie le fichier
git checkout main  
git checkout -b branch-modify  
echo "More important data" >> data.txt  
git add data.txt  
git commit -m "Update data.txt with new info"

# Tenter de merger
git merge branch-delete
```

**Résultat :**
```
CONFLICT (modify/delete): data.txt deleted in branch-delete  
and modified in HEAD. Version HEAD of data.txt left in tree.
```

**Résolution - Trois options :**

**Option 1 - Garder le fichier modifié :**
```bash
git add data.txt  
git commit -m "Merge: Kept modified data.txt"
```

**Option 2 - Confirmer la suppression :**
```bash
git rm data.txt  
git commit -m "Merge: Confirmed deletion of data.txt"
```

**Option 3 - Créer un nouveau fichier avec nouveau nom :**
```bash
mv data.txt data-legacy.txt  
git add data-legacy.txt  
git rm data.txt  
git commit -m "Merge: Renamed data.txt to data-legacy.txt"
```

---

## Simulation 6 : Divergence importante entre branches

**Objectif :** Gérer une situation où deux branches ont beaucoup divergé.

### Créer la divergence

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Base commune
echo "Base" > app.txt  
git add app.txt  
git commit -m "Initial app"

# Branche dev-alice : 5 commits
git checkout -b dev-alice  
for i in {1..5}; do
  echo "Alice change $i" >> app.txt
  git add app.txt
  git commit -m "Alice: Change $i"
done

# Branche dev-bob : 5 commits différents
git checkout main  
git checkout -b dev-bob  
for i in {1..5}; do
  echo "Bob change $i" >> app.txt
  git add app.txt
  git commit -m "Bob: Change $i"
done
```

**Visualiser la divergence :**

```bash
git log --oneline --graph --all
```

```
* Bob: Change 5 (HEAD -> dev-bob)
* Bob: Change 4
* Bob: Change 3
* Bob: Change 2
* Bob: Change 1
| * Alice: Change 5 (dev-alice)
| * Alice: Change 4
| * Alice: Change 3
| * Alice: Change 2
| * Alice: Change 1
|/
* Initial app (main)
```

**Stratégie 1 - Merge (préserve l'historique) :**

```bash
git checkout dev-alice  
git merge dev-bob

# Résoudre les conflits
nano app.txt  
git add app.txt  
git commit -m "Merge dev-bob into dev-alice"
```

**Stratégie 2 - Rebase (historique linéaire) :**

```bash
# Alternative : rebase (historique plus propre)
git checkout dev-bob  
git rebase dev-alice

# Résoudre les conflits un par un pour chaque commit
# Pour chaque conflit :
nano app.txt  
git add app.txt  
git rebase --continue
```

**Comparaison des résultats :**

Après merge :
```
*   Merge dev-bob into dev-alice
|\
| * Bob: Change 5
| * Bob: Change 4
* | Alice: Change 5
* | Alice: Change 4
|/
* Initial app
```

Après rebase :
```
* Bob: Change 5 (HEAD -> dev-bob)
* Bob: Change 4
* Bob: Change 3
* Bob: Change 2
* Bob: Change 1
* Alice: Change 5 (dev-alice)
* Alice: Change 4
* Alice: Change 3
* Alice: Change 2
* Alice: Change 1
* Initial app (main)
```

---

## Simulation 7 : Cherry-pick avec conflits

**Objectif :** Appliquer un commit spécifique d'une branche à une autre.

### Scénario

Vous voulez un seul commit d'une branche, pas toute la branche.

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Base
echo "Version 1" > feature.txt  
git add feature.txt  
git commit -m "Initial feature"

# Branche avec plusieurs commits
git checkout -b experimental  
echo "Version 2 - experimental" > feature.txt  
git add feature.txt  
git commit -m "Exp: Version 2"

echo "Version 3 - experimental" > feature.txt  
git add feature.txt  
git commit -m "Exp: Version 3"

echo "Version 4 - experimental" > feature.txt  
git add feature.txt  
git commit -m "Exp: Version 4"

# Noter le hash du commit "Version 3"
git log --oneline
# Exemple : abc1234 Exp: Version 3

# Retour sur main, développement parallèle
git checkout main  
echo "Version 2 - main" > feature.txt  
git add feature.txt  
git commit -m "Main: Version 2"

# Cherry-pick uniquement le commit Version 3
git cherry-pick abc1234
```

**Résultat :** Conflit possible !

```
CONFLICT (content): Merge conflict in feature.txt
```

**Résolution :**

```bash
# Résoudre le conflit
nano feature.txt  
git add feature.txt

# Continuer le cherry-pick
git cherry-pick --continue

# Voir le résultat
git log --oneline --graph --all
```

---

## Simulation 8 : Annuler un merge problématique

**Objectif :** Revenir en arrière après un merge qui a mal tourné.

### Scénario

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Setup
echo "Stable code" > app.js  
git add app.js  
git commit -m "Stable version"

# Branche buggy
git checkout -b buggy-feature  
echo "Buggy code with errors" > app.js  
git add app.js  
git commit -m "Add buggy feature"

# Merger dans main (oups!)
git checkout main  
git merge buggy-feature  
git commit -m "Merge buggy-feature"

# Réaliser l'erreur
echo "Oh non, le code est cassé!"
```

**Solution 1 - Revert (recommandé si déjà pushé) :**

```bash
# Créer un commit qui annule le merge
git revert -m 1 HEAD

# Explication de -m 1 :
# Dans un merge, il y a 2 parents
# -m 1 = revenir au premier parent (main avant le merge)

# Vérifier
git log --oneline
```

**Solution 2 - Reset (seulement si pas pushé) :**

```bash
# Revenir au commit avant le merge
git reset --hard HEAD~1

# Vérifier
git log --oneline
```

**Solution 3 - Créer une branche de sauvegarde d'abord :**

```bash
# Toujours créer une sauvegarde avant d'annuler
git branch backup-avant-annulation

# Ensuite annuler en sécurité
git reset --hard HEAD~1

# Si problème, revenir à la sauvegarde
git checkout backup-avant-annulation
```

---

## Simulation 9 : Résoudre un historique enchevêtré avec rebase interactif

**Objectif :** Nettoyer un historique sale avant de partager.

### Créer un historique sale

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Plusieurs commits mal organisés
echo "Feature start" > feature.js  
git add feature.js  
git commit -m "Start feature"

echo "Fix typo" >> feature.js  
git add feature.js  
git commit -m "fix"

echo "More code" >> feature.js  
git add feature.js  
git commit -m "wip"

echo "Another fix" >> feature.js  
git add feature.js  
git commit -m "oops"

echo "Final version" >> feature.js  
git add feature.js  
git commit -m "done"

# Historique actuel (pas terrible)
git log --oneline
```

```
abc123 done  
def456 oops  
ghi789 wip  
jkl012 fix  
mno345 Start feature
```

**Nettoyer avec rebase interactif :**

```bash
# Rebaser les 4 derniers commits
git rebase -i HEAD~4
```

**L'éditeur s'ouvre avec :**

```
pick jkl012 fix  
pick ghi789 wip  
pick def456 oops  
pick abc123 done
```

**Modifier en :**

```
pick jkl012 fix  
squash ghi789 wip  
squash def456 oops  
squash abc123 done
```

Ou encore mieux :

```
pick jkl012 fix  
fixup ghi789 wip  
fixup def456 oops  
fixup abc123 done
```

**Différence :**
- `squash` : Combine les commits et permet de réécrire le message
- `fixup` : Combine les commits et garde seulement le premier message

**Sauvegarder et fermer l'éditeur.**

**Résultat :**

```bash
git log --oneline
```

```
xyz789 fix (contient maintenant tout le travail)  
mno345 Start feature
```

**Autres options de rebase interactif :**

```
pick   = utiliser le commit  
reword = utiliser le commit, mais modifier le message  
edit   = utiliser le commit, mais s'arrêter pour amender  
squash = utiliser le commit, mais fusionner avec le précédent  
fixup  = comme squash, mais ignorer le message de ce commit  
drop   = supprimer le commit
```

---

## Simulation 10 : Récupérer du travail après un reset --hard

**Objectif :** Utiliser reflog pour récupérer des commits "perdus".

### Scénario catastrophe

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Travail important
echo "Work 1" > important.txt  
git add important.txt  
git commit -m "Important work 1"

echo "Work 2" >> important.txt  
git add important.txt  
git commit -m "Important work 2"

echo "Work 3" >> important.txt  
git add important.txt  
git commit -m "Important work 3"

# Oups! Reset trop loin par accident
git reset --hard HEAD~3

# Tout semble perdu!
git log --oneline
# Vide ou presque!
```

**Sauvetage avec reflog :**

```bash
# Le reflog garde TOUT
git reflog

# Vous verrez quelque chose comme :
# abc1234 HEAD@{0}: reset: moving to HEAD~3
# def5678 HEAD@{1}: commit: Important work 3
# ghi9012 HEAD@{2}: commit: Important work 2
# jkl3456 HEAD@{3}: commit: Important work 1
```

**Récupération :**

```bash
# Option 1 : Revenir au commit perdu
git reset --hard def5678  # Hash de "Important work 3"

# Option 2 : Créer une branche depuis ce commit
git branch recuperation def5678  
git checkout recuperation

# Vérifier
git log --oneline
# Tout est revenu!
```

**📌 Important :** Le reflog garde l'historique pendant environ 30 jours.

---

## Simulation 11 : Conflit dans un fichier binaire

**Objectif :** Comprendre que les fichiers binaires ne peuvent pas être mergés automatiquement.

### Scénario

```bash
# Nouvelle simulation
cd ..  
rm -rf git-simulations  
mkdir git-simulations  
cd git-simulations  
git init

# Créer un "faux" fichier binaire (pour la démo)
echo "Binary content version 1" > image.bin  
git add image.bin  
git commit -m "Add binary file"

# Branche 1
git checkout -b branch-a  
echo "Binary content version A" > image.bin  
git add image.bin  
git commit -m "Update binary file (A)"

# Branche 2
git checkout main  
git checkout -b branch-b  
echo "Binary content version B" > image.bin  
git add image.bin  
git commit -m "Update binary file (B)"

# Merger
git merge branch-a
```

**Résultat :**

```
CONFLICT (content): Merge conflict in image.bin
```

**Problème :** Git ne peut pas fusionner les fichiers binaires comme il le fait pour le texte.

**Résolution - Choisir une version :**

```bash
# Option 1 : Garder la version actuelle (branch-b)
git checkout --ours image.bin  
git add image.bin  
git commit -m "Merge: Kept branch-b version of binary"

# Option 2 : Garder la version entrante (branch-a)
git checkout --theirs image.bin  
git add image.bin  
git commit -m "Merge: Kept branch-a version of binary"
```

**💡 Conseil :** Pour les fichiers binaires (images, PDF, etc.), utilisez des outils comme Git LFS (Large File Storage).

---

## Outils pour analyser les situations complexes

### Commandes utiles

```bash
# Voir l'historique graphique détaillé
git log --oneline --graph --all --decorate

# Voir qui a modifié quoi
git blame fichier.txt

# Trouver le commit qui a introduit un bug
git bisect start  
git bisect bad          # Le commit actuel est mauvais  
git bisect good abc123  # Ce vieux commit était bon
# Git vous aide à trouver le commit problématique

# Voir les différences entre branches
git diff branche1..branche2

# Trouver l'ancêtre commun de deux branches
git merge-base branche1 branche2

# Voir tous les commits d'une branche pas dans l'autre
git log branche1..branche2

# Rechercher dans l'historique
git log --grep="mot-clé"  
git log -S"fonction_recherchée"  # Chercher dans le code
```

---

## Stratégies de résolution de conflits

### 1. Conflits simples (même fichier, différentes lignes)

**Git peut résoudre automatiquement** - aucune action nécessaire.

### 2. Conflits de contenu (même ligne modifiée)

**Stratégie :**
1. Ouvrir le fichier
2. Analyser les deux versions
3. Décider quelle version garder (ou combiner)
4. Supprimer les marqueurs de conflit
5. Tester que ça fonctionne
6. `git add` puis `git commit/continue`

### 3. Conflits de structure (fichier supprimé vs modifié)

**Stratégie :**
1. Décider si le fichier doit exister ou non
2. Si oui : `git add fichier`
3. Si non : `git rm fichier`
4. Commit

### 4. Multiples conflits

**Stratégie :**
1. Résoudre un fichier à la fois
2. Tester après chaque résolution
3. `git add` chaque fichier résolu
4. Ne pas se précipiter

### 5. Conflit trop complexe

**Stratégie :**
1. `git merge --abort` ou `git rebase --abort`
2. Analyser le problème
3. Peut-être merger par étapes (branches intermédiaires)
4. Demander de l'aide à l'équipe

---

## Checklist de résolution de conflit

Avant de commiter une résolution de conflit :

- [ ] Tous les marqueurs `<<<<<<<`, `=======`, `>>>>>>>` sont supprimés
- [ ] Le code compile/fonctionne
- [ ] Les tests passent
- [ ] Le fichier est cohérent (pas de code en double ou manquant)
- [ ] Vous comprenez ce que vous avez gardé et pourquoi
- [ ] La résolution a du sens fonctionnellement

---

## Commandes de secours

### En cas de panique

```bash
# Abandonner un merge en cours
git merge --abort

# Abandonner un rebase en cours
git rebase --abort

# Abandonner un cherry-pick en cours
git cherry-pick --abort

# Créer une sauvegarde immédiate
git branch sauvegarde-[date]

# Revenir au dernier commit
git reset --hard HEAD

# Voir tout ce que vous avez fait (pour récupérer)
git reflog
```

---

## Conseils pour éviter les situations complexes

### Prévention

1. **Synchronisez souvent**
   ```bash
   git pull origin main  # Chaque jour
   ```

2. **Branches courtes**
   - Ne gardez pas une branche des semaines sans merger

3. **Commits fréquents**
   - Petits commits = conflits plus faciles à résoudre

4. **Communication**
   - Dites à l'équipe sur quels fichiers vous travaillez

5. **Tests**
   - Testez avant de merger

6. **Backup**
   - Créez des branches de sauvegarde avant opérations risquées
   ```bash
   git branch backup-avant-rebase
   ```

---

## Résumé des situations et solutions

| Situation | Commande pour sortir | Commande pour résoudre |
|-----------|---------------------|------------------------|
| Conflit de merge | `git merge --abort` | Éditer → `git add` → `git commit` |
| Conflit de rebase | `git rebase --abort` | Éditer → `git add` → `git rebase --continue` |
| Mauvais commit | - | `git commit --amend` ou `git revert` |
| Historique sale | - | `git rebase -i HEAD~n` |
| Commit perdu | - | `git reflog` puis `git reset` ou `git branch` |
| Fichier binaire | `git merge --abort` | `git checkout --ours/--theirs fichier` |
| Trop de conflits | `git merge --abort` | Merger par étapes ou demander aide |

---

## Exercice final : Simulation complète

Maintenant que vous avez vu toutes ces situations, créez votre propre scénario complexe :

1. Créez un nouveau dépôt
2. Faites 3 branches divergentes
3. Ajoutez des commits sur chacune
4. Tentez de les merger toutes ensemble
5. Résolvez les conflits
6. Nettoyez l'historique
7. Créez des tags de version

**Objectif :** Être à l'aise avec l'idée que les conflits et situations complexes sont normaux et gérables !

---

## Conclusion

Les situations complexes dans Git peuvent sembler effrayantes, mais avec :
- ✅ La pratique dans un environnement sûr
- ✅ La connaissance des commandes de secours
- ✅ Une approche méthodique
- ✅ Le reflog comme filet de sécurité

**Vous pouvez gérer n'importe quelle situation !**

**Règle d'or :** En cas de doute, créez une branche de sauvegarde d'abord :
```bash
git branch ma-sauvegarde
```

Vous pourrez toujours y revenir si quelque chose tourne mal. Git est très difficile à casser définitivement si vous utilisez le reflog ! 🛡️

**Conseil final :** Pratiquez ces simulations régulièrement. Plus vous serez exposé à ces situations dans un cadre contrôlé, plus vous serez confiant face aux vrais problèmes ! 💪

⏭️ [Quiz et certification finale](/module-10-cas-pratiques/06-quiz-et-certification.md)
