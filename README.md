# 9. Exemple pratique : créer une petite boutique
Maintenant que vous connaissez les principales fonctionnalités, voici un exemple qui les combine dans une seule application.

Vous allez créer une petite page présentant des produits avec :

* Une liste de produits réactive
  

* Un produit a un prix et un stock. (product.price product.stock)

* Un champ de recherche (v-model)

* Un bouton pour ajouter des produits au panier (button@click)

* Un total calculé automatiquement (computed)

* Un affichage conditionnel selon la disponibilité (v-if)



h1 titre principal
h2 titre d'une section (nos produits)
h3 sous titre ( le nom d'un produit)
p paragraphe ou info textuelle (le pri ou le stock)
label pour nom d'un champ de formulaire (recherche)
input champ dans lequel on saisit
button pour action à délencher
div regroupe des éléments (le nom, le prix et le bouton d'un même produits)


Application
├── Titre principal
├── Recherche : label + input
└── Section des produits
    ├── Titre de section
    └── Bloc d’un produit, répété avec v-for
        ├── Nom
        ├── Prix
        ├── Stock
        └── Bouton

je dois écrire un seul bloc de produit dans mon html, c'est v-for qui va le répéter pour chaque produit. 

aussi pour relier un lable $ son champ les valeurs doivent correspondre: label for=recherche et input id=recherche
id="recherche" → identifie le champ dans la page.
v-model="search" → relie sa valeur à la donnée Vue search.

filter() est une fonction qui s'utilise comme:
nombres.filter(nombre => nombre > 5)
ça veut dire "garde just eles nombres supérieurs à 5"

pour la recherche filtrée, on doit mettre la fonction dans computed car ça va dépendre de products et de search

this.products c'est le tableau de l'app
this.search c'est la recherche de l'app

includes renvoie true ou false

for (const product of this.products)
=> this.products va chercher le tableau déclaré dans data()
=> product est crée par const product dans la boucle. il v représenter chaque produit à la fois.


concernant le css de la prof;
y a des styles qui s appliquent grâce à des classes.
y'a product-card pour chaque bloc de produit
cart pour le bloc du panier
cart-total pour le paragraphe du total.
