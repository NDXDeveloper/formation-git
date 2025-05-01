# 8.1. Scénarios courants (erreurs, conflits, historique modifié)

Dans cette section, nous allons aborder les situations problématiques les plus fréquentes que vous rencontrerez probablement durant votre utilisation de Git. Pour chaque scénario, nous verrons comment diagnostiquer le problème et les solutions pour y remédier.

## Erreurs courantes et leur résolution

### 1. "Oups, j'ai commité dans la mauvaise branche !"

**Situation :** Vous venez de faire un commit, mais vous réalisez que vous êtes sur `main` alors que vous auriez dû être sur une branche de fonctionnalité.

**Solution :**
1. Créez la branche qui aurait dû recevoir le commit :
   ```bash
   git branch feature-branch
   ```

2. Revenez à l'état précédent de la branche principale :
   ```bash
   git reset --hard HEAD~1
   ```

3. Basculez sur votre nouvelle branche :
   ```bash
   git checkout feature-branch
   ```

Votre commit est maintenant sur la bonne branche !

### 2. "J'ai commité un fichier que je ne voulais pas inclure"

**Situation :** Vous avez inclus un fichier dans votre commit (mot de passe, fichier de configuration, etc.) que vous n'auriez pas dû envoyer.

**Solution si le commit est local (pas encore poussé) :**
```bash
git reset --soft HEAD~1   # Annule le commit mais garde les modifications
```

Puis retirez le fichier du staging et recommittez :
```bash
git restore --staged fichier-a-retirer.txt
git commit -m "Message de commit corrigé"
```

**Conseil :** Utilisez `.gitignore` pour éviter que cela ne se reproduise !

### 3. "J'ai besoin d'annuler mes dernières modifications"

**Situation :** Vous avez fait des changements que vous souhaitez abandonner complètement.

**Pour les fichiers non encore ajoutés au staging :**
```bash
git restore nom-du-fichier   # Pour un fichier spécifique
# OU
git restore .                # Pour tous les fichiers
```

**Pour les fichiers déjà dans le staging :**
```bash
git restore --staged nom-du-fichier   # Retire du staging
```

**Pour annuler le dernier commit (en gardant les modifications) :**
```bash
git reset --soft HEAD~1
```

**Pour supprimer complètement le dernier commit et ses modifications :**
```bash
git reset --hard HEAD~1   # Attention : action irréversible !
```

## Gestion des conflits

### 1. "Git me dit qu'il y a des conflits !"

**Situation :** Lors d'un merge ou d'un pull, Git signale des conflits dans certains fichiers.

**Solution étape par étape :**

1. Identifiez les fichiers en conflit :
   ```bash
   git status
   ```

2. Ouvrez chaque fichier en conflit dans votre éditeur. Vous verrez des marqueurs comme :
   ```
   <<<<<<< HEAD
   votre version
   =======
   leur version
   >>>>>>> branche-à-merger
   ```

3. Modifiez le fichier pour conserver le code souhaité (supprimez les marqueurs !)

4. Une fois les conflits résolus, ajoutez les fichiers au staging :
   ```bash
   git add nom-du-fichier-résolu
   ```

5. Terminez le merge :
   ```bash
   git commit
   ```
   (Git préparera automatiquement un message de commit pour ce merge)

### 2. "Je veux abandonner un merge qui a des conflits"

**Situation :** Vous avez commencé un merge mais les conflits sont trop complexes à résoudre pour le moment.

**Solution :**
```bash
git merge --abort
```

Cela restaurera votre branche à l'état avant le merge.

## Modifier l'historique

### 1. "J'ai fait une faute dans mon message de commit"

**Situation :** Vous venez de commiter mais avez fait une erreur dans le message.

**Solution (pour le dernier commit) :**
```bash
git commit --amend -m "Nouveau message correct"
```

### 2. "J'ai oublié d'ajouter un fichier à mon dernier commit"

**Situation :** Vous avez fait un commit mais avez oublié d'inclure un fichier.

**Solution :**
```bash
git add fichier-oublié.txt
git commit --amend --no-edit   # --no-edit conserve le message existant
```

### 3. "J'ai poussé un commit que je souhaite modifier"

**Situation :** Vous avez déjà poussé un commit que vous souhaitez modifier.

**Solution :**
```bash
# Modifiez le commit localement
git commit --amend
# Puis forcez la mise à jour du dépôt distant
git push --force-with-lease origin votre-branche
```

⚠️ **ATTENTION** : Ne faites jamais cela sur des branches partagées comme `main` ! Cette opération réécrit l'historique et peut causer des problèmes pour les autres développeurs.

## Récupération après erreurs graves

### 1. "J'ai perdu des commits avec un reset --hard !"

**Situation :** Vous avez utilisé `git reset --hard` et perdu des commits importants.

**Solution :** Git conserve généralement les commits "perdus" pendant un certain temps.

1. Utilisez reflog pour voir l'historique de toutes les actions :
   ```bash
   git reflog
   ```

2. Trouvez le SHA du commit perdu et récupérez-le :
   ```bash
   git checkout SHA-du-commit-perdu
   git checkout -b branche-recuperation
   ```

### 2. "J'ai supprimé une branche par accident !"

**Situation :** Vous avez supprimé une branche contenant du travail important.

**Solution :**
1. Trouvez le dernier commit de cette branche dans le reflog :
   ```bash
   git reflog
   ```

2. Recréez la branche à partir de ce commit :
   ```bash
   git checkout -b branche-restaurée SHA-du-dernier-commit
   ```

## Conseils pour éviter les problèmes

1. **Effectuez des commits fréquents** avec des messages clairs
2. **Créez de nouvelles branches** pour chaque fonctionnalité ou correction
3. **Vérifiez toujours** avec `git status` avant de commiter
4. **Utilisez `git diff`** pour examiner les modifications avant de les ajouter
5. **Configurez correctement votre `.gitignore`** pour éviter d'ajouter des fichiers inutiles

## Exercice pratique

Pour vous entraîner à résoudre ces scénarios courants, suivez ces étapes :

1. Créez un dépôt de test
2. Commettez dans la mauvaise branche et corrigez l'erreur
3. Créez un conflit volontairement et résolvez-le
4. Essayez de récupérer un commit "perdu" avec reflog

En pratiquant ces situations dans un environnement contrôlé, vous serez mieux préparé pour les gérer quand elles surviendront réellement dans vos projets.

---

Dans la prochaine section, nous aborderons des ateliers guidés qui simulent un travail d'équipe sur un projet Git.
