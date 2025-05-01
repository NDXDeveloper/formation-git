# 2.4. Exploration du log : git log, git show, git diff

L'un des grands avantages de Git est qu'il conserve l'historique complet de votre projet. Pour tirer pleinement parti de cette fonctionnalité, vous devez savoir comment explorer cet historique. Dans cette section, nous allons découvrir trois commandes essentielles qui vous permettront de voyager dans le temps à travers votre projet : `git log`, `git show` et `git diff`.

## Pourquoi explorer l'historique ?

Explorer l'historique de votre projet vous permet de :
- Comprendre comment le projet a évolué
- Identifier quand et par qui un changement spécifique a été introduit
- Retrouver une ancienne version d'un fichier
- Comprendre la logique derrière certaines décisions de développement
- Résoudre des bugs en identifiant quand ils sont apparus

## La commande git log : visualiser l'historique des commits

La commande `git log` vous montre l'historique des commits dans votre dépôt, du plus récent au plus ancien.

### Syntaxe de base

```bash
git log
```

### Exemple de sortie standard

```
commit a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9
Author: Votre Nom <votre.email@exemple.com>
Date:   Mer Avr 5 14:30:15 2023 +0200

    Ajoute la fonction de recherche avancée

commit b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
Author: Votre Nom <votre.email@exemple.com>
Date:   Mar Avr 4 10:22:33 2023 +0200

    Corrige le bug d'affichage sur mobile

commit c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0u1
Author: Votre Nom <votre.email@exemple.com>
Date:   Lun Avr 3 16:45:01 2023 +0200

    Premier commit : structure initiale du projet
```

Pour chaque commit, vous voyez :
- L'identifiant unique (SHA-1)
- L'auteur et son email
- La date et l'heure
- Le message de commit

> **Astuce** : Si la liste est longue, vous pouvez naviguer avec les touches fléchées et quitter avec la touche `q`.

### Options utiles pour git log

#### Format condensé et visuel

Pour obtenir une visualisation plus compacte et graphique :

```bash
git log --oneline --graph --decorate
```

Cette commande affiche :
- `--oneline` : Une ligne par commit (identifiant court + message)
- `--graph` : Un graphique ASCII montrant les branches et fusions
- `--decorate` : Affiche les branches et tags qui pointent vers chaque commit

Exemple de sortie :
```
* a1b2c3d (HEAD -> main) Ajoute la fonction de recherche avancée
* b2c3d4e Corrige le bug d'affichage sur mobile
* c3d4e5f Premier commit : structure initiale du projet
```

#### Limiter le nombre de commits affichés

Pour voir seulement les n derniers commits :

```bash
git log -n 5
```

Ou plus simplement :

```bash
git log -5
```

#### Filtrer par auteur

Pour voir les commits d'un auteur spécifique :

```bash
git log --author="Nom"
```

#### Filtrer par date

Pour voir les commits après une certaine date :

```bash
git log --after="2023-04-01"
```

Ou avant une certaine date :

```bash
git log --before="2023-04-30"
```

#### Filtrer par contenu du message

Pour rechercher des commits dont le message contient un certain texte :

```bash
git log --grep="bug"
```

#### Voir les modifications introduites par chaque commit

Pour voir les différences introduites par chaque commit :

```bash
git log -p
```

Ou pour une version plus concise :

```bash
git log --stat
```

#### Format personnalisé

Pour un format personnalisé et coloré :

```bash
git log --pretty=format:"%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)"
```

Cela donne un format compact avec :
- L'identifiant court en bleu
- La date relative en vert
- Le message en blanc
- L'auteur en blanc atténué

## La commande git show : examiner un commit spécifique

Alors que `git log` vous montre une liste de commits, `git show` vous permet d'examiner en détail un commit spécifique.

### Syntaxe de base

```bash
git show <commit-id>
```

Où `<commit-id>` est l'identifiant SHA-1 du commit que vous souhaitez examiner. Vous pouvez utiliser l'identifiant complet ou les premiers caractères (généralement 7 ou 8 sont suffisants).

### Exemple d'utilisation

```bash
git show a1b2c3d
```

Cette commande affichera :
- Les métadonnées du commit (auteur, date, message)
- Les modifications exactes introduites par ce commit (comme `git diff`)

### Options utiles pour git show

#### Voir seulement les statistiques

Pour voir un résumé des fichiers modifiés sans le détail des modifications :

```bash
git show --stat a1b2c3d
```

#### Voir un fichier spécifique dans un commit

Pour voir l'état d'un fichier particulier dans un commit spécifique :

```bash
git show a1b2c3d:index.html
```

#### Utiliser des références spéciales

Git offre des références spéciales pour désigner facilement certains commits :

