🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 6 : Fonctions avancées
## 5. Déboguer avec git bisect

### Introduction

Imaginez que vous devez trouver un mot spécifique dans un dictionnaire de 1000 pages. Vous pourriez parcourir chaque page une par une (méthode linéaire), ou ouvrir le dictionnaire au milieu, voir si le mot est avant ou après, puis répéter le processus. Cette seconde méthode est bien plus rapide !

C'est exactement ce que fait **git bisect** : il utilise une recherche binaire pour trouver rapidement quel commit a introduit un bug dans votre code. Au lieu de tester 100 commits un par un, vous n'en testerez que 7 !

**Git bisect** est votre détective personnel qui vous aide à répondre à la question : "Quel commit a cassé mon code ?"

---

### Le problème : Quand un bug apparaît

**Situation typique :**

- Votre application fonctionnait il y a 2 semaines
- Aujourd'hui, elle a un bug
- Entre temps, il y a eu 50 commits
- Lequel a introduit le bug ? 🤔

**Solution classique (inefficace) :**

Tester chaque commit un par un : `git checkout <commit>`, tester, recommencer... Avec 50 commits, cela prendrait des heures !

**Solution avec bisect (efficace) :**

Utiliser la recherche binaire pour trouver le commit coupable en seulement 6-7 tests !

---

### Comment fonctionne la recherche binaire

La recherche binaire divise l'espace de recherche en deux à chaque étape :

**Exemple avec 16 commits :**

```
1. Vous avez 16 commits à tester
2. Git teste le commit du milieu (8)
   → Si le bug est présent : chercher dans commits 1-7
   → Si le bug est absent : chercher dans commits 9-16
3. Il reste 8 commits → Git teste le milieu (4 ou 12)
4. Il reste 4 commits → Git teste le milieu
5. Il reste 2 commits → Git teste le milieu
6. 1 commit restant → C'est le coupable !
```

**Nombre de tests nécessaires :**

| Nombre de commits | Tests linéaires | Tests avec bisect |
|-------------------|-----------------|-------------------|
| 10 commits | 10 | 4 |
| 50 commits | 50 | 6 |
| 100 commits | 100 | 7 |
| 1000 commits | 1000 | 10 |

La différence est énorme ! 🚀

---

### Les commandes de base

#### Démarrer une session bisect

```bash
git bisect start
```

Cette commande lance le processus de débogage.

#### Marquer le commit actuel comme mauvais (avec le bug)

```bash
git bisect bad
```

Indique à Git que le commit actuel contient le bug.

#### Marquer un ancien commit comme bon (sans le bug)

```bash
git bisect good <hash-du-commit>
```

Indique à Git un commit où le bug n'existait pas.

**Exemple :**

```bash
# Vous êtes sur le commit actuel qui a le bug
git bisect start
git bisect bad

# Il y a 3 mois, le bug n'existait pas (commit a1b2c3d)
git bisect good a1b2c3d

# Git va maintenant tester le commit du milieu
```

#### Terminer la session bisect

```bash
git bisect reset
```

Retourne sur la branche où vous étiez avant de commencer le bisect.

---

### Workflow complet : Exemple pas à pas

Voici un scénario réaliste pour bien comprendre :

#### Situation

Votre fonction de calcul de prix affiche maintenant des valeurs incorrectes. Le problème n'existait pas il y a 1 mois.

#### Étape 1 : Démarrer bisect

```bash
# Vous êtes sur main avec le bug
git bisect start

# Marquer le commit actuel comme mauvais
git bisect bad
```

#### Étape 2 : Identifier un commit "bon"

```bash
# Trouver un commit ancien sans le bug
git log --oneline
# ... regarder l'historique ...

# Disons que le commit d'il y a 1 mois était bon
git bisect good a1b2c3d
```

**Git répond :**

```
Bisecting: 25 revisions left to test after this (roughly 5 steps)
[m6n5o4p3q2r1] Refactor payment module
```

Git vous place automatiquement sur le commit du milieu (m6n5o4p3q2r1).

#### Étape 3 : Tester le commit

```bash
# Tester votre application à ce commit
npm start  # ou python app.py, etc.

# Vérifier si le bug est présent
# - Utiliser l'interface
# - Lancer les tests
# - Exécuter la fonctionnalité problématique
```

#### Étape 4 : Indiquer le résultat

