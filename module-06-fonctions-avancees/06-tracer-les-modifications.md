🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 6. Tracer les modifications (git blame et git log -S)

### Introduction

Imaginez que vous lisez un livre et que vous voulez savoir qui a écrit un paragraphe spécifique et pourquoi. Ou que vous cherchez quand un mot particulier a été ajouté au texte. C'est exactement ce que permettent **git blame** et **git log -S** !

Ces deux outils sont essentiels pour :
- Comprendre **qui** a écrit une ligne de code et **pourquoi**
- Retrouver **quand** une fonctionnalité a été ajoutée ou supprimée
- Suivre l'évolution d'une partie spécifique du code
- Déboguer en remontant à l'origine d'un problème

**git blame** répond à : "Qui a modifié cette ligne ?"
**git log -S** répond à : "Quand ce texte est-il apparu ou disparu ?"

---

## Git Blame : Qui a modifié quoi ?

### Qu'est-ce que git blame ?

**Git blame** vous montre, ligne par ligne, qui a modifié chaque partie d'un fichier, quand, et dans quel commit. C'est comme avoir l'historique complet annoté de chaque ligne de code.

**Attention au nom :** "Blame" signifie "blâmer" en anglais, mais ce n'est pas pour pointer du doigt ! C'est simplement pour tracer l'origine du code. Pensez plutôt à "praise" (louange) quand vous trouvez du bon code ! 😊

---

### La commande de base

```bash
git blame <nom-du-fichier>
```

**Exemple :**

```bash
git blame src/utils.js
```

**Sortie typique :**

```
a1b2c3d4 (Alice Martin  2024-09-15 10:30:45 +0200  1) function calculatePrice(items) {
a1b2c3d4 (Alice Martin  2024-09-15 10:30:45 +0200  2)   let total = 0;
e5f6g7h8 (Bob Durant    2024-10-01 14:22:11 +0200  3)   for (const item of items) {
e5f6g7h8 (Bob Durant    2024-10-01 14:22:11 +0200  4)     total += item.price * item.quantity;
a1b2c3d4 (Alice Martin  2024-09-15 10:30:45 +0200  5)   }
i9j0k1l2 (Charlie Lee   2024-10-05 09:15:33 +0200  6)   return Math.round(total * 100) / 100;
a1b2c3d4 (Alice Martin  2024-09-15 10:30:45 +0200  7) }
```

**Comment lire cette sortie :**

| Élément | Description | Exemple |
|---------|-------------|---------|
| Hash du commit | Identifiant court du commit | `a1b2c3d4` |
| Auteur | Qui a modifié la ligne | `Alice Martin` |
| Date | Quand la modification a été faite | `2024-09-15 10:30:45` |
| Numéro de ligne | Position dans le fichier | `1)`, `2)`, etc. |
| Contenu | Le code de la ligne | `function calculatePrice(items) {` |

**Interprétation :**

- Les lignes 1, 2, 5, 7 ont été écrites par Alice le 15 septembre
- Les lignes 3, 4 ont été modifiées par Bob le 1er octobre
- La ligne 6 a été modifiée par Charlie le 5 octobre

---

### Options utiles de git blame

#### Voir uniquement un intervalle de lignes

```bash
git blame -L 10,20 fichier.js
```

Montre uniquement les lignes 10 à 20.

**Exemple pratique :**

```bash
# Voir qui a modifié les lignes 50 à 60
git blame -L 50,60 src/app.js

# Voir à partir de la ligne 100 jusqu'à la fin
git blame -L 100, src/app.js
```

#### Afficher l'adresse email de l'auteur

```bash
git blame -e fichier.js
```

**Sortie :**

```
a1b2c3d4 (<alice@example.com> 2024-09-15 10:30:45 +0200  1) function calculatePrice(items) {
```

#### Format plus lisible

```bash
git blame -w fichier.js
```

L'option `-w` ignore les changements de whitespace (espaces, tabulations).

```bash
git blame -M fichier.js
```

