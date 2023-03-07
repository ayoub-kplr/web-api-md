# Workshop: automatiser la conversion.

1.  Importez la bibliothèque Pandas en utilisant l'instruction d'importation `import pandas as pd`. Cela permettra d'utiliser les fonctions de Pandas dans votre script.
    
2.  Chargez la feuille de calcul Excel en tant que dataframe en utilisant la méthode `pd.read_excel()`. Assurez-vous que le chemin d'accès du fichier est correct. Pour cela, vous pouvez spécifier le nom du fichier Excel, le chemin complet du fichier, ou le chemin relatif à votre script Python.
    
    Par exemple, si le fichier Excel est stocké dans le même répertoire que votre script Python.

1.  Utilisez la méthode `to_html()` pour convertir le dataframe en HTML. Cette méthode prend en charge plusieurs options qui vous permettent de personnaliser le formatage du HTML généré. Par exemple, vous pouvez spécifier la largeur de la table en pixels, inclure ou exclure les en-têtes de colonnes, et bien plus encore.
    
    Par défaut, `to_html()` génère un tableau HTML simple sans style CSS. Pour personnaliser l'apparence de la table HTML, vous pouvez utiliser des styles CSS. Vous pouvez ajouter une balise `<style>` dans le fichier HTML et y définir les styles CSS, puis référencer cette balise dans la balise `<head>` de la table HTML.
    
2.  Écrivez le HTML dans un fichier en utilisant la méthode `write()`. Cette méthode prend en entrée le contenu HTML à écrire dans le fichier, ainsi que le chemin d'accès du fichier. Assurez-vous que le chemin d'accès est correct et que vous avez les permissions nécessaires pour écrire dans le fichier.