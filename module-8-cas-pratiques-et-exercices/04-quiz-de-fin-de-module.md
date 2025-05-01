# 8.4. Quiz de fin de module

Félicitations pour être arrivé jusqu'à la fin de cette formation Git ! Pour consolider vos connaissances et vérifier votre compréhension, voici un quiz couvrant les concepts clés que nous avons explorés tout au long de cette formation, avec un accent particulier sur les cas pratiques et la résolution de problèmes.

## Comment utiliser ce quiz

- Essayez de répondre aux questions sans consulter vos notes ou la documentation
- Pour chaque question, choisissez la meilleure réponse
- Vérifiez ensuite vos réponses avec les solutions à la fin
- Pour les questions auxquelles vous avez mal répondu, relisez les sections correspondantes dans les modules précédents

## Partie 1 : Concepts fondamentaux

**Question 1:** Quelle commande permet de voir les fichiers modifiés mais pas encore ajoutés au staging area ?
- A) `git diff --staged`
- B) `git status`
- C) `git log --modified`
- D) `git modified`

**Question 2:** Quelle est la différence entre `git pull` et `git fetch` ?
- A) Il n'y a aucune différence, ce sont des synonymes
- B) `git pull` télécharge les changements et les fusionne automatiquement, tandis que `git fetch` télécharge uniquement sans fusionner
- C) `git fetch` est obsolète et a été remplacé par `git pull`
- D) `git pull` fonctionne uniquement sur la branche principale, tandis que `git fetch` fonctionne sur toutes les branches

**Question 3:** Quel fichier est utilisé pour indiquer à Git quels fichiers ignorer dans un dépôt ?
- A) `.gitconfig`
- B) `.gittrack`
- C) `.gitignore`
- D) `.gitexclude`

## Partie 2 : Branches et fusion

**Question 4:** Comment créer une nouvelle branche et y basculer immédiatement ?
- A) `git branch new-feature && git checkout new-feature`
- B) `git checkout -b new-feature`
- C) `git switch --create new-feature`
- D) Les réponses B et C sont correctes

**Question 5:** Lors d'un conflit de fusion, que signifient les marqueurs `<<<<<<< HEAD`, `=======` et `>>>>>>> feature-branch` ?
- A) Ils indiquent le début, le milieu et la fin du fichier
- B) Ils délimitent le code qui existait avant la fusion, la séparation, et le code de la branche qu'on essaie de fusionner
- C) Ce sont des erreurs de syntaxe générées par Git
- D) Ils identifient les fichiers à supprimer

**Question 6:** Quelle commande permet d'annuler une fusion en cours qui a généré des conflits ?
- A) `git merge --abort`
- B) `git reset --hard`
- C) `git cancel merge`
- D) `git checkout main`

## Partie 3 : Collaboration et dépôts distants

**Question 7:** Quelle est la bonne séquence pour contribuer à un projet open source sur GitHub ?
- A) Fork, clone, modifier, push, pull request
- B) Clone, branch, modifier, commit, push
- C) Download, modifier, upload
- D) Clone, modifier, commit, request access, push

**Question 8:** Comment pouvez-vous voir toutes les branches distantes ?
- A) `git branch --remote`
- B) `git remote show`
- C) `git show branches`
- D) `git fetch --all && git branch --list`

**Question 9:** Que fait la commande `git push -u origin feature-branch` ?
- A) Elle met à jour la branche distante avec votre branche locale
- B) Elle met à jour votre branche locale avec la branche distante
- C) Elle pousse votre branche locale et configure le suivi de la branche distante
- D) Elle supprime la branche locale après l'avoir poussée

## Partie 4 : Correction d'erreurs

**Question 10:** Comment annuler les modifications d'un fichier dans votre répertoire de travail qui n'a pas encore été ajouté au staging area ?
- A) `git reset --hard nom-du-fichier`
- B) `git checkout -- nom-du-fichier`
- C) `git restore nom-du-fichier`
- D) Les réponses B et C sont correctes

**Question 11:** Vous avez commité un changement mais vous réalisez que le message de commit contient une erreur. Quelle commande utiliseriez-vous pour modifier ce message ?
- A) `git commit --edit`
- B) `git commit --amend`
- C) `git message --edit HEAD`
- D) `git update-ref -m "nouveau message"`

**Question 12:** Comment supprimer le dernier commit de votre branche, tout en conservant les modifications dans votre répertoire de travail ?
- A) `git reset --soft HEAD~1`
- B) `git reset --hard HEAD~1`
- C) `git revert HEAD`
- D) `git remove HEAD`

## Partie 5 : Fonctionnalités avancées

**Question 13:** Quelle commande utiliseriez-vous pour mettre temporairement de côté vos modifications actuelles afin de pouvoir travailler sur autre chose ?
- A) `git save`
- B) `git stash`
- C) `git hide`
- D) `git temp`

**Question 14:** Dans Git Flow, quelle branche est utilisée pour préparer une nouvelle version du logiciel ?
- A) `main`
- B) `develop`
- C) `release`
- D) `hotfix`

