Ce workshop consiste à vous apprendre à créer une api REST simple avec le framework adonisjs, le framework adonisjs est un framework en node.js et en typescript. 

Prérequis à télécharger npm, adonis, postman, node :
    npm :
        - sudo dnf install npm
    adonis:
        - npm init adonis-ts-app@latest workshop_adonis
            -"Select the project structure" : Choisir api
            -"Enter the project name" : Entrez un nom pour votre API
            -"Setup eslint" : appuyer sur 'y', eslint facilite le repérage de problème lié au code.
            -"Setup prettier" : appuyer sur 'y', prettier est un outil pour faciliter le formattage de vos fichier et ainsi garder la même syntaxe sur chacun de vos fichier.
            Voila, vous avez setup votre api, d'autre configuration sont à realisé dans la suite.
        vous allez ensuite vous déplacer vers le nouveau dossier contenant votre la base de votre api, "cd workshop_adonis"
        télécharger ces trois packages, ils nous seront utile pour setup l'api:
        - "npm i @adonisjs/lucid"
        - "npm i @adonisjs/auth"
        - "npm i phc-argon2"

    Postman:
        Postman est une application permettant d'envoyer des requêtes à une api à l'aide de cet outil vous pourrez donc tester vos routes.
        lien de téléchargement : "https://www.postman.com/downloads/"

    Node:
        - "sudo dnf install -y nodejs"

Configuration de l'api:
    1.Configurons d'abord la base de données en utilisant le package liée au framework Lucid :
        -`node ace configure @adonisjs/lucid`,
            -ici nous allons utiliser SQLite, appuyer sur espace pour selectionner le champs SQLite et ensuite appuyer sur entrée.
            -puis nous allons selectionner l'affichage d'instruction dans le terminal.
    2.`node ace configure @adonisjs/auth`:
       - Choisissez Lucid étant donnée qu'on a configuré lucid avant
       - Pour le guard nous allons utiliser uniquement l'api-tokens
       - Nous allons appeler le model User, puis acceptez la creation de migration, le model correspond au modele utiliser lors de l'authentification , la migration correspond au modele dans la database, elle va reprendre ses champs et ses categories.
       - puis on va mettre les tokens-api dans la database, choisissez database, on va également créer la migration pour la table api_tokens, appuyez sur y.


        Nous allons maintenant crée les champs dans notre model afin que la base de données puissent enregistrer les "username" des utilisateurs, ainsi que leurs "email".
        Pour cela il faudra se rendre dans le model, "app/models/user.ts" précédemment crée et non allons ajouté un champ username.
        "@column()
        public username: string"
        Puis dans le fichier de migration ajouter des champs "username" et "email":
        "table.string('username', 255).notNullable()
        table.string('email', 255)->notNullable().unique()"
        Puis lancer la migration pour ainsi créer les champs dans la base de donnée avec la commande "node ace migration:run"
        Et voila nous avons finis la configuration de l'api rest.

Explication de l'architecture :
    Comme vous pouvez le voir le dossier initié par le package est très bien construit vous pouvez retrouvez chaque partie dans des dossiers séparé, vous pouvez également retrouvées la liste des commandes executables par le framework en utilisant la commande "node ace" elle vous sera utile si vous avez besoin de crée des controller, des tests, des nouveaux models, etc ...

    Les dossiers situé dans le l'app correspond aux fichier qui vont définir vos fonctionnalité, donc controller, model, middleware utilisées, etc ...

    Les dossiers dans database concerne votre base de donnée, elle contient, les fichier de migrations qui servent a définir votre base de donnée lié à votre api.

Création de l'authentification
    Ex1: Création d'une route pour s'inscrire.
        1.Dans un premier temps vous allez devoir 'déclarer' votre routes à l'api (tous comme un variable) nous allons l'appeler "register".
        2.Ensuite créez un validator, les validators permettent de verifiez les paramètres envoyée.
        3.Créer votre controller, il s'agit des fonctions que vont exécuter vos routes.
        4.Créer une fonction qui va enregistrer des utilisateurs.

    Ex2: Création d'une route pour se connecter.
        1.Tout comme l'exo précédent vous allez devoir déclarer votre routes à l'api nous l'appellerons "login".
        2.Nous allons également crée un validator.
        3.Pas besoin de créer un nouveau controller nous pouvons utiliser le controller crée précédemment puisque les deux fonctions sont liées.
        4.Créer une fonction qui connecter les utilisateurs.
        tips: Adonis possède des fonctions intégrées pour directement checker la validité des credentials entrées en paramètre.Donc il n'y a pas besoin de vérifier directement dans la base de donnée les mot de passer et email fournis.

    
    Ex3: Création d'une route pour récupérer les informations de votre utilisateur
        La route s'appelera "info". Cette route renverra les informations de l'utilisateur authentifié.
        tips: utiliser le paramètre auth. Pensez également a déclarer le middleware dans le fichier kernel.ts

    Ex4: Création d'une route pour modifier ses informations.
        La route s'appeleraa "info/update".
        tips: jetez un oeil sur la fonction merge.

    Pour finir, testez votre api en la lancant avec "node ace serve -watch" et en utilisant postman pour lancer vos requêtes.
    Pour les plus aventuriers vous pouvez également crée des tests pour tester vos routes. (Adonis possèdent un système de tests pouvant êtres éxécuté avec la commande "node ace test", l'ensemble des fichiers tests se situe dans le dossier "./tests")
