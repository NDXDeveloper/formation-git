🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 10 : Cas pratiques et projets
## 1. Scénarios courants et résolutions

Dans ce chapitre, nous allons explorer les situations les plus fréquentes que vous rencontrerez en utilisant Git au quotidien. Pour chaque scénario, nous verrons le problème, pourquoi il arrive, et comment le résoudre de manière simple et efficace.

---

### Scénario 1 : J'ai oublié d'ajouter un fichier dans mon dernier commit

**Situation :** Vous venez de faire un commit, mais vous réalisez qu'il manque un fichier important ou une petite modification.

**Pourquoi ça arrive :** C'est très courant ! On se concentre sur la fonctionnalité principale et on oublie un fichier de configuration, un import manquant, ou une petite correction.

**Solution :**

```bash
# Ajoutez le(s) fichier(s) oublié(s)
git add fichier-oublie.txt

# Modifiez le dernier commit pour y inclure ce fichier
git commit --amend --no-edit
```

L'option `--no-edit` garde le même message de commit. Si vous voulez aussi modifier le message :

```bash
git commit --amend
```

**⚠️ Attention :** N'utilisez `--amend` que si vous n'avez **pas encore poussé** votre commit sur le dépôt distant. Sinon, cela créera des problèmes pour vos collaborateurs.

---

### Scénario 2 : J'ai commité sur la mauvaise branche

**Situation :** Vous travailliez sur une nouvelle fonctionnalité, mais vous avez oublié de créer une branche et vous avez commité directement sur `main` ou `dev`.

