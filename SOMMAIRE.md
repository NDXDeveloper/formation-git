# Formation Complète Git – Table des Matières

## [Module 1 : Introduction à Git](module-01-introduction-a-git/README.md)
1. [Qu'est-ce que Git ?](module-01-introduction-a-git/01-quest-ce-que-git.md)
2. [Histoire de Git et son importance](module-01-introduction-a-git/02-histoire-de-git-et-son-importance.md)
3. [Git vs autres systèmes de contrôle de version](module-01-introduction-a-git/03-git-vs-autres-systemes-de-controle-de-version.md)
4. [Installation de Git (Windows, macOS, Linux)](module-01-introduction-a-git/04-installation-de-git.md)
5. [Configuration initiale (git config)](module-01-introduction-a-git/05-configuration-initiale.md)
6. [Premier projet Git : création d'un dépôt local](module-01-introduction-a-git/06-premier-projet-git.md)

---

## [Module 2 : Concepts fondamentaux](module-02-concepts-fondamentaux/README.md)
1. [Les 3 états de Git : Working Directory, Staging Area, Repository](module-02-concepts-fondamentaux/01-les-3-etats-de-git.md)
2. [Les fichiers .git et l'architecture interne](module-02-concepts-fondamentaux/02-les-fichiers-git-et-architecture-interne.md)
3. [Suivi des fichiers : git status, git add, git commit](module-02-concepts-fondamentaux/03-suivi-des-fichiers.md)
4. [Exploration de l'historique : git log, git show, git diff](module-02-concepts-fondamentaux/04-exploration-de-historique.md)
5. [Visualiser l'historique : git log --graph et la notion de graphe](module-02-concepts-fondamentaux/05-visualiser-historique.md)
6. [.gitignore : ignorer des fichiers](module-02-concepts-fondamentaux/06-gitignore.md)

---

## [Module 3 : Corriger et modifier](module-03-corriger-et-modifier/README.md)
1. [Modifier le dernier commit (git commit --amend)](module-03-corriger-et-modifier/01-modifier-le-dernier-commit.md)
2. [Annuler des modifications (git restore, git checkout)](module-03-corriger-et-modifier/02-annuler-des-modifications.md)
3. [Comprendre git reset (--soft, --mixed, --hard)](module-03-corriger-et-modifier/03-comprendre-git-reset.md)
4. [Annuler un commit publié (git revert)](module-03-corriger-et-modifier/04-annuler-un-commit-publie.md)
5. [Récupérer des fichiers d'anciennes versions](module-03-corriger-et-modifier/05-recuperer-des-fichiers-anciennes-versions.md)

---

## [Module 4 : Travailler avec les branches](module-04-travailler-avec-les-branches/README.md)
1. [Qu'est-ce qu'une branche ?](module-04-travailler-avec-les-branches/01-quest-ce-quune-branche.md)
2. [Créer, lister, supprimer des branches](module-04-travailler-avec-les-branches/02-creer-lister-supprimer-des-branches.md)
3. [Changer de branche (git checkout et git switch)](module-04-travailler-avec-les-branches/03-changer-de-branche.md)
4. [Fusion de branches (git merge)](module-04-travailler-avec-les-branches/04-fusion-de-branches.md)
5. [Stratégies de merge (fast-forward, merge commit, squash)](module-04-travailler-avec-les-branches/05-strategies-de-merge.md)
6. [Résolution de conflits](module-04-travailler-avec-les-branches/06-resolution-de-conflits.md)
7. [Rebasage : le principe de git rebase](module-04-travailler-avec-les-branches/07-rebasage-principe.md)
8. [Rebase vs Merge : la règle d'or et quand choisir](module-04-travailler-avec-les-branches/08-rebase-vs-merge.md)

---

## [Module 5 : Git à plusieurs – Dépôts distants](module-05-git-a-plusieurs/README.md)
1. [Qu'est-ce qu'un dépôt distant ?](module-05-git-a-plusieurs/01-quest-ce-quun-depot-distant.md)
2. [Introduction à GitHub, GitLab et Bitbucket](module-05-git-a-plusieurs/02-introduction-plateformes-git.md)
3. [Cloner un dépôt (git clone)](module-05-git-a-plusieurs/03-cloner-un-depot.md)
4. [Publier un projet local : git remote add et git push -u](module-05-git-a-plusieurs/04-publier-un-projet-local.md)
5. [Gérer les dépôts distants (git remote)](module-05-git-a-plusieurs/05-gerer-les-depots-distants.md)
6. [Synchroniser : git fetch, git pull, git push](module-05-git-a-plusieurs/06-synchroniser-fetch-pull-push.md)
7. [Authentification : Clés SSH et Personal Access Tokens (PAT)](module-05-git-a-plusieurs/07-gerer-les-cles-ssh-et-pat.md)
8. [Fork et Pull Request (PR)](module-05-git-a-plusieurs/08-fork-et-pull-request.md)

---

## [Module 6 : Fonctions avancées](module-06-fonctions-avancees/README.md)
1. [Mettre de côté temporairement (git stash)](module-06-fonctions-avancees/01-git-stash.md)
2. [Cherry-pick : appliquer des commits spécifiques](module-06-fonctions-avancees/02-cherry-pick.md)
3. [Nettoyer l'historique avant de partager (rebase interactif)](module-06-fonctions-avancees/03-nettoyer-historique-rebase-i.md)
4. [Reflog : récupérer des commits perdus](module-06-fonctions-avancees/04-reflog-et-recuperation.md)
5. [Déboguer avec git bisect](module-06-fonctions-avancees/05-deboger-avec-git-bisect.md)
6. [Tracer les modifications (git blame et git log -S)](module-06-fonctions-avancees/06-tracer-les-modifications.md)
7. [Tags et releases](module-06-fonctions-avancees/07-tags-et-releases.md)
8. [Submodules et sous-projets](module-06-fonctions-avancees/08-submodules.md)
9. [Hooks Git (automatisation)](module-06-fonctions-avancees/09-hooks-git.md)
10. [Alias Git pour la productivité](module-06-fonctions-avancees/10-alias-git.md)

---

## [Module 7 : Bonnes pratiques et workflows](module-07-bonnes-pratiques-et-workflows/README.md)
1. [Écrire de bons messages de commit](module-07-bonnes-pratiques-et-workflows/01-messages-de-commit.md)
2. [Commits atomiques et organisation du travail](module-07-bonnes-pratiques-et-workflows/02-commits-atomiques.md)
3. [.gitignore et .gitattributes avancés](module-07-bonnes-pratiques-et-workflows/03-gitignore-et-gitattributes-avances.md)
4. [Workflows collaboratifs (Git Flow, GitHub Flow, Trunk-Based)](module-07-bonnes-pratiques-et-workflows/04-workflows-collaboratifs.md)
5. [Organisation des branches (main, dev, feature/, hotfix/)](module-07-bonnes-pratiques-et-workflows/05-organisation-des-branches.md)
6. [Revue de code avec Pull Requests](module-07-bonnes-pratiques-et-workflows/06-revue-de-code-avec-pr.md)
7. [Versionnement sémantique (SemVer)](module-07-bonnes-pratiques-et-workflows/07-versionnement-semantique.md)

---

## [Module 8 : Dépannage et résolution de problèmes](module-08-depannage/README.md)
1. [Résoudre "detached HEAD"](module-08-depannage/01-resoudre-detached-head.md)
2. [Annuler un merge ou un rebase](module-08-depannage/02-annuler-merge-ou-rebase.md)
3. [Récupérer du travail perdu (avec reflog et reset)](module-08-depannage/03-recuperer-travail-perdu.md)
4. [Gérer les gros fichiers et nettoyer le dépôt](module-08-depannage/04-gerer-gros-fichiers.md)
5. [Erreurs fréquentes et solutions](module-08-depannage/05-erreurs-frequentes-et-solutions.md)

---

## [Module 9 : Outils et intégration](module-09-outils-et-integration/README.md)
1. [Interfaces graphiques (Sourcetree, GitKraken, Fork)](module-09-outils-et-integration/01-interfaces-graphiques.md)
2. [Git dans les IDE (VSCode, IntelliJ, PyCharm)](module-09-outils-et-integration/02-git-dans-les-ide.md)
3. [Extensions Git utiles](module-09-outils-et-integration/03-extensions-git-utiles.md)
4. [Intégration continue (CI/CD) et Git](module-09-outils-et-integration/04-integration-continue-et-git.md)
5. [Déploiement automatisé avec Git](module-09-outils-et-integration/05-deploiement-automatise.md)
6. [Git LFS pour les gros fichiers](module-09-outils-et-integration/06-git-lfs.md)

---

## [Module 10 : Cas pratiques et projets](module-10-cas-pratiques/README.md)
1. [Scénarios courants et résolutions](module-10-cas-pratiques/01-scenarios-courants.md)
2. [Atelier : Contribuer à un projet open source](module-10-cas-pratiques/02-atelier-contribuer-open-source.md)
3. [Atelier : Projet en équipe avec Git Flow](module-10-cas-pratiques/03-atelier-projet-en-equipe.md)
4. [Projet personnel guidé](module-10-cas-pratiques/04-projet-personnel-guide.md)
5. [Exercices de simulation (conflits, historique complexe)](module-10-cas-pratiques/05-exercices-de-simulation.md)
6. [Quiz et certification finale](module-10-cas-pratiques/06-quiz-et-certification.md)

---

## [Annexes](annexes/README.md)
- [Aide-mémoire des commandes Git](annexes/a1-aide-memoire.md)
- [Glossaire Git](annexes/a2-glossaire.md)
- [Ressources complémentaires](annexes/a3-ressources.md)
- [Configuration Git avancée](annexes/a4-configuration-avancee.md)