- `HEAD` : Le commit actuel sur lequel vous vous trouvez
- `HEAD~1` ou `HEAD^` : Le commit précédent (parent de HEAD)
- `HEAD~2` : Deux commits en arrière
- `main` : Le dernier commit de la branche main

Exemples :
```bash
git show HEAD           # Commit actuel
git show HEAD~1         # Commit précédent
git show main           # Dernier commit de la branche main
```

## La commande git diff : comparer des modifications

`git diff` vous permet de voir les différences entre différents états de votre projet.

### Syntaxe de base

```bash
git diff
```

Sans arguments, cette commande montre les modifications non stagées (différences entre votre working directory et la staging area).

### Différents usages de git diff

#### Voir les modifications stagées

Pour voir les modifications qui sont dans la staging area (ce qui sera inclus dans le prochain commit) :

```bash
git diff --staged
```

ou

```bash
git diff --cached
```

#### Comparer deux commits

Pour voir les différences entre deux commits :

```bash
git diff commit1 commit2
```

Exemple :
```bash
git diff HEAD~1 HEAD    # Différences entre le commit précédent et le commit actuel
```

#### Comparer des branches

Pour voir les différences entre deux branches :

```bash
git diff main feature   # Différences entre la branche main et la branche feature
```

#### Comparer des fichiers spécifiques

Pour voir les modifications d'un fichier particulier :

```bash
git diff index.html
```

Ou entre deux commits :

```bash
git diff HEAD~1 HEAD index.html   # Modifications de index.html entre le commit précédent et le commit actuel
```

### Comprendre la sortie de git diff

La sortie de `git diff` peut sembler intimidante au premier abord, mais elle est structurée de façon logique :

```
diff --git a/index.html b/index.html
index 1234567..abcdefg 100644
--- a/index.html
+++ b/index.html
@@ -10,6 +10,8 @@
  <h1>Mon projet Git</h1>
  <p>Bienvenue sur mon site !</p>

+<p>Nouvelle ligne ajoutée</p>
+
  <footer>
    <p>Contact : example@example.com</p>
  </footer>
```

Explication :
- La première ligne indique les fichiers comparés
- Les lignes avec `-` (en rouge généralement) sont les lignes supprimées
- Les lignes avec `+` (en vert généralement) sont les lignes ajoutées
- Les sections commençant par `@@` indiquent la position des modifications dans le fichier

## Exercice pratique : Explorer l'historique d'un projet

Voici un exercice pour mettre en pratique ces commandes :

1. **Créez quelques commits** dans votre projet (si ce n'est pas déjà fait) :
   ```bash
   echo "<h2>Sous-titre</h2>" >> index.html
   git add index.html
   git commit -m "Ajoute un sous-titre"

   echo "<p>Un paragraphe</p>" >> index.html
   git add index.html
   git commit -m "Ajoute un paragraphe"
   ```

2. **Explorez l'historique** avec différentes options :
   ```bash
   git log
   git log --oneline
   git log --oneline --graph
   ```

3. **Examinez un commit spécifique** :
   ```bash
   # Remplacez commit-id par l'ID d'un de vos commits
   git show <commit-id>
   ```

4. **Comparez des versions** :
   ```bash
   git diff HEAD~1 HEAD    # Différences introduites par le dernier commit
   ```

## Astuces avancées pour explorer l'historique

### Créer un alias pour un format de log personnalisé

Si vous trouvez une option de `git log` particulièrement utile, vous pouvez créer un alias pour l'utiliser facilement :

```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

Vous pourrez ensuite utiliser :

```bash
git lg
```

### Rechercher du contenu dans l'historique

Pour rechercher quand une certaine ligne de code a été ajoutée ou supprimée :

```bash
git log -S"fonction rechercher()" --patch
```

Cette commande trouve les commits qui ont ajouté ou supprimé des occurrences de "fonction rechercher()".

### Visualiser l'évolution d'un fichier spécifique

Pour voir l'historique d'un fichier particulier :

```bash
git log --follow -p -- index.html
```

L'option `--follow` permet de suivre les renommages du fichier.

## Conclusion

Les commandes `git log`, `git show` et `git diff` sont vos outils d'investigation dans Git. Elles vous permettent d'explorer l'historique de votre projet, de comprendre comment il a évolué, et d'identifier quand et pourquoi des modifications spécifiques ont été introduites.

En maîtrisant ces commandes, vous pouvez :
- Retrouver quand un bug a été introduit
- Comprendre les décisions de développement passées
- Récupérer des portions de code d'anciennes versions
- Voir ce qui a changé entre deux points dans le temps

Dans le prochain module, nous explorerons le concept de branches dans Git, qui vous permettra de travailler sur différentes fonctionnalités en parallèle.