**Solution (si vous n'avez pas encore pushé) :**

```bash
# 1. Créez la branche qui aurait dû recevoir vos commits
git branch ma-feature

# 2. Revenez en arrière sur la branche actuelle (main)
git reset --hard HEAD~1
# (remplacez ~1 par ~2 si vous avez fait 2 commits, etc.)

# 3. Allez sur votre nouvelle branche
git switch ma-feature
```

**Explication :** Votre commit existe toujours (sur `ma-feature`), mais vous avez "reculé" la branche `main` pour qu'elle ne pointe plus vers ce commit.

**Alternative (méthode plus sûre) :**

```bash
# 1. Notez l'identifiant de votre commit (les 7 premiers caractères)
git log --oneline

# 2. Revenez en arrière sur main
git reset --hard HEAD~1

# 3. Créez une nouvelle branche et appliquez le commit
git checkout -b ma-feature  
git cherry-pick [identifiant-du-commit]
```

---

### Scénario 3 : Je veux annuler complètement mes modifications locales

**Situation :** Vous avez modifié plusieurs fichiers, mais finalement vous voulez tout annuler et revenir à l'état du dernier commit.

**Solution :**

```bash
# Annuler les modifications d'un fichier spécifique
git restore nom-du-fichier.txt

# Annuler TOUTES les modifications (fichiers modifiés)
git restore .

# Si vous avez aussi des fichiers non suivis à supprimer
git clean -fd
```

**Options de `git clean` :**
- `-f` : force la suppression
- `-d` : supprime aussi les dossiers non suivis
- `-n` : mode simulation (pour voir ce qui serait supprimé)

**⚠️ Attention :** Ces commandes sont **irréversibles**. Vos modifications seront définitivement perdues.

---

### Scénario 4 : J'ai des conflits lors d'un merge

**Situation :** Vous essayez de fusionner une branche, mais Git vous signale des conflits.

```
Auto-merging fichier.txt  
CONFLICT (content): Merge conflict in fichier.txt  
Automatic merge failed; fix conflicts and then commit the result.
```

**Solution étape par étape :**

1. **Identifiez les fichiers en conflit :**
   ```bash
   git status
   ```

2. **Ouvrez le fichier en conflit** dans votre éditeur. Vous verrez :
   ```
   <<<<<<< HEAD
   Votre code actuel
   =======
   Le code de la branche à fusionner
   >>>>>>> nom-de-la-branche
   ```

3. **Résolvez le conflit manuellement :**
   - Supprimez les marqueurs `<<<<<<<`, `=======`, `>>>>>>>`
   - Gardez le code correct (le vôtre, le leur, ou une combinaison)
   - Sauvegardez le fichier

4. **Indiquez que le conflit est résolu :**
   ```bash
   git add fichier-resolu.txt
   ```

5. **Finalisez le merge :**
   ```bash
   git commit
   ```

**Astuce :** Vous pouvez annuler le merge en cours à tout moment avec :
```bash
git merge --abort
```

---

### Scénario 5 : Mon push est rejeté (rejected)

**Situation :** Vous essayez de pousser vos commits, mais Git refuse :

```
! [rejected]        main -> main (fetch first)
error: failed to push some refs
```

**Pourquoi ça arrive :** Quelqu'un d'autre a poussé des commits sur la même branche avant vous. Votre version locale est "en retard".

**Solution :**

```bash
# 1. Récupérez les changements distants
git pull

# 2. Si pas de conflits, poussez vos changements
git push

# Si conflits, résolvez-les (voir Scénario 4), puis :
git push
```

**Alternative avec rebase (historique plus propre) :**

```bash
git pull --rebase  
git push
```

Le rebase place vos commits locaux *après* les commits distants, évitant ainsi un commit de merge inutile.

---

### Scénario 6 : J'ai supprimé un fichier par erreur

**Situation :** Vous avez supprimé un fichier important et vous l'avez même commité.

**Solution (si pas encore pushé) :**

```bash
# Annulez le dernier commit mais gardez les modifications
git reset HEAD~1

# Restaurez le fichier supprimé
git restore fichier-supprime.txt
```

**Solution (si déjà pushé ou commit plus ancien) :**

```bash
# 1. Trouvez le commit où le fichier existait encore
git log --all -- chemin/vers/fichier.txt

# 2. Restaurez le fichier depuis ce commit
git checkout [identifiant-commit] -- chemin/vers/fichier.txt

# 3. Committez la restauration
git add chemin/vers/fichier.txt  
git commit -m "Restauration du fichier supprimé"
```

---

### Scénario 7 : Je suis en "detached HEAD"

**Situation :** Vous voyez ce message en faisant `git status` :

```
HEAD detached at abc1234
```

**Pourquoi ça arrive :** Vous avez fait `git checkout` sur un commit spécifique (ou un tag) au lieu d'une branche.

**Solution (si vous n'avez rien modifié) :**

```bash
# Retournez simplement sur une branche
git switch main
```

**Solution (si vous avez fait des commits en detached HEAD) :**

```bash
# 1. Créez une branche pour sauvegarder votre travail
git branch sauvetage-detached

# 2. Allez sur cette branche
git switch sauvetage-detached

# 3. Vous pouvez maintenant merger cette branche ailleurs
git switch main  
git merge sauvetage-detached
```

---

### Scénario 8 : Mon message de commit contient une faute

**Situation :** Vous venez de commiter avec un message mal orthographié ou incomplet.

**Solution (dernier commit, pas encore pushé) :**

```bash
git commit --amend -m "Nouveau message corrigé"
```

**Solution (commit déjà pushé) :**

Il n'est pas recommandé de modifier l'historique pushé. Mais si c'est vraiment nécessaire et que vous êtes seul sur la branche :

```bash
git commit --amend -m "Nouveau message corrigé"  
git push --force-with-lease
```

**⚠️ Danger :** `--force-with-lease` réécrit l'historique distant. À n'utiliser que si vous êtes certain que personne n'a récupéré ce commit.

---

### Scénario 9 : J'ai des fichiers non suivis qui encombrent

**Situation :** Votre `git status` affiche plein de fichiers "Untracked files" que vous ne voulez pas commiter.

**Solution temporaire :**

```bash
# Supprimer les fichiers non suivis
git clean -fd

# Voir d'abord ce qui serait supprimé (simulation)
git clean -fdn
```

**Solution permanente (recommandée) :**

Ajoutez ces fichiers à `.gitignore` :

```bash
# Créez ou éditez .gitignore
echo "*.log" >> .gitignore  
echo "node_modules/" >> .gitignore  
echo ".env" >> .gitignore

# Committez le .gitignore
git add .gitignore  
git commit -m "Ajout de règles .gitignore"
```

---

### Scénario 10 : Je veux annuler un commit déjà pushé

**Situation :** Vous avez pushé un commit qui contient une erreur et d'autres personnes ont peut-être déjà récupéré votre code.

**Solution (la bonne méthode) :**

```bash
# Créez un nouveau commit qui annule les changements
git revert [identifiant-du-commit]

# Poussez ce nouveau commit
git push
```

**Pourquoi revert et pas reset ?**
- `git revert` crée un **nouveau commit** qui défait les changements → historique préservé
- `git reset` **réécrit l'historique** → problématique pour les collaborateurs

**Si vous devez annuler plusieurs commits :**

```bash
# Annuler les 3 derniers commits
git revert HEAD~2..HEAD

# Ou un par un
git revert [commit3]  
git revert [commit2]  
git revert [commit1]
```

---

### Scénario 11 : Je veux récupérer un commit que j'ai supprimé

**Situation :** Vous avez fait un `git reset --hard` et vous regrettez d'avoir perdu des commits.

**Solution :**

```bash
# 1. Affichez l'historique complet des actions Git
git reflog

# Vous verrez quelque chose comme :
# abc1234 HEAD@{0}: reset: moving to HEAD~1
# def5678 HEAD@{1}: commit: Mon commit perdu

# 2. Récupérez le commit perdu
git checkout def5678

# 3. Créez une branche pour le sauvegarder
git branch recuperation  
git switch recuperation
```

**Conseil :** Le reflog garde l'historique pendant environ 30 jours. C'est votre filet de sécurité !

---

### Scénario 12 : J'ai trop de commits locaux brouillons

**Situation :** Vous avez fait 10 petits commits locaux ("fix", "encore un fix", "ça marche", etc.) et vous voulez les regrouper avant de pusher.

**Solution (rebase interactif) :**

```bash
# Regroupez les 5 derniers commits
git rebase -i HEAD~5
```

Un éditeur s'ouvre avec :

```
pick abc1234 Premier commit  
pick def5678 fix  
pick ghi9012 encore un fix  
pick jkl3456 ça marche  
pick mno7890 Commit final
```

Modifiez en :

```
pick abc1234 Premier commit  
squash def5678 fix  
squash ghi9012 encore un fix  
squash jkl3456 ça marche  
squash mno7890 Commit final
```

Sauvegardez, et tous les commits seront fusionnés en un seul.

**⚠️ Règle d'or :** Ne rebasez jamais des commits déjà pushés et partagés avec d'autres !

---

## Résumé des commandes par situation

| Situation | Commande principale |
|-----------|---------------------|
| Oublié un fichier dans le commit | `git commit --amend` |
| Commit sur mauvaise branche | `git reset --hard HEAD~1` puis `git switch` |
| Annuler modifications locales | `git restore .` |
| Résoudre un conflit | Éditer → `git add` → `git commit` |
| Push rejeté | `git pull` puis `git push` |
| Fichier supprimé par erreur | `git checkout [commit] -- fichier` |
| Detached HEAD | `git switch [branche]` |
| Corriger message de commit | `git commit --amend -m "..."` |
| Fichiers non suivis | `git clean -fd` |
| Annuler commit pushé | `git revert [commit]` |
| Récupérer commit supprimé | `git reflog` puis `git checkout` |
| Regrouper plusieurs commits | `git rebase -i HEAD~n` |

---

## Conseils généraux pour éviter les problèmes

1. **Committez souvent** : Des petits commits fréquents sont plus faciles à gérer qu'un gros commit complexe.

2. **Vérifiez votre branche** : Avant de commencer à travailler, vérifiez toujours sur quelle branche vous êtes avec `git branch` ou `git status`.

3. **Utilisez git status** : Prenez l'habitude de faire `git status` régulièrement pour voir l'état de votre dépôt.

4. **Pullez avant de pusher** : Faites `git pull` avant de commencer à travailler chaque jour pour récupérer les dernières modifications.

5. **Ne paniquez pas** : Avec Git, il est très rare de perdre définitivement du code. Le reflog est votre ami !

6. **Testez en local** : Avant de pusher, assurez-vous que votre code fonctionne.

7. **Branches de fonctionnalités** : Travaillez toujours sur des branches séparées pour les nouvelles fonctionnalités, jamais directement sur `main`.

8. **Sauvegardez avec git stash** : Si vous devez changer de branche rapidement, utilisez `git stash` pour mettre de côté votre travail en cours.

---

## En cas de doute

Quand vous ne savez pas quoi faire :

1. **Ne forcez rien** : Les commandes avec `--force` sont dangereuses
2. **Faites une copie** : `git branch backup` crée une sauvegarde de votre branche actuelle
3. **Consultez le reflog** : `git reflog` montre tout ce que vous avez fait
4. **Demandez de l'aide** : Il vaut mieux demander que de casser l'historique

**La règle d'or :** Si vous avez pushé quelque chose, ne réécrivez pas l'historique (pas de `reset --hard`, pas de `rebase`, pas de `--force`). Utilisez plutôt `git revert` pour créer de nouveaux commits qui annulent les changements.

---

Ces scénarios couvrent 90% des situations courantes que vous rencontrerez. Avec de la pratique, ces résolutions deviendront des réflexes naturels !

⏭️ [Atelier : Contribuer à un projet open source](/module-10-cas-pratiques/02-atelier-contribuer-open-source.md)
