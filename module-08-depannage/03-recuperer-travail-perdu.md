🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 8 : Dépannage et résolution de problèmes

## 3. Récupérer du travail perdu (avec reflog et reset)

### Introduction : Git n'oublie (presque) jamais

Une des grandes forces de Git est qu'il **conserve pratiquement tout** pendant un certain temps, même ce qui semble supprimé. Si vous avez accidentellement perdu des commits, effacé une branche, ou fait un reset trop agressif, il y a de très bonnes chances de pouvoir récupérer votre travail.

Le secret ? Le **reflog** (reference log), votre machine à remonter le temps dans Git.

### Qu'est-ce que le reflog ?

Le **reflog** est un journal qui enregistre **tous les mouvements de HEAD** dans votre dépôt local. Chaque fois que vous :
- Faites un commit
- Changez de branche
- Faites un merge, rebase, reset
- Faites pratiquement n'importe quelle opération

Git enregistre où était HEAD avant et après l'opération. C'est comme un historique de navigation pour votre dépôt.

**Point important :** Le reflog est **local** à votre machine. Il n'est pas partagé avec d'autres via `git push`.

### Consulter le reflog

La commande de base :

```bash
git reflog
```

Vous verrez quelque chose comme :

```
a1b2c3d HEAD@{0}: commit: Ajout de la nouvelle fonctionnalité  
e4f5g6h HEAD@{1}: checkout: moving from feature to main  
i7j8k9l HEAD@{2}: commit: Correction du bug critique  
m0n1o2p HEAD@{3}: rebase finished: returning to refs/heads/feature  
q3r4s5t HEAD@{4}: commit: Travail en cours  
u5v6w7x HEAD@{5}: reset: moving to HEAD~2  
y8z9a0b HEAD@{6}: commit: Version complète de la fonction
```

**Comment lire le reflog :**
- **Colonne 1** (ex: `a1b2c3d`) : Le hash du commit
- **Colonne 2** (ex: `HEAD@{0}`) : La référence temporelle (0 = le plus récent)
- **Colonne 3** : La description de l'action effectuée

L'ordre va du **plus récent** (en haut) au **plus ancien** (en bas).

### Format plus détaillé du reflog

Pour avoir plus d'informations avec les dates :

```bash
git reflog show --date=relative
```

Résultat :
```
a1b2c3d HEAD@{5 minutes ago}: commit: Ajout de la nouvelle fonctionnalité  
e4f5g6h HEAD@{2 hours ago}: checkout: moving from feature to main  
i7j8k9l HEAD@{3 hours ago}: commit: Correction du bug critique
```

Ou avec les dates complètes :

```bash
git reflog show --date=iso
```

### Scénario 1 : Récupérer un commit après un reset --hard

**Situation :** Vous avez fait un `git reset --hard HEAD~3` et vous regrettez d'avoir perdu ces 3 commits.

**Solution :**

**Étape 1 : Consulter le reflog**
```bash
git reflog
```

Résultat :
```
a1b2c3d HEAD@{0}: reset: moving to HEAD~3  
e4f5g6h HEAD@{1}: commit: Commit important 3  
i7j8k9l HEAD@{2}: commit: Commit important 2  
m0n1o2p HEAD@{3}: commit: Commit important 1  
q3r4s5t HEAD@{4}: commit: Commit de base
```

**Étape 2 : Identifier le commit avant le reset**

Vous voyez que `e4f5g6h HEAD@{1}` était l'état avant le reset (votre dernier commit avant la catastrophe).

**Étape 3 : Revenir à cet état**
```bash
git reset --hard e4f5g6h
```

Ou en utilisant la référence HEAD :
```bash
git reset --hard HEAD@{1}
```

**✅ Vos 3 commits sont récupérés !**

**Visualisation :**

Avant le reset malheureux :
```
... --- A --- B --- C --- D (HEAD, main)
```

Après `git reset --hard HEAD~3` :
```
... --- A (HEAD, main)
        B --- C --- D (perdus... mais pas vraiment !)
```

Après la récupération avec reflog :
```
... --- A --- B --- C --- D (HEAD, main)
```

### Scénario 2 : Récupérer une branche supprimée

**Situation :** Vous avez supprimé une branche par erreur et vous voulez la récupérer.

```bash
# Suppression accidentelle
git branch -D ma-branche-importante
```

**Solution :**

**Étape 1 : Trouver le dernier commit de la branche**
```bash
git reflog
```

