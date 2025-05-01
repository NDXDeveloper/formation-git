# 8.2. Ateliers guidés : projet en équipe

Dans cette section, nous allons simuler un projet collaboratif pour vous permettre de mettre en pratique les compétences Git que vous avez acquises tout au long de cette formation. Ces ateliers guidés vous placeront dans des situations réelles de travail en équipe, similaires à celles que vous rencontrerez dans votre vie professionnelle.

## Préparation de l'environnement

Pour ces ateliers, vous pouvez travailler :
- Seul, en simulant plusieurs développeurs sur votre machine
- Avec des amis ou des collègues pour une expérience plus réaliste
- En classe, si vous suivez cette formation dans un cadre pédagogique

### Configuration initiale

1. **Créez un dépôt central sur GitHub/GitLab** :
   - Connectez-vous à votre compte GitHub
   - Créez un nouveau dépôt public nommé "atelier-git-equipe"
   - Initialisez-le avec un fichier README.md

2. **Clonez le dépôt sur votre machine** :
   ```bash
   git clone https://github.com/votre-username/atelier-git-equipe.git
   cd atelier-git-equipe
   ```

3. **Si vous travaillez seul**, créez deux dossiers pour simuler deux développeurs :
   ```bash
   mkdir dev1 dev2
   cp -r .git dev1/
   cp -r .git dev2/
   ```
   Vous pourrez alors alterner entre ces deux dossiers pour simuler deux développeurs différents.

## Atelier 1 : Création collaborative d'un site web simple

### Objectif
Créer ensemble un petit site web avec une page d'accueil, une page "À propos" et une page de contact.

### Étapes guidées

#### Étape 1 : Structure initiale (Développeur 1)

1. Créez une structure de base pour le site :
   ```bash
   mkdir css js images
   touch index.html about.html contact.html css/style.css js/main.js
   ```

2. Ajoutez du contenu basique à index.html :
   ```html
   <!DOCTYPE html>
   <html lang="fr">
   <head>
     <meta charset="UTF-8">
     <title>Accueil - Projet d'équipe</title>
     <link rel="stylesheet" href="css/style.css">
   </head>
   <body>
     <header>
       <h1>Notre Projet d'Équipe</h1>
       <nav>
         <ul>
           <li><a href="index.html">Accueil</a></li>
           <li><a href="about.html">À propos</a></li>
           <li><a href="contact.html">Contact</a></li>
         </ul>
       </nav>
     </header>
     <main>
       <h2>Bienvenue sur notre site!</h2>
       <p>Ceci est la page d'accueil.</p>
     </main>
     <footer>
       <p>&copy; 2025 - Projet d'équipe Git</p>
     </footer>
     <script src="js/main.js"></script>
   </body>
   </html>
   ```

3. Ajoutez quelques styles de base dans css/style.css :
   ```css
   body {
     font-family: Arial, sans-serif;
     margin: 0;
     padding: 0;
     line-height: 1.6;
   }

   header {
     background-color: #333;
     color: white;
     padding: 1rem;
   }

   nav ul {
     list-style: none;
     display: flex;
     padding: 0;
   }

   nav ul li {
     margin-right: 1rem;
   }

   nav a {
     color: white;
     text-decoration: none;
   }

   main {
     padding: 2rem;
   }

   footer {
     background-color: #333;
     color: white;
     text-align: center;
     padding: 1rem;
   }
   ```

4. Committez et poussez ces changements :
   ```bash
   git add .
   git commit -m "Ajout de la structure initiale du site"
   git push origin main
   ```

#### Étape 2 : Page À propos (Développeur 2)

1. Récupérez les derniers changements :
   ```bash
   git pull origin main
   ```

2. Créez une branche pour votre travail :
   ```bash
   git checkout -b page-about
   ```

3. Ajoutez du contenu à about.html :
   ```html
   <!DOCTYPE html>
   <html lang="fr">
   <head>
     <meta charset="UTF-8">
     <title>À propos - Projet d'équipe</title>
     <link rel="stylesheet" href="css/style.css">
   </head>
   <body>
     <header>
       <h1>Notre Projet d'Équipe</h1>
       <nav>
         <ul>
           <li><a href="index.html">Accueil</a></li>
           <li><a href="about.html">À propos</a></li>
           <li><a href="contact.html">Contact</a></li>
         </ul>
       </nav>
     </header>
     <main>
       <h2>À propos de notre équipe</h2>
       <p>Nous sommes une équipe passionnée par le développement web et la collaboration avec Git.</p>
       <h3>Notre mission</h3>
       <p>Créer des sites web exceptionnels tout en maîtrisant les outils de versioning.</p>
     </main>
     <footer>
       <p>&copy; 2025 - Projet d'équipe Git</p>
     </footer>
     <script src="js/main.js"></script>
   </body>
   </html>
   ```

