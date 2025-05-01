# 1.4. Installation de Git (Windows, macOS, Linux)

Avant de pouvoir utiliser Git, vous devez l'installer sur votre ordinateur. Ce processus est assez simple, quelle que soit votre plateforme. Dans cette section, nous vous guiderons étape par étape à travers l'installation de Git sur les systèmes d'exploitation les plus courants.

## Installation sur Windows

### Méthode 1 : Installation avec l'installateur officiel (recommandée)

1. **Téléchargez l'installateur** :
   - Rendez-vous sur le site officiel de Git : [https://git-scm.com/download/win](https://git-scm.com/download/win)
   - Le téléchargement devrait démarrer automatiquement avec la version la plus adaptée à votre système

2. **Lancez l'installateur** :
   - Une fois le téléchargement terminé, ouvrez le fichier `.exe`
   - Si une boîte de dialogue de sécurité apparaît, cliquez sur "Oui" pour autoriser l'installation

3. **Options de l'assistant d'installation** :
   - **Licence** : Lisez et acceptez la licence GNU

   - **Emplacement d'installation** : Le répertoire par défaut (`C:\Program Files\Git`) convient généralement

   - **Sélection des composants** : Laissez les options par défaut, mais assurez-vous que ces éléments sont cochés :
     - Git Bash Here
     - Git GUI Here
     - Git LFS (Large File Support)
     - Associate .git* configuration files with the default text editor
     - Associate .sh files to be run with Bash

   - **Menu de démarrage** : Créez un dossier dans le menu démarrage (option recommandée)

   - **Choix de l'éditeur** : Sélectionnez l'éditeur que vous préférez utiliser avec Git (Notepad++ ou VS Code sont de bons choix pour les débutants)

   - **Nom de la branche initiale** : Laissez l'option par défaut ("Let Git decide")

   - **Ajustement du PATH** : Sélectionnez "Git from the command line and also from 3rd-party software" (recommandé)

   - **Client SSH** : Laissez l'option par défaut (OpenSSH)

   - **Configuration HTTPS** : Laissez l'option par défaut (OpenSSL)

   - **Configuration des fins de ligne** : Sélectionnez "Checkout Windows-style, commit Unix-style" (recommandé pour Windows)

   - **Émulateur de terminal** : Choisissez "Use MinTTY"

   - **Comportement de Git Pull** : Laissez l'option par défaut (fast-forward or merge)

   - **Outil de mise en cache des identifiants** : Laissez l'option par défaut (Git Credential Manager)

   - **Options supplémentaires** : Laissez les options par défaut

   - **Options expérimentales** : Vous pouvez les laisser décochées

4. **Terminez l'installation** :
   - Cliquez sur "Installer"
   - Une fois l'installation terminée, vous pouvez cliquer sur "Terminer"

### Méthode 2 : Installation via un gestionnaire de paquets

Si vous utilisez déjà un gestionnaire de paquets comme Chocolatey ou Scoop, vous pouvez installer Git avec une simple commande :

**Chocolatey** :
```
choco install git
```

**Scoop** :
```
scoop install git
```

## Installation sur macOS

### Méthode 1 : Installation avec l'installateur officiel

1. **Téléchargez l'installateur** :
   - Visitez [https://git-scm.com/download/mac](https://git-scm.com/download/mac)
   - Cliquez sur le lien pour télécharger la dernière version

2. **Installez le package** :
   - Ouvrez le fichier `.dmg` téléchargé
   - Double-cliquez sur le package d'installation
   - Suivez les instructions à l'écran

### Méthode 2 : Installation via Homebrew (recommandée si vous utilisez déjà Homebrew)

Si vous avez déjà Homebrew installé, cette méthode est la plus simple :

1. **Ouvrez le Terminal**

2. **Exécutez la commande** :
   ```
   brew install git
   ```

### Méthode 3 : Utiliser la version préinstallée ou Xcode

macOS inclut souvent une version de Git :

1. **Vérifiez si Git est déjà installé** :
   - Ouvrez le Terminal
   - Tapez `git --version`

2. **Si Git n'est pas installé et que vous avez Xcode** :
   - L'installation de Xcode ou des "Command Line Tools" de Xcode installera Git
   - Pour installer uniquement les outils en ligne de commande, exécutez :
     ```
     xcode-select --install
     ```

## Installation sur Linux

L'installation de Git sur Linux dépend de votre distribution. Voici les méthodes pour les distributions les plus courantes :

### Ubuntu, Debian et dérivés

1. **Ouvrez un terminal**

2. **Mettez à jour les listes de paquets** :
   ```
   sudo apt update
   ```

3. **Installez Git** :
   ```
   sudo apt install git
   ```

### Fedora, Red Hat, CentOS

1. **Ouvrez un terminal**

2. **Sur Fedora** :
   ```
   sudo dnf install git
   ```

   **Sur CentOS/RHEL (versions plus anciennes)** :
   ```
   sudo yum install git
   ```

### Arch Linux et dérivés

1. **Ouvrez un terminal**

2. **Installez Git** :
   ```
   sudo pacman -S git
   ```

## Vérification de l'installation

Quelle que soit la plateforme, vous pouvez vérifier que Git est correctement installé en exécutant cette commande dans votre terminal ou invite de commande :

```
git --version
```

Vous devriez voir s'afficher la version de Git installée, par exemple :
```
git version 2.33.0
```

Si cette commande fonctionne, félicitations ! Git est installé correctement.

## Résolution des problèmes courants d'installation

### Windows : "Git n'est pas reconnu comme une commande interne ou externe"

Ce message indique que Git n'est pas dans votre PATH système.

**Solution** :
1. Réinstallez Git en choisissant l'option "Git from the command line and also from 3rd-party software"
2. Ou ajoutez manuellement Git à votre PATH système

### macOS/Linux : "Command not found: git"

**Solutions possibles** :
1. Assurez-vous que l'installation s'est terminée correctement
2. Essayez de redémarrer votre terminal
3. Vérifiez votre PATH avec `echo $PATH` pour voir si le répertoire d'installation de Git y figure

### Problèmes de connexion HTTPS (tous systèmes)

Si vous rencontrez des problèmes pour vous connecter à des dépôts distants via HTTPS :

**Solution** :
1. Vérifiez votre connexion internet
2. Si vous êtes derrière un pare-feu d'entreprise, vous devrez peut-être configurer un proxy Git :
   ```
   git config --global http.proxy http://proxy.example.com:8080
   ```

## Prochaines étapes

Maintenant que Git est installé sur votre système, vous êtes prêt à passer à la configuration initiale. Dans la prochaine section, nous verrons comment configurer Git avec vos informations personnelles et vos préférences.