**Si le bug est présent :**

```bash
git bisect bad
```

**Si le bug est absent :**

```bash
git bisect good
```

Git vous place alors sur un nouveau commit à tester.

#### Étape 5 : Répéter

Continuez à tester et marquer chaque commit comme `good` ou `bad`.

**Exemple de session complète :**

```bash
git bisect start
git bisect bad                    # Commit actuel (bad)
git bisect good a1b2c3d          # Ancien commit (good)

# [Git vous place sur commit m6n5o4p]
# Test... le bug est présent
git bisect bad

# [Git vous place sur commit w8x9y0z]
# Test... le bug est absent
git bisect good

# [Git vous place sur commit t3s2r1q]
# Test... le bug est présent
git bisect bad

# [Git vous place sur commit u4v5w6x]
# Test... le bug est présent
git bisect bad
```

#### Étape 6 : Git trouve le coupable !

```bash
e5f6g7h8i9j0 is the first bad commit
commit e5f6g7h8i9j0
Author: John Doe <john@example.com>
Date:   Mon Sep 15 14:32:18 2024

    Refactor price calculation

:100644 100644 abc123 def456 M    src/price.js
```

Git vous indique exactement quel commit a introduit le bug ! 🎯

#### Étape 7 : Terminer et analyser

```bash
# Voir les détails du commit coupable
git show e5f6g7h8i9j0

# Revenir à votre branche
git bisect reset
```

Vous pouvez maintenant corriger le bug en sachant exactement quelle modification l'a causé !

---

### Visualiser le processus

Voici comment Git navigue dans l'historique :

```
État initial :
GOOD ← ? ← ? ← ? ← ? ← ? ← ? ← ? ← ? ← ? ← BAD
a1b2c3d                                        HEAD

Après git bisect good a1b2c3d :
GOOD ← ? ← ? ← ? ← [?] ← ? ← ? ← ? ← ? ← ? ← BAD
a1b2c3d            TEST                        HEAD

Supposons que TEST est BAD :
GOOD ← ? ← [?] ← ? ← BAD
       TEST

Supposons que TEST est GOOD :
GOOD ← ? ← [?] ← BAD
            TEST

Et ainsi de suite jusqu'à trouver le premier commit BAD
```

---

### Bisect automatique avec un script

Si vous pouvez automatiser le test (avec un test unitaire par exemple), bisect peut fonctionner tout seul !

#### Syntaxe

```bash
git bisect start
git bisect bad
git bisect good <hash>
git bisect run <commande-de-test>
```

La `<commande-de-test>` doit retourner :
- **0** si le test passe (commit bon)
- **1-127** (sauf 125) si le test échoue (commit mauvais)
- **125** si le commit ne peut pas être testé (Git le saute)

#### Exemple avec un test unitaire

```bash
# Démarrer bisect
git bisect start
git bisect bad
git bisect good a1b2c3d

# Lancer bisect automatique avec les tests
git bisect run npm test

# Git teste automatiquement tous les commits
# et trouve le coupable sans intervention !
```

**Git affichera :**

```
running npm test
✓ Test passed
Bisecting: 12 revisions left to test
running npm test
✗ Test failed
Bisecting: 6 revisions left to test
running npm test
✓ Test passed
...
e5f6g7h8i9j0 is the first bad commit
```

#### Exemple avec un script personnalisé

```bash
# Créer un script de test
cat > test.sh << 'EOF'
#!/bin/bash
# Compiler le projet
make || exit 125  # 125 = impossible à tester

# Lancer le test
./app --test-price-calculation
EOF

chmod +x test.sh

# Lancer bisect automatique
git bisect start
git bisect bad
git bisect good a1b2c3d
git bisect run ./test.sh
```

---

### Options et commandes avancées

#### Sauter un commit

Si un commit ne peut pas être testé (ne compile pas, dépendances manquantes) :

```bash
git bisect skip
```

Git marquera ce commit comme non testable et continuera avec un autre.

#### Marquer plusieurs commits à la fois

```bash
# Marquer une plage de commits comme bons
git bisect good v2.0..v2.5

# Marquer plusieurs commits comme mauvais
git bisect bad <commit1> <commit2>
```

#### Voir l'état actuel du bisect

```bash
git bisect log
```

Affiche tout l'historique de votre session bisect.

**Exemple de sortie :**

