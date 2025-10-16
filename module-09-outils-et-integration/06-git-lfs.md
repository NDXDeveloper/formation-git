🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 9 : Outils et intégration

## 6. Git LFS pour les gros fichiers

### Introduction

Imaginez que vous travaillez sur un jeu vidéo, une application de design, ou un projet de machine learning. Votre dépôt Git contient des images haute résolution, des vidéos, des modèles 3D ou des datasets de plusieurs gigaoctets. Vous faites un `git clone` et... vous attendez. Longtemps. Très longtemps. Et quand c'est enfin terminé, votre dossier `.git` fait 10 Go !

C'est là que **Git LFS** (Large File Storage) entre en jeu. C'est une extension de Git spécialement conçue pour gérer les gros fichiers de manière efficace.

Dans cette section, nous allons découvrir :
- Pourquoi Git n'est pas adapté aux gros fichiers par défaut
- Comment fonctionne Git LFS
- Comment l'installer et l'utiliser
- Quand l'utiliser (et quand ne pas l'utiliser)
- Les alternatives à Git LFS

**Note pour les débutants :** Si vous travaillez uniquement avec du code (fichiers texte, HTML, CSS, JavaScript, Python, etc.), vous n'aurez probablement jamais besoin de Git LFS. Mais si vous manipulez des images, vidéos, fichiers audio, modèles 3D, ou gros datasets, cette section est pour vous !

---

### Le problème : Git et les gros fichiers

#### Comment Git stocke les fichiers

Git n'a pas été conçu pour les gros fichiers. Voici pourquoi :

**Chaque version est conservée**
Quand vous modifiez un fichier et que vous faites un commit, Git conserve TOUTES les versions :
```
video.mp4 (version 1) - 500 Mo
video.mp4 (version 2) - 500 Mo  ← légèrement modifiée
video.mp4 (version 3) - 500 Mo  ← encore modifiée
```

Résultat : 1.5 Go dans votre dépôt pour un seul fichier !

**Analogie :**
Imaginez que vous écrivez un livre. Avec Git, c'est comme si vous gardiez une photocopie complète du livre à chaque fois que vous changez un seul mot. Après 100 révisions, vous avez 100 copies complètes du livre !

Pour du code (fichiers texte), ce n'est pas un problème car :
- Les fichiers sont petits (quelques Ko)
- Git utilise des algorithmes de compression intelligents
- Git ne stocke que les différences (delta compression)

Mais pour une vidéo de 500 Mo, la compression ne peut pas faire de miracle.

#### Les problèmes concrets

**1. Clonage très lent**
```bash
git clone https://github.com/username/projet-avec-videos.git
# Téléchargement de 10 Go... ⏳⏳⏳
# Durée estimée : 2 heures
```

**2. Opérations Git lentes**
Chaque commande Git doit parcourir l'historique complet :
```bash
git status   # Lent
git log      # Lent
git diff     # Très lent
```

**3. Espace disque énorme**
Votre dossier `.git` devient gigantesque, même pour un petit projet.

**4. Limites des plateformes**
- GitHub limite les fichiers à 100 Mo
- GitLab limite les dépôts à quelques Go (plan gratuit)
- Votre réseau pleure quand vous faites un push

**5. Versions inutiles**
Vous avez besoin de la version actuelle de `model.pth` (modèle de machine learning), pas des 50 versions précédentes que vous avez testées pendant le développement !

---

### La solution : Git LFS (Large File Storage)

#### Qu'est-ce que Git LFS ?

Git LFS est une extension de Git qui change la façon dont les gros fichiers sont stockés.

**L'idée principale :**
Au lieu de stocker les gros fichiers directement dans Git, on les stocke ailleurs (sur un serveur de stockage) et on met seulement un "pointeur" dans Git.

**Analogie :**
C'est comme une bibliothèque :
- **Sans LFS :** Vous gardez tous les livres chez vous (encombrant, lourd)
- **Avec LFS :** Vous gardez seulement un catalogue avec les références des livres. Quand vous avez besoin d'un livre, vous allez le chercher à la bibliothèque.

#### Comment ça fonctionne ?

**Sans Git LFS :**
```
Dépôt Git
├── code.js (10 Ko)
├── image.png (5 Mo) ← stockée directement
└── .git/
    └── objects/
        ├── code.js (toutes versions)
        └── image.png (toutes versions) ← Très lourd !
```

