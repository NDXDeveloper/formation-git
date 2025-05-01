# 6.4. Revue de code avec Pull Requests

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Qu'est-ce qu'une Pull Request ?

Une **Pull Request** (PR), également appelée **Merge Request** sur GitLab, est une fonctionnalité des plateformes comme GitHub, GitLab et Bitbucket qui permet de **proposer des modifications** à un projet avant de les intégrer à la branche principale.

C'est un peu comme si vous disiez : « J'ai fait ces changements dans ma version du code, pouvez-vous les vérifier et, si tout va bien, les intégrer au projet principal ? »

Les Pull Requests sont au cœur de la collaboration sur Git et constituent un excellent moyen d'organiser la **revue de code** (code review).

## Pourquoi utiliser des Pull Requests ?

### Pour les équipes

- **Contrôle qualité** : chaque modification est vérifiée avant intégration
- **Partage de connaissances** : les membres de l'équipe apprennent les uns des autres
- **Détection précoce des bugs** : les erreurs sont trouvées avant d'arriver en production
- **Documentation vivante** : historique des décisions et discussions sur le code
- **Standardisation** : assurance que le code respecte les conventions d'équipe

### Pour les projets open source

- **Contributions externes** : permet à n'importe qui de proposer des améliorations
- **Facilite l'intégration** : les mainteneurs peuvent vérifier les modifications avant fusion
- **Discussions ouvertes** : permet d'échanger sur les approches techniques

### Pour les débutants

- **Apprentissage sécurisé** : obtenez des retours avant intégration du code
- **Moins de stress** : vous savez que votre code sera vérifié avant d'être fusionné
- **Source de progression** : les commentaires reçus vous aident à vous améliorer

## Le cycle de vie d'une Pull Request

### 1. Préparation

Avant de créer une Pull Request, assurez-vous que :

- Votre code fonctionne correctement
- Vous avez ajouté des tests si nécessaire
- Vos commits sont clairs et suivent les conventions du projet
- Votre branche est à jour avec la branche cible (souvent `main` ou `develop`)

### 2. Création de la Pull Request

Sur GitHub, après avoir poussé votre branche :

1. Allez sur la page du dépôt
2. Cliquez sur "Pull requests" puis "New pull request"
3. Sélectionnez votre branche comme "compare" et la branche cible comme "base"
4. Cliquez sur "Create pull request"
5. Remplissez le titre et la description

