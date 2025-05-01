# 2.1. Les 3 états de Git : Working Directory, Staging Area, Repository

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

L'un des concepts les plus importants pour comprendre Git est celui des trois états dans lesquels vos fichiers peuvent se trouver. Comprendre ces états vous aidera à mieux visualiser ce qui se passe lorsque vous utilisez les commandes Git.

## Les trois états en bref

Dans Git, vos fichiers passent toujours par trois zones principales :

1. **Working Directory** (Répertoire de travail)
2. **Staging Area** (Zone de préparation, aussi appelée "index")
3. **Repository** (Dépôt Git)

Imaginez ces trois états comme un processus de publication d'un livre :
- Le **Working Directory** est votre manuscrit en cours d'écriture
- La **Staging Area** est la version que vous envoyez à votre éditeur pour relecture
- Le **Repository** est le livre imprimé et publié officiellement

## Le Working Directory

Le **Working Directory** est simplement le dossier sur votre ordinateur où vous travaillez sur vos fichiers.

### Caractéristiques :
- C'est votre espace de travail normal où vous créez, modifiez et supprimez des fichiers
- Git surveille cet espace mais n'enregistre pas automatiquement les modifications
- Les fichiers peuvent être suivis (tracked) ou non suivis (untracked) par Git

### État des fichiers dans le Working Directory :
- **Fichiers non suivis (untracked)** : Nouveaux fichiers que Git ne connaît pas encore
- **Fichiers suivis (tracked)** : Fichiers que Git connaît déjà et qui peuvent être :
  - **Inchangés** : Identiques à la dernière version dans le repository
  - **Modifiés** : Différents de la dernière version dans le repository

### Exemple concret :
Imaginons que vous avez un projet avec trois fichiers :
- `index.html` (suivi par Git, non modifié)
- `style.css` (suivi par Git, modifié récemment)
- `script.js` (nouveau fichier, non suivi par Git)

Lorsque vous exécutez `git status`, Git vous indiquera que `style.css` a été modifié et que `script.js` n'est pas suivi.

## La Staging Area (zone de préparation)

La **Staging Area** est une zone intermédiaire où vous préparez les modifications que vous voulez enregistrer dans votre prochain commit.

### Caractéristiques :
- C'est un espace virtuel où vous sélectionnez quels changements inclure dans le prochain commit
- Elle vous permet de ne committer que certaines modifications, pas forcément toutes
- Techniquement, c'est un fichier, généralement stocké dans votre dossier `.git`, qui garde la trace de ce qui ira dans votre prochain commit

### Pourquoi une zone de préparation ?
Elle vous permet de :
- Créer des commits cohérents et logiques
- Séparer les modifications différentes en commits distincts
- Réviser ce que vous allez committer avant de le faire définitivement

### Comment utiliser la Staging Area :
- `git add <fichier>` : Ajoute les modifications d'un fichier à la zone de préparation
- `git add .` : Ajoute toutes les modifications à la zone de préparation

### Exemple concret :
Reprenons notre exemple précédent. Si vous exécutez :
```bash
git add style.css
```
Vous ajoutez les modifications de `style.css` à la zone de préparation. Si vous faites ensuite `git status`, vous verrez que `style.css` est prêt à être commité, tandis que `script.js` reste non suivi.

## Le Repository (dépôt)

Le **Repository** (ou dépôt) est là où Git stocke de façon permanente les données comme une série de "snapshots" (instantanés) de votre projet.

### Caractéristiques :
- C'est la base de données Git qui contient tout l'historique de votre projet
- Physiquement, il se trouve dans le dossier `.git` à la racine de votre projet
- Chaque commit crée un nouvel instantané permanent de votre projet
- Une fois les modifications committées, elles sont "en sécurité" dans le repository

### Comment enregistrer dans le Repository :
- `git commit -m "Message"` : Crée un nouveau commit avec les changements présents dans la Staging Area

### Exemple concret :
Après avoir exécuté `git add style.css`, vous pouvez créer un commit :
```bash
git commit -m "Mise à jour du style du header"
```
Après cette commande, les modifications de `style.css` sont désormais enregistrées de façon permanente dans l'historique de votre projet.

