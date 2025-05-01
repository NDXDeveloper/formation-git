# 1.5. Configuration initiale (git config)

Après avoir installé Git sur votre ordinateur, vous devez effectuer quelques configurations initiales avant de l'utiliser. Cette étape est importante car Git a besoin de savoir qui vous êtes pour enregistrer correctement l'auteur de chaque modification.

Rassurez-vous, la configuration de base est simple et ne prend que quelques minutes !

## Pourquoi configurer Git ?

Lorsque vous utilisez Git pour enregistrer des modifications (faire des "commits"), deux informations essentielles sont associées à chaque modification :
- Votre nom
- Votre adresse email

Ces informations sont importantes pour :
- Identifier qui a fait chaque modification
- Vous permettre de collaborer efficacement avec d'autres personnes
- Associer correctement votre travail à votre compte sur des plateformes comme GitHub ou GitLab

## Configuration de base : votre identité

Ouvrez votre terminal (ou invite de commande / Git Bash sur Windows) et exécutez les deux commandes suivantes :

```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@exemple.com"
```

Remplacez "Votre Nom" par votre vrai nom (ou le nom que vous souhaitez utiliser) et "votre.email@exemple.com" par votre adresse email.

> **Conseil pour les débutants :** Utilisez la même adresse email que celle de votre compte GitHub, GitLab ou Bitbucket si vous prévoyez d'utiliser ces services.

## Vérifier votre configuration

Pour vous assurer que vos informations ont été correctement enregistrées, vous pouvez utiliser cette commande :

```bash
git config --list
```

Vous devriez voir vos paramètres affichés, y compris votre nom et votre email.

Vous pouvez aussi vérifier une valeur spécifique :

```bash
git config user.name
git config user.email
```

## Configuration de l'éditeur de texte

Lorsque Git a besoin que vous saisissiez un message (par exemple pour un commit), il ouvre un éditeur de texte. Par défaut, Git utilise généralement l'éditeur configuré dans votre système, qui peut être Vi ou Vim (parfois déroutant pour les débutants).

Vous pouvez configurer Git pour qu'il utilise un éditeur plus familier :

### Pour utiliser Notepad (Windows) :

```bash
git config --global core.editor notepad
```

### Pour utiliser VS Code :

```bash
git config --global core.editor "code --wait"
```

### Pour utiliser Notepad++ (Windows) :

```bash
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```
(Ajustez le chemin si Notepad++ est installé à un autre endroit)

### Pour utiliser Sublime Text :

```bash
git config --global core.editor "'subl' -w"
```

Le paramètre `--wait` ou `-w` est important car il indique à Git d'attendre que vous fermiez l'éditeur avant de continuer.

## Configuration des sauts de ligne

Les différents systèmes d'exploitation traitent les fins de ligne différemment :
- Windows utilise CRLF (Carriage Return + Line Feed)
- macOS et Linux utilisent LF (Line Feed)

Pour éviter des problèmes quand vous travaillez avec des personnes utilisant différents systèmes, configurez Git ainsi :

### Sur Windows :

```bash
git config --global core.autocrlf true
```

### Sur macOS ou Linux :

```bash
git config --global core.autocrlf input
```

## Configuration des couleurs

Git peut utiliser des couleurs dans sa sortie de terminal pour faciliter la lecture. C'est généralement activé par défaut, mais vous pouvez vous en assurer avec :

```bash
git config --global color.ui auto
```

## Configuration des alias (raccourcis)

Vous pouvez créer des raccourcis pour les commandes que vous utilisez fréquemment. Par exemple, au lieu de taper `git status`, vous pourriez simplement taper `git st`.

Voici quelques alias utiles que vous pourriez configurer :

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --decorate"
```

Une fois configurés, vous pouvez utiliser ces raccourcis comme ceci :
- `git st` au lieu de `git status`
- `git co` au lieu de `git checkout`
- etc.

> **Note pour les débutants :** Ne vous inquiétez pas de configurer des alias dès le début. Commencez par vous familiariser avec les commandes standard, et créez des alias plus tard quand vous saurez quelles commandes vous utilisez le plus souvent.

## Configuration de la branche par défaut

Depuis 2020, la convention pour nommer la branche principale a évolué de "master" vers "main". Pour configurer "main" comme nom de branche par défaut pour les nouveaux dépôts :

```bash
git config --global init.defaultBranch main
```

## Les trois niveaux de configuration de Git

Git stocke ses configurations à trois niveaux différents :

1. **Niveau système** (`--system`) : s'applique à tous les utilisateurs du système
   ```bash
   git config --system [clé] [valeur]
   ```

2. **Niveau global** (`--global`) : s'applique à tous vos projets (c'est celui que nous avons utilisé)
   ```bash
   git config --global [clé] [valeur]
   ```

3. **Niveau local** (par défaut) : s'applique uniquement au projet en cours
   ```bash
   git config [clé] [valeur]
   ```

En général, vous utiliserez principalement la configuration globale, sauf si vous avez besoin d'une configuration spécifique pour un projet particulier.

## Où sont stockées les configurations ?

Si vous êtes curieux, les configurations Git sont stockées dans des fichiers texte simples :

- Niveau système : `/etc/gitconfig` (sur Linux/macOS) ou `C:\Program Files\Git\etc\gitconfig` (sur Windows)
- Niveau global : `~/.gitconfig` ou `~/.config/git/config` (sur Linux/macOS) ou `C:\Users\<username>\.gitconfig` (sur Windows)
- Niveau local : `.git/config` dans le répertoire du projet

Vous pouvez éditer ces fichiers directement si vous préférez, mais utiliser les commandes `git config` est généralement plus sûr.

## Résumé des configurations essentielles

Pour résumer, voici les configurations minimales que vous devriez faire :

```bash
# Votre identité
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@exemple.com"

# Gestion des fins de ligne (selon votre système)
# Pour Windows :
git config --global core.autocrlf true
# Pour macOS/Linux :
git config --global core.autocrlf input

# Branche par défaut
git config --global init.defaultBranch main
```

## Prochaines étapes

Félicitations ! Git est maintenant installé et configuré sur votre système. Dans la prochaine section, nous allons créer notre premier projet Git et apprendre les commandes de base pour commencer à suivre des modifications.