![Création d'une Pull Request](https://docs.github.com/assets/cb-28613/mw-1440/images/help/pull_requests/pull-request-start-review-button.webp)

### 3. Rédaction d'une bonne description

Une bonne description de PR contient :

- **Quel problème résout cette PR** (lié à une issue si possible)
- **Comment vous l'avez résolu** (approche technique)
- **Comment tester** les changements
- **Captures d'écran** (pour les changements visuels)
- **Points d'attention** particuliers pour les reviewers

Exemple de template de PR :
```markdown
## Description
Cette PR ajoute la fonctionnalité de recherche avancée.

## Issue liée
Fixes #42

## Comment tester
1. Aller sur la page de recherche
2. Entrer un terme de recherche
3. Utiliser les filtres sur la gauche
4. Vérifier que les résultats sont correctement filtrés

## Captures d'écran
![Capture d'écran de la recherche](lien_vers_image.png)

## Points d'attention
J'ai dû modifier la structure de la base de données, vérifiez si l'approche vous convient.
```

### 4. Le processus de revue

Une fois la PR créée, les reviewers peuvent :

- **Examiner les changements** ligne par ligne
- **Commenter** directement sur le code
- **Suggérer des modifications** que vous pouvez appliquer en un clic
- **Approuver** la PR ou demander des changements

![Revue de code](https://docs.github.com/assets/cb-33584/mw-1440/images/help/pull_requests/pull-request-review-page.webp)

### 5. Itération sur les retours

En fonction des commentaires reçus :

1. Discutez des suggestions si nécessaire
2. Effectuez les modifications demandées
3. Poussez de nouveaux commits sur votre branche
4. La PR se met à jour automatiquement

Pas besoin de créer une nouvelle PR !

### 6. Fusion (Merge)

Une fois la PR approuvée, vous pouvez la fusionner avec différentes options :

- **Create a merge commit** : garde l'historique de la branche (crée un commit de fusion)
- **Squash and merge** : combine tous vos commits en un seul
- **Rebase and merge** : réapplique vos commits un par un sur la branche cible

Pour les débutants, l'option "Squash and merge" est souvent recommandée pour garder un historique propre.

![Options de fusion](https://docs.github.com/assets/cb-70869/mw-1440/images/help/pull_requests/select-squash-and-merge.webp)

## Bonnes pratiques pour les revues de code

### Pour ceux qui soumettent une PR

1. **Gardez vos PR de taille raisonnable**
   - Une PR volumineuse est difficile à réviser
   - Idéalement, moins de 400 lignes modifiées

2. **Décrivez clairement vos intentions**
   - Une bonne description facilite la revue
   - Expliquez le "pourquoi" pas juste le "quoi"

3. **Répondez aux commentaires**
   - Même si c'est juste pour dire "Corrigé" ou "Je ne suis pas d'accord parce que..."

4. **Soyez patient**
   - La revue prend du temps
   - N'hésitez pas à relancer gentiment après quelques jours

5. **N'hésitez pas à demander de l'aide**
   - Si vous bloquez sur un retour, demandez des précisions

### Pour les reviewers

1. **Soyez bienveillant**
   - Commentez le code, pas la personne
   - Félicitez les bonnes pratiques

2. **Soyez précis**
   - Expliquez pourquoi quelque chose pose problème
   - Proposez des solutions concrètes

3. **Hiérarchisez vos commentaires**
   - Distinguez les problèmes critiques des suggestions mineures

4. **Posez des questions plutôt qu'imposer**
   - "As-tu envisagé cette approche ?" plutôt que "Fais comme ça"

5. **Vérifiez la fonctionnalité**
   - Testez localement si possible
   - Vérifiez que les tests passent

## Exemple concret : déroulement d'une PR

### 1. Sarah travaille sur une fonctionnalité

Sarah crée une branche pour ajouter un formulaire de contact :
```bash
git checkout -b feature/contact-form
```

### 2. Elle développe et pousse ses modifications

```bash
git add .
git commit -m "feat: ajouter formulaire de contact"
git push -u origin feature/contact-form
```

### 3. Elle crée une Pull Request sur GitHub

- Elle va sur GitHub et crée une PR
- Elle décrit sa fonctionnalité et comment la tester
- Elle ajoute son collègue Thomas comme reviewer

### 4. Thomas fait une revue de code

- Il suggère d'ajouter une validation d'email
- Il recommande un petit refactoring
- Il pose des questions sur certaines parties

### 5. Sarah répond et fait des modifications

```bash
git add .
git commit -m "refactor: améliorer validation du formulaire suite aux retours"
git push
```

### 6. Thomas approuve la PR

- Il vérifie que ses commentaires ont été pris en compte
- Il teste la fonctionnalité localement
- Il approuve la PR

### 7. Sarah fusionne la PR

- Elle utilise l'option "Squash and merge"
- Elle supprime sa branche après la fusion

## Outils pour améliorer les Pull Requests

### 1. Templates de PR

GitHub et GitLab permettent de définir des templates de PR pour standardiser les descriptions :

Créez un fichier `.github/PULL_REQUEST_TEMPLATE.md` dans votre dépôt.

### 2. Intégration continue (CI)

Configurez des tests automatiques qui s'exécutent sur chaque PR :
- Tests unitaires et d'intégration
- Vérification du style de code (linting)
- Analyse de sécurité

### 3. Protection de branches

Configurez des règles pour la branche principale :
- Exiger des revues avant fusion
- Exiger que les tests CI passent
- Interdire les push directs

## Conseils pour les débutants

1. **Commencez petit** : vos premières PR peuvent être simples (correction de typo, petit bug)

2. **N'ayez pas peur des retours** : la revue n'est pas une critique personnelle, mais une opportunité d'apprentissage

3. **Observez les PR des autres** : vous apprendrez beaucoup en regardant comment les autres font

4. **Demandez une pré-revue** si vous n'êtes pas sûr de votre approche

5. **Proposez-vous comme reviewer** même en tant que débutant : c'est aussi formateur !

## Exercice pratique

1. Créez un dépôt sur GitHub (ou utilisez un existant)
2. Créez une branche pour une petite modification
3. Poussez cette branche et créez une Pull Request
4. Demandez à un ami de faire une revue (ou faites-la vous-même)
5. Appliquez des modifications suite aux commentaires
6. Fusionnez la PR

## Conclusion

Les Pull Requests sont bien plus qu'un simple mécanisme de fusion de code. Elles constituent :
- Un **espace de discussion** technique
- Un **outil d'apprentissage** pour toute l'équipe
- Un **filet de sécurité** contre les erreurs
- Une **documentation vivante** des décisions de conception

En prenant l'habitude d'utiliser des Pull Requests, même pour des projets personnels ou des petites équipes, vous adoptez une discipline qui améliore significativement la qualité de votre code et votre expérience de développement collaborative.

N'oubliez pas : la revue de code est une compétence qui s'apprend et s'améliore avec la pratique. Plus vous participez à des PR (comme auteur ou comme reviewer), plus vous deviendrez efficace !

⏭️ [Organisation des branches (main, dev, feature/, hotfix/)](/module-6-bonnes-pratiques-et-workflows/05-organisation-des-branches.md)
