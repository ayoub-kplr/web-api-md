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
            excel_data

```