```
git bisect start
# bad: [current] Current commit
git bisect bad
# good: [a1b2c3d] Old working commit
git bisect good a1b2c3d
# bad: [m6n5o4p] Middle commit
git bisect bad m6n5o4p
# good: [w8x9y0z] Another middle commit
git bisect good w8x9y0z
```

#### Visualiser les commits restants

```bash
git bisect visualize
# ou
git bisect view
```

Ouvre gitk ou git log pour voir graphiquement où vous en êtes.

#### Rejouer une session bisect

```bash
# Sauvegarder la session
git bisect log > bisect-session.txt

# Plus tard, rejouer la même session
git bisect replay bisect-session.txt
```

---

### Cas d'usage pratiques

#### Cas 1 : Test de régression

```bash
# Un test qui passait il y a 2 semaines échoue maintenant
git bisect start
git bisect bad
git bisect good HEAD~50
git bisect run npm test -- specific-test.js
```

#### Cas 2 : Bug visuel dans l'interface

```bash
git bisect start
git bisect bad

# Trouver un commit où l'interface était correcte
git log --oneline
git bisect good <hash-d-il-y-a-1-mois>

# Tester manuellement à chaque étape
# Lancer l'app et vérifier visuellement
# Marquer good ou bad selon ce que vous voyez
```

#### Cas 3 : Performance dégradée

```bash
# Créer un script de benchmark
cat > bench.sh << 'EOF'
#!/bin/bash
result=$(node benchmark.js)
# Si le temps est > 500ms, c'est mauvais
if [ $result -gt 500 ]; then
  exit 1
else
  exit 0
fi
EOF

git bisect start
git bisect bad
git bisect good <commit-rapide>
git bisect run ./bench.sh
```

#### Cas 4 : Bug de compilation

```bash
git bisect start
git bisect bad
git bisect good <dernier-commit-qui-compilait>

# Script qui teste la compilation
git bisect run sh -c "make clean && make"
```

---

### Conseils et bonnes pratiques

✅ **Ayez un critère de test clair** : Définissez précisément ce qui constitue un "good" et un "bad"

✅ **Commencez avec un bon commit certain** : Choisissez un commit où vous êtes sûr que le bug n'existe pas

✅ **Automatisez quand c'est possible** : `git bisect run` fait gagner beaucoup de temps

✅ **Testez de manière cohérente** : Utilisez toujours la même méthode de test à chaque étape

✅ **Documentez vos trouvailles** :

```bash
# Quand vous trouvez le commit coupable
git show <commit-coupable> > bug-analysis.txt
git log -p <commit-coupable> >> bug-analysis.txt
```

✅ **Utilisez des tags ou branches pour les commits "good" connus** :

```bash
git tag last-working-version a1b2c3d
git bisect good last-working-version
```

❌ **N'oubliez pas de faire `git bisect reset`** : Sinon vous restez en mode bisect

❌ **Ne modifiez pas de fichiers pendant le bisect** : Cela fausserait les tests

❌ **Attention aux dépendances externes** : Un commit peut échouer pour des raisons externes (API down, base de données)

---

### Gérer les cas particuliers

#### Le commit ne compile pas

```bash
# Si un commit ne compile pas
git bisect skip

# Ou dans un script automatique
git bisect run sh -c "make || exit 125"
# 125 = code pour dire "sauter ce commit"
```

#### Plusieurs commits introduisent des bugs différents

Si vous trouvez un commit qui a un *autre* bug :

```bash
# Noter le commit avec l'autre bug
git bisect skip

# Ou terminer cette session et en démarrer une autre
git bisect reset
# Corriger d'abord le bug trouvé
# Puis relancer bisect pour l'autre bug
```

#### Le bug est intermittent

Pour les bugs intermittents, testez plusieurs fois :

```bash
# Script qui teste 3 fois
cat > test-multiple.sh << 'EOF'
#!/bin/bash
for i in 1 2 3; do
  npm test || exit 1
done
exit 0
EOF

git bisect run ./test-multiple.sh
```

---

### Récapitulatif des commandes

