# 4.5. Gérer les clés SSH

## Qu'est-ce que SSH et pourquoi l'utiliser ?

**SSH** (Secure Shell) est un protocole sécurisé qui permet de se connecter à des services distants, comme GitHub, sans avoir à saisir votre nom d'utilisateur et mot de passe à chaque fois.

Jusqu'à présent, nous avons utilisé des URL HTTPS pour communiquer avec GitHub (comme `https://github.com/utilisateur/projet.git`). Cette méthode vous demande vos identifiants à chaque `push`.

Avec SSH, vous :
- **N'avez plus besoin** de saisir votre mot de passe à chaque push
- Bénéficiez d'une **connexion plus sécurisée**
- Pouvez travailler plus **efficacement**

## L'analogie de la clé et de la serrure

Les clés SSH fonctionnent comme un système de clé et serrure :

- La **clé privée** reste sur votre ordinateur (comme votre clé de maison)
- La **clé publique** est ajoutée à GitHub (comme installer votre serrure sur la porte de GitHub)
- Quand vous vous connectez, votre clé privée "ouvre" la serrure, prouvant que c'est bien vous

## Configuration des clés SSH : étape par étape

### Étape 1 : Vérifier si vous avez déjà des clés SSH

Avant de créer de nouvelles clés, vérifiez si vous en avez déjà :

```bash
ls -la ~/.ssh
```

Si vous voyez des fichiers comme `id_rsa` (clé privée) et `id_rsa.pub` (clé publique), vous avez déjà des clés SSH.

### Étape 2 : Générer de nouvelles clés SSH

Si vous n'avez pas encore de clés, ou si vous souhaitez en créer de nouvelles, utilisez cette commande :

#### Pour Windows, macOS et Linux

```bash
ssh-keygen -t ed25519 -C "votre_email@exemple.com"
```

> **Note** : Remplacez l'adresse e-mail par celle associée à votre compte GitHub.

Le système vous demandera où enregistrer la clé. Appuyez simplement sur Entrée pour accepter l'emplacement par défaut.

```
> Enter a file in which to save the key (/Users/vous/.ssh/id_ed25519): [Appuyez sur Entrée]
```

Ensuite, vous serez invité à créer une phrase de passe. Pour une sécurité maximale, entrez une phrase de passe forte. Si vous préférez ne pas en utiliser (moins sécurisé mais plus pratique), appuyez simplement sur Entrée deux fois.

```
> Enter passphrase (empty for no passphrase): [Entrez une phrase de passe ou appuyez sur Entrée]
> Enter same passphrase again: [Confirmez la phrase de passe]
```

### Étape 3 : Ajouter votre clé SSH à l'agent SSH

L'agent SSH est un programme qui garde vos clés en mémoire pour que vous n'ayez pas à entrer votre phrase de passe à chaque utilisation.

#### Pour macOS et Linux

Démarrez l'agent SSH en arrière-plan :

```bash
eval "$(ssh-agent -s)"
```

Ajoutez votre clé privée à l'agent :

```bash
ssh-add ~/.ssh/id_ed25519
```

#### Pour Windows (avec Git Bash)

Démarrez l'agent SSH :

```bash
eval "$(ssh-agent -s)"
```

Ajoutez votre clé privée :

```bash
ssh-add ~/.ssh/id_ed25519
```

### Étape 4 : Ajouter votre clé publique à GitHub

Maintenant, vous devez copier votre clé publique et l'ajouter à votre compte GitHub.

#### 1. Copier la clé publique

**Sur macOS**, vous pouvez copier la clé dans le presse-papiers avec :

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

**Sur Linux**, utilisez :

```bash
cat ~/.ssh/id_ed25519.pub
```
Puis sélectionnez et copiez la sortie affichée.

**Sur Windows** (avec Git Bash) :

```bash
cat ~/.ssh/id_ed25519.pub
```
Puis sélectionnez et copiez la sortie affichée.

#### 2. Ajouter la clé à GitHub

1. Connectez-vous à votre compte GitHub
2. Cliquez sur votre photo de profil en haut à droite, puis sur **Settings** (Paramètres)
3. Dans le menu latéral gauche, cliquez sur **SSH and GPG keys**
4. Cliquez sur le bouton vert **New SSH key** (Nouvelle clé SSH)
5. Donnez un titre descriptif à votre clé (par exemple, "Ordinateur personnel" ou "Laptop de travail")
6. Collez votre clé publique dans le champ "Key"
7. Cliquez sur **Add SSH key** (Ajouter la clé SSH)
8. Si demandé, confirmez avec votre mot de passe GitHub

### Étape 5 : Tester votre connexion SSH

Pour vérifier que tout fonctionne correctement :

```bash
ssh -T git@github.com
```

Vous verrez peut-être un avertissement comme celui-ci :

```
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
> Are you sure you want to continue connecting (yes/no)?
```

Tapez `yes` et appuyez sur Entrée.

Si tout est configuré correctement, vous verrez un message comme :

```
> Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

Félicitations ! Votre connexion SSH est maintenant configurée.

## Utiliser SSH avec vos dépôts Git

### Pour les nouveaux dépôts

Lorsque vous clonez un nouveau dépôt, utilisez l'URL SSH au lieu de HTTPS :

```bash
git clone git@github.com:utilisateur/projet.git
```

### Pour les dépôts existants

Si vous avez déjà cloné un dépôt avec HTTPS, vous pouvez changer l'URL pour utiliser SSH :

```bash
git remote set-url origin git@github.com:utilisateur/projet.git
```

Pour vérifier que l'URL a bien été modifiée :

```bash
git remote -v
```

## Dépannage des problèmes courants

### "Permission denied (publickey)"

Ce message signifie que GitHub n'a pas accepté votre clé SSH.

**Solutions possibles** :
1. Vérifiez que vous avez bien ajouté votre clé publique à GitHub
2. Assurez-vous que l'agent SSH est en cours d'exécution avec `ssh-agent -s`
3. Ajoutez à nouveau votre clé à l'agent avec `ssh-add`

### "Could not open a connection to your authentication agent"

Cela signifie que l'agent SSH n'est pas en cours d'exécution.

**Solution** : Démarrez l'agent avec `eval "$(ssh-agent -s)"` avant d'utiliser `ssh-add`.

### "Host key verification failed"

Cela peut se produire si l'empreinte digitale du serveur GitHub a changé.

**Solution** : Éditez le fichier `~/.ssh/known_hosts` et supprimez la ligne concernant github.com, puis réessayez.

## Sécurité des clés SSH

Pour maintenir la sécurité de vos clés SSH :

1. **Ne partagez jamais** votre clé privée (`id_ed25519`) avec qui que ce soit
2. Utilisez une **phrase de passe** forte pour protéger votre clé
3. **Sauvegardez** vos clés dans un endroit sûr
4. **Créez des clés différentes** pour différents services ou ordinateurs
5. **Révoquez** les clés depuis GitHub si vous perdez l'accès à votre ordinateur

## Résumé

- Les clés SSH offrent une connexion sécurisée et pratique à GitHub
- Une paire de clés comprend une clé privée (sur votre ordinateur) et une clé publique (sur GitHub)
- L'utilisation de SSH vous évite de saisir votre mot de passe à chaque push
- La configuration est un processus en plusieurs étapes, mais vous n'avez besoin de le faire qu'une fois par ordinateur

---

Dans la prochaine section, nous verrons comment contribuer à des projets existants via les forks et les pull requests (PR), les mécanismes fondamentaux de la collaboration sur GitHub.
