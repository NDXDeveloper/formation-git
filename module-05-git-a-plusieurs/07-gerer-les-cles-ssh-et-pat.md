🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 5 : Git à plusieurs – Dépôts distants

## 7. Authentification : Clés SSH et Personal Access Tokens (PAT)

### Pourquoi l'authentification est nécessaire

Lorsque vous interagissez avec un dépôt distant (push, pull, clone d'un dépôt privé), vous devez prouver votre identité. C'est comme montrer votre carte d'identité avant d'entrer dans un bâtiment sécurisé.

**Sans authentification, n'importe qui pourrait :**
- Modifier le code de vos projets
- Supprimer des dépôts
- Accéder à du code privé

L'authentification garantit que seules les personnes autorisées peuvent accéder et modifier les dépôts.

### Les deux méthodes d'authentification

Il existe deux principales façons de s'authentifier avec Git :

**1. HTTPS avec Personal Access Token (PAT)**
- Utilise une URL du type `https://github.com/...`
- Nécessite un token (mot de passe spécial) généré sur la plateforme
- Simple à configurer
- Recommandé pour débuter

**2. SSH avec clés cryptographiques**
- Utilise une URL du type `git@github.com:...`
- Nécessite de générer une paire de clés sur votre ordinateur
- Plus technique mais plus pratique à l'usage
- Recommandé pour un usage quotidien

Les deux méthodes sont sécurisées. Le choix dépend de vos préférences et de votre niveau de confort technique.

### Comprendre les protocoles

**HTTPS (HyperText Transfer Protocol Secure)**

C'est le même protocole que vous utilisez pour naviguer sur le web. Quand vous voyez `https://` dans votre navigateur, c'est du HTTPS.

**Avantages :**
- Facile à comprendre
- Fonctionne partout (même derrière des firewalls stricts)
- Pas de configuration système complexe

**Inconvénients :**
- Demande de s'authentifier à chaque opération (ou presque)
- Moins pratique pour un usage intensif

**SSH (Secure Shell)**

SSH est un protocole de communication sécurisé utilisé pour se connecter à des serveurs distants.

**Avantages :**
- Authentification automatique après configuration
- Pas besoin de taper ses identifiants
- Très sécurisé
- Idéal pour un usage quotidien

**Inconvénients :**
- Configuration initiale plus technique
- Peut être bloqué par certains firewalls d'entreprise

### Méthode 1 : HTTPS avec Personal Access Token (PAT)

#### Qu'est-ce qu'un Personal Access Token ?

Un **Personal Access Token (PAT)** est un mot de passe spécial généré par GitHub, GitLab ou Bitbucket. C'est comme un badge d'accès temporaire ou permanent que vous pouvez révoquer à tout moment.

**Pourquoi ne pas utiliser votre mot de passe normal ?**

Depuis 2021, GitHub (et d'autres plateformes) a désactivé l'authentification par mot de passe pour des raisons de sécurité. Les tokens sont plus sûrs car :
- Vous pouvez limiter leurs permissions (lecture seule, écriture, etc.)
- Vous pouvez les révoquer sans changer votre mot de passe
- Vous pouvez avoir plusieurs tokens pour différents usages
- Ils peuvent avoir une date d'expiration

#### Créer un Personal Access Token sur GitHub

**Étape 1 : Accéder aux paramètres**

1. Connectez-vous à GitHub
2. Cliquez sur votre photo de profil (en haut à droite)
3. Cliquez sur "Settings"
4. Dans le menu de gauche, descendez et cliquez sur "Developer settings"
5. Cliquez sur "Personal access tokens" puis "Tokens (classic)"
6. Cliquez sur "Generate new token" puis "Generate new token (classic)"

**Étape 2 : Configurer le token**

1. **Note** : Donnez un nom descriptif (ex: "Ordinateur portable", "Projet X")
2. **Expiration** : Choisissez une durée (30 jours, 90 jours, pas d'expiration)
3. **Scopes** : Cochez les permissions nécessaires
   - Pour un usage normal, cochez : `repo` (accès complet aux dépôts)
   - Pour plus de contrôle, explorez les autres options

4. Cliquez sur "Generate token" en bas de la page

**Étape 3 : Copier et sauvegarder le token**

⚠️ **IMPORTANT** : Le token ne s'affichera qu'une seule fois !

```
ghp_1234567890abcdefghijklmnopqrstuvwxyz
```

Copiez-le immédiatement et sauvegardez-le dans un endroit sûr :
- Un gestionnaire de mots de passe (recommandé)
- Un fichier texte sécurisé
- Ne le partagez JAMAIS publiquement

Si vous le perdez, vous devrez en générer un nouveau.

#### Utiliser le token avec Git

**Lors d'un clone, push ou pull :**

```bash
git clone https://github.com/utilisateur/projet.git
```

Git vous demandera :
```
Username for 'https://github.com': votre-nom-utilisateur
Password for 'https://votre-nom-utilisateur@github.com':
```

Pour le "Password", collez votre token (pas votre mot de passe GitHub !).

**Stocker le token localement (optionnel)**

Pour ne pas retaper le token à chaque fois, utilisez le gestionnaire d'identifiants :

**Sur macOS :**
```bash
git config --global credential.helper osxkeychain
```

**Sur Windows :**
```bash
git config --global credential.helper wincred
```
Ou installez [Git Credential Manager](https://github.com/GitCredentialManager/git-credential-manager).

**Sur Linux :**
```bash
git config --global credential.helper store
```
⚠️ Attention : ceci stocke le token en clair dans un fichier. Pour plus de sécurité, utilisez `cache` :
```bash
git config --global credential.helper cache
```

Après configuration, Git mémorisera votre token après la première utilisation.

#### Créer un PAT sur GitLab

1. Cliquez sur votre avatar > "Preferences"
2. Dans le menu de gauche : "Access Tokens"
3. Remplissez :
   - Token name (nom descriptif)
   - Expiration date (date d'expiration)
   - Scopes : cochez `read_repository` et `write_repository`
4. Cliquez sur "Create personal access token"
5. Copiez le token affiché

#### Créer un PAT sur Bitbucket

1. Cliquez sur votre avatar > "Personal settings"
2. Dans le menu de gauche : "App passwords"
3. Cliquez sur "Create app password"
4. Donnez un nom et sélectionnez les permissions
5. Cliquez sur "Create"
6. Copiez le mot de passe généré

### Méthode 2 : SSH avec clés cryptographiques

#### Qu'est-ce que SSH et les clés ?

SSH utilise une paire de clés cryptographiques :
- **Clé privée** : reste secrète sur votre ordinateur (comme votre carte d'identité)
- **Clé publique** : partagée avec GitHub/GitLab/Bitbucket (comme votre photo d'identité publique)

**Analogie :** C'est comme une serrure et une clé. Vous gardez la clé (clé privée), et vous donnez la serrure (clé publique) à GitHub. Quand vous vous connectez, GitHub vérifie que votre clé correspond à la serrure.

#### Vérifier si vous avez déjà des clés SSH

Avant de créer de nouvelles clés, vérifiez si vous en avez déjà :

```bash
ls -al ~/.ssh
```

Cherchez des fichiers comme :
- `id_rsa` et `id_rsa.pub`
- `id_ed25519` et `id_ed25519.pub`

Si vous voyez ces fichiers, vous avez déjà des clés et pouvez passer à l'étape "Ajouter la clé à GitHub".

#### Générer une nouvelle paire de clés SSH

**Commande recommandée (algorithme moderne) :**

```bash
ssh-keygen -t ed25519 -C "votre-email@example.com"
```

Si votre système ne supporte pas ed25519, utilisez :

```bash
ssh-keygen -t rsa -b 4096 -C "votre-email@example.com"
```

**Processus interactif :**

```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/vous/.ssh/id_ed25519):
```

**Appuyez sur Entrée** pour accepter l'emplacement par défaut.

```
Enter passphrase (empty for no passphrase):
```

**Optionnel** : Entrez une phrase de passe pour plus de sécurité (recommandé).
Cette phrase protège votre clé privée. Si quelqu'un vole votre clé, il ne pourra pas l'utiliser sans la phrase.

Vous pouvez aussi appuyer sur Entrée pour ne pas mettre de phrase (moins sécurisé mais plus pratique).

```
Enter same passphrase again:
```

Répétez la phrase de passe (ou Entrée à nouveau).

Vos clés sont maintenant créées :
- Clé privée : `~/.ssh/id_ed25519`
- Clé publique : `~/.ssh/id_ed25519.pub`

#### Démarrer l'agent SSH (si nécessaire)

Sur certains systèmes, vous devez démarrer l'agent SSH :

```bash
eval "$(ssh-agent -s)"
```

Puis ajouter votre clé :

```bash
ssh-add ~/.ssh/id_ed25519
```

Si vous avez défini une phrase de passe, entrez-la maintenant.

#### Copier votre clé publique

**Sur macOS :**
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

**Sur Linux :**
```bash
cat ~/.ssh/id_ed25519.pub
# Puis sélectionnez et copiez le résultat
```

**Sur Windows (Git Bash) :**
```bash
clip < ~/.ssh/id_ed25519.pub
```

Vous devriez voir quelque chose comme :
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJl3dIeudNqd0DPMRD6OIh65A9pu9hj+mUBzEKrjmF3 votre-email@example.com
```

#### Ajouter la clé publique à GitHub

1. Allez sur GitHub et connectez-vous
2. Cliquez sur votre photo de profil > "Settings"
3. Dans le menu de gauche : "SSH and GPG keys"
4. Cliquez sur "New SSH key"
5. Remplissez :
   - **Title** : Nom descriptif (ex: "MacBook Pro", "PC Bureau")
   - **Key** : Collez votre clé publique (celle que vous venez de copier)
6. Cliquez sur "Add SSH key"
7. Confirmez avec votre mot de passe GitHub si demandé

#### Ajouter la clé publique à GitLab

1. Cliquez sur votre avatar > "Preferences"
2. Dans le menu de gauche : "SSH Keys"
3. Collez votre clé publique dans le champ "Key"
4. Donnez un titre
5. Optionnel : définissez une date d'expiration
6. Cliquez sur "Add key"

#### Ajouter la clé publique à Bitbucket

1. Cliquez sur votre avatar > "Personal settings"
2. Dans le menu de gauche : "SSH keys"
3. Cliquez sur "Add key"
4. Donnez un nom
5. Collez votre clé publique
6. Cliquez sur "Add key"

#### Tester votre connexion SSH

**Pour GitHub :**
```bash
ssh -T git@github.com
```

Réponse attendue :
```
Hi utilisateur! You've successfully authenticated, but GitHub does not provide shell access.
```

**Pour GitLab :**
```bash
ssh -T git@gitlab.com
```

**Pour Bitbucket :**
```bash
ssh -T git@bitbucket.org
```

Si vous voyez un message de bienvenue avec votre nom d'utilisateur, c'est réussi !

**Première connexion :**

Lors de la première connexion, vous verrez :
```
The authenticity of host 'github.com (xxx.xxx.xxx.xxx)' can't be established.
ED25519 key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no)?
```

Tapez `yes` et appuyez sur Entrée.

#### Utiliser SSH avec Git

**Cloner avec SSH :**
```bash
git clone git@github.com:utilisateur/projet.git
```

**Changer une URL HTTPS existante en SSH :**
```bash
git remote set-url origin git@github.com:utilisateur/projet.git
```

Vérifiez :
```bash
git remote -v
```

Maintenant, `git push` et `git pull` utiliseront SSH automatiquement, sans demander de token !

### Comparaison HTTPS (PAT) vs SSH

| Critère | HTTPS + PAT | SSH |
|---------|-------------|-----|
| **Configuration initiale** | Simple | Plus technique |
| **Facilité d'usage** | Token à gérer | Transparent après config |
| **Sécurité** | Très sécurisé | Très sécurisé |
| **Compatibilité firewall** | Excellente | Peut être bloqué |
| **Authentification** | À chaque fois (ou stockée) | Automatique |
| **Gestion multiple comptes** | Un token par compte | Une clé par compte |
| **Recommandé pour** | Débuter, usage occasionnel | Usage quotidien intensif |

### Quelle méthode choisir ?

**Choisissez HTTPS + PAT si :**
- Vous débutez avec Git
- Vous travaillez derrière un firewall strict (entreprise)
- Vous voulez une solution simple rapidement
- Vous utilisez Git occasionnellement

**Choisissez SSH si :**
- Vous utilisez Git quotidiennement
- Vous voulez éviter de gérer des tokens
- Vous êtes à l'aise avec le terminal
- Vous travaillez sur plusieurs projets

**Vous pouvez aussi utiliser les deux !**
HTTPS sur un ordinateur, SSH sur l'autre. Git s'adapte en fonction de l'URL du remote.

### Gérer plusieurs comptes

**Scénario :** Vous avez un compte GitHub personnel et un compte professionnel.

#### Avec HTTPS (PAT)

Créez un token différent pour chaque compte et utilisez le bon lors de l'authentification.

#### Avec SSH (recommandé)

Créez une clé différente pour chaque compte :

```bash
# Clé personnelle
ssh-keygen -t ed25519 -C "perso@email.com" -f ~/.ssh/id_ed25519_perso

# Clé professionnelle
ssh-keygen -t ed25519 -C "pro@email.com" -f ~/.ssh/id_ed25519_pro
```

Créez un fichier de configuration SSH `~/.ssh/config` :

```
# Compte personnel
Host github.com-perso
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_perso

# Compte professionnel
Host github.com-pro
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_pro
```

Utilisez les URLs correspondantes :

```bash
# Projet personnel
git clone git@github.com-perso:utilisateur-perso/projet.git

# Projet professionnel
git clone git@github.com-pro:utilisateur-pro/projet.git
```

### Résoudre les problèmes d'authentification

#### "Permission denied (publickey)"

**Avec SSH :**

Vérifications :
1. La clé publique est bien ajoutée sur GitHub/GitLab ?
2. L'agent SSH tourne : `eval "$(ssh-agent -s)"`
3. La clé est chargée : `ssh-add ~/.ssh/id_ed25519`
4. Test de connexion : `ssh -T git@github.com`

#### "Authentication failed" ou "Invalid username or password"

**Avec HTTPS :**

Causes possibles :
- Vous utilisez votre mot de passe au lieu du token
- Le token a expiré
- Le token n'a pas les bonnes permissions
- Le token a été révoqué

Solution : Générez un nouveau token et réessayez.

#### "Could not read from remote repository"

Vérifiez :
1. L'URL du remote : `git remote -v`
2. Vos permissions sur le dépôt
3. Que le dépôt existe bien

### Sécurité et bonnes pratiques

#### Pour les Personal Access Tokens

**Ne commitez JAMAIS un token dans votre code**
```bash
# ❌ MAUVAIS
API_TOKEN = "ghp_1234567890abcdef"
```

**Utilisez des variables d'environnement**
```bash
# ✅ BON
API_TOKEN = os.getenv("GITHUB_TOKEN")
```

**Définissez une expiration**
Préférez des tokens avec une durée de vie limitée (30-90 jours).

**Donnez les permissions minimales**
Ne cochez que les scopes nécessaires, pas plus.

**Révoquez les tokens inutilisés**
Allez régulièrement dans vos paramètres supprimer les vieux tokens.

**Un token par usage**
Créez différents tokens pour différents projets ou machines.

#### Pour les clés SSH

**Protégez votre clé privée**
Ne partagez JAMAIS votre clé privée (`id_ed25519` sans `.pub`).

**Utilisez une phrase de passe**
Ajoutez une couche de sécurité supplémentaire à vos clés.

**Permissions correctes des fichiers**
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

**Sauvegardez vos clés (sécurisé)**
Gardez une copie chiffrée de vos clés dans un endroit sûr.

**Utilisez des clés différentes par machine**
Créez une paire de clés pour chaque ordinateur. Si une machine est compromise, révoquez seulement cette clé.

### Révoquer l'accès

#### Révoquer un PAT sur GitHub

1. Settings > Developer settings > Personal access tokens
2. Trouvez le token dans la liste
3. Cliquez sur "Delete"
4. Confirmez

#### Révoquer une clé SSH sur GitHub

1. Settings > SSH and GPG keys
2. Trouvez la clé dans la liste
3. Cliquez sur "Delete"
4. Confirmez

Faites de même sur GitLab et Bitbucket si nécessaire.

### Résumé des commandes

**Personal Access Token :**
```bash
# Configurer le stockage d'identifiants
git config --global credential.helper cache

# Cloner avec HTTPS
git clone https://github.com/utilisateur/projet.git

# Username: votre-nom
# Password: votre-token (pas votre mot de passe !)
```

**SSH :**
```bash
# Générer une clé
ssh-keygen -t ed25519 -C "email@example.com"

# Copier la clé publique
cat ~/.ssh/id_ed25519.pub

# Tester la connexion
ssh -T git@github.com

# Cloner avec SSH
git clone git@github.com:utilisateur/projet.git

# Changer HTTPS vers SSH
git remote set-url origin git@github.com:utilisateur/projet.git
```

### Ce qu'il faut retenir

L'authentification est essentielle pour sécuriser vos dépôts distants. Vous avez deux options principales :

**HTTPS + PAT** : Simple à configurer, parfait pour débuter. Nécessite de gérer des tokens.

**SSH** : Configuration initiale plus technique, mais ensuite totalement transparent. Idéal pour un usage quotidien.

Les deux méthodes sont sécurisées. Commencez par HTTPS si vous débutez, puis migrez vers SSH quand vous serez plus à l'aise.

**Points de sécurité critiques :**
- Ne partagez jamais vos tokens ou clés privées
- Utilisez des phrases de passe pour vos clés SSH
- Donnez les permissions minimales nécessaires
- Révoquez les accès inutilisés régulièrement

Avec une authentification correctement configurée, vous pouvez collaborer en toute sécurité sur vos projets Git !

⏭️ [Fork et Pull Request (PR)](/module-05-git-a-plusieurs/08-fork-et-pull-request.md)
