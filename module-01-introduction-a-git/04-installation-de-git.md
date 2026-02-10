🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 1 : Introduction à Git

## 4. Installation de Git (Windows, macOS, Linux)

### Avant de commencer

Git est un outil en ligne de commande, ce qui signifie que vous l'utiliserez principalement via un terminal ou une invite de commande. Ne vous inquiétez pas si cela vous semble intimidant au début : nous allons tout expliquer pas à pas.

L'installation de Git est simple et gratuite, quel que soit votre système d'exploitation.

### Vérifier si Git est déjà installé

Avant d'installer Git, vérifiez s'il n'est pas déjà présent sur votre système. Ouvrez un terminal (ou invite de commande sur Windows) et tapez :

```bash
git --version
```

Si Git est installé, vous verrez s'afficher quelque chose comme :
```
git version 2.43.0
```

Si vous voyez un message d'erreur indiquant que la commande n'est pas reconnue, alors Git n'est pas installé et vous devez procéder à l'installation.

---

## Installation sur Windows

Il existe plusieurs façons d'installer Git sur Windows. Voici les méthodes les plus courantes :

### Méthode 1 : Git pour Windows (recommandée pour les débutants)

C'est la méthode la plus simple et la plus populaire.

#### Étapes d'installation

**1. Télécharger l'installateur**

