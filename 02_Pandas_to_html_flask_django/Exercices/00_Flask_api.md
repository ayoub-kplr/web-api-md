## : Créer une API REST Flask pour une application pilotée par les données

### Objectifs

-   Comprendre comment configurer une connexion de base de données à l'aide de psycopg2 et ElephantSQL
-   Comprendre comment écrire des fonctions Flask pour récupérer, ajouter, mettre à jour et supprimer des données d'une base de données
-   Comprendre comment créer des routes Flask pour les fonctions de l'API REST
-   Comprendre comment utiliser Postman pour tester des routes d'API REST

### Instructions

1.  Créez une nouvelle base de données dans ElephantSQL et nommez-la `mydata`.
2.  Créez une table dans la base de données nommée `data` avec les colonnes `id`, `name` et `value`.
3.  Créez une nouvelle application Flask dans un fichier nommé `app.py`.
4.  Importez les modules nécessaires, y compris Flask et psycopg2 (un adaptateur de base de données PostgreSQL).
5.  Configurez une connexion de base de données en utilisant psycopg2 et les identifiants ElephantSQL.
6.  Créez une fonction pour récupérer toutes les données de la base de données et les retourner sous forme de réponse JSON.
7.  Créez une fonction pour récupérer un seul élément de données par son ID et le retourner sous forme de réponse JSON.
8.  Créez une fonction pour ajouter un nouvel élément de données à la base de données et retourner une réponse JSON indiquant le succès ou l'échec.
9.  Créez une fonction pour mettre à jour un élément de données existant dans la base de données et retourner une réponse JSON indiquant le succès ou l'échec.
10.  Créez une fonction pour supprimer un élément de données existant de la base de données et retourner une réponse JSON indiquant le succès ou l'échec.
11.  Configurez des routes pour chacune des fonctions ci-dessus en utilisant le décorateur `route()` de Flask.
12.  Exécutez l'application Flask et testez chacune des routes à l'aide de Postman.

### Indices

-   Utilisez la fonction `psycopg2.connect()` pour établir une connexion à ElephantSQL. N'oubliez pas de remplacer les espaces réservés `USER`, `PASSWORD`, `HOST` et `PORT` par vos véritables identifiants ElephantSQL.
-   Utilisez la méthode `cursor()` sur la connexion de la base de données pour créer un objet curseur qui peut être utilisé pour exécuter des requêtes SQL.
-   Utilisez la méthode `fetchall()` sur l'objet curseur pour récupérer toutes les lignes d'un résultat de requête.
-   Utilisez la méthode `fetchone()` sur l'objet curseur pour récupérer une seule ligne d'un résultat de requête.
-   Utilisez la méthode `execute()` sur l'objet curseur pour exécuter une requête SQL avec des paramètres. N'oubliez pas d'utiliser `%s` comme espaces réservés pour les valeurs des paramètres.
-   Utilisez la méthode `commit()` sur la connexion de la base de données pour valider les modifications apportées à la base de données.
-   Utilisez la fonction `jsonify()` de Flask pour convertir des objets Python en réponses JSON.
-   Utilisez l'objet `request` de Flask pour récupérer des données à partir d'un corps de requête POST ou PUT.

Bonne chance !