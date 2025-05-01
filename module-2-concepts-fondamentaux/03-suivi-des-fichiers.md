# 2.3. Suivi des fichiers : git status, git add, git commit

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

Dans cette section, nous allons approfondir les trois commandes essentielles que vous utiliserez quotidiennement pour suivre vos fichiers avec Git : `git status`, `git add` et `git commit`. Ces commandes constituent le cœur de votre workflow Git, et les maîtriser vous permettra de travailler efficacement.

## Le trio incontournable : status, add, commit

Ces trois commandes fonctionnent ensemble comme une équipe bien rodée :

- `git status` vous montre l'état actuel de votre projet
- `git add` prépare vos modifications pour le prochain commit
- `git commit` enregistre ces modifications dans l'historique

Voyons chacune de ces commandes en détail, avec des exemples pratiques et des options utiles.

## La commande git status : votre meilleure amie

`git status` est probablement la commande Git que vous utiliserez le plus souvent. Elle vous donne un aperçu complet de l'état de votre dépôt :

```bash
git status
```

### Ce que git status vous indique

Voici un exemple de sortie de `git status` et comment l'interpréter :

```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html
        new file:   images/logo.png

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   style.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        script.js
```

Cette sortie vous dit :

1. **Vous êtes sur la branche** `main` et elle est synchronisée avec la branche distante
2. **Changes to be committed** (Modifications prêtes à être committées) : les fichiers dans la staging area
   - `index.html` a été modifié
   - `images/logo.png` est un nouveau fichier
3. **Changes not staged for commit** (Modifications non préparées) : les fichiers modifiés mais pas encore dans la staging area
   - `style.css` a été modifié
4. **Untracked files** (Fichiers non suivis) : les fichiers que Git ne suit pas encore
   - `script.js` est un nouveau fichier non suivi

### Options utiles pour git status

#### Format court

Pour obtenir une sortie plus concise :

```bash
git status -s
```

ou

```bash
git status --short
```

Exemple de sortie :
```
M  index.html
A  images/logo.png
 M style.css
?? script.js
```

La signification des symboles :
- `M` = modifié
- `A` = ajouté
- `??` = non suivi
- La colonne de gauche = staging area, la colonne de droite = working directory

#### Ignorer les fichiers non suivis

Pour ne voir que les fichiers déjà suivis :

```bash
git status -uno
```

ou

```bash
git status --untracked-files=no
```

## La commande git add : préparer vos modifications

`git add` est utilisée pour ajouter des fichiers à la staging area (zone de préparation), les rendant prêts à être commités.

### Syntaxe de base

```bash
git add <fichier_ou_dossier>
```

### Exemples d'utilisation

#### Ajouter un fichier spécifique :

```bash
git add index.html
```

#### Ajouter plusieurs fichiers :

```bash
git add file1.txt file2.txt file3.txt
```

#### Ajouter tous les fichiers modifiés et nouveaux :

```bash
git add .
```

> **Attention** : `git add .` ajoute TOUS les fichiers modifiés et nouveaux dans le répertoire courant et ses sous-dossiers. Utilisez cette commande avec précaution.

#### Ajouter tous les fichiers d'un certain type :

```bash
git add *.html
```

Cette commande ajoute tous les fichiers `.html`.

### Options avancées de git add

#### Ajout interactif

Pour choisir précisément quelles parties de chaque fichier ajouter :

```bash
git add -p
```

ou

```bash
git add --patch
```

Cette option vous permet d'examiner chaque "hunk" (morceau) de modification et de décider s'il doit être ajouté ou non. C'est très utile lorsque vous avez fait plusieurs modifications indépendantes dans un même fichier mais que vous voulez les commiter séparément.

Lorsque vous utilisez le mode interactif, Git vous présentera des options comme :
- `y` : ajouter ce hunk
- `n` : ne pas ajouter ce hunk
- `s` : diviser le hunk en plus petits morceaux
- `q` : quitter

#### Mettre à jour les fichiers déjà suivis

Pour ajouter uniquement les modifications dans des fichiers déjà suivis par Git :

```bash
git add -u
```

ou

```bash
git add --update
```

## La commande git commit : enregistrer vos modifications

`git commit` enregistre les modifications préparées (dans la staging area) dans l'historique de votre dépôt.

### Syntaxe de base

```bash
git commit -m "Message de commit"
```

L'option `-m` vous permet de spécifier le message de commit directement dans la ligne de commande.

### L'importance des bons messages de commit

Un bon message de commit devrait :
- Être clair et concis
- Expliquer CE QUI a été modifié et POURQUOI
- Commencer par un verbe à l'impératif ("Ajoute", "Corrige", "Supprime", etc.)

Exemples de bons messages de commit :
- "Ajoute la fonction de recherche dans la barre de navigation"
- "Corrige le bug d'affichage sur les écrans mobiles"
- "Améliore les performances de chargement des images"

### Options utiles pour git commit

#### Commit avec un message détaillé

Pour écrire un message plus détaillé avec plusieurs lignes :

```bash
git commit
```

