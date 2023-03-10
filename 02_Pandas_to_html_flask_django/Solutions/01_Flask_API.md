## Création d'une API REST Flask pour une application pilotée par les données avec ElephantSQL et Postman

Dans ce tutoriel, nous allons vous montrer comment créer une API REST simple en utilisant Flask pour une application pilotée par les données, en utilisant ElephantSQL comme base de données et Postman pour tester l'API.

### Prérequis

-   Python 3 installé sur votre système
-   Connexion à ElephantSQL et une instance de base de données créée
-   Postman installé sur votre système

### Étape 1 : Installation de Flask

La première étape consiste à installer Flask en utilisant le gestionnaire de paquets pip. Ouvrez une console et exécutez la commande suivante :

```bash
pip install Flask
```

### Étape 2 : Création de l'application Flask

Créez un fichier `app.py` et copiez le code ci-dessous :

```python
from flask import Flask, jsonify, request
import psycopg2

app = Flask(__name__)

# Connect to ElephantSQL database
conn = psycopg2.connect(database="nom_de_votre_base_de_données",
                        user="votre_nom_d'utilisateur",
                        password="votre_mot_de_passe",
                        host="nom_d'hôte_de_votre_base_de_données",
                        port="port_de_votre_base_de_données")

# Create a cursor object
cur = conn.cursor()

# Create a table if it doesn't exist
cur.execute("CREATE TABLE IF NOT EXISTS data (id SERIAL PRIMARY KEY, name VARCHAR(255), value INTEGER)")

# Add some data to the table
cur.execute("INSERT INTO data (name, value) VALUES ('donnée 1', 100), ('donnée 2', 200)")

# Commit the changes
conn.commit()

# Define a route for getting all data
@app.route('/data', methods=['GET'])
def get_data():
    cur.execute("SELECT * FROM data")
    rows = cur.fetchall()
    result = []
    for row in rows:
        result.append({'id': row[0], 'name': row[1], 'value': row[2]})
    return jsonify(result)

# Define a route for getting a single data item by ID
@app.route('/data/<int:data_id>', methods=['GET'])
def get_data_by_id(data_id):
    cur.execute("SELECT * FROM data WHERE id=%s", (data_id,))
    row = cur.fetchone()
    if row:
        result = {'id': row[0], 'name': row[1], 'value': row[2]}
        return jsonify(result)
    else:
        return jsonify({'message': 'Data not found'})

# Define a route for adding data
@app.route('/data', methods=['POST'])
def add_data():
    data = request.get_json()
    cur.execute("INSERT INTO data (name, value) VALUES (%s, %s)", (data['name'], data['value']))
    conn.commit()
    return jsonify({'message': 'Data added successfully'})

# Define a route for updating data
@app.route('/data/<int:data_id>', methods=['PUT'])
def update_data(data_id):
    data = request.get_json()
    cur.execute("UPDATE data SET name=%s, value=%s WHERE id=%s", (data['name'], data['value'], data_id))
    conn.commit()
    return jsonify({'message': 'Data updated successfully'})

# Define a route for deleting data
@app.route('/data/<int:data_id>', methods=['DELETE'])
def delete_data(data_id):
    cur.execute("DELETE FROM data WHERE id=%s", (data_id,))
    conn.commit()
    return jsonify

```

### Étape 3 : Exécution de l'application Flask

Pour exécuter l'application Flask, ouvrez une console et naviguez vers le dossier où vous avez enregistré le fichier `app.py`. Ensuite, exécutez la commande suivante :

```bash
python app.py
```

L'application Flask devrait maintenant être en cours d'exécution sur le port 5000.

### Étape 4 : Test de l'API avec Postman

Maintenant que nous avons créé notre API REST, nous pouvons la tester en utilisant Postman. Voici comment tester chaque route :

1.  **Obtenir toutes les données** : Envoyez une requête GET à l'URL `http://localhost:5000/data`. Vous devriez recevoir une réponse JSON contenant toutes les données.
    
2.  **Obtenir une donnée par ID** : Envoyez une requête GET à l'URL `http://localhost:5000/data/<data_id>`, où `<data_id>` est l'ID de la donnée que vous voulez récupérer. Vous devriez recevoir une réponse JSON contenant la donnée correspondante.
    
3.  **Ajouter une donnée** : Envoyez une requête POST à l'URL `http://localhost:5000/data` avec un corps JSON contenant les valeurs `name` et `value` de la nouvelle donnée. Vous devriez recevoir une réponse JSON indiquant que la donnée a été ajoutée avec succès.
    
4.  **Mettre à jour une donnée** : Envoyez une requête PUT à l'URL `http://localhost:5000/data/<data_id>`, où `<data_id>` est l'ID de la donnée que vous voulez mettre à jour, avec un corps JSON contenant les nouvelles valeurs `name` et `value`. Vous devriez recevoir une réponse JSON indiquant que la donnée a été mise à jour avec succès.
    
5.  **Supprimer une donnée** : Envoyez une requête DELETE à l'URL `http://localhost:5000/data/<data_id>`, où `<data_id>` est l'ID de la donnée que vous voulez supprimer. Vous devriez recevoir une réponse JSON indiquant que la donnée a été supprimée avec succès.

