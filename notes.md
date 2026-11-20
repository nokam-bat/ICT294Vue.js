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

**Liste déroulante select**
