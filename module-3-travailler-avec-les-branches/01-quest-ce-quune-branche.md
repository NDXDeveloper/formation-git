# 3.1. Qu'est-ce qu'une branche ?

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Définition simple

Une **branche** dans Git peut être vue comme une ligne de développement indépendante au sein de votre projet. Imaginez que votre projet est un arbre : le tronc principal représente votre branche principale (généralement appelée `main` ou `master`), et les branches sont des chemins alternatifs qui partent de ce tronc.

## Pour mieux comprendre

Prenons une analogie : vous écrivez un livre et vous avez terminé les 5 premiers chapitres. Maintenant, vous hésitez entre deux façons différentes de poursuivre l'histoire :

- Sans Git, vous devriez soit choisir une seule version, soit créer une copie complète de votre livre pour essayer l'autre version.
- Avec Git et ses branches, vous pouvez créer une "branche alternative" qui part de votre version actuelle. Vous pouvez alors explorer cette nouvelle direction sans affecter votre travail original.

## Comment ça fonctionne techniquement

En réalité, une branche dans Git est simplement un pointeur léger et mobile qui référence un commit spécifique. Chaque fois que vous faites un nouveau commit dans une branche, ce pointeur avance automatiquement pour pointer vers ce nouveau commit.

Lorsque vous créez une nouvelle branche, Git crée juste un nouveau pointeur — il ne fait pas de copie de tous vos fichiers. C'est pourquoi créer une branche dans Git est extrêmement rapide et peu coûteux en espace disque.

## La branche par défaut

Quand vous initialisez un dépôt Git, une branche nommée `master` (ou `main` dans les versions récentes) est créée automatiquement. C'est la branche principale de votre projet.

## Visualisation des branches

Voici comment les branches se présentent conceptuellement :

```
                    C4 (feature)
                   /
C0 -- C1 -- C2 -- C3 (main)
```

Dans ce diagramme :
- Chaque point (C0, C1, etc.) représente un commit
- La branche `main` pointe vers le commit C3
- La branche `feature` pointe vers le commit C4, qui est issu de C2

## Pourquoi utiliser des branches ?

Les branches sont utiles pour :

1. **Développer des fonctionnalités** : Créez une branche pour travailler sur une nouvelle fonctionnalité sans affecter le code principal.
2. **Corriger des bugs** : Isolez les corrections de bugs dans des branches dédiées.
3. **Expérimenter** : Testez de nouvelles idées sans risque.
4. **Collaborer** : Permettre à plusieurs personnes de travailler sur différentes parties du projet simultanément.

## En résumé

Une branche est comme un univers parallèle de votre projet où vous pouvez travailler librement sans affecter la version principale. C'est l'un des concepts les plus puissants de Git, car il permet de maintenir plusieurs versions du code simultanément et de les fusionner quand elles sont prêtes.

Dans la prochaine section, nous verrons comment créer et gérer concrètement ces branches !

⏭️ [Créer, lister, supprimer des branches](/module-3-travailler-avec-les-branches/02-creer-lister-supprimer-des-branches.md)