**Avec Git LFS :**
```
Dépôt Git
├── code.js (10 Ko)
├── image.png → pointeur (200 octets) ← juste une référence !
└── .git/
    └── lfs/
        └── objects/ ← fichiers LFS (téléchargés à la demande)

Serveur LFS distant
└── image.png (5 Mo) ← Le vrai fichier est ici
```

**Le pointeur ressemble à ça :**
```
version https://git-lfs.github.com/spec/v1
oid sha256:4d7a214614ab2935c943f9e0ff69d22eadbb8f32b1258daaa5e2ca24d17e2393
size 5242880
```

C'est tout ! Juste 3 lignes au lieu de 5 Mo.

#### Les avantages

**Clonage rapide**
```bash
git clone https://github.com/username/projet-avec-videos.git
# Télécharge uniquement les pointeurs (quelques Ko)
# Les gros fichiers sont téléchargés seulement quand nécessaire
```

**Historique léger**
Votre `.git` reste petit. L'historique des gros fichiers est géré par LFS, pas par Git.

**Versions à la demande**
Vous ne téléchargez que la version actuelle des gros fichiers, pas tout l'historique.

**Gestion du bandwidth**
Vous pouvez choisir quels fichiers télécharger.

---

### Installation de Git LFS

#### Sur Windows

**Option 1 : Installeur**
1. Téléchargez l'installeur depuis git-lfs.github.com
2. Exécutez l'installeur
3. Suivez les instructions

**Option 2 : Git pour Windows**
Git LFS est souvent inclus avec Git pour Windows. Vérifiez :
```bash
git lfs version
```

Si c'est installé, vous verrez quelque chose comme :
```
git-lfs/3.4.0 (GitHub; windows amd64; go 1.21.0)
```

#### Sur macOS

**Avec Homebrew (recommandé) :**
```bash
brew install git-lfs
```

**Avec MacPorts :**
```bash
port install git-lfs
```

#### Sur Linux

**Ubuntu/Debian :**
```bash
sudo apt-get install git-lfs
```

**Fedora :**
```bash
sudo dnf install git-lfs
```

**Arch Linux :**
```bash
sudo pacman -S git-lfs
```

#### Configuration initiale (une seule fois)

Après l'installation, configurez Git LFS pour votre utilisateur :
```bash
git lfs install
```

Vous verrez :
```
Updated git hooks.
Git LFS initialized.
```

Cette commande configure les hooks Git nécessaires. Vous ne devez la faire qu'une seule fois par utilisateur (elle modifie votre configuration Git globale).

---

### Utilisation de base de Git LFS

#### Scénario : Nouveau projet avec des images

Vous créez un site web avec des photos haute résolution.

**Étape 1 : Initialiser le dépôt Git**
```bash
mkdir mon-portfolio
cd mon-portfolio
git init
```

**Étape 2 : Configurer LFS pour suivre les images**
```bash
git lfs track "*.png"
git lfs track "*.jpg"
git lfs track "*.jpeg"
```

Cette commande dit à Git LFS : "Tous les fichiers `.png`, `.jpg`, `.jpeg` doivent être gérés par LFS."

**Ce qui se passe :**
Un fichier `.gitattributes` est créé :
```
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
```

**Important :** Ce fichier `.gitattributes` doit être commité !
```bash
git add .gitattributes
git commit -m "Configure Git LFS pour les images"
```

**Étape 3 : Ajouter vos gros fichiers**
```bash
# Ajouter des images
cp ~/Downloads/photo1.jpg images/
cp ~/Downloads/photo2.png images/

# Git LFS les gère automatiquement !
git add images/
git commit -m "Ajouter photos du portfolio"
```

**Étape 4 : Vérifier que LFS fonctionne**
```bash
git lfs ls-files
```

Vous devriez voir :
```
4d7a214614 * images/photo1.jpg
a8b3c52741 * images/photo2.png
```

L'astérisque `*` indique que le fichier est suivi par LFS !

**Étape 5 : Pousser vers GitHub/GitLab**
```bash
git remote add origin https://github.com/username/mon-portfolio.git
git push -u origin main
```

Git LFS pousse automatiquement les gros fichiers vers le serveur LFS !

---

### Commandes principales Git LFS

#### Suivre des types de fichiers

**Suivre par extension :**
```bash
git lfs track "*.psd"          # Fichiers Photoshop
git lfs track "*.mp4"          # Vidéos
git lfs track "*.zip"          # Archives
git lfs track "*.bin"          # Fichiers binaires
```