Rendez-vous sur le site officiel : [https://git-scm.com/download/win](https://git-scm.com/download/win)

Le téléchargement devrait démarrer automatiquement. Si ce n'est pas le cas, cliquez sur le lien de téléchargement correspondant à votre système (64-bit ou 32-bit). La plupart des ordinateurs modernes sont en 64-bit.

**2. Lancer l'installateur**

Double-cliquez sur le fichier téléchargé (par exemple `Git-2.43.0-64-bit.exe`).

**3. Suivre l'assistant d'installation**

Vous allez voir plusieurs écrans avec des options. Voici ce que nous recommandons pour les débutants :

- **Licence** : Acceptez la licence GNU
- **Emplacement d'installation** : Laissez le chemin par défaut (`C:\Program Files\Git`)
- **Composants à installer** : Laissez toutes les options cochées par défaut
- **Menu Démarrer** : Laissez "Git" par défaut
- **Éditeur par défaut** : Choisissez votre éditeur préféré. Si vous débutez, **Visual Studio Code** est un excellent choix
- **Nom de la branche initiale** : Sélectionnez "Let Git decide" ou "main"
- **Ajustement de la variable PATH** : Choisissez **"Git from the command line and also from 3rd-party software"** (option recommandée)
- **Client SSH** : Laissez "Use bundled OpenSSH"
- **Backend HTTPS** : Laissez "Use the OpenSSL library"
- **Fins de ligne** : Choisissez **"Checkout Windows-style, commit Unix-style line endings"**
- **Émulateur de terminal** : Choisissez **"Use MinTTY"** (plus moderne et agréable)
- **git pull** : Laissez l'option par défaut
- **Gestionnaire d'identifiants** : Laissez "Git Credential Manager"
- **Options supplémentaires** : Laissez les options cochées par défaut

**4. Terminer l'installation**

Cliquez sur "Install" puis "Finish".

#### Vérifier l'installation

Ouvrez **Git Bash** (une application installée avec Git) ou l'invite de commande Windows et tapez :

```bash
git --version
```

Vous devriez voir le numéro de version de Git s'afficher.

### Méthode 2 : Via Windows Package Manager (winget)

Si vous préférez utiliser un gestionnaire de packages, Windows 10/11 dispose de **winget** :

```bash
winget install --id Git.Git -e --source winget
```

### Méthode 3 : Via Chocolatey

Si vous utilisez **Chocolatey** (un gestionnaire de packages pour Windows) :

```bash
choco install git
```

### Qu'est-ce qui est installé sur Windows ?

L'installation de Git pour Windows inclut :
- **Git Bash** : un terminal Linux-like pour Windows
- **Git GUI** : une interface graphique simple
- **Git CMD** : pour utiliser Git dans l'invite de commande Windows

---

## Installation sur macOS

Git peut être installé de plusieurs façons sur macOS :

### Méthode 1 : Via Xcode Command Line Tools (la plus simple)

macOS peut installer automatiquement Git quand vous en avez besoin.

**1. Ouvrir le Terminal**

Allez dans Applications > Utilitaires > Terminal (ou utilisez Spotlight avec Cmd + Espace et tapez "Terminal")

**2. Taper cette commande**

```bash
git --version
```

Si Git n'est pas installé, macOS vous proposera automatiquement d'installer les outils de ligne de commande Xcode, qui incluent Git. Cliquez sur "Installer" et suivez les instructions.

Cette méthode est la plus simple mais installe une version de Git qui peut ne pas être la toute dernière.

### Méthode 2 : Via Homebrew (recommandée)

**Homebrew** est le gestionnaire de packages le plus populaire pour macOS. Si vous envisagez de faire du développement, c'est un outil indispensable à avoir.

**1. Installer Homebrew (si ce n'est pas déjà fait)**

Ouvrez le Terminal et collez cette commande :

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Suivez les instructions à l'écran.

**2. Installer Git**

Une fois Homebrew installé, tapez :

```bash
brew install git
```

**3. Vérifier l'installation**

```bash
git --version
```

**Avantage** : Homebrew vous permet de maintenir Git à jour facilement avec `brew upgrade git`.

### Méthode 3 : Télécharger l'installateur officiel

Vous pouvez aussi télécharger un installateur depuis le site officiel :

[https://git-scm.com/download/mac](https://git-scm.com/download/mac)

Téléchargez le fichier `.dmg`, ouvrez-le et suivez les instructions.

### Méthode 4 : Via MacPorts

Si vous utilisez **MacPorts** plutôt que Homebrew :

```bash
sudo port install git
```

### Vérifier l'installation sur macOS

Quelle que soit la méthode choisie, vérifiez l'installation avec :

```bash
git --version
```

---

## Installation sur Linux

L'installation sur Linux dépend de votre distribution. Voici les commandes pour les distributions les plus populaires :

### Ubuntu / Debian / Linux Mint

Ouvrez un terminal et utilisez le gestionnaire de packages **apt** :

```bash
sudo apt update  
sudo apt install git
```

**Note** : `sudo` vous donne les permissions administrateur et vous demandera votre mot de passe.

### Fedora / Red Hat / CentOS

Utilisez **dnf** (ou **yum** sur les anciennes versions) :

**Fedora 22 et versions ultérieures :**
```bash
sudo dnf install git
```

**Anciennes versions de Fedora, Red Hat ou CentOS :**
```bash
sudo yum install git
```

### Arch Linux / Manjaro

Utilisez **pacman** :

```bash
sudo pacman -S git
```

### openSUSE

Utilisez **zypper** :

```bash
sudo zypper install git
```

### Alpine Linux

Utilisez **apk** :

```bash
apk add git
```

### Gentoo

Utilisez **emerge** :

```bash
emerge --ask dev-vcs/git
```

### Installer depuis les sources (avancé)

Si vous voulez la toute dernière version ou si votre distribution n'est pas listée, vous pouvez compiler Git depuis les sources :

```bash
# Télécharger la dernière version
wget https://github.com/git/git/archive/v2.43.0.tar.gz

# Extraire l'archive
tar -xzf v2.43.0.tar.gz

# Se déplacer dans le dossier
cd git-2.43.0

# Compiler et installer
make prefix=/usr/local all  
sudo make prefix=/usr/local install
```

**Note** : Cette méthode nécessite d'avoir les outils de compilation installés.

### Vérifier l'installation sur Linux

```bash
git --version
```

---

## Vérification finale de l'installation

Une fois Git installé, vérifiez que tout fonctionne correctement :

### 1. Vérifier la version

```bash
git --version
```

Vous devriez voir quelque chose comme :
```
git version 2.43.0
```

Le numéro exact peut varier selon votre système et le moment de l'installation.

### 2. Vérifier l'aide

Tapez :

```bash
git --help
```

Vous devriez voir s'afficher une liste de commandes Git disponibles.

### 3. Tester une commande simple

```bash
git config --list
```

Cette commande affiche la configuration actuelle de Git (probablement vide pour l'instant).

---

## Après l'installation : prochaines étapes

Maintenant que Git est installé, vous êtes prêt pour les étapes suivantes :

1. **Configuration initiale** : Vous devrez configurer votre nom et votre email (nous verrons cela dans la prochaine section)
2. **Choisir un éditeur de texte** : Pour écrire vos messages de commit
3. **Commencer à utiliser Git** : Créer votre premier dépôt

### Mettre à jour Git

Il est recommandé de garder Git à jour pour bénéficier des dernières fonctionnalités et corrections de sécurité.

**Sur Windows** : Téléchargez et installez la dernière version depuis le site officiel

**Sur macOS avec Homebrew** :
```bash
brew upgrade git
```

**Sur Linux** :
```bash
# Ubuntu/Debian
sudo apt update && sudo apt upgrade git

# Fedora
sudo dnf upgrade git

# Arch Linux
sudo pacman -Syu git
```

### Obtenir de l'aide

Si vous rencontrez des problèmes lors de l'installation :

- Consultez la documentation officielle : [https://git-scm.com/book/fr/v2](https://git-scm.com/book/fr/v2)
- Vérifiez les forums et Stack Overflow
- Assurez-vous d'avoir les permissions administrateur
- Redémarrez votre terminal ou votre ordinateur après l'installation

---

## En résumé

Vous avez maintenant installé Git sur votre système ! Les points clés à retenir :

- **Windows** : Utilisez Git pour Windows (Git Bash) pour une expérience complète
- **macOS** : Homebrew est la méthode recommandée pour rester à jour facilement
- **Linux** : Utilisez le gestionnaire de packages de votre distribution

La commande `git --version` vous permet de vérifier que tout est bien installé.

Maintenant que Git est opérationnel, nous allons procéder à sa configuration initiale pour pouvoir commencer à l'utiliser.

---

*Dans la section suivante, nous configurerons Git avec votre identité et vos préférences.*

⏭️ [Configuration initiale (git config)](/module-01-introduction-a-git/05-configuration-initiale.md)