L'option `-M` détecte les lignes déplacées dans le même fichier.

```bash
git blame -C fichier.js
```

L'option `-C` détecte les lignes copiées depuis d'autres fichiers.

#### Voir le commit complet d'une ligne

```bash
git blame -L 15,15 fichier.js | cut -d' ' -f1 | xargs git show
```

Cette commande enchaînée :
1. Trouve le commit de la ligne 15
2. Affiche les détails complets de ce commit

**Astuce plus simple :**

```bash
git log -p -L 15,15:fichier.js
```

---

### Cas d'usage pratiques avec git blame

#### Cas 1 : Comprendre une ligne de code mystérieuse

**Situation :** Vous trouvez un code étrange et voulez savoir pourquoi il existe.

```bash
# Vous voyez cette ligne bizarre dans le code :
# if (price < 0) price = 0;

# Qui l'a ajoutée et pourquoi ?
git blame src/price.js

# Résultat :
# e5f6g7h8 (Bob Durant 2024-10-01 14:22:11 +0200 42) if (price < 0) price = 0;

# Voir le commit complet avec le message
git show e5f6g7h8

# Message du commit :
# "fix: Prevent negative prices from API errors
#  Some external APIs sometimes return negative values
#  due to data corruption. This prevents display issues."

# Maintenant vous comprenez pourquoi cette vérification existe !
```

#### Cas 2 : Trouver l'origine d'un bug

**Situation :** Une fonction retourne des valeurs incorrectes.

```bash
# Examiner la fonction problématique
git blame src/calculator.js -L 50,70

# Identifier les lignes suspectes
# Voir les commits associés
git show <hash-du-commit-suspect>

# Contacter l'auteur si nécessaire pour comprendre
```

#### Cas 3 : Audit de sécurité

**Situation :** Vous devez vérifier qui a modifié une fonction sensible.

```bash
# Voir l'historique de modifications d'une fonction d'authentification
git blame src/auth.js -L 100,150

# Vérifier si toutes les modifications ont été reviewées
git log --author="suspicious@email.com" -- src/auth.js
```

#### Cas 4 : Révision de code legacy

**Situation :** Vous héritez d'un vieux projet et devez comprendre le code.

```bash
# Pour chaque fichier important
git blame src/core.js

# Identifier les sections anciennes (dates anciennes)
# Identifier les sections récentes (dates récentes)
# Comprendre l'évolution du code
```

---

### Ignorer certains commits avec git blame

Parfois, des commits de formatage (prettier, linter) obscurcissent l'historique réel. Git permet de les ignorer !

#### Créer un fichier .git-blame-ignore-revs

```bash
# Créer le fichier
cat > .git-blame-ignore-revs << 'EOF'
# Reformatage avec prettier
a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6

# Migration vers eslint
q9r8s7t6u5v4w3x2y1z0a9b8c7d6e5f4
EOF
```

#### Configurer Git pour l'utiliser

```bash
git config blame.ignoreRevsFile .git-blame-ignore-revs
```

Maintenant, `git blame` ignorera automatiquement ces commits !

---

## Git Log -S : Rechercher dans l'historique

### Qu'est-ce que git log -S ?

**Git log -S** (aussi appelé "pickaxe") recherche tous les commits qui ont **ajouté ou supprimé** une chaîne de caractères spécifique. C'est comme faire une recherche dans tout l'historique du projet.

**Utilité :**
- Trouver quand une fonction a été ajoutée
- Découvrir quand une variable a été supprimée
- Retracer l'évolution d'une fonctionnalité
- Déboguer en trouvant quand un bug a été introduit

---

### La commande de base

```bash
git log -S "texte-à-rechercher"
```

**Exemple :**

```bash
git log -S "calculatePrice"
```

Cette commande trouve tous les commits qui ont ajouté ou supprimé le texte "calculatePrice".

**Sortie typique :**

