On va créer deux composants Vue.js: un composant principal App.vue et un composant enfant Bonjour.vue
Puis on va afficher l'enfant dans le parent.

On créer un projet vue avec `npm create vite@latest mon-projet-vue -- --template vue`
on entre dans le dossier du projet: 
`cd mon-projet-vue
npm install
npm run dev`

On va utiliser App.vue et créer un fichier `Bonjour.vue` dans components.

**Infos**
`template` contient la structure affichée à l'écran.
`h1` affiche un titre.
`p` affiche un paragraphe.
`import Bonjour` importe le composant enfant. et le chemin indique où se trouve le fichier.
`<Bonjour />` affiche le composant enfant dans le parent.
`<script setup>`, grâce à script setup, le composant importé est automatiquement utilisable dans le template.

**Inclure l'enfant dans le composant principal**
dans App.vue on écrit dans le:
 `<script setup>` import Bonjour from './components/Bonjour.vue'`</script setup>`
 et on écrit dans template, dans un div avec un titre h1, la balise `<Bonjour />`

 **Explication de l'imbrication de composants**
 Le titre principal vient de App.vue et le titre "Bonjour !" ainsi que le paragraphe viennent de Bonjour.vue.

