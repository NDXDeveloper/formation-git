🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 1 : Introduction à Git

## 5. Configuration initiale (git config)

### Pourquoi configurer Git ?

Maintenant que Git est installé, il faut le configurer avant de pouvoir l'utiliser efficacement. La configuration permet de :

- **Identifier qui vous êtes** : Git enregistre votre nom et email avec chaque modification
- **Personnaliser votre environnement** : Choisir votre éditeur de texte, vos couleurs, etc.
- **Définir vos préférences** : Comment Git doit se comporter dans certaines situations

Ces configurations ne se font qu'une seule fois (ou presque), mais elles sont essentielles pour bien commencer.

### Les trois niveaux de configuration

Git utilise un système de configuration à trois niveaux. Comprendre cela vous aidera à mieux maîtriser vos paramètres.

#### 1. Configuration système (--system)

- S'applique à **tous les utilisateurs** de l'ordinateur
- Stockée dans `/etc/gitconfig` (Linux/macOS) ou `C:\Program Files\Git\etc\gitconfig` (Windows)
- Nécessite les droits administrateur pour la modifier
- Rarement utilisée

#### 2. Configuration globale (--global)

- S'applique à **votre utilisateur** sur l'ordinateur
- Stockée dans `~/.gitconfig` ou `~/.config/git/config`
- **C'est celle que vous utiliserez le plus souvent**
- Affecte tous vos projets Git

#### 3. Configuration locale (--local)

- S'applique uniquement au **dépôt actuel**
- Stockée dans `.git/config` à l'intérieur de votre projet
- Prioritaire sur les configurations globale et système
- Utile pour des paramètres spécifiques à un projet

**Ordre de priorité** : Local > Global > Système

Si une même configuration existe à plusieurs niveaux, c'est la plus spécifique (locale) qui l'emporte.

---

## Configuration minimale obligatoire

Avant de faire votre premier commit, vous **devez** configurer votre identité. Git en a besoin pour enregistrer qui fait quelles modifications.

### Configurer votre nom

Ouvrez un terminal (ou Git Bash sur Windows) et tapez :

```bash
git config --global user.name "Votre Nom"
```

Remplacez "Votre Nom" par votre vrai nom. Par exemple :

```bash
git config --global user.name "Marie Dupont"
```

**Note** : Les guillemets sont nécessaires si votre nom contient des espaces.

### Configurer votre email

```bash
git config --global user.email "votre.email@example.com"
```

Par exemple :

```bash
git config --global user.email "marie.dupont@email.com"
```

**Important** : Si vous prévoyez d'utiliser GitHub, GitLab ou un autre service en ligne, utilisez la même adresse email que celle de votre compte sur ces plateformes.

### Vérifier votre configuration

Pour voir ce que vous avez configuré :

```bash
git config --global user.name
git config --global user.email
```

Ou pour voir toute votre configuration globale :

```bash
git config --global --list
```

---

## Configuration de l'éditeur de texte

Git a besoin d'un éditeur de texte pour que vous puissiez écrire des messages de commit et modifier certains fichiers. Par défaut, Git utilise l'éditeur système (souvent Vim ou Nano), qui peut être déroutant pour les débutants.

### Choisir un éditeur moderne

Voici comment configurer les éditeurs les plus populaires :

#### Visual Studio Code (recommandé pour les débutants)

```bash
git config --global core.editor "code --wait"
```

**Note** : VS Code doit être installé et accessible depuis le terminal.

#### Sublime Text

```bash
git config --global core.editor "subl -n -w"
```

#### Atom

```bash
git config --global core.editor "atom --wait"
```

#### Notepad++ (Windows)

```bash
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

#### Nano (simple et intégré sur Linux/macOS)

```bash
git config --global core.editor "nano"
```

#### Vim (pour les utilisateurs avancés)

C'est souvent l'éditeur par défaut. Si vous voulez l'utiliser explicitement :

```bash
git config --global core.editor "vim"
```

**Astuce** : Si vous ne savez pas quoi choisir, **Visual Studio Code** est un excellent choix pour débuter. Il est gratuit, moderne et facile à utiliser.

---

## Autres configurations utiles

### Activer la coloration

Les couleurs rendent la sortie de Git beaucoup plus lisible :

```bash
git config --global color.ui auto
```

Cette option est généralement activée par défaut dans les versions récentes de Git.

### Définir le nom de la branche par défaut

Depuis quelques années, la communauté Git recommande d'utiliser `main` plutôt que `master` comme nom de branche principale :

```bash
git config --global init.defaultBranch main
```

**Contexte** : Historiquement, Git utilisait "master" par défaut, mais "main" est maintenant le standard adopté par GitHub et d'autres plateformes.

### Configurer la fin de ligne (important sur Windows)

Les systèmes Windows et Unix/macOS gèrent différemment les fins de ligne dans les fichiers texte. Pour éviter des problèmes :

**Sur Windows** :
```bash
git config --global core.autocrlf true
```

**Sur macOS/Linux** :
```bash
git config --global core.autocrlf input
```

Cette configuration garantit que vos fichiers auront les bonnes fins de ligne quel que soit votre système.

### Améliorer l'affichage de git log

Pour que l'historique soit plus lisible :

```bash
git config --global log.date relative
```

Cela affichera les dates de manière plus naturelle ("il y a 2 jours" plutôt qu'une date exacte).

---

## Visualiser et modifier la configuration

### Voir toute la configuration

Pour afficher tous vos paramètres Git :

```bash
git config --list
```

Pour voir d'où vient chaque paramètre (système, global, local) :

```bash
git config --list --show-origin
```

### Voir une valeur spécifique

Pour vérifier une configuration particulière :

```bash
git config user.name
git config user.email
git config core.editor
```

### Modifier une configuration

Pour changer une valeur existante, utilisez simplement la même commande qu'à la création :

```bash
git config --global user.name "Nouveau Nom"
```

### Supprimer une configuration

Pour enlever un paramètre :

```bash
git config --global --unset user.name
```

### Éditer le fichier de configuration directement

Vous pouvez aussi ouvrir le fichier de configuration dans votre éditeur :

```bash
git config --global --edit
```

Cela ouvrira le fichier `~/.gitconfig` dans l'éditeur que vous avez configuré. Vous verrez quelque chose comme :

```ini
[user]
    name = Marie Dupont
    email = marie.dupont@email.com
