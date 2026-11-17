# 7. v-model - Syncrhoniser les données et les formulaires

`v-model`permet de créer une liaison entre une donnée Vue et un champ de formulaire HTML.

(aussi appelé liaison bidirectionnelle - two-way data binding)

Dans **Javascript**: on mets `return( username:" ")` dans `data()`
Dans **HTML**: on mets un label avec dedans "Votre nom: "
Puis un `<input type="text" v-model="username">`
Avec un `<p>Bonjour {{username}} !</p> `pour afficher le non que l'utilisateur écrit.

d'autres champs à utiliser avec v-model
Case à cocher:
type="checkbox" v-model="accepted"
Liste déroulante:
select v-model="selectedCountry">
    <option value="fr">France</option

Zone de texte:
textarea v-model="description"