**Question 15:** Quelle commande permet d'appliquer un commit spécifique d'une autre branche à votre branche actuelle ?
- A) `git cherry-pick <commit-hash>`
- B) `git apply <commit-hash>`
- C) `git merge <commit-hash>`
- D) `git copy <commit-hash>`

## Partie 6 : Scénarios pratiques

**Question 16:** Vous avez modifié plusieurs fichiers et souhaitez commiter uniquement certaines de ces modifications. Quelle est la meilleure approche ?
- A) Créer une nouvelle branche pour les autres modifications
- B) Utiliser `git add` fichier par fichier, puis `git commit`
- C) Utiliser `git commit -p` ou `git add -p` pour ajouter des changements spécifiques
- D) Commiter tout puis utiliser `git reset` pour séparer les commits

**Question 17:** Vous souhaitez voir qui a modifié chaque ligne d'un fichier et quand. Quelle commande utiliseriez-vous ?
- A) `git log -p fichier.txt`
- B) `git blame fichier.txt`
- C) `git history fichier.txt`
- D) `git show --lines fichier.txt`

**Question 18:** Après un `git pull`, vous remarquez que votre collègue a apporté des modifications qui posent problème. Comment restaurer votre branche à l'état précédent ?
- A) `git reset --hard ORIG_HEAD`
- B) `git checkout HEAD~1`
- C) `git revert pull`
- D) `git undo`

## Partie 7 : Dépannage Git

**Question 19:** Votre dépôt Git semble lent et volumineux. Quelle commande peut aider à l'optimiser ?
- A) `git optimize`
- B) `git gc`
- C) `git compress`
- D) `git clean -fd`

**Question 20:** Vous avez perdu un commit récent mais vous ne connaissez pas son hash. Comment pouvez-vous le retrouver ?
- A) Il est impossible de le retrouver sans le hash
- B) `git log --reflog`
- C) `git reflog`
- D) `git history --all`

**Question 21:** Comment vérifier si vous avez des fichiers ignorés par Git mais qui existent dans votre répertoire ?
- A) `git status --ignored`
- B) `git check-ignore -v *`
- C) `git ls-files --ignored`
- D) `git show --ignored`

## Partie 8 : Questions de réflexion

**Question 22:** Vous travaillez sur un projet avec plusieurs contributeurs. Quelle pratique est généralement recommandée ?
- A) Tous les développeurs travaillent directement sur la branche principale
- B) Chaque développeur crée une branche pour chaque fonctionnalité
- C) Un seul développeur est autorisé à push à la fois
- D) Désactiver les fusions pour éviter les conflits

**Question 23:** Quelle affirmation sur les messages de commit est la plus correcte ?
- A) Ils devraient être courts, un ou deux mots suffisent
- B) Ils devraient être détaillés, expliquer ce qui a été fait et pourquoi
- C) Ils ne sont pas importants car seul le code compte
- D) Ils doivent toujours contenir le numéro de ticket/issue

**Question 24:** Quelle est la meilleure façon de trouver quand et par qui une ligne spécifique a été introduite dans un projet ?
- A) Parcourir manuellement tous les commits
- B) Utiliser `git blame` pour identifier qui a modifié chaque ligne
- C) Utiliser `git bisect` pour trouver quand le code a été ajouté
- D) Examiner le premier commit du projet

## Réponses

Pour vérifier vos réponses, comparez-les avec la liste ci-dessous :

1. B
2. B
3. C
4. D
5. B
6. A
7. A
8. A
9. C
10. D
11. B
12. A
13. B
14. C
15. A
16. C
17. B
18. A
19. B
20. C
21. A
22. B
23. B
24. B

## Évaluation

- **0-12 points** : Vous avez besoin de revoir certains concepts fondamentaux. N'hésitez pas à revenir sur les modules précédents.
- **13-18 points** : Bonne compréhension des bases de Git. Continuez à pratiquer régulièrement.
- **19-22 points** : Très bonne maîtrise de Git. Vous êtes prêt pour des projets plus complexes.
- **23-24 points** : Excellent ! Vous avez une compréhension approfondie de Git et de ses fonctionnalités.

## Prochaines étapes

Maintenant que vous avez terminé ce module et l'ensemble de la formation, voici quelques suggestions pour continuer à progresser :

1. **Pratiquez régulièrement** - Intégrez Git dans tous vos projets, même personnels
2. **Explorez les outils** - Essayez différentes interfaces graphiques pour Git
3. **Contribuez à des projets open source** - Mettez en pratique vos compétences dans un contexte réel
4. **Approfondissez vos connaissances** - Explorez des fonctionnalités avancées comme les hooks, les submodules, et plus

Félicitations pour avoir terminé cette formation Git ! Vous disposez maintenant des compétences nécessaires pour utiliser efficacement cet outil puissant dans vos projets personnels et professionnels.

---

**Bonus : Question d'auto-évaluation**

Prenez quelques instants pour répondre à cette question ouverte : *"Comment Git a-t-il changé ou va-t-il changer votre façon de travailler sur vos projets ?"*

Votre réponse vous aidera à réfléchir sur l'impact concret de Git dans votre workflow et à identifier comment l'utiliser au mieux à l'avenir.
