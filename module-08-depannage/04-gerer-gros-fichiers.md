🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 8 : Dépannage et résolution de problèmes

## 4. Gérer les gros fichiers et nettoyer le dépôt

### Introduction : Pourquoi les gros fichiers sont un problème

Git est conçu pour gérer du **code source** : des fichiers texte généralement petits (quelques Ko à quelques centaines de Ko). Quand vous ajoutez des **gros fichiers** (images haute résolution, vidéos, bases de données, fichiers compilés), plusieurs problèmes apparaissent :

1. **Clonage lent** : Chaque personne qui clone le dépôt doit télécharger tout l'historique, y compris tous les gros fichiers
2. **Opérations Git ralenties** : Les commits, pushs et pulls deviennent de plus en plus longs
3. **Espace disque** : Le dépôt peut rapidement atteindre plusieurs Go
4. **Limitations des plateformes** : GitHub, GitLab ont des limites (souvent 100 Mo par fichier)

### Partie 1 : Identifier les gros fichiers

#### Vérifier la taille de votre dépôt

```bash
# Voir la taille totale du dossier .git
du -sh .git
```

Exemple de résultat :
```
450M    .git
```

Si cette taille dépasse 100-200 Mo, il est temps d'enquêter.

#### Trouver les gros fichiers dans votre répertoire de travail

```bash
# Linux/macOS : Lister les fichiers par taille
find . -type f -size +10M -exec ls -lh {} \;

# Ou plus lisible avec du
du -ah . | sort -rh | head -20
```

Sur Windows avec Git Bash :
```bash
find . -type f -size +10M
```

#### Trouver les gros fichiers dans l'historique Git

C'est ici que ça devient intéressant : un fichier peut ne plus être dans votre répertoire de travail, mais rester dans l'historique Git et occuper de l'espace.

**Script pour trouver les plus gros fichiers de l'historique :**

```bash
git rev-list --objects --all |
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' |
  sed -n 's/^blob //p' |
  sort --numeric-sort --key=2 --reverse |
  head -20 |
  cut -c 1-12,41- |
  numfmt --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest
```

Résultat typique :
```
5f3a8b9c1d2e    15.2MiB presentation.pptx
7e4f2a1b8c9d    12.8MiB video.mp4
9d8c7b6a5e4f    8.5MiB dataset.csv
```

**Version simplifiée (moins précise mais plus facile) :**

```bash
git ls-tree -r -t -l --full-name HEAD | sort -n -k 4 | tail -20
```

### Partie 2 : Prévenir l'ajout de gros fichiers

#### Utiliser .gitignore correctement

La **meilleure solution** est de ne jamais commiter les gros fichiers en premier lieu. Utilisez `.gitignore` pour les exclure :

```gitignore
# Fichiers compilés
*.exe
*.dll
*.so
*.o
*.class

# Archives
*.zip
*.tar.gz
*.rar

# Médias
*.mp4
*.mov
*.avi
*.psd
*.ai

# Bases de données
*.db
*.sqlite
*.sql

# Logs et dumps
*.log
*.dump

# Dossiers de build
build/
dist/
node_modules/
target/

# Fichiers de données volumineux
data/large/
*.csv
```

**Important :** `.gitignore` ne fonctionne que pour les fichiers **non encore trackés**. Si vous avez déjà commité un fichier, l'ajouter au `.gitignore` ne le supprimera pas de l'historique.

#### Vérifier avant de commiter

Avant chaque commit, vérifiez ce que vous ajoutez :

```bash
# Voir la taille des fichiers staged
git diff --cached --stat

# Ou plus détaillé
git diff --cached --numstat
```

### Partie 3 : Git LFS (Large File Storage)

#### Qu'est-ce que Git LFS ?

**Git LFS** (Large File Storage) est une extension de Git qui remplace les gros fichiers par des **pointeurs** dans votre dépôt, et stocke le contenu réel sur un serveur séparé.

**Avantages :**
- Dépôt léger et rapide
- Les gros fichiers sont téléchargés seulement quand nécessaire
- Compatible avec GitHub, GitLab, Bitbucket

