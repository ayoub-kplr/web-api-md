# Solution

Si le fichier Excel est stocké dans le même répertoire que votre script Python, vous pouvez utiliser cette ligne de code :

````python
df = pd.read_excel('nom_du_fichier.xlsx')
````

Si le fichier Excel est stocké dans un sous-dossier nommé "donnees", vous pouvez utiliser cette ligne de code :

````python
df = pd.read_excel('donnees/nom_du_fichier.xlsx')

````

Voici le code complet qui utilise ces étapes :

````python
import pandas as pd

# Charge la feuille de calcul Excel en tant que dataframe
df = pd.read_excel('chemin_vers_fichier.xlsx')

# Convertir le dataframe en HTML
table_html = df.to_html()

# Écrire le HTML dans un fichier
with open('fichier.html', 'w') as f:
    f.write(table_html)

````

