Nous allons apprendre à créer une application Django qui permettra aux utilisateurs de télécharger un fichier Excel et de le visualiser en HTML.

### Étape 1: Installer les dépendances requises

Nous aurons besoin d'installer les bibliothèques suivantes:

-   `django` - Le framework Django
-   `openpyxl` - Une bibliothèque Python pour manipuler les fichiers Excel

Vous pouvez installer ces bibliothèques en utilisant `pip`.

```
pip install django openpyxl
```

### Étape 2: Configurer le projet Django

Créez un projet Django à l'aide de la commande suivante:

```
python manage.py startapp monapplication
```

### Étape 3: Créer un modèle de données

Créez un modèle de données pour stocker les informations du fichier Excel. Par exemple, nous pouvons créer un modèle `ExcelData` avec les champs suivants:

-   `nom` - Le nom du fichier
-   `fichier` - Le fichier Excel
-   `donnees` - Les données du fichier Excel au format JSON

```python
# monapplication/models.py
from django.db import models

class ExcelData(models.Model):
    nom = models.CharField(max_length=255)
    fichier = models.FileField(upload_to='uploads/')
    donnees = models.JSONField(null=True, blank=True)

```

### Étape 4: Créer un formulaire pour télécharger le fichier Excel

Créez un formulaire Django qui permettra aux utilisateurs de télécharger un fichier Excel. Le formulaire devrait avoir un champ de fichier pour sélectionner le fichier Excel.

```python
# monapplication/forms.py
from django import forms

class ExcelUploadForm(forms.Form):
    fichier = forms.FileField()

```

### Étape 5: Écrire une vue pour le formulaire de téléchargement

Créez une vue Django pour le formulaire de téléchargement. La vue doit afficher le formulaire lorsqu'elle est appelée avec une méthode GET, et traiter le formulaire lorsqu'elle est appelée avec une méthode POST.

Lorsque le formulaire est soumis, la vue doit lire les données du fichier Excel à l'aide de la bibliothèque `openpyxl`, convertir les données en JSON et les enregistrer dans le modèle `ExcelData`. Ensuite, la vue doit rediriger l'utilisateur vers une autre vue qui affiche les données du fichier Excel en HTML.

```python
# monapplication/views.py
from django.shortcuts import render, redirect
from openpyxl import load_workbook
from .forms import ExcelUploadForm
from .models import ExcelData

def upload_excel(request):
    if request.method == 'POST':
        form = ExcelUploadForm(request.POST, request.FILES)
        if form.is_valid():
            fichier = form.cleaned_data['fichier']
            workbook = load_workbook(fichier)
            worksheet = workbook.active
            rows = list(worksheet.rows)
            header = [cell.value for cell in rows[0]]
            data = []
            for row in rows[1:]:
                row_data = {}
                for i, cell in enumerate(row):
                    row_data[header[i]] = cell.value
                data.append(row_data)
            excel_data = ExcelData(
                nom=fichier.name,
                fichier=fichier,
                donnees=data
            )
            excel_data.save()
            return redirect('view_excel', pk=excel_data.pk)
    else:
        form = ExcelUploadForm()
    return render(request, 'monapplication/upload_excel.html', {'form': form})

def view_excel(request, pk):
    excel_data = ExcelData.objects.get(pk=pk)
    return render(request, 'monapplication/view_excel.html', {'excel_data': excel_data})


```


Voici un exemple de base de modèle `base.html` qui utilise Bootstrap pour rendre notre application plus esthétique et conviviale:

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>{% block title %}Titre par défaut{% endblock %}</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0/dist/css/bootstrap.min.css">
</head>
<body>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Mon application</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <a class="nav-link" href="{% url 'home' %}">Accueil</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="{% url 'upload_excel' %}">Télécharger un fichier Excel</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <div class="container mt-5">
    {% block content %}{% endblock %}
  </div>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>

```

-   Nous utilisons Bootstrap en important sa feuille de style CSS et son fichier JavaScript à partir des CDN.
-   Nous utilisons la classe `navbar` de Bootstrap pour afficher une barre de navigation en haut de chaque page.
-   Nous définissons une marque `navbar-brand` et un bouton de basculement pour afficher les éléments de navigation sur les petits écrans.
-   Nous utilisons la classe `container` de Bootstrap pour envelopper le contenu principal de chaque page et créer une marge supérieure.
-   Nous utilisons la balise `{% block content %}` pour inclure le contenu spécifique à chaque page à l'intérieur du conteneur.

Un exemple de modèle pour la vue `upload_excel` qui affiche un formulaire permettant de télécharger un fichier Excel:

```html
<!-- templates//upload_excel.html -->
{% extends 'base.html' %}

{% block content %}
  <h2>Télécharger un fichier Excel</h2>
  <form method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Télécharger</button>
  </form>
{% endblock %}

```

-   Nous étendons la base de modèle `base.html` qui contiendra les éléments communs à toutes les pages de notre application.
-   Nous utilisons la balise `{% block content %}` pour définir le contenu spécifique à cette page.
-   Nous affichons un titre `h2` et un formulaire permettant de télécharger un fichier Excel en utilisant la méthode POST et l'encodage `multipart/form-data`.
-   Nous incluons un jeton CSRF pour protéger notre formulaire contre les attaques CSRF.
-   Nous affichons le champ de fichier du formulaire à l'aide de la méthode `form.as_p`.
-   Nous affichons un bouton pour soumettre le formulaire.

Un exemple de modèle pour la vue `view_excel` qui affiche le contenu du fichier Excel téléchargé sous forme de tableau HTML en utilisant les données fournies par la vue:

```html
<!-- monapplication/templates/monapplication/view_excel.html -->
{% extends 'base.html' %}

{% block content %}
  <h2>{{ excel_file.name }}</h2>
  <table class="table">
    <thead>
      <tr>
        {% for column in columns %}
          <th>{{ column }}</th>
        {% endfor %}
      </tr>
    </thead>
    <tbody>
      {% for row in rows %}
        <tr>
          {% for value in row %}
            <td>{{ value }}</td>
          {% endfor %}
        </tr>
      {% endfor %}
    </tbody>
  </table>
{% endblock %}

```

-   Nous étendons la base de modèle `base.html` pour inclure les éléments communs à toutes les pages de notre application.
-   Nous utilisons la balise `{% block content %}` pour définir le contenu spécifique à cette page.
-   Nous affichons le nom du fichier Excel téléchargé à l'aide de la propriété `name` fournie par la vue.
-   Nous affichons les données du fichier Excel téléchargé sous forme de tableau HTML en utilisant une boucle `for` pour parcourir les colonnes et les lignes de la table.
-   Nous affichons les en-têtes de colonne en utilisant une boucle `for` pour parcourir les noms de colonnes stockés dans la liste `columns`.
-   Nous affichons les valeurs de chaque ligne en utilisant une boucle `for` imbriquée pour parcourir les valeurs de chaque ligne stockées dans la liste `rows`.

