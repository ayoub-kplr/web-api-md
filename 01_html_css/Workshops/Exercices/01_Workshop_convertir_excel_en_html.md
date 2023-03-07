Workshop: Convertir excel en html

Exercice 1 : Créer un tableau simple

Créez un tableau HTML simple avec 3 colonnes et 3 lignes contenant les chiffres de 1 à 9.


<table>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr>
    <td>4</td>
    <td>5</td>
    <td>6</td>
  </tr>
  <tr>
    <td>7</td>
    <td>8</td>
    <td>9</td>
  </tr>
</table>

Exercice 2 : Ajouter un titre et une bordure au tableau

Modifiez le tableau créé précédemment en ajoutant un titre et une bordure.

<table border="1">
  <caption>Mon tableau</caption>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr>
    <td>4</td>
    <td>5</td>
    <td>6</td>
  </tr>
  <tr>
    <td>7</td>
    <td>8</td>
    <td>9</td>
  </tr>
</table>

Exercice 3 : Fusionner des cellules de tableau

Créez un tableau HTML avec 3 colonnes et 3 lignes. Fusionnez les cellules de la première colonne et de la première ligne en une seule cellule.

<table border="1">
  <tr>
    <td rowspan="3">1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr>
    <td>4</td>
    <td>5</td>
  </tr>
  <tr>
    <td>6</td>
    <td>7</td>
  </tr>
</table>

Exercice 4 : Ajouter une couleur de fond à une ligne de tableau

Ajoutez une couleur de fond rouge à la deuxième ligne du tableau créé précédemment.

<table border="1">
  <tr>
    <td rowspan="3">1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr style="background-color: red;">
    <td>4</td>
    <td>5</td>
  </tr>
  <tr>
    <td>6</td>
    <td>7</td>
  </tr>
</table>

Exercice 5 : Créer un tableau avec une en-tête

Créez un tableau HTML avec une en-tête contenant deux colonnes "Nom" et "Âge" et deux lignes de données contenant des noms et des âges.

<table border="1">
  <thead>
    <tr>
      <th>Nom</th>
      <th>Âge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John</td>
      <td>25</td>
    </tr>
    <tr>
      <td>Jane</td>
      <td>30</td>
    </tr>
  </tbody>
</table>


