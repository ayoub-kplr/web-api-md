# Création d'une application de suivi de dépenses avec Django et Chart.js

Dans ce guide, nous allons vous montrer comment créer une application de suivi de dépenses en utilisant Django, un framework web Python, et Chart.js, une bibliothèque JavaScript pour la visualisation de données.

## Étape 1 : Installation de Django

La première étape consiste à installer Django sur votre machine. Pour ce faire, vous pouvez suivre les instructions de la documentation officielle de Django pour l'installation : [https://docs.djangoproject.com/fr/3.2/intro/install/](https://docs.djangoproject.com/fr/3.2/intro/install/)

Assurez-vous également d'avoir une installation récente de Python sur votre machine.

## Étape 2 : Création d'un projet Django

Une fois que vous avez installé Django, vous pouvez créer un nouveau projet en exécutant la commande suivante dans votre terminal :



```bash
$ django-admin startproject nom_du_projet
````

Cette commande crée un nouveau projet Django avec le nom spécifié dans le répertoire actuel.

## Étape 3 : Création d'une application Django

Une fois que vous avez créé un projet Django, vous pouvez créer une application en exécutant la commande suivante dans le répertoire racine de votre projet :



````bash
python manage.py startapp nom_de_l_application
````

Cette commande crée une nouvelle application Django avec le nom spécifié.

## Étape 4 : Configuration de la base de données

Maintenant que nous avons notre application Django, nous devons configurer la base de données. Django prend en charge plusieurs bases de données, mais pour cet exemple, nous allons utiliser SQLite, qui est facile à configurer.

Dans le fichier settings.py de votre projet Django, ajoutez la configuration de la base de données suivante :

python

```python
DATABASES = {
			 'default': {
			          'ENGINE': 
			          'django.db.backends.sqlite3',
			           'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
			}
 }
````

Cette configuration indique à Django d'utiliser SQLite comme moteur de base de données par défaut et de stocker le fichier de base de données dans le répertoire de base de votre projet.

## Étape 5 : Définition du modèle de données

Maintenant que nous avons configuré la base de données, nous pouvons définir le modèle de données pour notre application. Le modèle de données est une représentation des tables de la base de données et des relations entre elles.

Dans le fichier models.py de votre application Django, ajoutez le modèle de données suivant :

python

````python 
from django.db import models

class Expense(models.Model):
	name = models.CharField(max_length=255)
	amount = models.DecimalField(max_digits=10, decimal_places=2)
	date = models.DateField()
````

Ce modèle de données définit une table de base de données Expense avec trois champs : name, amount et date.

## Étape 6 : Création des vues

Maintenant que nous avons défini le modèle de données, nous pouvons créer des vues pour afficher et ajouter des dépenses.

Dans le fichier views.py de votre application Django, ajoutez les vues suivantes :

```python
from django.shortcuts import render, redirect
from .models import Expense

def expense_list(request):
	expenses = Expense.objects.all()
	return render(request, 'expense_list.html', {'expenses': expenses})
def expense_add(request):     
	if request.method == 'POST':         
		name = request.POST['name']         
		amount = request.POST['amount']         
		date = request.POST['date']         
		Expense.objects.create(name=name` `, amount=amount, date=date)     
		return redirect('expense_list') 
	return render(request, 'expense_add.html')
````


La vue expense_list récupère toutes les dépenses à partir de la base de données et les affiche dans un template HTML appelé expense_list.html.  La vue expense_add permet à l'utilisateur d'ajouter une nouvelle dépense. Elle vérifie si la requête est une requête POST (c'est-à-dire si l'utilisateur a soumis un formulaire), récupère les données du formulaire et crée une nouvelle dépense dans la base de données. Si la requête est une requête GET, elle affiche un formulaire vide pour permettre à l'utilisateur d'ajouter une nouvelle dépense.  ## Étape 7 : Création des templates  Maintenant que nous avons défini les vues, nous pouvons créer les templates HTML correspondants. Les templates HTML définissent la structure et le style de la page web.  Dans le répertoire templates de votre application Django, créez deux fichiers HTML : expense_list.html et expense_add.html.  Le template expense_list.html doit afficher toutes les dépenses dans un tableau. Voici un exemple de code HTML pour le template expense_list.html :  
`````html
<table>
<thead>
<tr>
<th>Nom</th>
<th>Montant</th> 
<th>Date</th>
</tr>
</thead>
<tbody>
{% for expense in expenses %}
<tr>
<td>{{ expense.name }}</td>
<td>{{ expense.amount }}</td>
<td>{{ expense.date }}</td>
</tr>
{% endfor %}
</tbody> </table>
`````

Le template expense_add.html doit afficher un formulaire pour permettre à l'utilisateur d'ajouter une nouvelle dépense. Voici un exemple de code HTML pour le template expense_add.html :



```html
<form method="post">     
	{% csrf_token %}     
	<label for="name">Nom :</label>     
	<input type="text" name="name" required>     
	<label for="amount">Montant :</label>     
	<input type="number" name="amount" step="0.01" required>     
	<label for="date">Date :</label>     
	<input type="date" name="date" required>     
	<button type="submit">Ajouter</button> 
</form>
````

## Étape 8 : Visualisation des données avec Chart.js

Maintenant que nous avons une application de suivi de dépenses fonctionnelle, nous pouvons utiliser Chart.js pour visualiser les données.

Tout d'abord, vous devez inclure la bibliothèque Chart.js dans votre template HTML. Vous pouvez télécharger la bibliothèque depuis le site officiel de Chart.js et l'inclure dans votre template HTML :

html

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
````

Ensuite, vous pouvez créer un graphique dans votre template HTML en utilisant les données récupérées à partir de la base de données.

Voici un exemple de code HTML pour créer un graphique en barres avec Chart.js :

html

```html
<canvas id="myChart"></canvas>
<script>
     var ctx = document.getElementById('myChart').getContext('2d');
     var data = {
        labels: [{% for expense in expenses %}"{{ expense.name }}",{% endfor %}],         datasets: [{
        label: "Dépenses",
        backgroundColor: 'rgba(255, 99, 132, 0.2)',
		borderColor: 'rgba(` `255, 99, 132, 1)',
		borderWidth: 1,
data: [{% for expense in expenses %}{{ expense.amount }},{% endfor %}],
}] };
var options = {
	scales: {
	         yAxes: [{
	                      ticks: {
	                      beginAtZero: true
	                      }
	                }]
	                } };
var myChart = new Chart(ctx, {
type: 'bar',
data: data,
options: options });

</script> ```

Ce code HTML crée un graphique en barres avec les noms des dépenses sur l'axe X et les montants des dépenses sur l'axe Y.

## Étape 9 : Conclusion

Dans ce tutoriel, nous avons créé une application de suivi de dépenses en utilisant Django et Chart.js. Nous avons créé une base de données pour stocker les dépenses, défini des vues pour afficher et ajouter des dépenses, créé des templates HTML pour afficher les données et visualisé les données avec Chart.js.

Vous pouvez maintenant personnaliser cette application en ajoutant des fonctionnalités supplémentaires telles que la suppression et la modification des dépenses, l'ajout de catégories de dépenses, la gestion des utilisateurs, etc.