```
commit e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
Author: Bob Durant <bob@example.com>
Date:   Tue Oct 1 14:22:11 2024 +0200

    Refactor price calculation

commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
Author: Alice Martin <alice@example.com>
Date:   Mon Sep 15 10:30:45 2024 +0200

    Add price calculation feature
```

---

### Afficher les différences avec -S

```bash
git log -S "calculatePrice" -p
```

L'option `-p` (patch) montre les modifications exactes dans chaque commit.

**Exemple de sortie :**

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
Author: Alice Martin <alice@example.com>
Date:   Mon Sep 15 10:30:45 2024 +0200

    Add price calculation feature

diff --git a/src/utils.js b/src/utils.js
index abc123..def456 100644
--- a/src/utils.js
+++ b/src/utils.js
@@ -1,0 +1,7 @@
+function calculatePrice(items) {
+  let total = 0;
+  for (const item of items) {
+    total += item.price;
+  }
+  return total;
+}
```

Le `+` indique que ces lignes ont été **ajoutées**.

---

### Rechercher dans des fichiers spécifiques

```bash
git log -S "calculatePrice" -- src/utils.js
```

Recherche uniquement dans le fichier `src/utils.js`.

**Avec un pattern de fichiers :**

```bash
git log -S "calculatePrice" -- "*.js"
```

Recherche dans tous les fichiers JavaScript.

---

### Git log -G : Recherche avec expressions régulières

Pour des recherches plus complexes, utilisez `-G` avec des regex :

```bash
git log -G "function calculate.*Price"
```

**Différence entre -S et -G :**

| `-S "texte"` | `-G "pattern"` |
|--------------|----------------|
| Recherche exacte | Recherche regex |
| Plus rapide | Plus flexible |
| Trouve les ajouts/suppressions | Trouve les modifications |
| Compte les occurrences | Matche le pattern |

**Exemple :**

```bash
# Trouver tous les commits mentionnant des fonctions de prix
git log -G "function.*[Pp]rice"

# Trouver tous les commits avec des TODO ou FIXME
git log -G "TODO|FIXME"

# Trouver les commits qui modifient des imports
git log -G "^import.*from"
```

---

### Options utiles avec git log -S

#### Afficher uniquement les noms de fichiers

```bash
git log -S "calculatePrice" --name-only
```

**Sortie :**

```
commit e5f6g7h8...
Author: Bob Durant
Date: Tue Oct 1 14:22:11 2024

    Refactor price calculation

src/utils.js
src/price.js
```

#### Format plus compact

```bash
git log -S "calculatePrice" --oneline
```

**Sortie :**

```
e5f6g7h8 Refactor price calculation
a1b2c3d4 Add price calculation feature
```

#### Limiter le nombre de résultats

```bash
git log -S "calculatePrice" -n 5
```

Montre seulement les 5 derniers commits.

#### Rechercher dans une période

```bash
git log -S "calculatePrice" --since="2024-09-01" --until="2024-10-01"
```

Recherche entre deux dates.

#### Exclure des branches

```bash
git log -S "calculatePrice" main ^develop
```

Recherche dans `main` mais pas dans `develop`.

---

### Cas d'usage pratiques avec git log -S

#### Cas 1 : Quand une fonction a-t-elle été ajoutée ?

```bash
# Rechercher la première apparition de la fonction
git log -S "function loginUser" --reverse

# Le premier résultat est le commit qui l'a ajoutée
```

#### Cas 2 : Quand une variable a-t-elle été supprimée ?

```bash
# Rechercher l'ancienne variable
git log -S "oldVariableName" -p

# Regarder les diff avec des lignes commençant par -
# Cela montre quand elle a été supprimée
```

#### Cas 3 : Tracer l'évolution d'une API

```bash
# Voir tous les changements dans une fonction API
git log -S "apiEndpoint" -p -- src/api/

# Analyser comment l'endpoint a évolué
```

#### Cas 4 : Trouver qui a introduit une dépendance

```bash
# Rechercher dans package.json
git log -S "express" -- package.json -p