4. Committez vos changements :
   ```bash
   git add about.html
   git commit -m "Ajout de la page À propos"
   ```

5. Poussez votre branche :
   ```bash
   git push origin page-about
   ```

6. Créez une Pull Request (PR) sur GitHub pour fusionner votre branche dans main.

#### Étape 3 : Page de contact (Développeur 1)

1. Pendant ce temps, le Développeur 1 crée une branche pour la page de contact :
   ```bash
   git checkout -b page-contact
   ```

2. Ajoutez du contenu à contact.html :
   ```html
   <!DOCTYPE html>
   <html lang="fr">
   <head>
     <meta charset="UTF-8">
     <title>Contact - Projet d'équipe</title>
     <link rel="stylesheet" href="css/style.css">
   </head>
   <body>
     <header>
       <h1>Notre Projet d'Équipe</h1>
       <nav>
         <ul>
           <li><a href="index.html">Accueil</a></li>
           <li><a href="about.html">À propos</a></li>
           <li><a href="contact.html">Contact</a></li>
         </ul>
       </nav>
     </header>
     <main>
       <h2>Contactez-nous</h2>
       <form>
         <div>
           <label for="name">Nom :</label>
           <input type="text" id="name" name="name">
         </div>
         <div>
           <label for="email">Email :</label>
           <input type="email" id="email" name="email">
         </div>
         <div>
           <label for="message">Message :</label>
           <textarea id="message" name="message"></textarea>
         </div>
         <button type="submit">Envoyer</button>
       </form>
     </main>
     <footer>
       <p>&copy; 2025 - Projet d'équipe Git</p>
     </footer>
     <script src="js/main.js"></script>
   </body>
   </html>
   ```

3. Ajoutez des styles pour le formulaire dans css/style.css :
   ```css
   form {
     max-width: 600px;
     margin: 0 auto;
   }

   form div {
     margin-bottom: 1rem;
   }

   label {
     display: block;
     margin-bottom: 0.5rem;
   }

   input, textarea {
     width: 100%;
     padding: 0.5rem;
   }

   button {
     background-color: #333;
     color: white;
     padding: 0.5rem 1rem;
     border: none;
     cursor: pointer;
   }
   ```

4. Committez et poussez vos changements :
   ```bash
   git add contact.html css/style.css
   git commit -m "Ajout de la page de contact avec styles de formulaire"
   git push origin page-contact
   ```

5. Créez une Pull Request sur GitHub.

#### Étape 4 : Revue et fusion des Pull Requests

1. Passez en revue la PR "Ajout de la page À propos" :
   - Vérifiez le code
   - Laissez un commentaire positif
   - Approuvez et fusionnez dans main

2. Mettez à jour votre branche principale locale :
   ```bash
   git checkout main
   git pull origin main
   ```

3. Passez en revue la PR "Ajout de la page de contact" :
   - Vérifiez le code
   - Suggérez une modification (par exemple, ajouter un champ téléphone)
   - Une fois la modification effectuée, fusionnez dans main

#### Étape 5 : Création d'un conflit et résolution

1. Le Développeur 1 crée une branche pour modifier le header :
   ```bash
   git checkout -b header-update
   ```

2. Modifiez le header dans index.html :
   ```html
   <header>
     <h1>Notre Super Projet Collaboratif</h1>
     <nav>
       <ul>
         <li><a href="index.html">Accueil</a></li>
         <li><a href="about.html">À propos</a></li>
         <li><a href="contact.html">Contact</a></li>
         <li><a href="services.html">Services</a></li>
       </ul>
     </nav>
   </header>
   ```

3. Committez et poussez :
   ```bash
   git add index.html
   git commit -m "Mise à jour du titre et ajout d'un lien Services"
   git push origin header-update
   ```

4. Pendant ce temps, le Développeur 2 crée une autre branche pour le même header :
   ```bash
   git checkout main
   git pull origin main
   git checkout -b new-header-design
   ```

5. Modifiez le même header différemment :
   ```html
   <header>
     <h1>Projet d'Équipe Git</h1>
     <p>Apprendre Git en s'amusant</p>
     <nav>
       <ul>
         <li><a href="index.html">Accueil</a></li>
         <li><a href="about.html">À propos</a></li>
         <li><a href="contact.html">Contact</a></li>
       </ul>
     </nav>
   </header>
   ```