## Le cycle de vie complet d'un fichier

Voici comment un fichier passe typiquement par les trois états :

1. Vous créez ou modifiez un fichier dans votre **Working Directory**
2. Vous ajoutez ce fichier à la **Staging Area** avec `git add`
3. Vous enregistrez les modifications dans le **Repository** avec `git commit`

Voici une représentation visuelle du cycle :

```
                   git add               git commit
Working Directory ----------> Staging Area ----------> Repository
      ^                                                   |
      |                                                   |
      +--------------------- git checkout ----------------+
```

## Visualiser les trois états avec `git status`

La commande `git status` est votre meilleure amie pour comprendre dans quel état se trouvent vos fichiers :

```bash
git status
```

La sortie vous indiquera clairement :
- Quels fichiers sont modifiés mais pas encore dans la Staging Area
- Quels fichiers sont dans la Staging Area, prêts à être commités
- Quels fichiers ne sont pas suivis par Git

## Exemple pratique complet

Voyons un exemple pratique qui illustre le passage par les trois états :

1. **Créez un nouveau fichier** dans votre Working Directory :
   ```bash
   echo "# Mon projet" > README.md
   ```

2. **Vérifiez le statut** :
   ```bash
   git status
   ```
   Vous verrez que `README.md` est listé comme "untracked" (non suivi).

3. **Ajoutez le fichier** à la Staging Area :
   ```bash
   git add README.md
   ```

4. **Vérifiez à nouveau le statut** :
   ```bash
   git status
   ```
   Vous verrez maintenant que `README.md` est sous "Changes to be committed" (prêt à être commité).

5. **Modifiez à nouveau le fichier** :
   ```bash
   echo "Une description de mon projet" >> README.md
   ```

6. **Vérifiez le statut** :
   ```bash
   git status
   ```
   Vous verrez que `README.md` apparaît maintenant à deux endroits :
   - Une version est dans la Staging Area (prête à être committée)
   - Une version modifiée est dans le Working Directory

7. **Ajoutez les nouvelles modifications** à la Staging Area :
   ```bash
   git add README.md
   ```

8. **Créez un commit** pour enregistrer dans le Repository :
   ```bash
   git commit -m "Ajout du fichier README"
   ```

9. **Vérifiez une dernière fois le statut** :
   ```bash
   git status
   ```
   Vous devriez voir "nothing to commit, working tree clean", indiquant que tout est synchronisé entre vos trois états.

## Astuces et bonnes pratiques

### 1. Commits atomiques
Essayez de faire des commits qui représentent une seule modification logique. La Staging Area vous aide à préparer de tels commits :
- Ajoutez uniquement les fichiers liés à une même modification
- Faites plusieurs petits commits plutôt qu'un gros fourre-tout

### 2. Vérifiez toujours avant de committer
Utilisez ces commandes avant de faire un commit :
- `git status` pour voir quels fichiers seront commités
- `git diff --staged` pour voir exactement quelles modifications seront committées

### 3. Retirer des fichiers de la Staging Area
Si vous avez ajouté un fichier par erreur à la Staging Area :
```bash
git restore --staged <fichier>
```
(Dans les anciennes versions de Git, utilisez `git reset HEAD <fichier>`)

## Conclusion

Comprendre les trois états de Git est fondamental pour utiliser cet outil efficacement :

- Le **Working Directory** est où vous travaillez sur vos fichiers
- La **Staging Area** est où vous préparez votre prochain commit
- Le **Repository** est où l'historique de votre projet est stocké de façon permanente

Cette architecture en trois étapes est l'un des aspects les plus puissants de Git, car elle vous donne un contrôle précis sur ce que vous enregistrez et quand vous l'enregistrez.

Dans la prochaine section, nous explorerons plus en détail le dossier `.git` et l'architecture interne qui permet à Git de fonctionner comme il le fait.

⏭️ [Les fichiers .git et l'architecture interne](/module-2-concepts-fondamentaux/02-les-fichiers-git-et-architecture-interne.md)
