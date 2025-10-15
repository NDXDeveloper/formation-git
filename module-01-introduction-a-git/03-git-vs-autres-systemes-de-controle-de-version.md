🔝 Retour au [Sommaire](/SOMMAIRE.md)

# Formation Git - Module 1 : Introduction à Git

## 3. Git vs autres systèmes de contrôle de version

### Les différentes familles de systèmes de contrôle de version

Avant de plonger dans les comparaisons, il est important de comprendre qu'il existe deux grandes catégories de systèmes de contrôle de version :

1. **Les systèmes centralisés** (CVS, SVN, Perforce...)
2. **Les systèmes distribués** (Git, Mercurial, Bazaar...)

### Les systèmes centralisés : l'ancienne approche

#### Comment ça fonctionne ?

Imaginez une bibliothèque traditionnelle avec un seul exemplaire de chaque livre. Si vous voulez lire ou modifier un livre, vous devez :
1. Aller à la bibliothèque (le serveur central)
2. Emprunter le livre
3. Le ramener une fois vos modifications terminées

C'est exactement ainsi que fonctionnent les systèmes centralisés comme **SVN** (Subversion) ou **CVS** (Concurrent Versions System).

#### Caractéristiques principales

- **Un serveur central** : toute l'histoire du projet est stockée à un seul endroit
- **Connexion obligatoire** : vous devez être connecté au serveur pour la plupart des opérations
- **Numéros de version séquentiels** : révision 1, 2, 3, 4...
- **Modèle "checkout/commit"** : vous récupérez une copie et vous renvoyez vos modifications

#### Les problèmes avec cette approche

**Point de défaillance unique** : Si le serveur tombe en panne ou si les données sont corrompues, vous perdez tout l'historique.

**Dépendance au réseau** : Impossible de consulter l'historique ou de faire des commits sans connexion internet.

**Lenteur** : Chaque opération nécessite une communication avec le serveur, ce qui peut être lent.

**Branches coûteuses** : Créer une branche était souvent lourd et compliqué, donc peu utilisé.

### Les systèmes distribués : la révolution Git

#### Comment ça fonctionne ?

Maintenant, imaginez que chaque personne possède sa propre bibliothèque complète avec tous les livres. Vous pouvez :
- Lire n'importe quel livre quand vous voulez
- Faire des annotations
- Réorganiser votre bibliothèque
- Synchroniser avec les bibliothèques des autres quand vous le souhaitez

C'est le principe des systèmes distribués comme **Git** ou **Mercurial**.

#### Caractéristiques principales

- **Copie complète** : chaque développeur a l'historique complet du projet
- **Autonomie totale** : travail possible hors ligne
- **Identifiants uniques** : chaque commit a un identifiant cryptographique (hash)
- **Synchronisation flexible** : vous choisissez quand synchroniser avec les autres

#### Les avantages de cette approche

**Robustesse** : Même si un serveur est détruit, chaque développeur possède une copie complète.

**Rapidité** : Presque toutes les opérations sont locales et donc instantanées.

**Flexibilité des branches** : Créer et fusionner des branches est trivial et rapide.

**Travail hors ligne** : Vous pouvez travailler normalement sans connexion internet.

### Comparaison détaillée : SVN vs Git

Prenons **SVN** (le système centralisé le plus populaire avant Git) et comparons-le avec **Git** :

| Aspect | SVN (Centralisé) | Git (Distribué) |
|--------|------------------|-----------------|
| **Architecture** | Serveur central unique | Chaque clone est un dépôt complet |
| **Historique** | Sur le serveur uniquement | Local et complet |
| **Travail hors ligne** | Très limité | Totalement possible |
| **Vitesse** | Dépend du réseau | Ultra-rapide (local) |
| **Branches** | Lourdes et coûteuses | Légères et rapides |
| **Numérotation** | Révisions séquentielles (r1, r2...) | Hash cryptographique |
| **Intégrité** | Moins robuste | Garantie par cryptographie |
| **Courbe d'apprentissage** | Plus simple au début | Plus complexe mais plus puissant |

### Pourquoi Git a gagné la bataille ?

#### 1. La vitesse

Avec Git, presque tout se passe localement :
- Consulter l'historique : **instantané**
- Créer une branche : **moins d'une seconde**
- Faire un commit : **quelques millisecondes**