**Inconvénients :**
- Nécessite une installation supplémentaire
- Peut avoir des coûts sur certaines plateformes (quotas de bande passante)
- Plus complexe à configurer

#### Installation de Git LFS

```bash
# Sur macOS avec Homebrew
brew install git-lfs

# Sur Ubuntu/Debian
sudo apt-get install git-lfs

# Sur Windows
# Télécharger depuis : https://git-lfs.github.com/
```

Puis initialiser LFS dans votre système :
```bash
git lfs install
```

#### Utiliser Git LFS

**Étape 1 : Spécifier quels types de fichiers utiliser avec LFS**

```bash
# Tracker tous les fichiers PSD
git lfs track "*.psd"

# Tracker tous les fichiers dans un dossier
git lfs track "videos/**"

# Tracker un fichier spécifique
git lfs track "large-dataset.csv"
```

Cela crée/modifie un fichier `.gitattributes` qui doit être commité :

```bash
git add .gitattributes
git commit -m "Configure Git LFS"
```

**Étape 2 : Ajouter vos fichiers normalement**

```bash
git add mon-gros-fichier.psd
git commit -m "Ajout du design"
git push
```

Git LFS s'occupe automatiquement de tout !

**Vérifier quels fichiers sont trackés par LFS :**

```bash
git lfs ls-files
```

#### Migrer des fichiers existants vers LFS

Si vous avez déjà commité des gros fichiers et voulez les migrer vers LFS :

```bash
# Migrer tous les fichiers PSD de l'historique
git lfs migrate import --include="*.psd" --everything
```

**⚠️ Attention :** Cette commande réécrit l'historique ! Coordonnez avec votre équipe avant de faire ça.

### Partie 4 : Supprimer un fichier de l'historique

#### Cas d'usage

Vous devez supprimer un fichier de tout l'historique Git si :
- Vous avez accidentellement commité un gros fichier
- Vous avez commité des données sensibles (mots de passe, clés API)
- Vous voulez réduire la taille du dépôt

#### Méthode 1 : git filter-branch (ancienne méthode)

**⚠️ Cette méthode est obsolète mais encore largement utilisée.**

```bash
# Supprimer un fichier spécifique de tout l'historique
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch chemin/vers/gros-fichier.zip" \
  --prune-empty --tag-name-filter cat -- --all
```

**Explication :**
- `--index-filter` : Modifie l'index pour chaque commit
- `git rm --cached` : Supprime le fichier de l'index
- `--ignore-unmatch` : Ne pas échouer si le fichier n'existe pas dans certains commits
- `--prune-empty` : Supprimer les commits qui deviennent vides
- `-- --all` : Appliquer à toutes les branches et tags

#### Méthode 2 : BFG Repo-Cleaner (recommandée)

**BFG** est un outil spécialement conçu pour nettoyer les dépôts Git. Il est **beaucoup plus rapide** que `filter-branch`.

**Installation :**
```bash
# Télécharger depuis https://rtyley.github.io/bfg-repo-cleaner/
# Ou avec Homebrew sur macOS
brew install bfg
```

**Utilisation :**

```bash
# Cloner le dépôt avec son historique complet
git clone --mirror git@github.com:user/repo.git

# Supprimer tous les fichiers de plus de 100M
bfg --strip-blobs-bigger-than 100M repo.git

# Ou supprimer un fichier spécifique
bfg --delete-files gros-fichier.zip repo.git

# Nettoyer le dépôt
cd repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Pousser les changements
git push
```

**Avantages de BFG :**
- 10 à 720 fois plus rapide que `filter-branch`
- Plus simple à utiliser
- Protège automatiquement votre dernier commit

#### Méthode 3 : git filter-repo (méthode moderne)

**git filter-repo** est l'outil moderne recommandé par Git lui-même.

**Installation :**
```bash
# Sur macOS
brew install git-filter-repo

# Sur Linux
pip3 install git-filter-repo
```