| Commande | Description |
|----------|-------------|
| `git bisect start` | Démarre une session de débogage |
| `git bisect bad [commit]` | Marque un commit comme mauvais (avec bug) |
| `git bisect good [commit]` | Marque un commit comme bon (sans bug) |
| `git bisect skip` | Saute un commit non testable |
| `git bisect reset` | Termine la session et revient à la branche |
| `git bisect run <cmd>` | Exécute automatiquement avec une commande |
| `git bisect log` | Affiche l'historique de la session |
| `git bisect visualize` | Visualise graphiquement le bisect |
| `git bisect replay <file>` | Rejoue une session depuis un fichier |

---

### Comprendre les codes de sortie pour bisect run

Pour que `git bisect run` fonctionne correctement, votre script doit retourner les bons codes :

| Code de sortie | Signification | Action de Git |
|----------------|---------------|---------------|
| 0 | Test réussi | Marque le commit comme `good` |
| 1-124, 126-127 | Test échoué | Marque le commit comme `bad` |
| 125 | Impossible à tester | Saute le commit |
| Autres (128+) | Erreur du script | Arrête le bisect |

**Exemple de script robuste :**

```bash
#!/bin/bash
set -e  # Arrêter si erreur

# Construire le projet
if ! make clean && make; then
  exit 125  # Impossible à compiler, sauter
fi

# Lancer les tests
if npm test; then
  exit 0  # Tests passent, commit bon
else
  exit 1  # Tests échouent, commit mauvais
fi
```

---

### Bisect dans un projet réel

Voici un exemple complet avec un vrai projet JavaScript :

```bash
# 1. Le bug : La fonction totalPrice retourne NaN
git bisect start
git bisect bad

# 2. Trouver un commit où ça marchait
git log --since="2024-09-01" --oneline
git bisect good 5f3e8c2

# 3. Créer un script de test automatique
cat > test-price.js << 'EOF'
const { totalPrice } = require('./src/price.js');
const result = totalPrice([10, 20, 30]);
if (isNaN(result)) {
  console.error('FAILED: totalPrice returns NaN');
  process.exit(1);
}
console.log('PASSED: totalPrice returns', result);
process.exit(0);
EOF

# 4. Lancer le bisect automatique
git bisect run node test-price.js

# 5. Résultat :
# e7f8g9h is the first bad commit
# Author: Alice <alice@example.com>
# Date:   Tue Sep 17 10:45:32 2024
#
#     Refactor price calculation to use map

# 6. Examiner le commit coupable
git show e7f8g9h

# 7. Vous trouvez la ligne problématique :
# - return items.reduce((sum, item) => sum + item.price, 0);
# + return items.map(item => item.price);  ← Bug ! map retourne un array

# 8. Terminer
git bisect reset

# 9. Corriger le bug
# ... modifications ...
git commit -m "fix: Correct totalPrice to return sum instead of array"
```

---

### Alternatives à bisect

Git bisect n'est pas toujours la meilleure solution :

| Situation | Solution alternative |
|-----------|---------------------|
| Le bug vient du dernier commit | Utiliser `git log -p` et `git show` |
| Vous savez quelle fonctionnalité cause le bug | Utiliser `git log --grep` ou `git log -- <file>` |
| Le bug apparaît après un merge | Utiliser `git log --merge` |
| Vous voulez voir qui a modifié une ligne | Utiliser `git blame` |

---

### Conclusion

**Git bisect** est un outil extrêmement puissant pour trouver rapidement quel commit a introduit un bug. Grâce à la recherche binaire, vous pouvez localiser un problème parmi des centaines de commits en seulement quelques minutes.

**Points clés à retenir :**

- `git bisect start` pour commencer
- `git bisect bad` pour le commit avec le bug
- `git bisect good <hash>` pour un commit sans le bug
- Tester et marquer chaque commit que Git propose
- `git bisect reset` pour terminer
- `git bisect run <script>` pour automatiser complètement le processus

**La formule magique :**

Avec n commits à tester, bisect ne nécessite que **log₂(n)** tests !

**Quand utiliser bisect :**

- Vous savez qu'un bug existe maintenant mais pas avant
- Vous avez un test reproductible du bug
- L'historique est trop long pour tester manuellement
- Vous voulez gagner du temps dans le débogage

Avec git bisect, vous n'aurez plus jamais à dire : "Je ne sais pas quel commit a cassé mon code !" 🎯

---

**Prochaine étape :** Module 6.6 - Tracer les modifications (git blame et git log -S)

⏭️ [Tracer les modifications (git blame et git log -S)](/module-06-fonctions-avancees/06-tracer-les-modifications.md)