**Suivre par dossier :**
```bash
git lfs track "assets/videos/**"    # Tous les fichiers dans assets/videos/
git lfs track "datasets/*.csv"      # Tous les CSV dans datasets/
```

**Suivre un fichier spécifique :**
```bash
git lfs track "model_final.pth"     # Un fichier précis
```

#### Voir les fichiers suivis

**Liste des patterns suivis :**
```bash
git lfs track
```

Affiche :
```
Listing tracked patterns
    *.png (.gitattributes)
    *.jpg (.gitattributes)
    *.psd (.gitattributes)
Listing excluded patterns
```

**Liste des fichiers LFS dans le dépôt :**
```bash
git lfs ls-files
```

#### Arrêter de suivre un type de fichier

```bash
git lfs untrack "*.zip"
```

**Attention :** Cela ne convertit pas les fichiers existants ! Ils restent dans LFS. Cette commande affecte seulement les futurs fichiers.

#### Télécharger les fichiers LFS

**Télécharger tous les fichiers LFS :**
```bash
git lfs pull
```

**Télécharger des fichiers spécifiques :**
```bash
git lfs pull --include="images/*.jpg"
```

**Pré-télécharger avant un checkout :**
```bash
git lfs fetch
git checkout autre-branche
```

#### Voir les informations sur LFS

**Vérifier l'installation :**
```bash
git lfs version
```

**Voir l'environnement LFS :**
```bash
git lfs env
```

Affiche la configuration, l'URL du serveur LFS, etc.

**Statistiques :**
```bash
git lfs status
```

---

### Migration de fichiers existants vers LFS

Vous avez déjà un dépôt avec de gros fichiers ? Vous pouvez les migrer vers LFS.

#### Scénario : Projet existant avec des vidéos

Votre dépôt existe déjà, et vous vous rendez compte que les vidéos ralentissent tout.

**Étape 1 : Sauvegarder (important !)**
```bash
# Créer une copie de sécurité
cp -r mon-projet mon-projet-backup
```

**Étape 2 : Installer et configurer LFS**
```bash
git lfs install
git lfs track "*.mp4"
git add .gitattributes
git commit -m "Configure LFS pour les vidéos"
```

**Étape 3 : Migrer l'historique**

**Option A : Migrer uniquement les futurs fichiers (simple)**

Les fichiers déjà commités restent dans Git, mais les nouveaux seront dans LFS :
```bash
# Les vidéos existantes restent dans Git
# Les nouvelles vidéos iront dans LFS
```

**Option B : Migrer tout l'historique (avancé)**

Attention, cela réécrit l'historique !

```bash
git lfs migrate import --include="*.mp4"
```

Cette commande :
- Parcourt tout l'historique
- Convertit tous les `*.mp4` en fichiers LFS
- Réécrit les commits

**Conséquences :**
- Les hash des commits changent
- Vous devez forcer le push : `git push --force`
- Les collaborateurs doivent re-cloner le dépôt

**Étape 4 : Vérifier**
```bash
git lfs ls-files
```

**Étape 5 : Pousser (peut nécessiter --force)**
```bash
git push origin main --force
```

#### Avertissement sur la réécriture d'historique

La migration complète (Option B) est **dangereuse** car elle réécrit l'historique. Ne le faites que si :
- Vous êtes seul sur le projet
- Ou vous coordonnez avec toute l'équipe
- Vous avez des sauvegardes

Pour un dépôt en production avec plusieurs développeurs, privilégiez l'Option A (futurs fichiers seulement).

---

### Cloner un dépôt avec LFS

Quand vous clonez un dépôt qui utilise LFS, deux options :

#### Clone standard (recommandé)

```bash
git clone https://github.com/username/projet.git
```

Git LFS télécharge automatiquement tous les fichiers LFS de la branche actuelle.

#### Clone sans les fichiers LFS

Si vous voulez cloner rapidement sans télécharger les gros fichiers :

```bash
GIT_LFS_SKIP_SMUDGE=1 git clone https://github.com/username/projet.git
```

Les fichiers LFS ne sont pas téléchargés. Vous voyez les pointeurs à la place.

**Télécharger ensuite :**
```bash
cd projet
git lfs pull
```

**Ou télécharger seulement certains fichiers :**
```bash
git lfs pull --include="images/*.jpg"
```

