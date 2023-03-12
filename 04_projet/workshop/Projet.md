![Project tree](https://user-images.githubusercontent.com/123752166/224330105-f14ada9a-52f0-4f9a-a655-27079f2bee11.png "Project Tree")

## app.py

```python
from application import app

if __name__ == "__main__":
    app.run()

```

Ce fichier `app.py` est le point d'entrée de votre application Flask. Il importe l'instance de l'application `app` depuis le module `application` et lance l'application en appelant la méthode `run()` de l'objet `app`. La méthode `run()` démarre le serveur web pour que votre application puisse répondre aux requêtes des clients.

La condition `if __name__ == "__main__":` est utilisée pour s'assurer que le code ne sera exécuté que lorsque le fichier est exécuté directement et non lorsqu'il est importé en tant que module dans un autre fichier. Cela permet à ce fichier `app.py` d'être exécuté en tant que script et de démarrer l'application.


# Ficher application/models.py

```python
url = 'DATABASE_URL'
engine = sqa.create_engine(url, echo=True)
Base = declarative_base() #
# create a Session 
Session = sessionmaker(bind=engine)
SESSION = Session()
```

Dans ce fichier nommé `models.py`, les dépendances sont importées, notamment SQLAlchemy qui est utilisé pour effectuer des opérations de base de données.

La variable `url` contient l'URL de la base de données que le script se connectera. `engine` est l'instance SQLAlchemy qui est créée en utilisant l'URL de la base de données.

Ensuite, `Base` est l'instance `declarative_base()` de SQLAlchemy. Cela est utilisé pour définir des classes de modèles qui seront utilisées pour créer des tables dans la base de données.

La classe `Session` est créée en utilisant la fonction `sessionmaker()` de SQLAlchemy, et est liée à l'instance `engine`. Cette classe est utilisée pour créer des sessions de base de données qui seront utilisées pour effectuer des opérations sur la base de données.

Enfin, l'objet `SESSION` est créé en instanciant la classe `Session()`. Cet objet sera utilisé pour effectuer des opérations sur la base de données tout au long de l'application.

```python
class IncomeExpenses(Base):  
    __tablename__ = 'api_sold'  
  
    id = sqa.Column(sqa.Integer, primary_key=True)  
    type = sqa.Column(sqa.String(30), default='income', nullable=False)  
    category = sqa.Column(sqa.String(30), default='rent', nullable=False)  
    date = sqa.Column(sqa.DateTime, default=datetime.utcnow, nullable=False)  
    amount = sqa.Column(sqa.Integer, nullable=False)  
  
    def __repr__(self):  
        return "IncomeExpenses(id = {}, type = {}, category = {}, date = {}, amount = {})".format(  
            self.id,  
            self.type,  
            self.category,  
            self.date,  
            self.amount)
```

Cette classe `IncomeExpenses` hérite de la classe `Base` qui est une instance de `declarative_base()` de SQLAlchemy. Cette classe est utilisée pour définir la table `api_sold` dans la base de données.

Les colonnes de la table sont définies en utilisant la classe `Column()` de SQLAlchemy. La colonne `id` est définie comme une clé primaire avec le type `Integer`. Les autres colonnes sont également définies avec des types de données spécifiques. Par exemple, la colonne `type` est une chaîne de caractères avec une longueur maximale de 30 caractères et est définie avec une valeur par défaut de 'income' si rien n'est spécifié.

La méthode `__repr__()` est définie pour afficher les données de la table de manière plus lisible. Elle renvoie une chaîne de caractères contenant les valeurs des différentes colonnes pour chaque instance de la classe.

```python
class User(Base, UserMixin):  
    __tablename__ = 'user'  
    id = sqa.Column(sqa.Integer, primary_key=True)  # primary keys are required by SQLAlchemy  
    email = sqa.Column(sqa.String(100), unique=True)  
    password = sqa.Column(sqa.String(100))  
    name = sqa.Column(sqa.String(1000))  
  
    def __repr__(self):  
        return "User(id = {}, email = {}, password = {}, name = {})".format(  
            self.id,  
            self.email,  
            self.password,  
            self.name)
```

La classe User est un modèle SQLAlchemy qui représente une table dans la base de données. Elle hérite de la classe Base de SQLAlchemy, ainsi que de la classe UserMixin de Flask-Login.

La classe User contient les colonnes suivantes :

-   id: un entier qui représente l'identifiant de l'utilisateur. Il s'agit d'une clé primaire de la table, donc elle est unique pour chaque utilisateur.
-   email: une chaîne de caractères qui représente l'adresse email de l'utilisateur. Cette colonne est unique, ce qui signifie qu'il ne peut y avoir qu'un seul utilisateur avec la même adresse email.
-   password: une chaîne de caractères qui représente le mot de passe de l'utilisateur. Ce champ n'est pas unique et n'est pas nul, il est donc obligatoire.
-   name: une chaîne de caractères qui représente le nom de l'utilisateur. Ce champ n'est pas unique et n'est pas nul, il est donc obligatoire.

La méthode **repr**() est utilisée pour représenter l'objet User sous forme de chaîne de caractères. Elle renvoie une chaîne qui contient les valeurs de toutes les colonnes de l'utilisateur.

# Fichier application/form.py

```python
class UserInputForm(FlaskForm):  
    type = SelectField('Type', validators=[DataRequired()],  
                       choices=[('income', 'income'),  
                                 ('expense','expense')  
                                ])  
    category = SelectField('Category', validators=[DataRequired()],  
                       choices=[('rent', 'rent'),  
                                 ('salary','salary'),  
                                 ('investement','investement'),  
                                 ('side_hustle','side_hustle')  
                                ])  
    amount = IntegerField('Amount', validators=[DataRequired()])  
    submit=SubmitField("Generate Report")
```

Voici une classe FlaskForm appelée UserInputForm qui inclut des champs pour le type, la catégorie et le montant d'un revenu ou d'une dépense. Le champ type est un SelectField avec des choix de "revenu" et "dépense". Le champ catégorie est également un SelectField avec des choix de "loyer", "salaire", "investissement" et "travail indépendant". Le champ montant est un IntegerField où l'utilisateur saisit le montant de l'élément. Enfin, il y a un bouton soumettre que l'utilisateur clique pour soumettre le formulaire et générer un rapport.


# Fichier application/routes.py

Ce fichier contient les routes de l'application Flask, qui sont des URL qui mènent à différentes vues web. Voici une explication détaillée de chaque fonction :

```python
from application import app
from application.models import engine, SESSION
from flask_login import login_required, current_user
from flask import *
from . import app
from application.form import UserInputForm
from application.models import IncomeExpenses, url
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.sql import func
import json
from werkzeug.security import check_password_hash
from application.models import User
from flask_login import login_user, logout_user, current_user

```

La première partie de ce code importe les modules nécessaires.

```python
@app.route('/')
@login_required
def index():
    entries = SESSION.query(IncomeExpenses).order_by(
        IncomeExpenses.date.desc()).all()
    return render_template('index.html', title='index', entries=entries)

```

-   Cette fonction `index()` est la page d'accueil. Elle est décorée par `@app.route('/')` ce qui signifie que lorsque l'utilisateur accède à la racine de l'application, cette fonction sera exécutée. Elle nécessite également que l'utilisateur soit connecté grâce à `@login_required`.
-   Elle utilise la session de base de données pour extraire toutes les entrées de la table `IncomeExpenses` triées par date décroissante, puis les passe à un fichier HTML `index.html` pour les afficher.

```python

@app.route('/add', methods=["GET", "POST"])
@login_required
def add_expense():
    form = UserInputForm()
    if form.validate_on_submit():
        entry = IncomeExpenses(type=form.type.data, amount=form.amount.data,
                               category=form.category.data)
        try:
            SESSION.add(entry)
            SESSION.commit()
            flash("Sucessful entry", "success")
        except:
            SESSION.rollback()
            flash("Error", "danger")
        return redirect(url_for('index'))
    return render_template("add.html", title='add', form=form)

```

-   Cette fonction `add_expense()` est la page d'ajout de dépenses. Elle est décorée par `@app.route('/add', methods=["GET", "POST"])` ce qui signifie qu'elle est appelée lorsqu'un utilisateur accède à `/add` et peut être atteinte soit par une requête GET, soit par une requête POST.
-   La fonction affiche un formulaire qui utilise la classe `UserInputForm` importée à partir de `application/form.py`. Lorsque l'utilisateur soumet le formulaire, les données sont validées en utilisant la méthode `validate_on_submit()` de la classe `UserInputForm()`. Si les données sont valides, une nouvelle instance `entry` de la classe `IncomeExpenses` est créée avec les données du formulaire, ajoutée à la base de données, puis enregistrée avec `SESSION.commit()`. Si une erreur se produit lors de l'ajout d'une entrée, la session est annulée avec `SESSION.rollback()` et un message d'erreur est affiché.
-   Si l'ajout est réussi, l'utilisateur est redirigé vers la page d'accueil à l'aide de la fonction `redirect(url_for('index'))`. Si le formulaire n'a pas été validé, il est affiché avec la fonction `render_template()`.

### La fonction `delete()`
La fonction `delete()` est chargée de supprimer une dépense existante de la base de données. La fonction prend un paramètre `entry_id`, qui est l'ID de la dépense à supprimer.

```python
@app.route('/delete/<int:entry_id>')
@login_required
def delete(entry_id):
    try:
        engine.execute("DELETE FROM api_sold WHERE id = {}".format(entry_id))
        flash("Deleting was success", "success")
    except:
        SESSION.rollback()
        flash("Error", "danger")
    return redirect(url_for('index'))

```

La fonction tente d'abord d'exécuter une instruction SQL `DELETE`
pour supprimer la dépense avec l'`entry_id` spécifié de la table `api_sold` de la base de données. Si la suppression réussit, un message de réussite est envoyé à l'utilisateur et il est redirigé vers la page d'index. Si la suppression échoue, un message d'erreur est envoyé à l'utilisateur et il reste sur la page d'index.

Dans l'ensemble, ces deux fonctions offrent la possibilité d'ajouter et de supprimer des dépenses dans l'application Web.

### Dashboard

```python
  
@app.route('/dashboard')  
@login_required  
def dashboard():  
    SESSION.rollback()  
    income_vs_expense = SESSION.query(func.sum(IncomeExpenses.amount), IncomeExpenses.type).group_by(  
        IncomeExpenses.type).order_by(IncomeExpenses.type).all()  
  
    category_comparison = SESSION.query(func.sum(IncomeExpenses.amount), IncomeExpenses.category).group_by(  
        IncomeExpenses.category).order_by(IncomeExpenses.category).all()  
  
    dates = SESSION.query(func.sum(IncomeExpenses.amount), IncomeExpenses.date).group_by(  
        IncomeExpenses.date).order_by(IncomeExpenses.date).all()  
  
    income_category = []  
    labels = []  
    for amount, label in category_comparison:  
        income_category.append(amount)  
        labels.append(label)  
  
    income_expense = []  
    for total_amount, _ in income_vs_expense:  
        income_expense.append(total_amount)  
  
    over_time_expenditure = []  
    dates_labels = []  
    for amount, date in dates:  
        over_time_expenditure.append(amount)  
        dates_labels.append(date.strftime("%m-%d-%y"))  
  
    return render_template("dashboard.html", title='dashboard',  
                           income_vs_expense=json.dumps(income_expense),  
                           income_category=json.dumps(income_category),  
                           income_category_labels=json.dumps(labels),  
                           over_time_expenditure=json.dumps(over_time_expenditure),  
                           dates_labels=json.dumps(dates_labels),  
                           )
```

Cette fonction, nommée `dashboard()`, est une vue Flask qui est décorée avec l'annotation `@app.route('/dashboard')`. Elle nécessite également que l'utilisateur soit connecté, donc elle est décorée avec l'annotation `@login_required` fournie par Flask-Login.

La fonction commence par appeler `SESSION.rollback()` pour annuler toutes les transactions en suspens sur la session SQLAlchemy, afin de s'assurer qu'elle commence avec une session propre.

Ensuite, elle exécute trois requêtes SQL distinctes sur la table `IncomeExpenses` à l'aide de la session SQLAlchemy `SESSION`. Chacune de ces requêtes utilise la fonction `func.sum()` de SQLAlchemy pour agréger les montants de dépenses ou de revenus par type, catégorie ou date, en les regroupant selon le champ correspondant et en les triant par ordre croissant.

Les résultats de chaque requête sont stockés dans des variables distinctes : `income_vs_expense`, `category_comparison` et `dates`.

Ensuite, la fonction prépare les données pour le rendu du template. Elle crée trois listes `income_category`, `labels` et `over_time_expenditure`, qui contiennent les agrégats de la requête de comparaison de catégorie, la liste de labels pour cette même requête, et les agrégats de la requête par date, respectivement.

Ces listes sont utilisées pour construire des dictionnaires JSON qui seront passés en tant que variables de contexte pour le rendu du template. Les variables sont `income_vs_expense`, `income_category`, `income_category_labels`, `over_time_expenditure` et `dates_labels`. Les deux premières sont des données agrégées de la requête `income_vs_expense` et de la requête de comparaison de catégorie, les deux suivantes sont des labels pour ces agrégats, et la dernière est la liste de dates formatées pour l'affichage sur le graphique.

Enfin, la fonction appelle `render_template()` pour renvoyer le template `dashboard.html` avec les variables de contexte fournies. Le titre de la page est également spécifié comme `'dashboard'`.

```python
@app.route('/profile')  
@login_required  
def profile():  
    return render_template('profile.html', name=current_user.name)
```
Cette fonction, nommée `profile()`, est une vue Flask qui est décorée avec l'annotation `@app.route('/profile')`. Elle nécessite également que l'utilisateur soit connecté, donc elle est décorée avec l'annotation `@login_required` fournie par Flask-Login.

La fonction ne fait qu'appeler `render_template()` pour renvoyer le template `profile.html` avec une variable de contexte nommée `'name'` qui contient le nom de l'utilisateur actuellement connecté, stocké dans l'objet `current_user` fourni par Flask-Login.

```python
@app.route('/login')  
def login():  
    return render_template('login.html')
```
Cette fonction, nommée `login()`, est une vue Flask qui est décorée avec l'annotation `@app.route('/login')`.

La fonction ne fait qu'appeler `render_template()` pour renvoyer le template `login.html`. Ce template contiendra le formulaire de connexion pour l'utilisateur.

```python
  
@app.route('/login', methods=['POST'])  
def login_post():  
    # login code goes here  
    email = request.form.get('email')  
    password = request.form.get('password')  
    remember = True if request.form.get('remember') else False  
    SESSION.rollback()  
    # user = User.query.filter_by(email=email).first()  
    user = SESSION.query(User).filter(User.email == email).first()  
  
    # check if the user actually exists  
    # take the user-supplied password, hash it, and compare it to the hashed password in the database    if not user or not (user.password == password):  # check_password_hash(user.password, password):  
        flash('Please check your login details and try again.')  
        return redirect(url_for('login'))  # if the user doesn't exist or password is wrong, reload the page  
  
    login_user(user, remember=remember)  
    # if the above check passes, then we know the user has the right credentials  
    return redirect(url_for('profile'))
```

Cette fonction, nommée `login_post()`, est une vue Flask qui est décorée avec l'annotation `@app.route('/login', methods=['POST'])`. Elle répond à la soumission du formulaire de connexion par l'utilisateur.

La fonction commence par extraire les champs `email`, `password`, et `remember` du formulaire soumis par l'utilisateur en utilisant la méthode `request.form.get()` de Flask.

Ensuite, la fonction effectue une requête pour trouver un utilisateur ayant l'adresse email spécifiée dans la base de données. Cela est réalisé en utilisant l'objet `SESSION` pour interroger la table `User`, et en filtrant les résultats pour retourner uniquement l'utilisateur avec l'adresse email spécifiée.

La fonction vérifie ensuite que l'utilisateur existe et que le mot de passe fourni correspond à celui stocké dans la base de données. Si c'est le cas, la fonction utilise la fonction `login_user()` de Flask-Login pour connecter l'utilisateur.

Si l'authentification échoue, un message d'erreur est affiché et l'utilisateur est redirigé vers la page de connexion avec la fonction `redirect()` de Flask. Si l'authentification réussit, l'utilisateur est redirigé vers la page de profil avec la fonction `redirect()` de Flask.
```python
@app.route('/logout')  
@login_required  
def logout():  
    logout_user()  
    return redirect(url_for('index'))
```

Cette fonction, nommée `logout()`, est une vue Flask qui est décorée avec l'annotation `@app.route('/logout')`. Elle répond à la soumission de la page de déconnexion par l'utilisateur.

La fonction commence par utiliser la fonction `logout_user()` de Flask-Login pour déconnecter l'utilisateur actuellement connecté.

Ensuite, la fonction redirige l'utilisateur vers la page d'accueil en utilisant la fonction `redirect()` de Flask.