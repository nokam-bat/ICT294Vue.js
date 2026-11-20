<template>
  <div class="container">
    <h1>Apprendre les formulaires avec Vue.js</h1>

    <p>
      Bienvenue dans ce tutoriel consacré aux formulaires.
    </p>
  </div>
  <form @submit.prevent="envoyerFormulaire">

    <!-- champ pour le prénom, avec v-model pour synchroniser avec data() -->
    <div clas="form-group">
      <label>Prénom</label>
      <input type="text" v-model="prenom" placeholder="Votre prénom">
    </div>

    <!-- champ pour le nom de famille, avec v-model pour synchroniser avec data() -->
    <div class="form-group">
      <label>Nom</label>
      <input type="text" v-model="nom" placehold="Votre nom">
    </div>

    <!-- Champ email -->
     <div class="form-group">
      <label>Adresse email</label>

      <input type="email" v-model="email" placeholder="exemple@email.com">
     </div>

     <!-- Champ mot de passe-->
      <div class="form-group">
        <label>Mot de passe</label>

        <input type="password" v-model="motDePasse" placeholder="Votre mot de passe">
      </div>

    <!-- bouton qui soumet le formulaire-->
    <button type="submit">Envoyer</button>
  </form>

  <!-- Afficher les données saisies dans le champ du formulaire (prénom et nom)-->
   <div class="result">
    <h3>Résultat</h3>

    <p>Prénom : {{ prenom }} </p>
    <p>Nom : {{ nom }} </p>
   </div>


</template>

<script>
export default {
  name: 'App?',

  data() {
    return {
      prenom: "",
      nom: "",
      email: "",
      motDePasse: ""
    }
  },
  methods: {
    envoyerFormulaire() {
      alert(
        'Bonjour ' + this.prenom + '' + this.nom  // on utilise .this pour utiliser des données du composant.
      )
    }
  }
}
</script>