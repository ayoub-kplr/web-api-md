### Un premier exemple

Dans la suite, on va voir comment créer une API en utilisant Python et le cadre de travail Flask.

Notre exemple d’API portera sur un annuaire des employés d’une entreprise qui fournira leur nom, prénom, fonction et ancienneté.

On commencera par utiliser [Flask](http://flask.pocoo.org/) pour créer une page web pour notre site. Cela nous permettra de voir comment Flask fonctionne. Une fois qu’on aura une petite application Flask fonctionnelle sous la forme d’une page Web, on transformera le site en une API.

Pourquoi Flask ? En effet, [Python](https://datascientest.com/formation-langage-python) dispose de plusieurs cadres de développement (frameworks) permettant de produire des pages Web et des API.

Le plus connu est Django, qui est très riche mais qui peut toutefois être écrasant pour les utilisateurs non expérimentés.

Les applications Flask sont construites à partir de canevas très simples et sont donc plus adaptées au prototypage d’APIs.

On commence par créer un nouveau répertoire sur l’ordinateur, qui servira de répertoire de projet et qu’on nommera **projects**.

Les fichiers de notre projet seront stockés dans un sous-**répertoire de projects**, nommé **api**.

On lance ensuite Spyder 3 ou tout autre EDI et on saisit le code suivant :

import flask

app = flask.Flask(__name__)

app.config["DEBUG"] = True

@app.route('/', methods=['GET'])

def home():

return "<h1>Annuaire Internet</h1><p>Ce site est le prototype d’une API mettant à disposition des données sur les employés d’une entreprise.</p>"

app.run()

[view raw](https://gist.github.com/ssime-git/47e012db9c69a8fd2b7661b84031e76e/raw/328fb1c51f65fa08e24e3da860e40b89ee917900/Flask_main.py)[Flask_main.py](https://gist.github.com/ssime-git/47e012db9c69a8fd2b7661b84031e76e#file-flask_main-py) hosted with ❤ by [GitHub](https://github.com/)

On sauvegarde ensuite le programme sous le nom api.py dans le répertoire api précédemment créé.

Afin de l’exécuter, on lance une fenêtre ligne de commande à partir de ce répertoire et on saisit les commandes suivantes :

**$ export FLASK_APP = api.py**

**$ export FLASK_ENV = development**

**$ flask run**

Alternativement, on peut exécuter le programme sous Spyder ou tout autre EDI utilisé.

On obtient dans le console l’affichage suivant (entre autres sorties) :

*** Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)**

Il suffit de saisir le lien précédent dans un navigateur Web pour accéder à l’application.

On a ainsi créé une application Web fonctionnelle.

![](https://i0.wp.com/datascientest.com/wp-content/uploads/2022/02/API-1.png?w=800&ssl=1)

Examinons à présent la façon dont Flask fonctionne.

Flask envoie des requêtes HTTP à des fonctions Python. Dans notre cas, nous avons appliqué un chemin d’URL (‘/‘) sur une fonction : **home**.

Flask exécute le code de la fonction et affiche le résultat dans le navigateur. Dans notre exemple, le résultat est un code HTML de bienvenue sur le site hébergeant notre API.

Le processus consistant à appliquer des URL sur des fonctions est appelé routage (routing).

L’instruction :

**@app.route(‘/’, methods=[‘GET’])**

apparaissant dans le programme indique à Flask que la **fonction home** correspond au chemin /.

La liste methods **(methods=[‘GET’])** est un argument mot-clef qui indique à Flask le type de requêtes HTTP autorisées.

On utilisera uniquement des requêtes GET dans la suite, mais de nombreuses applications Web utilisent à la fois des requêtes GET (pour envoyer des données de l’application aux utilisateurs) et POST (pour recevoir les données des utilisateurs).

Commentons à présent le programme plus en détail.

**import Flask** : Cette instruction permet d’importer la bibliothèque Flask, qui est disponible par défaut sous Anaconda.

**app = flask.Flask(__name__)** : Crée l’objet application Flask, qui contient les données de l’application et les méthodes correspondant aux actions susceptibles d’être effectuées sur l’objet. La dernière instruction app.run( ) est un exemple d’utilisation de méthode.

**app.config[“DEBUG” = True]** : lance le débogueur, ce qui permet d’afficher un message autre que « Bad Gateway » s’il y a une erreur dans l’application.

**app.run( )** : permet d’exécuter l’application.

À présent, afin de créer l’API, on va spécifier nos données sous la forme d’une liste de dictionnaires Python.

À titre d’exemple, on va fournir des données sur trois employés de notre entreprise. Chaque dictionnaire contiendra un numéro d’identification, le nom, le prénom, la fonction et l’ancienneté d’un employé.

On introduira également une nouvelle fonction : une route permettant aux visiteurs d’accéder à nos données.

Ainsi, nous remplaçons le code sauvegardé dans **api.py** par le code suivant :

# Import de bibliothèques

import flask

from flask import request, jsonify

# Création de l'objet Flask

app = flask.Flask(__name__)

# Lancement du Débogueur

app.config["DEBUG"] = True

# Quelques données tests pour l’annuaire sous la forme d’une liste de dictionnaires

employees = [

{'id': 0,

'Nom': 'Dupont',

‘Prénom’': 'Jean',

'Fonction': 'Développeur',

'Ancienneté': '5'},

{'id': 1,

'Nom': 'Durand',

'Prénom': Elodie',

'Fonction': 'Directrice Commerciale',

'Ancienneté': '4'},

{'id': 2,

'Nom': 'Lucas',

'Prénom': 'Jérémie',

'Fonction': 'DRH',

'Ancienneté': '4'}

]

@app.route('/', methods=['GET'])

def home():

return '''<h1>Annuaire Internet</h1>

<p>Ce site est le prototype d’une API mettant à disposition des données sur les employés d’une entreprise.</p>'''

# Route permettant de récupérer toutes les données de l’annuaire

@app.route('/api/v1/resources/employees/all', methods=['GET'])

def api_all():

return jsonify(employees)

app.run()

[view raw](https://gist.github.com/ssime-git/2e26c382274e7c897e4364064490748f/raw/42a1a05dc659c2854e89a9a5b9a4c37b94900bf6/Flask_main_2.py)[Flask_main_2.py](https://gist.github.com/ssime-git/2e26c382274e7c897e4364064490748f#file-flask_main_2-py) hosted with ❤ by [GitHub](https://github.com/)

On exécute le code sous Spyder 3 et on met à jour la fenêtre du navigateur, ce qui donne :

![Annuaire internet](https://i0.wp.com/datascientest.com/wp-content/uploads/2022/02/Annuaire.png?w=800&ssl=1)

Pour accéder à l’ensemble des données, il suffit de saisir dans le navigateur l’adresse :

[http://127.0.0.1:5000/api/v1/resources/employees/all](http://127.0.0.1:5000/api/v1/resources/employees/all)

![](https://i0.wp.com/datascientest.com/wp-content/uploads/2022/02/unnamed.png?w=800&ssl=1)

On a utilisé la fonction **jsonify** de Flask. Celle-ci permet de convertir les listes et les dictionnaires au format JSON.

Via la route qu’on a créé, nos données sur les employés sont converties d’une liste de dictionnaires vers le format JSON, avant d’être fournies à l’utilisateur.

À ce stade, nous avons créé une **API fonctionnelle**, bien que limitée.

En effet, en l’état actuel de notre API, les utilisateurs ne peuvent accéder qu’à l’intégralité de nos données; il ne peuvent spécifier de filtre pour trouver des ressources spécifiques.

