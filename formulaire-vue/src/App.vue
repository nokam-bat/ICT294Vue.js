<template>
  <div class="container">
    <h1>Apprendre les formulaires avec Vue.js</h1>

    <p>
      Bienvenue dans ce tutoriel consacré aux formulaires.
    </p>
  </div>
  <form @submit.prevent="envoyerFormulaire">
    <p v-if="erreur" class="error">
      {{ erreur }}
    </p>

    <!-- champ pour le prénom, avec v-model pour synchroniser avec data() -->
    <div clas="form-group">
      <label>Prénom</label>
      <input type="text" v-model="prenom" placeholder="Votre prénom" required >
    </div>

    <!-- champ pour le nom de famille, avec v-model pour synchroniser avec data() -->
    <div class="form-group">
      <label>Nom</label>
      <input type="text" v-model="nom" placehold="Votre nom">
    </div>

    <!-- Champ email -->
     <div class="form-group">
      <label>Adresse email</label>

      <input type="email" v-model="email" placeholder="exemple@email.com" required>
     </div>

     <!-- Champ mot de passe-->
      <div class="form-group">
        <label>Mot de passe</label>

        <input type="password" v-model="motDePasse" placeholder="Votre mot de passe">
      </div>

      <!-- champ pour la sélection de plusieurs choix-->
      <div class="form-group">
        <label>Votre pays</label>
        <select v-model="pays">
          <option value="">Choisissez un pays</option>
          <option value="Suisse">Suisse</option>
          <option value="France">France</option>
          <option value="Belgique">Belgique</option>
          <option value="Canada">Canada</option>
       </select>
      </div> 

      <!-- Champ des boutons radio qui permettent de sélectionner une seule option-->
       <div class="form-group">
        <label>Genre</label>
        <label>
          <input type="radio" value="Femme" v-model="genre">
          Femme
        </label>

        <label type="radio" value="Homme" v-model="genre">
          Homme
        </label>
        
        <label type="radio" value="Autre" v-model="genre">
          Autre
        </label>
       </div>

       <!-- chap pour les cases à cocher checkbox-->
        <div class="form-group">
          <label>Vos loisirs</label>
          
          <label>
            <input type="checkbox" value="Sport" v-model="loisirs">
            Sport
          </label>

          <label>
            <input type="checkbox" value="Musique" v-model="loisirs">
            Musique
          </label>

          <label>
            <input type="checkbox" value="Lecture" v-model="loisirs">
            Lecture
          </label>
        </div>      

        <!-- Zone de texte-->
         <div class="form-group">
          <label>Votre message</label>

          <textarea
          v-model="message"
          placeholder="Écrivez votre message..."
          rows="4"
          ></textarea>
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
      motDePasse: "",
      pays: "",
      genre: "",
      loisirs: [],
      message: "",
      erreur: "",
      formulaireEnvoye: false
    }
  },
  methods: {
    envoyerFormulaire() {

      if (this.prenom.length <2) {
        alert('Le prénom doit contenir au moins 2 caractères')
        return // return sort
      }

      this.erreur = "" // else implicite
      this.formulaireEnvoye = true
    }
  }
}
</script>