---

### Cas d'usage : Quand utiliser Git LFS ?

#### ✅ Utilisez LFS pour :

**1. Assets de jeux vidéo**
- Textures (PNG, JPG)
- Modèles 3D (FBX, OBJ)
- Sons et musiques (WAV, MP3)
- Vidéos de cinématiques (MP4)

**2. Design et médias**
- Fichiers Photoshop (PSD)
- Fichiers Illustrator (AI)
- Fichiers vidéo (MP4, MOV)
- Fichiers audio (WAV, FLAC)

**3. Machine Learning**
- Modèles entraînés (`.pth`, `.h5`, `.pkl`)
- Datasets (gros CSV, images)
- Fichiers de poids (`.weights`)

**4. Documentation**
- PDFs volumineux
- Vidéos de démonstration
- Images haute résolution

**Règle générale :** Si un fichier fait plus de 1-5 Mo et change rarement, considérez LFS.

#### ❌ N'utilisez PAS LFS pour :

**1. Code source**
- JavaScript, Python, HTML, CSS, etc.
- Fichiers de configuration
- README, documentation texte

**2. Fichiers texte en général**
Git gère très bien les fichiers texte, même gros.

**3. Fichiers qui changent constamment**
Si un gros fichier change à chaque commit, LFS n'aidera pas beaucoup. Le problème est la fréquence de changement, pas la taille.

**4. Fichiers générés**
- Bundles JavaScript (`dist/bundle.js`)
- Fichiers compilés
- Dépendances (`node_modules/`)

Ces fichiers ne devraient pas être dans Git du tout (utilisez `.gitignore`).

---

### Git LFS sur différentes plateformes

#### GitHub

**Quota :**
- 1 Go de stockage gratuit
- 1 Go de bandwidth par mois gratuit
- Au-delà : achat de "data packs" (5$ pour 50 Go)

**Configuration :**
Aucune configuration spéciale. LFS fonctionne automatiquement.

**Limites :**
- 2 Go maximum par fichier

#### GitLab

**Quota :**
- 10 Go de stockage gratuit (plan gratuit)
- Bandwidth inclus

**Configuration :**
LFS activé par défaut sur GitLab.com

**Limites :**
- Limite de taille par fichier : configurable par l'administrateur

#### Bitbucket

**Quota :**
- 1 Go de stockage gratuit
- 1 Go de bandwidth par mois gratuit
- Au-delà : forfaits payants

**Configuration :**
LFS doit être activé dans les paramètres du dépôt.

#### Auto-hébergement (GitLab self-hosted, Gitea, etc.)

Vous devez configurer le serveur LFS vous-même. Plusieurs options :
- Stockage local sur le serveur
- Stockage cloud (S3, Azure Blob, etc.)

---

### Alternatives à Git LFS

Git LFS n'est pas la seule solution. Voici les alternatives :

#### Git Annex

**Ce que c'est :**
Alternative à LFS, plus ancienne et plus flexible.

**Avantages :**
- Plus de contrôle sur où stocker les fichiers
- Peut gérer des fichiers sur plusieurs emplacements (S3, serveur local, disque externe)
- Pas de limitations de quota

**Inconvénients :**
- Plus complexe à utiliser
- Moins de support sur les plateformes (GitHub, GitLab)

**Quand l'utiliser :**
Projets académiques ou scientifiques avec des besoins très spécifiques.

#### DVC (Data Version Control)

**Ce que c'est :**
Outil conçu spécifiquement pour les projets de machine learning et data science.

**Avantages :**
- Gestion des datasets et modèles
- Pipelines de traitement de données
- Gestion des expériences ML
- Support de S3, Google Cloud Storage, Azure, etc.

**Inconvénients :**
- Outil séparé (pas intégré à Git)
- Courbe d'apprentissage

**Quand l'utiliser :**
Projets de machine learning avec de gros datasets et modèles.

**Exemple :**
```bash
# Initialiser DVC
dvc init

# Suivre un gros fichier
dvc add dataset.csv

# Pousser vers le stockage distant
dvc push
```

#### Ne pas versionner du tout

**Option : Stockage externe**
Parfois, la meilleure solution est de ne PAS versionner les gros fichiers :
- Télécharger depuis une URL publique
- Utiliser un service de stockage (Dropbox, Google Drive)
- Générer les fichiers localement (si possible)