Cherchez dans l'historique la dernière fois que vous étiez sur cette branche :
```
a1b2c3d HEAD@{0}: checkout: moving from ma-branche-importante to main  
e4f5g6h HEAD@{1}: commit: Dernier commit sur ma-branche-importante  
i7j8k9l HEAD@{2}: commit: Avant-dernier commit
```

**Étape 2 : Recréer la branche à partir du bon commit**
```bash
git checkout -b ma-branche-importante e4f5g6h
```

Ou plus simplement avec la référence HEAD :
```bash
git checkout -b ma-branche-importante HEAD@{1}
```

**✅ Votre branche est restaurée !**

### Scénario 3 : Récupérer des modifications après un checkout malheureux

**Situation :** Vous aviez des modifications non commitées, vous avez fait un `git checkout` sur une autre branche et perdu vos changements.

**Important :** Si les modifications n'étaient **jamais commitées**, elles sont généralement perdues définitivement. **Mais** si vous les aviez ajoutées au staging (avec `git add`), il y a une chance !

**Solution :**

```bash
# Voir tous les objets non attachés
git fsck --lost-found
```

Cette commande trouve tous les "objets pendants" (dangling objects) dans votre dépôt.

Ensuite, examinez les commits trouvés :
```bash
# Voir le contenu d'un commit trouvé
git show <hash-du-commit-trouvé>
```

Si vous trouvez vos modifications, créez une branche :
```bash
git branch recuperation <hash-du-commit>
```

### Scénario 4 : Annuler un rebase raté avec reflog

**Situation :** Vous avez fait un rebase qui a mal tourné et vous voulez revenir en arrière.

**Solution :**

**Étape 1 : Consulter le reflog**
```bash
git reflog
```

```
a1b2c3d HEAD@{0}: rebase finished: returning to refs/heads/feature  
e4f5g6h HEAD@{1}: rebase: commit 3  
i7j8k9l HEAD@{2}: rebase: commit 2  
m0n1o2p HEAD@{3}: rebase: commit 1  
q3r4s5t HEAD@{4}: rebase: checkout main  
u5v6w7x HEAD@{5}: commit: Mon dernier commit avant le rebase
```

**Étape 2 : Revenir avant le rebase**

Trouvez la ligne juste avant `rebase: checkout` :
```bash
git reset --hard u5v6w7x
```

Ou :
```bash
git reset --hard HEAD@{5}
```

**✅ Vous êtes revenu à l'état d'avant le rebase !**

### Scénario 5 : Récupérer un commit après un commit --amend

**Situation :** Vous avez modifié votre dernier commit avec `git commit --amend` et vous voulez récupérer la version originale.

**Solution :**

```bash
git reflog
```

```
a1b2c3d HEAD@{0}: commit (amend): Message modifié  
e4f5g6h HEAD@{1}: commit: Message original
```

Pour revenir au commit original :
```bash
git reset --hard e4f5g6h
```

### Comprendre les différentes options de reset

Quand vous récupérez du travail avec le reflog, vous utiliserez souvent `git reset`. Voici les trois options principales :

#### 1. `git reset --soft <commit>`

- **Déplace HEAD** vers le commit spécifié
- **Conserve vos modifications** dans le staging area (prêtes à être commitées)
- **Conserve les fichiers modifiés** dans votre répertoire de travail

**Utilisation :** Quand vous voulez "défaire" des commits mais garder tous les changements.

#### 2. `git reset --mixed <commit>` (option par défaut)

- **Déplace HEAD** vers le commit spécifié
- **Retire les modifications du staging area** (elles ne sont plus "git add")
- **Conserve les fichiers modifiés** dans votre répertoire de travail

**Utilisation :** Quand vous voulez refaire le staging de vos fichiers.

#### 3. `git reset --hard <commit>`

- **Déplace HEAD** vers le commit spécifié
- **Supprime toutes les modifications** du staging area
- **Supprime toutes les modifications** de votre répertoire de travail

**⚠️ Attention :** C'est la plus dangereuse ! Elle supprime tout ce qui n'est pas commité.

**Utilisation :** Quand vous voulez vraiment revenir à un état précis et abandonner tout le reste.

**Visualisation :**

État initial :
```
Répertoire de travail : fichiers modifiés  
Staging area : fichiers "git add"  
Repository : commits A-B-C (HEAD)
```

Après `git reset --soft A` :
```
Répertoire de travail : fichiers modifiés ✓  
Staging area : fichiers "git add" ✓  
Repository : commits A (HEAD)
```

Après `git reset --mixed A` :
```
Répertoire de travail : fichiers modifiés ✓  
Staging area : vide ✗  
Repository : commits A (HEAD)
```