6. Committez et poussez :
   ```bash
   git add index.html
   git commit -m "Refonte du header avec slogan"
   git push origin new-header-design
   ```

7. Fusionnez d'abord header-update dans main via GitHub.

8. Ensuite, essayez de fusionner new-header-design. Un conflit sera détecté.

9. Résolvez le conflit localement :
   ```bash
   git checkout main
   git pull origin main
   git checkout new-header-design
   git merge main
   ```

10. Résolvez le conflit dans index.html en combinant les deux modifications :
    ```html
    <header>
      <h1>Notre Super Projet Collaboratif</h1>
      <p>Apprendre Git en s'amusant</p>
      <nav>
        <ul>
          <li><a href="index.html">Accueil</a></li>
          <li><a href="about.html">À propos</a></li>
          <li><a href="contact.html">Contact</a></li>
          <li><a href="services.html">Services</a></li>
        </ul>
      </nav>
    </header>
    ```

11. Terminez la fusion :
    ```bash
    git add index.html
    git commit -m "Résolution du conflit de header"
    git push origin new-header-design
    ```

12. Créez une PR et fusionnez-la dans main.

## Atelier 2 : Simulation de cycle de développement avec Git Flow

Dans ce deuxième atelier, nous allons simuler un cycle de développement complet en utilisant le workflow Git Flow.

### Objectif
Gérer plusieurs environnements (développement, test, production) et plusieurs fonctionnalités en parallèle.

### Étapes guidées

#### Étape 1 : Configuration de Git Flow

1. Installez Git Flow (si ce n'est pas déjà fait) :
   - Sur macOS : `brew install git-flow`
   - Sur Linux : `apt-get install git-flow`
   - Sur Windows : Disponible avec Git for Windows

2. Initialisez Git Flow dans votre projet :
   ```bash
   git flow init -d
   ```
   Cela créera deux branches principales : `main` (production) et `develop` (développement)

#### Étape 2 : Développement de fonctionnalités

1. Commencez une nouvelle fonctionnalité (thème sombre) :
   ```bash
   git flow feature start theme-sombre
   ```

2. Ajoutez un bouton de basculement et des styles pour le thème sombre :
   - Modifiez css/style.css
   - Ajoutez du JavaScript dans js/main.js

3. Terminez la fonctionnalité :
   ```bash
   git flow feature finish theme-sombre
   ```
   Cela fusionne votre branche dans `develop`

4. Simultanément, un autre développeur commence une fonctionnalité (galerie d'images) :
   ```bash
   git flow feature start galerie-images
   ```

5. Créez une page gallery.html et ajoutez les styles nécessaires.

6. Terminez cette fonctionnalité également :
   ```bash
   git flow feature finish galerie-images
   ```

#### Étape 3 : Préparation d'une release

1. Créez une branche de release :
   ```bash
   git flow release start 1.0.0
   ```

2. Effectuez les dernières corrections et ajustements.

3. Terminez la release :
   ```bash
   git flow release finish 1.0.0
   ```
   Cela fusionne la release dans `main` ET dans `develop`, et crée un tag.

4. Poussez tout vers le dépôt distant :
   ```bash
   git push origin --all
   git push origin --tags
   ```

#### Étape 4 : Correction d'un bug urgent

1. Découvrez un bug critique sur la page d'accueil en production.

2. Créez un hotfix :
   ```bash
   git flow hotfix start bug-accueil
   ```

3. Corrigez le bug.

4. Terminez le hotfix :
   ```bash
   git flow hotfix finish bug-accueil
   ```
   Cela fusionne le correctif dans `main` ET `develop`.

## Réflexion et discussion

À la fin de ces ateliers, prenez le temps de réfléchir aux questions suivantes :

1. **Quels ont été les défis les plus difficiles lors de la collaboration ?**
2. **Comment avez-vous géré les conflits entre les branches ?**
3. **Quels avantages avez-vous constatés en utilisant Git Flow par rapport à un workflow plus simple ?**
4. **Quelles améliorations pourriez-vous apporter à votre processus de collaboration ?**

Ces ateliers vous ont permis de vivre une expérience proche de celle d'une véritable équipe de développement. Les compétences que vous avez mises en pratique seront directement transférables à vos futurs projets professionnels.

---

Dans la prochaine section, nous explorerons des projets personnels que vous pourrez réaliser par vous-même pour continuer à développer vos compétences Git.
