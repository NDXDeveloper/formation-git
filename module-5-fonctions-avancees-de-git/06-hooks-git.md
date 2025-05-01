# 5.6. Hooks Git (pré-commit, post-merge, etc.)

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Automatiser Git : les scripts qui s'exécutent à des moments clés

Avez-vous déjà souhaité que Git puisse :
- Vérifier automatiquement votre code avant chaque commit ?
- Exécuter des tests après chaque fusion de branches ?
- Déployer votre site web quand vous poussez sur la branche principale ?

Les **hooks Git** permettent justement cela ! Ils représentent l'un des aspects les plus puissants mais souvent méconnus de Git, surtout pour les débutants.

## Qu'est-ce qu'un hook Git ?

Un **hook** (ou "crochet" en français) est un simple script qui s'exécute automatiquement lorsqu'un événement spécifique se produit dans votre dépôt Git. Ces scripts peuvent être écrits dans n'importe quel langage exécutable sur votre système (bash, Python, Ruby, etc.).

Pensez aux hooks comme à des déclencheurs ("triggers") qui surveillent votre dépôt Git et interviennent à des moments précis pour exécuter des actions personnalisées.

## Où se trouvent les hooks ?

Les hooks sont stockés dans le répertoire `.git/hooks/` de votre dépôt. Si vous listez ce répertoire, vous verrez plusieurs fichiers d'exemple :

```bash
ls -la .git/hooks/
```

Vous verrez des fichiers comme `pre-commit.sample`, `post-merge.sample`, etc. Ce sont des exemples fournis par Git.

## Types de hooks

Il existe deux grandes catégories de hooks Git :

1. **Hooks côté client** : s'exécutent sur votre machine locale
2. **Hooks côté serveur** : s'exécutent sur le serveur Git (comme GitHub ou GitLab)

Dans ce tutoriel, nous nous concentrerons sur les hooks côté client, qui sont plus faciles à mettre en place pour les débutants.

### Hooks côté client les plus utiles

Voici les hooks les plus couramment utilisés :

#### Hooks de commit

- **pre-commit** : s'exécute avant la création d'un commit
  * Idéal pour vérifier le code, formater, lancer des tests rapides

- **prepare-commit-msg** : s'exécute avant l'ouverture de l'éditeur de message de commit
  * Permet de pré-remplir ou modifier le message de commit

- **commit-msg** : s'exécute après que vous ayez écrit un message de commit
  * Vérifie que le message de commit suit les conventions de l'équipe

- **post-commit** : s'exécute après la création d'un commit
  * Peut être utilisé pour des notifications

#### Hooks de synchronisation

- **pre-push** : s'exécute avant que vos commits soient poussés vers le dépôt distant
  * Parfait pour lancer des tests plus complets

- **post-merge** : s'exécute après avoir fusionné une branche ou fait un pull
  * Utile pour mettre à jour l'environnement de développement

- **post-checkout** : s'exécute après un checkout ou un switch
  * Pratique pour ajuster l'environnement en fonction de la branche

## Créer votre premier hook

Créons ensemble un hook `pre-commit` simple qui vérifie qu'il n'y a pas de mots clés comme "DEBUG" ou "TODO" dans les fichiers que vous vous apprêtez à committer.

### Étape 1 : Créer le fichier de hook

1. Allez dans le répertoire des hooks :
   ```bash
   cd .git/hooks/
   ```

2. Créez un fichier nommé `pre-commit` (sans extension) :
   ```bash
   touch pre-commit
   ```

3. Rendez le fichier exécutable :
   ```bash
   chmod +x pre-commit
   ```

### Étape 2 : Écrire le script

Ouvrez le fichier `pre-commit` dans votre éditeur et ajoutez ce code :

```bash
#!/bin/sh

# Vérifier les fichiers modifiés pour les mots-clés interdits
if git diff --cached --name-only | xargs grep --with-filename -n "DEBUG\|TODO" >/dev/null
then
    echo "ERREUR DE COMMIT : Trouvé DEBUG ou TODO dans les fichiers suivants :"
    git diff --cached --name-only | xargs grep --with-filename -n "DEBUG\|TODO"
    echo "Veuillez corriger ces problèmes avant de committer."
    exit 1
fi

# Si tout est bon, continuer avec le commit
exit 0
```

### Étape 3 : Tester le hook

1. Modifiez un fichier de votre projet et ajoutez une ligne contenant "TODO: à terminer plus tard"
2. Ajoutez ce fichier à l'index : `git add fichier_modifié.txt`
3. Essayez de faire un commit : `git commit -m "Test du hook pre-commit"`
4. Vous devriez voir un message d'erreur indiquant que le commit a été bloqué à cause du mot clé "TODO"

Félicitations ! Vous venez de créer et de tester votre premier hook Git.

## Partager des hooks avec votre équipe

Les hooks sont stockés dans le répertoire `.git/`, qui n'est **pas** cloné lorsque quelqu'un récupère votre dépôt. Pour partager des hooks avec votre équipe, vous avez plusieurs options :

### Option 1 : Répertoire partagé de hooks