Sans l'option `-m`, Git ouvrira votre éditeur de texte configuré. La première ligne devrait être un résumé court (moins de 50 caractères), suivie d'une ligne vide, puis d'une description plus détaillée.

Exemple :
```
Ajoute la fonctionnalité de recherche avancée

- Implémente le filtrage par date
- Ajoute la recherche par tags
- Améliore l'interface utilisateur du formulaire de recherche
```

#### Commit en incluant toutes les modifications des fichiers suivis

Pour commiter directement toutes les modifications des fichiers déjà suivis, sans avoir à utiliser `git add` :

```bash
git commit -am "Message de commit"
```

L'option `-a` (ou `--all`) fait l'équivalent d'un `git add` pour tous les fichiers déjà suivis qui ont été modifiés. Notez que cela n'inclut pas les nouveaux fichiers non suivis.

#### Modifier le dernier commit

Si vous avez oublié quelque chose dans votre dernier commit ou fait une erreur dans le message :

```bash
git commit --amend
```

Cette commande remplace le dernier commit par un nouveau qui inclut à la fois les modifications du commit précédent et toutes les modifications actuellement dans la staging area.

> **Attention** : N'utilisez pas `--amend` si vous avez déjà partagé votre commit avec d'autres (par exemple, via `git push`), car cela peut créer des problèmes de synchronisation.

## Exercice pratique : Le workflow complet

Voyons comment ces trois commandes fonctionnent ensemble dans un workflow typique :

1. **Vérifiez l'état actuel** de votre dépôt :
   ```bash
   git status
   ```

2. **Créez ou modifiez des fichiers** dans votre projet. Par exemple, créez un fichier HTML simple :
   ```bash
   echo "<h1>Mon projet Git</h1>" > index.html
   ```

3. **Vérifiez les modifications** :
   ```bash
   git status
   ```
   Vous verrez `index.html` listé comme un fichier non suivi.

4. **Ajoutez le fichier** à la staging area :
   ```bash
   git add index.html
   ```

5. **Vérifiez à nouveau** :
   ```bash
   git status
   ```
   Maintenant `index.html` devrait apparaître sous "Changes to be committed".

6. **Faites le commit** :
   ```bash
   git commit -m "Ajoute la page d'accueil HTML"
   ```

7. **Vérifiez que tout est propre** :
   ```bash
   git status
   ```
   Vous devriez voir "nothing to commit, working tree clean".

8. **Ajoutez une autre modification** :
   ```bash
   echo "<p>Bienvenue sur mon site !</p>" >> index.html
   ```

9. **Vérifiez les modifications** :
   ```bash
   git status
   ```
   Vous verrez `index.html` listé comme modifié.

10. **Examinez les modifications** :
    ```bash
    git diff index.html
    ```
    Vous verrez la ligne que vous avez ajoutée.

11. **Ajoutez et committez en une seule étape** (comme le fichier est déjà suivi) :
    ```bash
    git commit -am "Ajoute un message de bienvenue"
    ```

## Bonnes pratiques pour le suivi des fichiers

### 1. Vérifiez souvent l'état de votre dépôt

Prenez l'habitude d'exécuter `git status` fréquemment pour toujours savoir où vous en êtes.

### 2. Faites des commits atomiques

Un commit devrait représenter une seule modification logique. Plutôt que de faire un grand commit avec de nombreuses modifications différentes, faites plusieurs petits commits, chacun avec un objectif clair.

### 3. Utilisez des messages de commit significatifs

Les messages de commit sont importants non seulement pour les autres, mais aussi pour votre futur vous. Un bon message de commit vous permet de comprendre rapidement ce qui a été modifié sans avoir à examiner le code.

### 4. Commitez souvent

N'attendez pas d'avoir terminé une grande fonctionnalité pour committer. Des commits fréquents vous donnent plus de points de sauvegarde auxquels vous pouvez revenir si quelque chose ne va pas.

### 5. N'oubliez pas .gitignore

Utilisez un fichier `.gitignore` pour exclure les fichiers que vous ne voulez jamais committer (fichiers de configuration locaux, dossiers de dépendances, fichiers compilés, etc.).

Exemple de contenu pour un fichier `.gitignore` basique :
```
# Fichiers spécifiques à l'environnement
.env
config.local.js

# Dossiers de dépendances
/node_modules/
/vendor/

# Fichiers compilés
*.class
*.o
*.pyc

# Dossiers de build
/build/
/dist/

# Fichiers système
.DS_Store
Thumbs.db
```

## Conclusion

Les commandes `git status`, `git add` et `git commit` sont les trois piliers du workflow quotidien avec Git. En maîtrisant ces commandes et leurs options, vous pouvez suivre efficacement les modifications de vos fichiers et construire un historique clair et organisé de votre projet.

Dans la prochaine section, nous verrons comment explorer cet historique à l'aide des commandes `git log`, `git show` et `git diff`.

⏭️ [Exploration du log : git log, git show, git diff](/module-2-concepts-fondamentaux/04-exploration-du-log.md)
