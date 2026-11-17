# 6. v-for - Parcourir une liste

`v-for` permet de parcourir une collection (liste) de données et de générer automatiquement plusieurs éléments HTML.

dans le fichier index.html, *pour la liste de tableau de textes*
`products` est le tableau défini dans data()
`product` (sans S) représente l'élément courant du tableau.
`in`indique que Vue doit parcourir la collection.
Vue génère un élément <li> pour chaque produit.

*pour la liste de tableau d'objets*
`:key="product.id"` est un attribut qui permet à Vue d'identifier chaque élément de la liste et de mettre à jour DOM
*une liste de tableau doit avoir un nom*

*À retenir :* v-for est particulièrement utile pour afficher des produits, des utilisateurs, des articles ou toute autre liste dynamique.