# Voir quand la dépendance a été ajoutée
```

#### Cas 5 : Déboguer une régression

```bash
# Vous savez qu'une fonction marchait avant
# Rechercher quand elle a été modifiée
git log -S "problematicFunction" -p --since="2024-09-01"

# Examiner chaque modification pour trouver le bug
```

---

### Combiner git blame et git log -S

Ces deux outils se complètent parfaitement !

**Workflow de débogage complet :**

```bash
# 1. Trouver la ligne problématique avec blame
git blame src/utils.js -L 42,42
# → commit e5f6g7h8 (Bob Durant 2024-10-01)

# 2. Voir le commit complet
git show e5f6g7h8

# 3. Chercher quand cette fonction a été ajoutée initialement
git log -S "calculatePrice" --reverse

# 4. Voir toute l'évolution de cette fonction
git log -S "calculatePrice" -p

# 5. Comprendre le contexte et l'histoire complète
```

**Exemple concret :**

```bash
# Situation : Un calcul de taxe est incorrect

# Étape 1 : Trouver qui a modifié la ligne
git blame src/tax.js -L 15,15
# → commit a1b2c3d4 (Alice 2024-09-20)

# Étape 2 : Voir ce commit
git show a1b2c3d4
# Message : "Update tax rate for new regulations"

# Étape 3 : Voir tous les changements de tax rate
git log -S "taxRate" -p

# Étape 4 : Comparer avec l'ancienne implémentation
git log -S "taxRate" --reverse -p -n 1

# Conclusion : Le taux a été mis à jour mais la formule est incorrecte
```

---

### Git log avec d'autres options de recherche

#### Rechercher dans les messages de commit

```bash
git log --grep="price"
```

Trouve les commits dont le message contient "price".

#### Rechercher par auteur

```bash
git log --author="Alice"
```

Trouve tous les commits d'Alice.

#### Combiner plusieurs critères

```bash
git log -S "calculatePrice" --author="Bob" --since="2024-09-01"
```

Trouve les commits de Bob depuis septembre qui modifient "calculatePrice".

#### Rechercher les commits qui touchent un fichier

```bash
git log -- src/utils.js
```

Montre tous les commits qui ont modifié ce fichier.

---

### Outils graphiques pour blame

Si vous préférez une interface visuelle :

#### Dans VS Code

- Installer l'extension "GitLens"
- Cliquer sur une ligne de code
- Voir automatiquement l'auteur et le commit

#### Dans GitHub

- Ouvrir un fichier sur github.com
- Cliquer sur "Blame" en haut à droite
- Voir une vue annotée du fichier

#### Dans GitKraken / Sourcetree

- Ouvrir le fichier
- Menu "Blame" ou "Annotate"
- Vue interactive de l'historique

---

### Conseils et bonnes pratiques

✅ **Utilisez git blame pour comprendre, pas pour blâmer** : Le but est de comprendre le contexte, pas de pointer du doigt

✅ **Combinez blame et show** : Toujours lire le commit complet pour comprendre le contexte

```bash
git blame fichier.js -L 42,42 | cut -d' ' -f1 | xargs git show
```

✅ **Utilisez -w pour ignorer les whitespaces** : Les changements de formatage ne sont pas intéressants

✅ **Documentez les commits de formatage** : Créez un fichier `.git-blame-ignore-revs`

✅ **Utilisez -S pour les recherches historiques** : C'est plus efficace que parcourir manuellement l'historique

✅ **Combinez avec grep pour filtrer** :

```bash
git log -S "calculatePrice" --oneline | grep "refactor"
```

❌ **Ne jugez pas sur un blame** : Le code peut avoir été écrit dans un contexte différent

❌ **N'oubliez pas les mouvements de fichiers** : Utilisez `-M` et `-C` si nécessaire

❌ **Attention aux gros historiques** : `git log -S` peut être lent sur de très gros projets

---

### Récapitulatif des commandes

#### Git Blame

| Commande | Description |
|----------|-------------|
| `git blame <fichier>` | Affiche qui a modifié chaque ligne |
| `git blame -L 10,20 <fichier>` | Blame sur les lignes 10 à 20 |
| `git blame -e <fichier>` | Affiche les emails des auteurs |
| `git blame -w <fichier>` | Ignore les whitespaces |
| `git blame -M <fichier>` | Détecte les lignes déplacées |
| `git blame -C <fichier>` | Détecte les lignes copiées |

#### Git Log -S

| Commande | Description |
|----------|-------------|
| `git log -S "texte"` | Commits qui ajoutent/suppriment "texte" |
| `git log -S "texte" -p` | Idem avec les différences |
| `git log -S "texte" -- <fichier>` | Recherche dans un fichier spécifique |
| `git log -G "pattern"` | Recherche avec regex |
| `git log -S "texte" --oneline` | Format compact |
| `git log -S "texte" --since="2024-01-01"` | Avec filtre de date |
| `git log --grep="texte"` | Recherche dans les messages |

---

### Exemples de workflow complets

#### Workflow 1 : Investiguer un bug

```bash
# 1. Trouver le fichier problématique
# 2. Utiliser blame pour voir qui a modifié la section
git blame src/bug.js -L 100,120

