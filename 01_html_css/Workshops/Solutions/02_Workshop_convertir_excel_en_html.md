````html
<style>

th {

    background-color: grey;

    color: white;

    padding: 5px;

    border: 1px solid black;

    border-radius: 10px;

  }

  td {

    background-color: white;

    padding: 5px;

    border: 1px solid black;

    border-radius: 10px;

  }

  table {

    border-collapse: collapse;

  }

</style>
````

Ce code CSS définit les styles pour une table HTML.

-   Les éléments `th` (cellules d'en-tête de table) ont un fond gris (`background-color: grey`) avec une police blanche (`color: white`). Ils ont également un padding de 5px (`padding: 5px`), une bordure de 1px solide noire (`border: 1px solid black`) et un rayon de bordure de 10px (`border-radius: 10px`).
    
-   Les éléments `td` (cellules de données de table) ont un fond blanc (`background-color: white`). Ils ont également un padding de 5px (`padding: 5px`), une bordure de 1px solide noire (`border: 1px solid black`) et un rayon de bordure de 10px (`border-radius: 10px`).
    
-   La propriété `border-collapse: collapse` sur l'élément `table` indique que les bordures des cellules de la table doivent être fusionnées en une seule ligne pour une apparence plus nette.

La partie HTML:

````html
<table>

 <tr>

  <td>Employee ID</td>

  <td>Full Name</td>

  <td>Job Title</td>

  <td>Department</td>

  <td>Business Unit</td>

  <td>Gender</td>

  <td>Ethnicity</td>

  <td>Age</td>

  <td>Hire Date</td>

  <td>Annual Salary</td>

  <td>Bonus %</td>

  <td>Country</td>

  <td>City</td>

  <td>Exit Date</td>

 </tr>

 <tr>

  <td>E02002</td>

  <td>Kai Le</td>

  <td>Controls Engineer</td>

  <td>Engineering</td>

  <td>Manufacturing</td>

  <td>Male</td>

  <td>Asian</td>

  <td>47</td>

  <td>2/5/2022</td>

  <td>92368</td>

  <td>0</td>

  <td>United States</td>

  <td>Columbus</td>

  <td>&nbsp;</td>

 </tr>

 <tr>

  <td>E02003</td>

  <td>Robert Patel</td>

  <td>Analyst</td>

  <td>Sales</td>

  <td>Corporate</td>

  <td>Male</td>

  <td>Asian</td>

  <td>58</td>

  <td>10/23/2013</td>

  <td>45703</td>

  <td>0</td>

  <td>United States</td>

  <td>Chicago</td>

  <td>&nbsp;</td>

 </tr>
 </table>
````