Avec SVN, ces opérations nécessitaient une connexion au serveur et prenaient beaucoup plus de temps.

#### 2. Les branches

Git a rendu les branches si faciles à utiliser qu'elles sont devenues centrales dans les workflows modernes. Avec SVN, créer une branche était un processus lourd que beaucoup évitaient.

**Conséquence** : Les développeurs peuvent expérimenter librement, créer des branches pour chaque fonctionnalité, sans crainte.

#### 3. Le travail distribué

Dans un monde où les équipes sont souvent réparties géographiquement, pouvoir travailler sans connexion permanente à un serveur central est un énorme avantage.

#### 4. L'écosystème

L'arrivée de **GitHub** en 2008 a été un catalyseur majeur :
- Interface web intuitive
- Facilitation de la collaboration open source
- Fonctionnalités sociales (stars, forks, pull requests)

Cet écosystème a rendu Git accessible au grand public et a créé une communauté massive.

### Et les autres systèmes distribués ?

Git n'est pas le seul système distribué. Il existe notamment :

#### Mercurial (Hg)

- Très similaire à Git dans sa philosophie
- Interface souvent considérée comme plus simple
- Utilisé par Facebook et quelques autres grandes entreprises
- Moins populaire que Git aujourd'hui

#### Bazaar

- Développé par Canonical (Ubuntu)
- Flexible, peut fonctionner en mode centralisé ou distribué
- Moins utilisé aujourd'hui

**Pourquoi Git a dominé ?**
- Meilleure performance
- Communauté plus large
- Écosystème plus riche (GitHub, GitLab, Bitbucket)
- Adoption massive par les grands projets open source

### Cas d'usage : quand utiliser quoi ?

#### Git est idéal pour :

- Projets de développement logiciel de toutes tailles
- Travail en équipe distribuée
- Projets open source
- Situations nécessitant des branches fréquentes
- Projets où la vitesse est importante

#### SVN peut encore avoir du sens pour :

- Équipes très habituées à SVN (coût de migration)
- Projets avec des fichiers binaires volumineux (bien que Git LFS existe)
- Environnements nécessitant un contrôle d'accès granulaire par dossier
- Cas où la simplicité est prioritaire sur la puissance

**Attention** : Aujourd'hui, même ces cas d'usage peuvent généralement être mieux gérés avec Git et les bons outils.

### Les idées reçues sur Git

#### "Git est trop compliqué"

**Réalité** : Git offre beaucoup de fonctionnalités avancées, mais les bases sont simples. Pour 80% de votre travail quotidien, vous n'utiliserez que quelques commandes.

#### "SVN est plus simple"

**Réalité** : SVN peut sembler plus simple au début, mais Git vous fait gagner du temps sur le long terme. La courbe d'apprentissage initiale de Git est un investissement rentable.

#### "Git nécessite toujours GitHub"

**Réalité** : Git fonctionne parfaitement en local ou avec n'importe quel serveur distant. GitHub est juste un service populaire qui héberge des dépôts Git.

### L'évolution du paysage

Le graphique d'adoption montre une tendance claire :

- **2005-2008** : Domination de SVN, émergence de Git
- **2008-2012** : Croissance rapide de Git avec GitHub
- **2012-2015** : Git dépasse SVN
- **2015-aujourd'hui** : Git devient l'écrasant standard

Aujourd'hui, Git représente plus de **90%** du marché des nouveaux projets.

### En résumé

Git fait partie de la famille des systèmes de contrôle de version **distribués**, par opposition aux systèmes **centralisés** comme SVN. Cette architecture distribuée apporte :

- Plus de robustesse (pas de point de défaillance unique)
- Plus de rapidité (opérations locales)
- Plus de flexibilité (branches légères, travail hors ligne)
- Plus de puissance (workflows complexes possibles)

Bien que Git ait une courbe d'apprentissage initiale, ses avantages l'ont rendu indispensable dans le monde moderne du développement logiciel. C'est pourquoi pratiquement toutes les entreprises et projets l'ont adopté.

Comprendre ces différences vous aide à apprécier pourquoi Git fonctionne comme il fonctionne, et pourquoi certaines décisions de design ont été prises.

---

*Dans la section suivante, nous verrons comment installer Git sur votre système d'exploitation.*

⏭️ [Installation de Git (Windows, macOS, Linux)](/module-01-introduction-a-git/04-installation-de-git.md)