# 3. Examiner le commit identifié
git show <hash>

# 4. Voir l'historique complet de cette section
git log -p -L 100,120:src/bug.js

# 5. Rechercher quand le bug a été introduit
git log -S "buggyFunction" -p

# 6. Trouver le commit coupable avec bisect si nécessaire
git bisect start
```

#### Workflow 2 : Documentation du code

```bash
# 1. Pour comprendre une fonction
git blame src/complex.js -L 50,80

# 2. Lire tous les commits associés
# Extraire les hashs avec cut/awk, puis faire des git show

# 3. Créer une documentation basée sur l'historique
git log -S "complexFunction" -p > docs/function-history.txt
```

#### Workflow 3 : Revue de sécurité

```bash
# 1. Identifier les fonctions sensibles
git blame src/auth.js src/payment.js

# 2. Vérifier qui a modifié ces fichiers
git log --author=".*" -- src/auth.js src/payment.js

# 3. Rechercher des patterns sensibles
git log -G "password|token|secret" -p

# 4. Auditer les commits suspects
git show <hash>
```

---

### Alternatives et outils complémentaires

| Besoin | Outil |
|--------|-------|
| Interface graphique de blame | GitLens (VS Code), GitHub UI |
| Recherche avancée dans l'historique | `git log --all --full-history` |
| Voir qui a modifié une fonction | `git log -L :<function>:<file>` |
| Statistiques par auteur | `git shortlog` |
| Voir les contributeurs d'un fichier | `git shortlog -sn -- <file>` |

---

### Conclusion

**Git blame** et **git log -S** sont deux outils complémentaires essentiels pour comprendre l'historique de votre code :

- **git blame** vous montre ligne par ligne l'origine du code actuel
- **git log -S** vous permet de remonter dans le temps pour voir l'évolution

**Points clés à retenir :**

- `git blame <fichier>` pour voir qui a modifié quoi
- `git log -S "texte"` pour trouver quand du code a été ajouté/supprimé
- `git log -G "pattern"` pour des recherches avec regex
- Combinez les deux pour une investigation complète
- Utilisez toujours ces outils pour comprendre, jamais pour blâmer

**Cas d'usage principaux :**

1. Comprendre du code mystérieux ou complexe
2. Déboguer en trouvant l'origine d'un problème
3. Auditer des modifications sensibles
4. Documenter l'évolution d'une fonctionnalité

Avec ces outils, vous pouvez devenir un véritable détective du code et comprendre l'histoire complète de votre projet ! 🔍

---

**Prochaine étape :** Module 6.7 - Tags et releases

⏭️ [Tags et releases](/module-06-fonctions-avancees/07-tags-et-releases.md)