[core]
    editor = code --wait
[init]
    defaultBranch = main
```

**Attention** : Soyez prudent en éditant directement ce fichier. Une erreur de syntaxe peut causer des problèmes.

---

## Configuration complète recommandée pour débutants

Voici un ensemble de commandes recommandées pour bien configurer Git quand on débute :

```bash
# Identité (OBLIGATOIRE - adaptez avec vos informations)
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Éditeur (choisissez celui que vous préférez)
git config --global core.editor "code --wait"

# Branche par défaut
git config --global init.defaultBranch main

# Fin de ligne (Windows)
git config --global core.autocrlf true

# Fin de ligne (macOS/Linux)
git config --global core.autocrlf input

# Coloration
git config --global color.ui auto

# Affichage plus lisible des dates
git config --global log.date relative
```

Copiez et collez ces commandes dans votre terminal en adaptant le nom, l'email et l'éditeur selon vos préférences.

---

## Configurations avancées (optionnelles)

Ces configurations ne sont pas nécessaires pour débuter, mais peuvent être utiles plus tard :

### Créer des alias (raccourcis)

Les alias vous permettent de créer des raccourcis pour les commandes Git fréquentes :

```bash
# Raccourci pour 'git status'
git config --global alias.st status

# Raccourci pour 'git commit'
git config --global alias.ci commit

# Afficher un log coloré et condensé
git config --global alias.lg "log --oneline --graph --all --decorate"
```

Ensuite, vous pourrez taper `git st` au lieu de `git status`.

### Configurer le comportement de git pull

```bash
git config --global pull.rebase false
```

Cela définit comment Git doit se comporter lors d'un `git pull`. Pour les débutants, `false` (merge) est plus simple.

### Activer le cache des identifiants

Pour éviter de retaper votre mot de passe à chaque fois :

**Sur macOS** :
```bash
git config --global credential.helper osxkeychain
```

**Sur Windows** :
```bash
git config --global credential.helper manager-core
```

**Sur Linux** :
```bash
git config --global credential.helper cache
```

---

## Vérification finale de votre configuration

Après avoir effectué toutes ces configurations, vérifiez que tout est correct :

```bash
git config --list
```

Vous devriez voir au minimum :
- `user.name=Votre Nom`
- `user.email=votre.email@example.com`
- `core.editor=...`
- `init.defaultbranch=main`

### Tester votre configuration

Pour vous assurer que tout fonctionne :

```bash
# Afficher votre nom
git config user.name

# Afficher votre email
git config user.email

# Ouvrir l'éditeur (fermez-le ensuite)
git config --global --edit
```

Si toutes ces commandes fonctionnent sans erreur, votre configuration est opérationnelle !

---

## Résolution de problèmes courants

### "Please tell me who you are"

Si vous voyez ce message :
```
*** Please tell me who you are.
Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```

Cela signifie que vous n'avez pas configuré votre identité. Exécutez simplement les commandes indiquées avec vos vraies informations.

### L'éditeur ne s'ouvre pas correctement

Si la commande `git config --global --edit` ne fonctionne pas ou ouvre un éditeur que vous ne connaissez pas :

1. Vérifiez que votre éditeur est bien installé
2. Vérifiez que la commande de l'éditeur est dans votre PATH
3. Essayez de reconfigurer l'éditeur avec une autre valeur

### Les modifications ne sont pas prises en compte

Si vos changements de configuration ne semblent pas s'appliquer :

1. Vérifiez que vous utilisez le bon niveau (--global, --local)
2. Une configuration locale peut écraser la configuration globale
3. Redémarrez votre terminal

---

## En résumé

La configuration de Git via `git config` est une étape essentielle avant de commencer à utiliser Git. Les points clés :

- **Trois niveaux** : system, global, local (du moins au plus spécifique)
- **Configuration minimale obligatoire** : nom et email
- **Configuration recommandée** : éditeur, branche par défaut, fin de ligne
- **La commande** : `git config --global clé valeur`
- **Visualisation** : `git config --list` pour voir toutes les configurations

Une fois cette configuration effectuée, vous êtes prêt à créer votre premier projet Git et à commencer à versionner votre code !

---

*Dans la section suivante, nous créerons notre premier dépôt Git et effectuerons nos premiers commits.*

⏭️ [Premier projet Git : création d'un dépôt local](/module-01-introduction-a-git/06-premier-projet-git.md)