**Exemple avec un script de téléchargement :**
```bash
# download_assets.sh
#!/bin/bash
echo "Téléchargement des assets..."
curl -o images/banner.jpg https://example.com/assets/banner.jpg
curl -o models/model.pth https://example.com/models/model.pth
echo "Téléchargement terminé !"
```

**README.md :**
```markdown
## Installation

1. Cloner le dépôt
2. Exécuter `./download_assets.sh`
3. Lancer l'application
```

---

### Bonnes pratiques avec Git LFS

#### 1. Configurer LFS dès le début du projet

Ne migrez pas vers LFS au milieu d'un projet avec beaucoup d'historique. Commencez avec LFS si vous savez que vous aurez de gros fichiers.

#### 2. Commiter .gitattributes

Le fichier `.gitattributes` doit être versionné :
```bash
git add .gitattributes
git commit -m "Configure Git LFS"
```

Sans ce fichier, les autres contributeurs ne sauront pas quels fichiers sont gérés par LFS.

#### 3. Documenter l'utilisation de LFS

Dans votre README :
```markdown
## Prérequis

Ce projet utilise Git LFS pour les gros fichiers.

### Installation de Git LFS
- macOS: `brew install git-lfs`
- Windows: Télécharger depuis git-lfs.github.com
- Linux: `sudo apt-get install git-lfs`

### Après le clone
```bash
git lfs install
git lfs pull
```
```

#### 4. Surveiller vos quotas

Les plateformes ont des limites de stockage et bandwidth. Surveillez votre utilisation :

**GitHub :**
Settings → Billing → Git LFS Data

**GitLab :**
Settings → Usage Quotas

#### 5. Exclure les fichiers générés

Ne mettez pas dans LFS des fichiers qui peuvent être régénérés :
```bash
# ❌ Ne faites pas ça
git lfs track "dist/*.js"  # Fichiers de build

# ✅ Mettez-les dans .gitignore
echo "dist/" >> .gitignore
```

#### 6. Utiliser .lfsconfig pour la configuration d'équipe

Créez un fichier `.lfsconfig` pour partager la configuration :
```ini
[lfs]
    url = https://lfs.example.com/project.git/info/lfs
```

Commitez ce fichier pour que toute l'équipe utilise la même configuration.

#### 7. Nettoyer régulièrement

Supprimez les anciens objets LFS dont vous n'avez plus besoin :
```bash
git lfs prune
```

Cela supprime les fichiers LFS qui ne sont plus référencés par aucun commit récent.

---

### Dépannage Git LFS

#### Problème : "Git LFS is not installed"

**Symptômes :**
```
git-lfs filter-process: git-lfs: command not found
error: external filter 'git-lfs filter-process' failed
```

**Solution :**
Installez Git LFS et configurez-le :
```bash
# Installer (selon votre OS)
brew install git-lfs  # macOS
sudo apt-get install git-lfs  # Ubuntu

# Configurer
git lfs install
```

#### Problème : Les fichiers LFS ne se téléchargent pas

**Symptômes :**
Vous voyez le contenu du pointeur au lieu du fichier :
```
version https://git-lfs.github.com/spec/v1
oid sha256:4d7a...
size 5242880
```

**Solution :**
```bash
# Télécharger les fichiers LFS
git lfs pull

# Ou réinstaller les hooks
git lfs install --force
```

#### Problème : "This repository is over its data quota"

**Symptômes :**
```
Remote "origin" does not support the LFS locking API. Consider disabling it with:
  $ git config lfs.https://github.com/user/repo.git/info/lfs.locksverify false
batch response: This repository is over its data quota...
```

**Solution :**
Vous avez dépassé le quota de votre plan. Options :
1. Acheter plus d'espace
2. Supprimer d'anciens fichiers LFS
3. Nettoyer avec `git lfs prune`

#### Problème : Push très lent

**Symptômes :**
Le push semble bloqué sur "Uploading LFS objects"

**Solutions :**
1. Vérifier votre connexion Internet
2. Utiliser un réseau plus rapide
3. Pousser en plusieurs fois :
```bash
# Pousser seulement les objets Git
git push origin main --no-verify

# Pousser les objets LFS séparément
git lfs push origin main
```

#### Problème : Fichier trop gros pour LFS

**Symptômes :**
```
remote: error: File model.bin is 2.50 GB; this exceeds GitHub's file size limit of 2.00 GB
```