**Utilisation :**

```bash
# Supprimer un fichier spécifique
git filter-repo --path chemin/vers/fichier.zip --invert-paths

# Supprimer tous les fichiers d'un certain type
git filter-repo --path-glob '*.zip' --invert-paths

# Supprimer tous les fichiers d'un dossier
git filter-repo --path dossier-volumineux/ --invert-paths
```

**Avantages :**
- Très rapide
- Recommandé officiellement par Git
- Syntaxe claire et moderne

#### Après la suppression : Pousser les changements

Après avoir nettoyé l'historique, vous devez forcer le push :

```bash
git push origin --force --all
git push origin --force --tags
```

**⚠️ Avertissements importants :**

1. **Prévenez votre équipe** : Tous les collaborateurs devront re-cloner le dépôt
2. **Faites une sauvegarde** : Gardez une copie de l'ancien dépôt au cas où
3. **Coordonnez-vous** : Choisissez un moment où personne ne travaille activement
4. **Pull requests ouvertes** : Elles devront être recréées

### Partie 5 : Nettoyer le dépôt local

#### Comprendre le garbage collection

Même après avoir supprimé des fichiers, Git garde les objets "orphelins" pendant un certain temps. Le **garbage collection** (ramasse-miettes) nettoie ces objets.

#### Nettoyage automatique

Git fait un nettoyage automatique de temps en temps, mais vous pouvez le déclencher manuellement :

```bash
# Nettoyage standard
git gc
```

Cette commande :
- Compresse les objets
- Supprime les objets inaccessibles (après une période de grâce)
- Optimise le dépôt

#### Nettoyage agressif

Pour un nettoyage plus profond (utile après avoir supprimé de gros fichiers) :

```bash
git gc --aggressive --prune=now
```

