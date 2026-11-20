**Infos**
`import` `import AppHeader` Importe un composant
`defineProps` `defineProps({recipe})` Reçoit les données.
`v-for` `v-for="recipe in recipes"` répète un élément autant de fois que le tableau le dit.
`:key`  `:ke="recipe.id"`   identifie un élément
`:recipe`   `:recipe="recipe"`  transmets une donnée
`span`?
`class="difficulty"` dans RecipeCard fait référence à la `difficulty: ""` de la donnée dans App.vue.



L'objectif est de créer et imbriquer des composants avec Vite.
Aussi, à transmettre des données entre composants avec `props`et à comprendre l'organisation d'une app Vue.


**L'architecture de notre application**:
App.vue est le composant principal.
AppHeader.vue affiche le haut de la page.
RecipeList.vue affiche la liste des recettes
RecipeCard.vue affiche une carte de recette individuelle. (ce composant est répété pour chaque recette)
qu'est-ce que l'imbrication? c'est quand un composant en contient d'autres.

**Explication des fichiers**
mains.js démarre l'app vue
App.vue est le composant principal
style.css pour les styles généraux
AppHeader.vue pour l'entête de l'application
RecipeList.vue  une liste de recetes
RecipeCard.vue  une carte de recette

**créer le composant en-tête `AppHeader.vue` dans components:**
`<template>` contient le HTML du composant. c'est ce que verra l'utilisateur a l'écran.
`<script setup>` permet d'écrire le JavaScript du composant. On a pas encore besoin du JS.
`<style scoped>` contient le CSS du composant.
`scoped` signifique que les styles sont limités à ce composant.

**Utiliser le composant dans un autre**
On va intégrer AppHeader.vue dans App.vue
Dans App.vue:
3 actions importantes à comprendre;
*Action 1 : Importer le composant* 
`import AppHeader from './components/AppHeader.vue'` dans `<script setup>` en haut.
=> cette instruction permet d'utiliser le composant dans App.vue.
*Action 2 : déclarer le composant*
Avec `<script setup>` l'importation suffit à le rendre dispo dans le template.
*Action 3 : afficher le composant*
`<AppHeader />` dans le template html. cette instruction affiche le composant à l'endroit où elle est écrite.
=> un composant Vue s'utilise comme une balise HTML personnalisée : `<AppHeader /> <RecipeList /> <RecipeCard />`

**Créer le composant RecipeCard**
on crée une carte qui représente une recette.
*les `props`* permettent à un composant parent de transmettre des données à un composant enfant.

```bash
defineProps({
    recipe: {
    type: Object,
    required: true
    }
})
```
=> ça veut dire que le composant attends une donnée appelées `recipe`. cette donnée est un objet javascript.
On peut accéder aux propriétés de cet objet dans le template via {{ recipe.name}} ou {{recipe.description}}.
*enfait, RecipeCard.vue utilise les propriétés qu'on a définit dans le script de App.vue* Mais pk???

**Créer le composant RecipeList**
nous avons déjà une carte de recette et nous voulons maintenant afficher plusieurs recettes.
on va créer un composant qui contient plusieurs RecipeCard.
On créer le composant RecipeList.vue

Comprenons ce qui suit:
`<RecipeCard v-for="recipe in recipes" :key="recipe.id" :recipe="recipe" />`

`v-for` sert à répéter un composant pour chaque élément d'un tableau. ainsi vue.js va automatiquement créer chaque carte du tableau.
`:key` est une propriété Vue qui permet d'identifier chaque élément de manière unique.
`:recipe` est une propriété qui transmet l'objet de la recette au composant enfant.

*Explication de notre imbrication* 
App.vue contient RecipeList
RecipeList.vue contient plusieurs RecipeCard
RecipeCard.vue affiche les informations d'une recette.

**Relier tous les composants dans App.vue**
dans App.vue: 

dans script setup:
on importe AppHeader et RecipeList.
on écrit les données des recettes avec const recipes = [ {}, {}]

dans template:
on mets le div class="app"
on inscrit le composant enfant : en-tête <AppHeader />
jsp pk on écrit un main.
l'autre composant enfant
un footer
on termine le template.
=> RecipeList répète RecipeCard 3 fois grâce à v-for.

Enfin on remplace le css par celui de la prof.

on check le tt avec `npm run dev`

Exercices: ajouter une recette, modifier le composant RecipeCard en ajoutant persons (dans app.vue aussi), créer un nouveau composant Recipe.Footer.vue.

**création du component RecipeFooter.vue**
Pour le footer, je fais selon ce qu'il y avait marqué dans App.vue le footer, et au feeling.
ensuite je dois l'importer dans App.vue
et inscrire sa balise à la place du footer initiale de app.vue.


**Modifier le design de RecipeCard.vue**

changer la couleur de fond: pas sure.

Arrondir les bords des cartes: sur recipe-card,  `border-radius` qu'il faut augmenter.

augmenter la taille des emojis:  sur recipe-image, font-size

Ajouter unue couleur différente pour les recettes hardcore: je créer le style .hardcore
dans span y'a un truc du genre `:class="recipe.difficulty.toLowerCase()"` mais j'ai pas compris.

**Comprendre l'imbrication avec mes mots**
src: components: AppHeader.vue, RecipeCard.vue, RecipeFooter.vue, RecipeList.vue
    App.vue
    style.css

Qu'est-ce qu'un composant parent? c'est un composant dans lequel on peut imbriquer un autre composant. c'est aussi un composant qui va pouvoir transmettre des infos via Props. pas encore sure de si il reçoit aussi des infos de son enfant.
Qu'est-ce qu'un composant enfant? c'est un composant qui sera mis dans un composant parent, et il peut recevoir des infos via Props. pas encore sure du délire de récupérer des infos de App.vue...
Quel composant contient les données? App.vue
Comment les données arrvient-elles dans RecipeCard? avec des imports et en utilisant {{}} des attributs d'objet définit dans App.vue.