1. Créez un répertoire `git-hooks/` à la racine de votre projet
2. Placez vos hooks dans ce répertoire (par exemple `git-hooks/pre-commit`)
3. Chaque membre de l'équipe devra configurer Git pour utiliser ce répertoire :
   ```bash
   git config core.hooksPath git-hooks
   ```

### Option 2 : Script d'installation

Créez un script qui copie les hooks dans le bon répertoire :

```bash
#!/bin/sh
# install-hooks.sh

# Copier tous les hooks dans .git/hooks/
cp git-hooks/* .git/hooks/
# Rendre les hooks exécutables
chmod +x .git/hooks/*

echo "Hooks Git installés avec succès !"
```

Demandez aux membres de l'équipe d'exécuter ce script après avoir cloné le dépôt.

### Option 3 : Outils de gestion de hooks

Des outils comme `husky` (pour les projets Node.js) ou `pre-commit` (pour Python) facilitent la gestion des hooks Git à travers les fichiers de configuration du projet.

## Exemples de hooks utiles

### Vérification du style de code

Un hook `pre-commit` qui lance un linter ou un formateur de code :

```bash
#!/bin/sh
# Pour un projet JavaScript
npx eslint --fix .
```

### Vérification des messages de commit

Un hook `commit-msg` qui vérifie si le message de commit suit un format spécifique :

```bash
#!/bin/sh

# Récupérer le message de commit depuis le fichier temporaire
commit_msg_file=$1
message=$(cat $commit_msg_file)

# Vérifier si le message commence par un type de commit
if ! echo "$message" | grep -qE "^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .+"; then
    echo "Erreur : le message de commit doit suivre le format 'type(scope): message'"
    echo "Types valides : feat, fix, docs, style, refactor, test, chore"
    exit 1
fi

exit 0
```

### Exécution de tests avant un push

Un hook `pre-push` qui exécute les tests unitaires :

```bash
#!/bin/sh
# Pour un projet Python
python -m pytest
```

### Mise à jour des dépendances après un pull

Un hook `post-merge` qui met à jour les dépendances :

```bash
#!/bin/sh
# Pour un projet Node.js
if git diff HEAD@{1} --name-only | grep -q "package.json"; then
    echo "Les dépendances ont changé, mise à jour en cours..."
    npm install
fi
```

## Désactiver temporairement un hook

Si vous avez besoin de contourner un hook pour un commit spécifique, vous pouvez utiliser l'option `--no-verify` (ou `-n`) :

```bash
git commit -m "Message de commit" --no-verify
```

Mais attention, n'abusez pas de cette option ! Les hooks sont généralement là pour une bonne raison.

## Hooks côté serveur (aperçu)

Les hooks côté serveur s'exécutent sur le serveur Git et permettent de contrôler ce qui se passe lorsque quelqu'un pousse des commits vers le dépôt.

Les plus courants sont :
- **pre-receive** : s'exécute avant que les références (branches, tags) ne soient mises à jour
- **update** : s'exécute pour chaque référence mise à jour
- **post-receive** : s'exécute après que toutes les références ont été mises à jour

Pour les plateformes comme GitHub ou GitLab, ces hooks sont généralement remplacés par leurs fonctionnalités d'intégration continue (CI/CD) ou de protection de branches.

## Bonnes pratiques

1. **Gardez les hooks rapides** : personne n'aime attendre plusieurs minutes pour faire un simple commit
2. **Faites des hooks idempotents** : ils doivent pouvoir être exécutés plusieurs fois sans problème
3. **Ajoutez des messages clairs** : expliquez pourquoi un hook bloque une action
4. **Documentez vos hooks** : expliquez leur fonctionnement dans le README du projet
5. **Permettez de les contourner** : dans certains cas légitimes, il doit être possible de passer outre
6. **Testez vos hooks** : assurez-vous qu'ils fonctionnent correctement dans différents scénarios

## Cas réels d'utilisation

### Pour une équipe de développement web

- **pre-commit** : formate le code, vérifie le style et les erreurs statiques
- **commit-msg** : vérifie que le message de commit suit la convention de l'équipe
- **pre-push** : exécute les tests unitaires et d'intégration
- **post-merge** : met à jour les dépendances et recompile les assets

### Pour un développeur solo

- **pre-commit** : vérifie les TODOs, les variables de débogage, et formate le code
- **post-commit** : crée une sauvegarde locale du commit
- **pre-push** : vérifie que tous les tests passent
- **post-checkout** : ajuste l'environnement en fonction de la branche

## En résumé

Les hooks Git sont des outils puissants qui vous permettent d'automatiser des tâches importantes à différentes étapes de votre workflow Git. Ils peuvent :

- Améliorer la qualité du code
- Renforcer les standards de l'équipe
- Éviter les erreurs courantes
- Automatiser des tâches répétitives

Bien qu'ils puissent sembler un peu techniques au début, les hooks sont relativement faciles à mettre en place et peuvent vous faire gagner beaucoup de temps à long terme. Commencez par des hooks simples, puis évoluez vers des solutions plus complexes à mesure que vous devenez plus à l'aise avec leur fonctionnement.

⏭️ [Module 6 : Bonnes pratiques et workflows](/module-6-bonnes-pratiques-et-workflows/README.md)
