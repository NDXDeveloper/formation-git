🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 3 : Corriger et modifier
## 1. Modifier le dernier commit (git commit --amend)

### Introduction

Vous venez de faire un commit et vous réalisez que vous avez oublié un fichier, fait une faute dans le message de commit, ou simplement besoin d'ajouter une petite modification ? Pas de panique ! Git vous permet de modifier votre dernier commit avec la commande `git commit --amend`.

**Analogie** : Imaginez que vous venez d'envoyer un email, mais vous réalisez juste après qu'il manque une pièce jointe. Au lieu d'envoyer un deuxième email séparé, vous aimeriez pouvoir "rappeler" l'email et le renvoyer avec la correction. C'est exactement ce que fait `--amend` avec votre dernier commit.

---

### Qu'est-ce que `git commit --amend` ?

La commande `git commit --amend` permet de **modifier le dernier commit** que vous avez effectué. Elle remplace complètement ce commit par un nouveau, en intégrant les modifications que vous souhaitez apporter.

**Utilisations courantes :**

- Corriger le message de commit (faute de frappe, message incomplet)
- Ajouter des fichiers oubliés au dernier commit
- Modifier du code que vous venez de committer
- Combiner plusieurs petites modifications en un seul commit propre

---

### Cas 1 : Modifier uniquement le message de commit

C'est le cas le plus simple. Vous avez fait un commit mais le message contient une erreur ou n'est pas assez clair.

**Étapes :**

```bash
git commit --amend
```

Cette commande va ouvrir votre éditeur de texte par défaut (souvent Vim ou Nano) avec le message de commit actuel. Vous pouvez alors :
1. Modifier le message comme vous le souhaitez
2. Sauvegarder et fermer l'éditeur
3. Le commit est maintenant modifié avec le nouveau message !

**Exemple pratique :**

Imaginons que vous avez fait ce commit :

```bash
git commit -m "Ajout de la fonctionalité de connexion"
```

Mais vous remarquez la faute : "fonctionalité" au lieu de "fonctionnalité". Pour corriger :

```bash
git commit --amend
```

L'éditeur s'ouvre avec le texte actuel. Corrigez la faute, sauvegardez et fermez. C'est corrigé !

**Raccourci rapide :**

Si vous voulez changer le message directement en ligne de commande sans ouvrir l'éditeur :

```bash
git commit --amend -m "Ajout de la fonctionnalité de connexion"
```

---

### Cas 2 : Ajouter des fichiers oubliés au dernier commit

Vous venez de faire un commit, mais vous réalisez que vous avez oublié d'ajouter un fichier important. Plutôt que de créer un nouveau commit "Oups, fichier oublié", vous pouvez l'ajouter au commit précédent.

**Étapes :**

```bash
# 1. Ajouter le(s) fichier(s) oublié(s) à la staging area
git add fichier_oublie.txt

# 2. Modifier le dernier commit pour y inclure ces fichiers
git commit --amend --no-edit
```

L'option `--no-edit` indique à Git que vous ne voulez pas modifier le message de commit, seulement ajouter des fichiers.

**Exemple détaillé :**

Imaginons cette situation :

```bash
# Vous créez deux fichiers
echo "Bonjour" > index.html  
echo "body { margin: 0; }" > style.css

# Vous ajoutez seulement index.html et faites un commit
git add index.html  
git commit -m "Ajout de la page d'accueil"

# Oups ! Vous avez oublié style.css qui fait partie de la même fonctionnalité
```

Pour corriger sans créer un nouveau commit :

```bash
# Ajoutez le fichier oublié
git add style.css

# Modifiez le dernier commit pour l'inclure
git commit --amend --no-edit
```

Maintenant, votre commit "Ajout de la page d'accueil" contient bien les deux fichiers : `index.html` ET `style.css`.

---

### Cas 3 : Modifier le contenu ET le message

Vous pouvez combiner les deux cas précédents : ajouter des fichiers ET modifier le message de commit en même temps.

**Étapes :**

```bash
# 1. Modifier vos fichiers
# 2. Ajouter les modifications à la staging area
git add fichier_modifie.js

# 3. Amender le commit avec un nouveau message
git commit --amend -m "Nouveau message descriptif"
```

Ou sans l'option `-m` pour ouvrir l'éditeur :

```bash
git add fichier_modifie.js  
git commit --amend
```

---

### Cas 4 : Modifier du code dans le dernier commit

Vous venez de committer du code mais vous remarquez une petite erreur (une faute de frappe, une variable mal nommée, etc.).

**Exemple :**