Après `git reset --hard A` :
```
Répertoire de travail : propre ✗  
Staging area : vide ✗  
Repository : commits A (HEAD)
```

### Limites du reflog

Le reflog a quelques limitations importantes :

1. **Il est local** : Le reflog n'est pas partagé entre machines. Si vous clonez un dépôt, vous n'aurez pas l'historique reflog.

2. **Il expire** : Par défaut, Git garde le reflog pendant :
   - **90 jours** pour les entrées accessibles depuis HEAD
   - **30 jours** pour les entrées non accessibles

3. **Commits non commitées** : Si vous n'avez jamais fait `git add` ou `git commit`, le reflog ne peut pas vous aider.

### Vérifier ce que vous allez récupérer

Avant de faire un reset, il est sage de vérifier ce que vous allez récupérer :

```bash
# Voir le contenu du commit
git show HEAD@{2}

# Voir les différences entre maintenant et cet état
git diff HEAD HEAD@{2}

# Voir les commits entre maintenant et cet état
git log HEAD@{2}..HEAD
```

### Technique de sécurité : La branche de sauvegarde

Avant toute manipulation risquée, créez une branche de sauvegarde :

```bash
# Créer une sauvegarde de l'état actuel
git branch sauvegarde-$(date +%Y%m%d-%H%M%S)

# Ou plus simplement
git branch sauvegarde-avant-reset

# Puis faire votre reset
git reset --hard HEAD@{3}

# Si ça tourne mal, revenez à la sauvegarde
git checkout sauvegarde-avant-reset
```

### Reflog pour d'autres références

Le reflog ne concerne pas que HEAD ! Vous pouvez voir l'historique d'une branche spécifique :

```bash
# Reflog de la branche main
git reflog show main

# Reflog de la branche feature
git reflog show feature
```

### Commandes utiles complémentaires

#### Voir le reflog avec un graphe
```bash
git log --graph --oneline --all --reflog
```

#### Chercher dans le reflog
```bash
# Chercher un message de commit spécifique
git reflog | grep "message recherché"
```

#### Nettoyer le reflog (à éviter généralement)
```bash
# Supprimer les entrées expirées
git reflog expire --expire=now --all

# Puis nettoyer le dépôt
git gc --prune=now
```

**⚠️ Attention :** Ces commandes suppriment définitivement les anciennes entrées. Ne les utilisez que si vous savez ce que vous faites !

### Cas pratiques résumés

| Problème | Solution rapide |
|----------|----------------|
| Reset trop agressif | `git reflog` puis `git reset --hard HEAD@{1}` |
| Branche supprimée | `git reflog` puis `git checkout -b nom-branche HEAD@{X}` |
| Rebase raté | `git reflog` puis `git reset --hard HEAD@{avant-rebase}` |
| Commit --amend regretté | `git reflog` puis `git reset --hard HEAD@{1}` |
| Merge annulé par erreur | `git reflog` puis `git reset --hard HEAD@{avant-annulation}` |

### Workflow de récupération en 5 étapes

1. **Restez calme** : Git a probablement encore vos données
2. **Consultez le reflog** : `git reflog`
3. **Identifiez le bon état** : Trouvez la référence HEAD@{X} correcte
4. **Vérifiez avant de récupérer** : `git show HEAD@{X}`
5. **Récupérez** : `git reset --hard HEAD@{X}` ou créez une branche

### Points clés à retenir

- Le **reflog est votre filet de sécurité** : Il enregistre tous vos mouvements
- **Consultez-le souvent** quand vous êtes perdu : `git reflog`
- **HEAD@{0} est le plus récent**, les nombres augmentent en remontant dans le temps
- **git reset --hard** est puissant mais dangereux : vérifiez toujours avant
- **Créez des branches de sauvegarde** avant les opérations risquées
- Le reflog est **local et temporaire** (30-90 jours)
- Si vous avez fait `git add` ou `git commit`, vos données sont probablement récupérables

### En résumé

Le reflog transforme Git en une véritable machine à remonter le temps. Même les erreurs les plus graves sont généralement réversibles. La clé est de :
1. Connaître l'existence du reflog
2. Savoir le consulter (`git reflog`)
3. Comprendre comment revenir en arrière (`git reset`)

Avec ces trois éléments, vous pouvez travailler avec Git en toute confiance, sachant que vos données sont bien protégées !

⏭️ [Gérer les gros fichiers et nettoyer le dépôt](/module-08-depannage/04-gerer-gros-fichiers.md)
