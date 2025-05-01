# 1.2. Histoire de Git et son importance

🔝 Retour à la [Table des matières](/SOMMAIRE.md)

## Les origines de Git : une naissance dans la nécessité

L'histoire de Git commence par un problème concret dans un des projets les plus importants du monde open source : le noyau Linux.

### La crise qui a tout déclenché

Entre 1991 et 2002, les développeurs du noyau Linux (le cœur du système d'exploitation Linux) géraient leurs modifications principalement par l'échange de fichiers et de correctifs par email. À partir de 2002, ils ont commencé à utiliser un système de contrôle de version propriétaire appelé BitKeeper, qui leur était fourni gratuitement.

En 2005, la situation a radicalement changé :
- Les relations entre la communauté Linux et la société derrière BitKeeper se sont détériorées
- L'accès gratuit à BitKeeper a été révoqué
- Linus Torvalds (le créateur de Linux) et son équipe se sont retrouvés sans outil adapté

### La naissance rapide de Git

Face à cette crise, Linus Torvalds a décidé de créer son propre système de contrôle de version. Ses exigences étaient claires :
- Performance exceptionnelle, même avec des milliers de développeurs
- Architecture distribuée (pas de dépendance à un serveur central)
- Protection contre la corruption des données
- Simplicité conceptuelle

En seulement quelques semaines, Linus a conçu et développé la première version de Git. Le 7 avril 2005, Git avait son premier commit. Le 16 juin 2005, Git gérait déjà le développement du noyau Linux !

### L'anecdote du nom

Le nom "Git" est typique de l'humour de Linus Torvalds. En argot britannique, "git" est un terme péjoratif désignant une personne désagréable. Linus a plaisanté en disant qu'il "nommait tous ses projets d'après lui-même", d'abord Linux, puis Git.

## L'importance révolutionnaire de Git

Git a transformé la façon dont les développeurs collaborent sur du code. Voici pourquoi :

### 1. Le modèle distribué change tout

Avant Git, la plupart des systèmes de contrôle de version étaient centralisés :
- Un serveur central détenait l'historique complet
- Les développeurs ne disposaient que d'une copie partielle
- Impossible de travailler sans connexion au serveur

Git a introduit une approche distribuée :
- Chaque développeur possède une copie complète du dépôt et de son historique
- Possibilité de travailler hors ligne et de synchroniser plus tard
- Pas de point unique de défaillance

### 2. Impact sur le monde du développement logiciel

L'arrivée de Git a coïncidé avec (et facilité) plusieurs transformations majeures :

- **L'essor de l'open source** : Git a rendu la contribution aux projets open source beaucoup plus accessible.
- **GitHub et l'aspect social** : Lancé en 2008, GitHub a construit une interface sociale autour de Git, créant un "réseau social pour développeurs".
- **DevOps et intégration continue** : Les pratiques modernes de déploiement continu s'appuient fortement sur Git.
- **Les méthodologies agiles** : Git facilite les cycles de développement courts et itératifs.

### 3. Adoption massive

En quelques années seulement, Git est passé d'un outil créé pour un besoin spécifique à un standard de l'industrie :

- En 2011, une enquête montrait déjà que plus de 27% des entreprises utilisaient Git
- En 2015, ce chiffre dépassait 70%
- Aujourd'hui, Git est utilisé par plus de 90% des équipes de développement

Même des entreprises autrefois attachées à d'autres systèmes (comme Microsoft avec Team Foundation Version Control) ont fini par adopter Git.

## Pourquoi cette histoire est importante pour vous

Comprendre l'histoire de Git vous aide à mieux saisir :

1. **Sa philosophie de conception** : Git a été conçu pour un projet complexe (Linux) par des experts pour des experts. Cela explique pourquoi certaines commandes peuvent sembler contre-intuitives au premier abord.

2. **Ses forces uniques** : La rapidité et la fiabilité sont dans l'ADN de Git depuis sa conception.

3. **Son importance dans votre carrière** : La maîtrise de Git est devenue une compétence fondamentale pour tout développeur, presque aussi essentielle que la connaissance d'un langage de programmation.

Dans la section suivante, nous comparerons Git à d'autres systèmes de contrôle de version pour mieux comprendre ce qui le rend spécial.

⏭️ [Git vs autres systèmes de contrôle de version](/module-1-introduction-a-git/03-git-vs-autres-systemes-de-controle-de-version.md)