```bash
# Vous avez fait un commit
git commit -m "Ajout de la fonction de calcul"

# Vous remarquez une erreur dans votre code
# Corrigez l'erreur dans votre fichier

# Ajoutez la correction
git add calcul.js

# Amendez le commit
git commit --amend --no-edit
```

Le commit original est maintenant remplacé par une version corrigée, sans créer d'historique inutile.

---

### Visualiser les changements avec `git log`

Pour vérifier que votre amendement a bien fonctionné, utilisez :

```bash
git log --oneline
```

Vous verrez que le dernier commit a été modifié, mais son identifiant (hash SHA) a changé. C'est normal : Git considère qu'il s'agit d'un nouveau commit qui remplace l'ancien.

**Avant l'amendement :**
```
a1b2c3d (HEAD -> main) Ajout de la fonctionalité de connexion  
e4f5g6h Mise à jour du README
```

**Après l'amendement :**
```
z9y8x7w (HEAD -> main) Ajout de la fonctionnalité de connexion  
e4f5g6h Mise à jour du README
```

Notez que le hash du dernier commit a changé (`a1b2c3d` → `z9y8x7w`), mais pas celui des commits précédents.

---

### ⚠️ Avertissement important : Ne pas amender un commit déjà publié

**RÈGLE D'OR** : N'utilisez `git commit --amend` que sur des commits qui n'ont **pas encore été partagés** avec d'autres personnes (non poussés avec `git push`).

**Pourquoi ?**

Quand vous amendez un commit, Git crée en réalité un **nouveau commit** avec un identifiant différent. Si vous avez déjà poussé le commit original sur un dépôt distant (GitHub, GitLab, etc.) et que d'autres personnes l'ont récupéré, l'amendement va créer des conflits et des problèmes de synchronisation.

**Cas acceptable :**
```bash
git add fichier.txt  
git commit -m "Mon commit"
# Oups, j'ai oublié quelque chose
git add autre_fichier.txt  
git commit --amend --no-edit
# OK ! Je n'ai pas encore fait git push
```

**Cas problématique :**
```bash
git add fichier.txt  
git commit -m "Mon commit"  
git push origin main        # ❌ Le commit est maintenant public !
# Oups, j'ai oublié quelque chose
git add autre_fichier.txt  
git commit --amend --no-edit  # ⚠️ ATTENTION ! Cela va créer des problèmes
```

Si vous avez déjà poussé le commit, il vaut mieux créer un nouveau commit de correction plutôt que d'amender.

---

### Que faire si vous amendez par erreur un commit public ?

Si vous avez déjà amendé un commit qui était public, vous devrez forcer le push :

```bash
git push --force origin main
```

**⚠️ DANGER** : Cette commande réécrit l'historique sur le serveur. Elle peut causer de sérieux problèmes pour vos collaborateurs. Utilisez-la uniquement si :
- Vous travaillez seul sur la branche
- Vous avez prévenu toute votre équipe
- Vous savez exactement ce que vous faites

Une alternative plus sûre :

```bash
git push --force-with-lease origin main
```

Cette option vérifie que personne n'a poussé de nouveaux commits depuis votre dernier `git pull`, réduisant le risque d'écraser le travail de quelqu'un d'autre.

---

### Résumé des commandes

| Commande | Usage |
|----------|-------|
| `git commit --amend` | Modifier le message du dernier commit (ouvre l'éditeur) |
| `git commit --amend -m "Nouveau message"` | Modifier le message du dernier commit (en ligne de commande) |
| `git commit --amend --no-edit` | Ajouter des fichiers au dernier commit sans changer le message |
| `git add fichier.txt && git commit --amend` | Ajouter un fichier oublié au dernier commit |

---

### Points clés à retenir

✅ `git commit --amend` remplace le dernier commit par un nouveau

✅ Très utile pour corriger des erreurs mineures juste après un commit

✅ Change l'identifiant (hash) du commit

❌ Ne jamais amender un commit déjà poussé sur un dépôt distant partagé

❌ Peut causer des conflits si d'autres personnes ont déjà récupéré le commit original

---

### Conclusion

La commande `git commit --amend` est un outil précieux pour maintenir un historique Git propre et professionnel. Elle vous permet de corriger rapidement les petites erreurs sans polluer votre historique avec des commits du type "fix typo" ou "oubli fichier".

Cependant, utilisez-la avec précaution et rappelez-vous : **uniquement sur des commits locaux non partagés** ! Une fois qu'un commit est public, il vaut mieux créer un nouveau commit de correction pour éviter les problèmes avec vos collaborateurs.

Dans la prochaine section, nous verrons comment annuler des modifications plus importantes avec `git restore` et `git reset`.

⏭️ [Annuler des modifications (git restore, git checkout)](/module-03-corriger-et-modifier/02-annuler-des-modifications.md)
