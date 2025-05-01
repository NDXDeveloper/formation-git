# 3.5. Résolution de conflits

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

Lorsque vous fusionnez des branches qui ont modifié les mêmes parties des mêmes fichiers, Git ne peut pas déterminer automatiquement quelle version conserver. C'est ce qu'on appelle un **conflit de fusion**. N'ayez pas peur : c'est une situation normale dans le développement collaboratif !

## Pourquoi les conflits se produisent-ils ?

Un conflit se produit lorsque :
- Deux branches ont modifié la même ligne d'un fichier
- Une branche a supprimé un fichier tandis que l'autre l'a modifié
- Deux branches ont ajouté un fichier avec le même nom mais un contenu différent

## Reconnaître un conflit

Lorsque Git rencontre un conflit durant une fusion, il vous avertit avec un message comme celui-ci :

```
Auto-merging fichier.txt
CONFLICT (content): Merge conflict in fichier.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Git modifie également le(s) fichier(s) concerné(s) en y ajoutant des marqueurs spéciaux pour indiquer les zones en conflit.

## Anatomie d'un fichier en conflit

Voici à quoi ressemble un fichier contenant un conflit :

```
<<<<<<< HEAD
Cette ligne est la version de la branche courante (celle dans laquelle vous fusionnez)
=======
Cette ligne est la version de la branche que vous essayez de fusionner
>>>>>>> feature/nouvelle-fonctionnalite
```

Les parties importantes sont :
- `<<<<<<< HEAD` : début de la section qui montre le contenu de votre branche actuelle
- `=======` : séparateur entre les deux versions
- `>>>>>>> feature/nouvelle-fonctionnalite` : fin de la section, montrant le nom de la branche que vous essayez de fusionner

## Résoudre un conflit pas à pas

### Étape 1 : Identifier les fichiers en conflit

```bash
git status
```

Git listera les fichiers qui ont des conflits sous la section "Unmerged paths".

### Étape 2 : Ouvrir les fichiers en conflit

Ouvrez chaque fichier en conflit dans votre éditeur de texte préféré. De nombreux éditeurs modernes (comme VS Code, Sublime Text, etc.) affichent ces conflits de manière colorée pour faciliter leur visualisation.

### Étape 3 : Éditer les fichiers pour résoudre les conflits

Pour chaque conflit, vous avez plusieurs options :
- Garder votre version (celle de la branche actuelle)
- Prendre la version de l'autre branche
- Combiner les deux versions
- Écrire quelque chose de complètement différent

Quelle que soit votre décision, vous devez :
1. Supprimer les marqueurs de conflit (`<<<<<<<`, `=======`, `>>>>>>>`)
2. Laisser le fichier dans l'état que vous souhaitez conserver

Par exemple, si vous décidez de combiner les deux versions, votre fichier pourrait devenir :

```
Cette ligne est la version de la branche courante
ET cette ligne est la version de l'autre branche
```

### Étape 4 : Marquer le conflit comme résolu

Une fois que vous avez édité le fichier pour résoudre le conflit, vous devez l'ajouter à la zone de staging :

```bash
git add fichier-avec-conflit.txt
```

Répétez les étapes 2 à 4 pour tous les fichiers en conflit.

### Étape 5 : Terminer la fusion

Une fois tous les conflits résolus et les fichiers ajoutés à la zone de staging, vous pouvez finaliser la fusion en créant un commit :

```bash
git commit
```

Git ouvrira votre éditeur avec un message par défaut indiquant que c'est un commit de fusion. Vous pouvez le modifier si vous le souhaitez, puis enregistrer et fermer l'éditeur pour créer le commit.

## Outils pour résoudre les conflits

### Utiliser un outil graphique de résolution de conflits

Git peut lancer un outil visuel pour vous aider à résoudre les conflits :

```bash
git mergetool
```

Selon votre configuration, Git ouvrira un outil comme vimdiff, meld, kdiff3, ou autre. Ces outils présentent les différentes versions côte à côte et facilitent la résolution.

### Configurer votre outil de fusion préféré

Vous pouvez configurer Git pour utiliser votre outil préféré :

```bash
git config --global merge.tool meld  # Remplacez meld par l'outil de votre choix
```

## Stratégies pour éviter les conflits

1. **Communication** : Parlez avec votre équipe de qui travaille sur quoi
2. **Petites branches** : Créez des branches pour des fonctionnalités spécifiques plutôt que pour de grands ensembles de changements
3. **Fusions fréquentes** : Fusionnez régulièrement la branche principale dans vos branches de fonctionnalités
4. **Mise à jour régulière** : Tirez (pull) régulièrement les dernières modifications de la branche principale
5. **Organisation des fichiers** : Structurez votre projet pour que différentes fonctionnalités touchent différents fichiers

## Exercice pratique : Créer et résoudre un conflit

Voici un exercice pour vous familiariser avec la résolution de conflits :

1. Créez une nouvelle branche depuis votre branche principale
   ```bash
   git switch -c feature/conflit-test
   ```

2. Modifiez un fichier (par exemple README.md) en ajoutant une ligne
   ```bash
   echo "Ligne ajoutée dans feature/conflit-test" >> README.md
   ```

3. Committez ce changement
   ```bash
   git add README.md
   git commit -m "Modification dans feature/conflit-test"
   ```

4. Revenez à la branche principale
   ```bash
   git switch main
   ```

5. Modifiez la même ligne du même fichier différemment
   ```bash
   echo "Ligne différente ajoutée dans main" >> README.md
   ```

6. Committez ce changement
   ```bash
   git add README.md
   git commit -m "Modification dans main"
   ```

7. Essayez de fusionner votre branche de fonctionnalité
   ```bash
   git merge feature/conflit-test
   ```

8. Résolvez le conflit en éditant le fichier README.md, puis
   ```bash
   git add README.md
   git commit
   ```

Ce processus vous aidera à comprendre comment les conflits se produisent et comment les résoudre.

N'oubliez pas : les conflits ne sont pas un problème, mais une opportunité de décider consciemment quelle version du code est la meilleure !

Dans la prochaine section, nous explorerons le rebasage (git rebase), une alternative à la fusion pour intégrer des modifications.

⏭️ [Rebasage : git rebase](/module-3-travailler-avec-les-branches/06-rebasage-git-rebase.md)