**Options :**
- `--aggressive` : Optimisation plus approfondie (mais plus lente)
- `--prune=now` : Supprime immédiatement les objets inaccessibles (au lieu d'attendre 2 semaines)

**⚠️ Attention :** `--prune=now` supprime définitivement les objets. Vous ne pourrez plus les récupérer avec `reflog`.

#### Vérifier l'intégrité du dépôt

Avant et après un nettoyage, vérifiez que tout va bien :

```bash
git fsck --full
```

Cette commande vérifie :
- L'intégrité de tous les objets
- Les connexions entre objets
- Les corruptions éventuelles

#### Nettoyer les références obsolètes

Supprimer les références aux branches distantes qui n'existent plus :

```bash
# Voir les branches distantes obsolètes
git remote prune origin --dry-run

# Les supprimer
git remote prune origin
```

### Partie 6 : Réduire la taille du dépôt

#### Workflow complet de nettoyage

Voici un processus complet pour réduire significativement la taille d'un dépôt :

```bash
# 1. Identifier les gros fichiers (voir Partie 1)
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -10 | awk '{print$1}')"

# 2. Supprimer ces fichiers de l'historique (choisir une méthode)
git filter-repo --path chemin/fichier --invert-paths

# 3. Expirer le reflog
git reflog expire --expire=now --all

# 4. Nettoyer agressivement
git gc --prune=now --aggressive

# 5. Vérifier la nouvelle taille
du -sh .git

# 6. Forcer le push
git push origin --force --all
```

#### Shallow clone pour les grands dépôts

Si vous voulez juste travailler sur un grand dépôt sans tout son historique :

```bash
# Cloner seulement les 10 derniers commits
git clone --depth 10 https://github.com/user/repo.git

# Cloner seulement une branche
git clone --single-branch --branch main https://github.com/user/repo.git
```

**Avantages :**
- Téléchargement beaucoup plus rapide
- Occupe moins d'espace

**Inconvénients :**
- Historique incomplet
- Certaines opérations Git sont limitées

#### Récupérer l'historique complet plus tard

Si vous avez fait un shallow clone et voulez l'historique complet :

```bash
# Récupérer tout l'historique
git fetch --unshallow
```

### Partie 7 : Bonnes pratiques

#### Ce qu'il faut faire ✅

1. **Utiliser .gitignore dès le début** du projet
2. **Vérifier la taille** des fichiers avant de les ajouter
3. **Utiliser Git LFS** pour les fichiers multimédias/binaires
4. **Nettoyer régulièrement** avec `git gc`
5. **Documenter** quels fichiers ne doivent pas être commitées
6. **Former l'équipe** sur les bonnes pratiques

#### Ce qu'il faut éviter ❌

1. **Ne pas commiter** :
   - Fichiers de build (binaires compilés)
   - Dépendances (node_modules, vendor, etc.)
   - Fichiers générés automatiquement
   - Médias non sources (vidéos finales, exports, etc.)
   - Bases de données
   - Fichiers temporaires et logs

2. **Ne pas faire de `git push --force`** sans coordination
3. **Ne pas utiliser `--prune=now`** sans sauvegarde

#### Tailles recommandées

- **Fichier individuel** : < 5 Mo (idéal), < 50 Mo (acceptable), > 100 Mo (Git LFS requis)
- **Dépôt total** : < 1 Go (bon), < 5 Go (acceptable), > 10 Go (problématique)
- **Commit individuel** : < 100 Mo pour tous les fichiers ajoutés

### Cas pratique : Résoudre "file is over 100 MB"

**Erreur courante sur GitHub :**
```
remote: error: File video.mp4 is 150.00 MB; this exceeds GitHub's file size limit of 100.00 MB
```

**Solutions :**

**Solution 1 : Annuler le commit et utiliser .gitignore**
```bash
# Annuler le dernier commit (garde les fichiers)
git reset --soft HEAD~1

# Ajouter au .gitignore
echo "video.mp4" >> .gitignore

# Recommiter sans le gros fichier
git add .gitignore
git commit -m "Ajout de video.mp4 au .gitignore"
```

**Solution 2 : Utiliser Git LFS**
```bash
# Installer et configurer Git LFS
git lfs install
git lfs track "*.mp4"

# Ajouter .gitattributes
git add .gitattributes
git commit -m "Configure Git LFS pour les vidéos"

# Push
git push
```

**Solution 3 : Supprimer de l'historique**
```bash
# Supprimer le fichier de tous les commits
git filter-repo --path video.mp4 --invert-paths

# Push avec force
git push --force
```

### Outils utiles

#### Commandes de diagnostic

```bash
# Taille du dépôt
du -sh .git

# Nombre d'objets dans le dépôt
git count-objects -vH

# Voir les packs et leur taille
ls -lh .git/objects/pack/
```

#### Scripts helper

**Lister les 10 plus gros fichiers :**
```bash
git rev-list --objects --all |
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' |
  awk '/^blob/ {print substr($0,6)}' |
  sort --numeric-sort --key=2 --reverse |
  head -10
```

### Points clés à retenir

- **Git n'est pas fait pour les gros fichiers** : Utilisez Git LFS ou stockez ailleurs
- **Le .gitignore est votre premier rempart** : Configurez-le dès le début
- **Nettoyez régulièrement** : `git gc` pour optimiser
- **Suppression d'historique = opération dangereuse** : Sauvegardez et coordonnez
- **BFG et git filter-repo** sont vos meilleurs outils pour le nettoyage
- **Prévenez plutôt que guérissez** : Il est plus facile de ne pas commiter que de supprimer

### En résumé

La gestion des gros fichiers dans Git suit trois principes :

1. **Prévention** : Ne commitez pas de gros fichiers (`.gitignore`)
2. **Alternative** : Utilisez Git LFS pour les fichiers volumineux nécessaires
3. **Nettoyage** : Si le mal est fait, utilisez les bons outils (`BFG`, `filter-repo`)

Un dépôt Git sain reste léger, rapide, et facile à cloner. Prenez le temps de bien le configurer dès le début, vous gagnerez énormément de temps par la suite !

⏭️ [Erreurs fréquentes et solutions](/module-08-depannage/05-erreurs-frequentes-et-solutions.md)
