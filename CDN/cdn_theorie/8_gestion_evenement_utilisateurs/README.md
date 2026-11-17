# 8. Gestion des événements – Réagir aux actions de l'utilisateur

une application web doit pouvroi réagir aux actions de ses utilisateurs que ce soit:
cliquer sur un bouton, une saisie au claiver, une séléection d'option ou la soumission d'un formulaire.

Vue utilise la directive v-on pour gérer les évéenements. OU @


@click	clic sur un élément
@input	modification d'un champ
@change	changement de valeur
@submit	soumission d'un formulaire
@keyup	relâchement d'une touche
@mouseover	passage de la souris


pour créer un compteru de clique on va rajouter dan resultat clicks=0 puis dans methods on créer la methode increment()  { this.click++} et dans l'html on va créer un paragraphe nombre de clicks {{clicks}} et un bouton @click="increment"

Pour transmettre des paramètres ànue méthode;
addProduct(name)  alert("Produit ajouté: " + name) et on rajoute le @click dans button