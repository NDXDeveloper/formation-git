🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 8 : Dépannage et résolution de problèmes

## 1. Résoudre "detached HEAD"

### Qu'est-ce qu'un "detached HEAD" ?

Avant de comprendre le problème, rappelons ce qu'est le HEAD en Git. Le **HEAD** est un pointeur qui indique où vous vous trouvez actuellement dans votre dépôt. En temps normal, HEAD pointe vers une branche (par exemple `main` ou `develop`), et cette branche pointe elle-même vers un commit spécifique.

Un **"detached HEAD"** (littéralement "tête détachée") se produit lorsque HEAD pointe directement vers un commit spécifique au lieu de pointer vers une branche. Dans cet état, vous n'êtes "attaché" à aucune branche.

### Visualisation du problème

**Situation normale :**
```
HEAD -> main -> commit C3
```

**Situation "detached HEAD" :**
```
HEAD -> commit C2
main -> commit C3
```

Dans ce second cas, vous êtes "dans le vide", sur un commit sans être sur une branche.

### Comment arrive-t-on en "detached HEAD" ?

Plusieurs actions peuvent vous mettre dans cet état :

1. **Checkout d'un commit spécifique :**
   ```bash
   git checkout 7a3f9c2
   ```

2. **Checkout d'un tag :**
   ```bash
   git checkout v1.0.0
   ```

3. **Checkout d'une référence distante :**
   ```bash
   git checkout origin/main
   ```

Lorsque vous effectuez l'une de ces commandes, Git vous avertit généralement :
```
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

### Pourquoi est-ce un problème ?

Le "detached HEAD" n'est pas dangereux en soi, mais il peut le devenir si vous ne comprenez pas ce qui se passe :

- **Si vous faites des commits** dans cet état, ils ne seront attachés à aucune branche
- **Si vous changez ensuite de branche**, ces commits risquent d'être perdus (ils deviendront orphelins)
- Git pourra les supprimer lors d'un nettoyage automatique (garbage collection)

### Scénario typique d'un débutant

Imaginons ce qui peut arriver :

```bash
# Vous voulez voir à quoi ressemblait le code il y a 3 commits
git checkout HEAD~3

# Vous faites des modifications et des commits
git add .
git commit -m "Correction importante"

# Vous revenez sur main
git checkout main

# ❌ Votre commit "Correction importante" est maintenant orphelin !
```

### Solutions : Comment résoudre un "detached HEAD"

#### Solution 1 : Vous n'avez fait aucun commit (cas simple)

Si vous n'avez fait aucune modification ou commit, il suffit de revenir à votre branche :

```bash
git checkout main
```

ou avec la commande plus récente :

```bash
git switch main
```

**C'est tout !** Vous êtes de retour sur votre branche et tout est normal.

#### Solution 2 : Vous avez fait des commits que vous voulez garder

Si vous avez fait des commits en "detached HEAD" et que vous voulez les conserver, vous devez créer une nouvelle branche pour les sauvegarder :

**Méthode A : Créer une branche depuis votre position actuelle**

```bash
# Vous êtes toujours en detached HEAD avec vos nouveaux commits
git branch ma-nouvelle-branche

# Puis basculer vers cette branche
git checkout ma-nouvelle-branche
```

Ou en une seule commande :

```bash
git checkout -b ma-nouvelle-branche
```

**Méthode B : Si vous avez déjà quitté le detached HEAD**

Si vous êtes déjà retourné sur `main` et réalisez que vous avez perdu des commits importants, pas de panique ! Utilisez `reflog` pour les retrouver :

```bash
# Afficher l'historique de tous vos déplacements
git reflog
```

Vous verrez quelque chose comme :
```
a1b2c3d HEAD@{0}: checkout: moving from a1b2c3d to main
a1b2c3d HEAD@{1}: commit: Correction importante
e4f5g6h HEAD@{2}: checkout: moving from main to HEAD~3
```

Trouvez le hash du commit que vous voulez récupérer (ici `a1b2c3d`) et créez une branche à partir de là :

```bash
git branch recuperation-commits a1b2c3d
git checkout recuperation-commits
```

#### Solution 3 : Vous voulez intégrer vos commits dans une branche existante

Si vous voulez ajouter vos commits à une branche existante (par exemple `main`) :

```bash
# Créer d'abord une branche temporaire pour sauvegarder votre travail
git branch branche-temporaire

# Revenir sur main
git checkout main

# Fusionner vos commits
git merge branche-temporaire

# Optionnel : supprimer la branche temporaire
git branch -d branche-temporaire
```

### Comment éviter le "detached HEAD" ?

1. **Créez toujours une branche avant de faire des modifications :**
   ```bash
   git checkout 7a3f9c2
   git checkout -b exploration-ancien-code
   # Maintenant vous êtes sur une branche !
   ```

2. **Utilisez des branches pour explorer l'historique :**
   Au lieu de faire `git checkout <commit>`, créez une branche :
   ```bash
   git checkout -b exploration-bug-ancien <commit>
   ```

3. **Soyez attentif aux avertissements de Git :**
   Quand Git vous dit "You are in detached HEAD state", c'est le moment de réfléchir à ce que vous voulez faire.

### Vérifier si vous êtes en "detached HEAD"

Pour savoir si vous êtes actuellement en "detached HEAD" :

```bash
git status
```

Si vous voyez :
```
HEAD detached at 7a3f9c2
```

Alors oui, vous êtes en "detached HEAD".

En situation normale, vous verriez :
```
On branch main
```

### Résumé visuel

```
Situation détectée : "detached HEAD"
         ↓
    Ai-je fait des commits ?
         ↓
    Non ────────→ git checkout main
         ↓           (Problème résolu !)
    Oui
         ↓
Je veux les garder ?
         ↓
    Oui ────────→ git checkout -b nouvelle-branche
         ↓           (Commits sauvegardés !)
    Non ────────→ git checkout main
                    (Commits abandonnés)
```

### Points clés à retenir

- Le "detached HEAD" n'est **pas une erreur**, c'est juste un état particulier de Git
- C'est utile pour **explorer l'historique** sans risque
- Le danger survient si vous **faites des commits** sans créer de branche
- La solution est simple : **créer une branche** avant ou après vos commits
- Le `reflog` est votre **filet de sécurité** pour récupérer des commits perdus

Maintenant que vous comprenez le "detached HEAD", vous savez comment naviguer dans l'historique Git en toute sécurité et comment récupérer votre travail si nécessaire !

⏭️ [Annuler un merge ou un rebase](/module-08-depannage/02-annuler-merge-ou-rebase.md)
