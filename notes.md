Dans src/App.vue

**Dans template:**
<form>
<div>
<label>
<input>
<button>


**Dans script:**
`export default { name: 'App', data()}`



**Infos:**
`watch` est une propriété qui surveille les changements.
<form> représente le formulaire
<label> indique le nom du champ
<inpu> permet de saisir une valeur
<button> permet de soumettre le forumulaire
`@submit` écoute l'événement de soumission
`.prevent` empêche le rechargement de la page.
`v-if` permet d'afficher un élément uniquement lorsqu'une condition est vraie.

<form `@submit.prevent="envoyerFormulaire">` appelle une méthode Vue qu'on va créer.
=> ça va empêcher la page de recharger pendant qu'elle reçoit la soumission du formulaire, en appelant la méthode envoyerFormulaire.

**Relier les champs avec v-model:**
`v-model=""` va dans le input entre type et placeholder. il petmet de synchroniser automatiquement le champ HTML avec une propriété. Si l'utilisateur écrit Marie, alors prenom dans data() est changé en Marie.

**Puis, afficher les données saisies dans le template html, après le formulaire**
avec h3 et des p dans un div.

**Gérer l'envoi du formulaire - intecepter la soumussion du formulaire**
@submit.prevent="envoyerFormulaire"
**Créer la méthode dans methods dans script**
`methods: {}`
`alert()` va afficher une notification.
`this.` est pour accéder aux données du composant. définit dans data(), et uniquement data().

**Ajouter différents champs de types différents**
`type="email"`          (ajouter dans data() { email: ''})
`type="password"` v-model="motDePasse" motDePasse sera dans data() et password est un attribut vue?

**Liste déroulante select** c'est pour choisir plusieurs options.
`<select v-model="pays">`
    `<option value="">miaou</option>`
**Boutons radio** pour choisir une seule option
<div class="form-group">
    <label>Genre</label>
    <label>
        <input type="radio" value="Femme" v-model="genre">
        Femme
    </label>
On a d'abord le label de titre, puis le label d'input où l'on définit les attributs type, value et v-model.

**Cases à cocher pour sélectionner plusieurs options** checkbox
input type="checkbox"
loisirs:  [ ] dans data() est un tableau qui contient toutes les options sélectionnées.

**zone de texte**
<textarea v-model="message" placeholder="" rows="4"></textarea>
=> mettre message: "" dans data()

**Valider les champs du formulaire**
un formulaire doit vérifier les informations avant de les accepter/valider.
*Vérifier les champs obligatoires*
`required` est un attribut dans les input qui force l'utilisateur à remplir le champ avant la soumission.

*Vérifier la longeur du prénom*
Pour ça on créer la méthode personnalisée envoyerFormulaire dans methods
On va rajouter une condition qui vérifie que le prénom contient au moins deux caractères. on rajoute ça avant le alert.
on mets la condition `if(...) {... alert(...) return}` Donc un if, une alerte dans ce cas et return pour sortir de la boucle. à l'alert suivant, le else est implicite.

*Afficher un message d'erreur dans la page*
on ajoute dans data()  
`erreur:""`  
et on modifie la méthode en rajoutant 
`{this.erreur = "..." 
return
} 
this.erreur = "" 
alert(...)
` 
et on modifie le template en ajoutant `<p v-if="erreur" class="error"> {{erreur}}</p>`

**Afficher un récapitulatif avec les données du formulaire**
On va afficher toutes les informations saisies dans le formulaire.

*Ajouter une propriété formulaireEnvoye*
dans data() on ajoute `formulaireEnvoye: false`

*Modifier la méthode d'envoi*
dans la méthode envoyerFormulaire(), à la fin, après `this.erreur = ""`, on rajoute `this.formulaireEnvoye = true` tout en supprimant le alert(..).

*Afficher le récapitulatif*
Après le formulaire, on ajoute un contenant `<div v-if="formulaireEnvoye" class="result">
on y mets le titre `<h2>Récapitulatif</h2>`
et on rajoute des paragraphes `<p>Prénom : {{ prenom }}</p>` et pareil avec tous le reste (prénom, nom, email, pays, genre, loisirs, message).
Le seul cas différent est `Loisirs` car il concerne un tableau;
`<p>Loisirs : {{ loisirs.join(', ') }}</p>`
la méthode `join(',')` transforme le tableau des loisirs en une chaîne de caractères.

**Ajouter le CSS dans un seul fichier**
On remplace le contenu de style.css par celui fourni par la prof.