**Solutions :**
1. Compresser le fichier
2. Diviser le fichier en plusieurs parties
3. Utiliser un service de stockage externe (S3, Google Drive)
4. Utiliser DVC au lieu de LFS

---

### Commandes avancées

#### Voir l'historique d'un fichier LFS

```bash
git log --follow -- path/to/file.jpg
```

#### Comparer deux versions d'un fichier LFS

```bash
# Impossible de voir le diff d'un fichier binaire
# Mais vous pouvez voir les métadonnées
git log -p -- path/to/file.jpg
```

#### Télécharger l'historique complet d'un fichier

```bash
# Télécharger toutes les versions
git lfs fetch --all
```

#### Lister les fichiers LFS par taille

```bash
git lfs ls-files -l | sort -k3 -n -r | head -10
```

Affiche les 10 plus gros fichiers LFS.

#### Pousser seulement certains fichiers LFS

```bash
git lfs push origin main --include="images/*.jpg"
```

---

### Coût et considérations budgétaires

#### GitHub

**Plan gratuit :**
- 1 Go de stockage LFS
- 1 Go de bandwidth par mois

**Data packs :**
- 50 Go de stockage + 50 Go de bandwidth : 5$/mois
- Multipliable (10 data packs = 500 Go pour 50$/mois)

**Calcul du coût :**
```
Exemple : 100 images de 2 Mo chacune
Stockage : 200 Mo (OK pour le plan gratuit)

5 contributeurs clonent le dépôt par mois
Bandwidth : 5 × 200 Mo = 1 Go (OK pour le plan gratuit)

10 contributeurs clonent le dépôt
Bandwidth : 2 Go (nécessite 1 data pack : 5$/mois)
```

#### Alternatives gratuites ou moins chères

**GitLab :**
- 10 Go gratuits (10x plus que GitHub)
- Meilleure option pour les projets open-source avec beaucoup de médias

**Auto-hébergement :**
- Serveur GitLab + stockage S3
- Coût uniquement du serveur et du stockage
- Contrôle total

---

### Ressources complémentaires

#### Documentation officielle

- **Git LFS :** git-lfs.github.com
- **GitHub et LFS :** docs.github.com/en/repositories/working-with-files/managing-large-files
- **GitLab et LFS :** docs.gitlab.com/ee/topics/git/lfs/

#### Tutoriels

- Atlassian Git LFS Tutorial : atlassian.com/git/tutorials/git-lfs
- GitHub Guide : git-lfs.github.com

#### Outils

- **BFG Repo-Cleaner :** Nettoyer l'historique (reclaimator.org)
- **git-filter-repo :** Alternative à BFG (github.com/newren/git-filter-repo)

---

### Conclusion

Git LFS est un outil puissant pour gérer les gros fichiers binaires dans Git. Il résout élégamment le problème des fichiers volumineux qui ralentissent et encombrent votre dépôt.

**Points clés à retenir :**

✅ **Git seul n'est pas adapté aux gros fichiers** - Chaque version est conservée, ce qui fait exploser la taille du dépôt

✅ **LFS utilise des pointeurs** - Les gros fichiers sont stockés séparément, seule une référence est dans Git

✅ **Installation simple** - `brew install git-lfs` puis `git lfs install`

✅ **Configuration facile** - `git lfs track "*.psd"` pour commencer à suivre des fichiers

✅ **Transparence** - Une fois configuré, LFS fonctionne automatiquement avec vos commandes Git habituelles

✅ **Quotas à surveiller** - Les plateformes ont des limites de stockage et bandwidth

✅ **Alternatives existent** - DVC pour ML, Git Annex pour des besoins spécifiques, ou stockage externe

**Recommandations finales :**

- **Petits projets avec quelques images** : LFS est parfait
- **Jeux vidéo / Design** : LFS est indispensable
- **Machine Learning** : Considérez DVC en plus de LFS
- **Fichiers énormes (>2 Go)** : Stockage externe + script de téléchargement

Git LFS n'est pas magique - il ne rend pas les gros fichiers petits - mais il permet de les gérer intelligemment sans compromettre l'efficacité de Git pour le reste de votre projet.

Avec ces connaissances, vous êtes équipé pour gérer n'importe quel type de fichier dans vos projets Git, des simples fichiers texte aux datasets de plusieurs gigaoctets !

⏭️ [Module 10 : Cas pratiques et projets](/module-10-cas-pratiques/README.md)
