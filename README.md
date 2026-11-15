# Vue.js — Fiche pour les exercices ICT294

## Sommaire

1. [Trouver rapidement quoi utiliser](#1-trouver-rapidement-quoi-utiliser)
2. [CDN, Vue CLI et Vite](#2-cdn-vue-cli-et-vite)
3. [Organisation et démarrage](#3-organisation-et-démarrage)
4. [Bases : données, actions et calculs](#4-bases--données-actions-et-calculs)
5. [Directives et événements](#5-directives-et-événements)
6. [Composants et props](#6-composants-et-props)
7. [Méthodes pour tes défis](#7-méthodes-pour-tes-défis)
8. [Méthodes pour Cookbook](#8-méthodes-pour-cookbook)
9. [CSS](#9-css)
10. [Erreurs fréquentes et vérifications](#10-erreurs-fréquentes-et-vérifications)

## 1. Trouver rapidement quoi utiliser

Vue.js est un framework JavaScript pour créer des interfaces. Avec des données réactives, Vue actualise l'affichage quand ces données changent. Un composant regroupe une partie de l'interface et sa logique.

| Ce que demande l'exercice | Outil | Exemple |
|---|---|---|
| Afficher une valeur dans du texte | Interpolation | `{{ username }}` |
| Définir des données modifiables (Options API) | `data()` | `return { quantity: 1 }` |
| Réagir à une action | `methods` et `@click` | `@click="increment"` |
| Calculer un total ou filtrer une liste | `computed` | `{{ totalPrice }}` |
| Afficher selon une condition | `v-if` / `v-else` | `v-if="stock > 0"` |
| Masquer sans retirer du DOM | `v-show` | `v-show="visible"` |
| Répéter des éléments | `v-for` et `:key` | `v-for="p in products" :key="p.id"` |
| Relier un champ à une donnée | `v-model` | `v-model="search"` |
| Donner une valeur dynamique à un attribut | `v-bind`, raccourci `:` | `:disabled="stock === 0"` |
| Réutiliser un bloc de page | Composant | `<RecipeCard />` |
| Envoyer une donnée du parent à l'enfant | Prop | `<RecipeCard :recipe="recipe" />` |
| Afficher une page selon l'URL | Vue Router | `<router-view />` |

## 2. CDN, Vue CLI et Vite

**Ce sont des façons de préparer le projet. Options API et Composition API sont des façons d'écrire les composants : ce sont deux choix distincts.** Un projet Vite peut utiliser l'Options API, et un projet Vue CLI peut utiliser `<script setup>` avec une configuration compatible.

### CDN : un fichier HTML

Pas d'installation npm nécessaire pour l'exemple du cours. Vue est chargé par une URL ; il faut pouvoir accéder au CDN.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Mon application</title>
</head>
<body>
  <div id="app">
    <h1>{{ message }}</h1>
  </div>

  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  <script>
    const { createApp } = Vue
    createApp({
      data() {
        return { message: 'Bonjour Vue !' }
      }
    }).mount('#app')
  </script>
</body>
</html>
```

`createApp(...)` crée l'application. L'objet passé entre parenthèses définit sa configuration. `mount('#app')` la monte dans l'élément portant `id="app"`. Les expressions Vue doivent être dans la zone contrôlée par l'application.

### Vite : les tutoriels composants et Cookbook

Créer un nouveau projet :

```bash
npm create vite@latest cookbook -- --template vue
cd cookbook
npm install
npm run dev
```

Pour reprendre un projet déjà créé : entrer dans son dossier et lancer `npm run dev`. Faire `npm install` si les dépendances ne sont pas installées ou ont changé. Ouvrir l'adresse donnée par le terminal ; le port peut varier. Arrêter le serveur avec `Ctrl+C`.

### Vue CLI : le support de création et les défis du ZIP

```bash
# Installer l'outil globalement si nécessaire
npm i -g @vue/cli

# Créer un projet
vue create mon-projet
cd mon-projet
npm run serve
```

`vue ui` ouvre l'interface graphique de Vue CLI.

**Correction de tes notes :** `npm i -g @vue/cli` n'est pas à refaire à chaque démarrage de projet. Le `-g` installe l'outil globalement. `npm install`, exécuté dans un projet, installe les dépendances de ce projet.

Vue CLI est en maintenance ; conserve-le pour les exercices qui l'utilisent. Pour un nouveau projet, la documentation oriente vers les outils basés sur Vite. Source : [Vue CLI](https://cli.vuejs.org/guide/).

**Réflexe :** consulter les commandes dans la section `scripts` de `package.json`. `serve` et `dev` sont des noms de scripts, pas des commandes interchangeables dans tous les projets. Les très anciennes versions de Node indiquées dans le support CLI ne constituent pas une référence pour les outils récents.

## 3. Organisation et démarrage

| Fichier / dossier | À quoi il sert |
|---|---|
| `package.json` | Dépendances du projet et scripts npm |
| `node_modules/` | Dépendances installées ; ne pas y écrire son code |
| `src/main.js` | Crée l'application, branche les modules et monte `App` |
| `src/App.vue` | Composant racine |
| `src/components/` | Blocs réutilisables : carte, en-tête, pied de page… |
| `src/views/` | Composants utilisés comme pages par le routeur, dans le ZIP |
| `src/router/index.js` | Correspondance entre URL et vue |
| `src/store/` | État partagé Vuex dans le projet CLI ; vide dans le ZIP |
| `src/assets/` | Images et autres ressources importées par le code |
| `src/style.css` | Styles généraux si ce fichier est importé |
| `public/` | Ressources statiques copiées sans traitement de compilation |

Dans Vite, `index.html` est à la racine et charge le point d'entrée. Dans le support CLI, le gabarit HTML est dans `public/index.html` et les scripts compilés y sont injectés.

```js
// main.js du Cookbook
import { createApp } from 'vue'
import App from './App.vue'
import './style.css'

createApp(App).mount('#app')
```

Avec les modules du projet CLI :

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

createApp(App).use(router).use(store).mount('#app')
```

**Attention au support de création :** `createAPP` doit être `createApp`. Après `import router` et `import store`, on écrit `.use(router)` et `.use(store)`. JavaScript distingue les majuscules des minuscules.

## 4. Bases : données, actions et calculs

### Options API : la syntaxe de tes notes CDN et de plusieurs fichiers du ZIP

Dans un composant `.vue` :

```vue
<script>
export default {
  data() {
    return { price: 12, quantity: 1 }
  },
  methods: {
    increaseQuantity() {
      this.quantity++
    },
    decreaseQuantity() {
      if (this.quantity > 0) this.quantity--
    }
  },
  computed: {
    totalPrice() {
      return this.price * this.quantity
    }
  }
}
</script>

<template>
  <p>Quantité : {{ quantity }}</p>
  <button @click="decreaseQuantity">-</button>
  <button @click="increaseQuantity">+</button>
  <p>Total : {{ totalPrice }} CHF</p>
</template>
```

- `data()` retourne un objet contenant les valeurs initiales réactives.
- Dans les méthodes et calculs de cette syntaxe, `this.quantity` accède à la donnée du composant.
- Dans le template, écrire directement `quantity`.
- Une méthode réalise une action ; un `computed` fournit une valeur dérivée et doit retourner son résultat.
- `{{ calculateTotal() }}` appelle une méthode ; `{{ totalPrice }}` lit une propriété calculée.
- `@click="increaseQuantity"` est valide : Vue appelle la méthode lors du clic. Avec un paramètre : `@click="addProduct(product)"`.
- Éviter les fonctions fléchées pour les méthodes Options API qui utilisent `this`.

Le résultat d'un `computed` est mis en cache selon ses dépendances réactives. Il est recalculé à la lecture lorsque ces dépendances ont changé. Une méthode est réexécutée à chaque appel. Source : [propriétés calculées](https://vuejs.org/guide/essentials/computed).

### `<script setup>` : la syntaxe des tutoriels Vite

Petit complément pour reconnaître la même logique dans cette syntaxe :

```vue
<script setup>
import { ref, computed } from 'vue'

const price = ref(12)
const quantity = ref(1)
const totalPrice = computed(() => price.value * quantity.value)

function increaseQuantity() {
  quantity.value++
}
</script>

<template>
  <button @click="increaseQuantity">+</button>
  <p>{{ quantity }} articles : {{ totalPrice }} CHF</p>
</template>
```

Ici, pas de `this`, de bloc `data()` ou de bloc `methods`. Un `ref` s'utilise avec `.value` dans le JavaScript ; les refs de premier niveau sont déballées automatiquement dans le template. Le Cookbook utilise un simple `const recipes = [...]` pour sa liste fixe : cela convient à cet affichage. Pour modifier une liste avec mise à jour réactive, utiliser par exemple `ref([...])`.

## 5. Directives et événements

### Texte ou attribut : distinguer `{{ }}` et `:`

```html
<p>{{ product.name }}</p>
<img :src="product.image" :alt="product.name">
<button :disabled="product.stock <= 0">Ajouter</button>
```

`{{ }}` affiche du texte. Pour un attribut dynamique, utiliser `:` (raccourci de `v-bind:`). Ne pas écrire `src="{{ product.image }}"`.

### Conditions

```html
<p v-if="stock > 10">Stock suffisant</p>
<p v-else-if="stock > 0">Stock limité</p>
<p v-else>Rupture de stock</p>
```

`v-else` n'a pas de condition. Le bloc `v-else` ou `v-else-if` doit suivre directement le bloc conditionnel précédent. `v-if` crée et retire les éléments du DOM ; `v-show` les conserve et change leur affichage CSS.

### Listes

```html
<ul>
  <li v-for="product in products" :key="product.id">
    {{ product.name }} : {{ product.price }} CHF
  </li>
</ul>
```

`products` est le tableau ; `product` est le nom local choisi pour l'élément courant. `:key` doit être unique et stable parmi les éléments frères : un identifiant convient bien. Une chaîne n'a pas de `.id` ; pour des noms tous uniques, on peut utiliser `:key="product"`. Éviter de mettre `v-if` et `v-for` sur le même élément : filtrer la liste dans un `computed`.

### Formulaires

```html
<input type="text" v-model="username">
<input type="number" v-model.number="quantity">
<input type="checkbox" v-model="accepted">
<select v-model="selectedCountry">
  <option value="ch">Suisse</option>
  <option value="fr">France</option>
</select>
<textarea v-model="description"></textarea>
```

Déclarer les données correspondantes, par exemple `username: ''`, `quantity: 1`, `accepted: false`, `selectedCountry: 'ch'`, `description: ''`. `v-model` synchronise le champ et la donnée dans les deux sens. `.number` convertit les valeurs convertibles en nombres ; prévoir aussi la validation des champs vides ou invalides.

### Événements

| Écriture | Déclenchement |
|---|---|
| `@click="save"` | Clic |
| `@input="handleInput"` | Modification de la valeur d'un champ pendant la saisie |
| `@change="handleChange"` | Changement validé, selon le type de champ |
| `@submit.prevent="save"` | Soumission ; `.prevent` évite l'action native du formulaire |
| `@keyup.enter="save"` | Relâchement de la touche Entrée |
| `@mouseover="showInfo"` | Passage du pointeur sur l'élément |

`@click` est le raccourci de `v-on:click`. Dans un formulaire, un bouton d'action qui ne doit pas soumettre doit avoir `type="button"`.

### Recherche : exemple réutilisable en Options API

```js
data() {
  return {
    search: '',
    products: [
      { id: 1, name: 'Chaussettes', price: 12 },
      { id: 2, name: 'Pull', price: 40 }
    ]
  }
},
computed: {
  filteredProducts() {
    const term = this.search.trim().toLowerCase()
    return this.products.filter(product =>
      product.name.toLowerCase().includes(term)
    )
  }
}
```

Associer le champ avec `v-model="search"` et parcourir `filteredProducts`. `filter` conserve les éléments qui vérifient la condition ; `includes` teste la présence d'un texte. Avec une recherche vide, tous les noms sont conservés.

## 6. Composants et props

Un fichier `.vue` contient généralement :

```vue
<script setup>
// Logique JavaScript et imports
</script>

<template>
  <h2>Mon composant</h2>
</template>

<style scoped>
h2 { color: #173d35; }
</style>
```

Les blocs inutiles peuvent être omis. `scoped` limite les sélecteurs au composant, avec notamment une exception pour la racine d'un composant enfant qui peut recevoir les styles de disposition du parent. Le CSS général reste susceptible de s'appliquer.

### Afficher un composant : deux écritures à reconnaître

Avec `<script setup>`, importer suffit :

```vue
<script setup>
import Bonjour from './components/Bonjour.vue'
</script>

<template>
  <Bonjour />
</template>
```

Avec l'Options API du ZIP, importer puis enregistrer :

```vue
<script>
import Bonjour from './components/Bonjour.vue'

export default {
  components: { Bonjour }
}
</script>

<template>
  <Bonjour />
</template>
```

Les chemins sont relatifs au fichier qui importe : `./` = dossier actuel ; `../` = dossier parent. Dans le ZIP, `@` correspond à `src`. Ne pas supposer que cet alias existe dans chaque nouveau projet Vite.

### Props : parent → enfant

Le parent transmet :

```html
<RecipeCard :recipe="recipe" />
```

L'enfant déclare et affiche :

```vue
<script setup>
defineProps({
  recipe: { type: Object, required: true }
})
</script>

<template>
  <h2>{{ recipe.name }}</h2>
  <p>{{ recipe.time }} minutes</p>
</template>
```

Dans `<script setup>`, `defineProps` est une macro et ne s'importe pas. Pour accéder aux props dans le JavaScript, on peut écrire `const props = defineProps(...)`, puis `props.recipe`.

Avec l'Options API : `props: { recipe: { type: Object, required: true } }` dans `export default`.

**Lire `:recipe="recipe"` :** à gauche, nom de la prop reçue ; à droite, expression du parent. `recipe="recipe"` envoie la chaîne de texte « recipe ». `:recipe="recipe"` envoie la valeur de la variable. Pour du texte fixe, `msg="Bienvenue"` convient.

Les props sont à considérer comme des données reçues en lecture seule : l'enfant demande au parent de faire une modification, généralement par un événement, au lieu de modifier directement une prop. Références : [composants](https://vuejs.org/guide/essentials/component-basics), [props](https://vuejs.org/guide/components/props).

## 7. Méthodes pour tes défis

Ces repères viennent des captures et du ZIP ; les captures seules ne donnent pas toutes les contraintes d'un éventuel énoncé.

### Défi 01 : personnaliser le message d'accueil

Le ZIP contient cette chaîne :

```text
App.vue → router-view → HomeView.vue → HelloWorld.vue
```

`HomeView.vue` transmet `msg` à `HelloWorld.vue`, qui l'affiche avec `{{ msg }}`. Pour obtenir le titre de la capture, modifier la valeur transmise :

```html
<HelloWorld msg="Welcome to Vue.js, Violeta" />
```

**Méthode générale :** chercher le texte affiché. Si le composant montre `{{ msg }}`, retrouver le parent qui lui transmet `msg`.

### Défi 02 : afficher une page personnalisée

Le ZIP contient déjà `VioletaPage.vue`, utilisé par `VioletaView.vue`, et une route `/violeta` qui pointe vers cette vue. Ouvrir `/violeta` affiche donc cette chaîne. La navigation de `App.vue` ne comporte cependant que Home et About : ajouter un lien si l'exercice demande d'accéder à la nouvelle page depuis le menu.

```html
<router-link to="/violeta">Violeta</router-link>
```

Repères du routeur :

- `<router-link to="/about">` navigue vers une URL.
- La route ayant `path: '/about'` détermine la vue correspondante.
- `<router-view />` affiche cette vue dans `App.vue`.
- Un composant imbriqué s'affiche avec sa balise ; il n'a pas besoin d'une route propre.

Le menu identique sur les captures ne permet pas de savoir si le résultat attendu doit remplacer Home, About ou être une nouvelle route. Choisir selon l'énoncé, sans modifier une route au hasard.

### Défi 03 : ajouter un composant qui affiche un nom

Le fichier texte contient le template d'une page avec `<VioletaName />`, mais ne contient ni l'import ni le composant. Dans le ZIP fourni, ce composant n'existe pas.

Créer `src/components/VioletaName.vue` :

```vue
<template>
  <h2>Violeta</h2>
</template>
```

Puis, dans la vue où doit apparaître le résultat (exemple avec la syntaxe du ZIP) :

```vue
<script>
import VioletaName from '@/components/VioletaName.vue'

export default {
  components: { VioletaName }
}
</script>

<template>
  <div class="about">
    <h1>A warm welcome to my page!</h1>
    <VioletaName />
  </div>
</template>
```

Si ce fichier a déjà un bloc `<script>` et un `export default`, compléter ceux qui existent. Ne pas coller un second `export default`. La classe `about` est une classe CSS : elle ne configure pas la route.

## 8. Méthodes pour Cookbook

```text
App.vue — possède recipes
├── AppHeader.vue
├── RecipeList.vue — reçoit recipes
│   └── RecipeCard.vue — répété ; reçoit une recipe
└── RecipeFooter.vue — à créer dans l'exercice
```

Dans `App.vue` :

```html
<RecipeList :recipes="recipes" />
```

Dans `RecipeList.vue` :

```vue
<script setup>
import RecipeCard from './RecipeCard.vue'
defineProps({ recipes: { type: Array, required: true } })
</script>

<template>
  <RecipeCard
    v-for="recipe in recipes"
    :key="recipe.id"
    :recipe="recipe"
  />
</template>
```

### Exercice 1 : ajouter une recette

Ajouter un objet au tableau `recipes` de `App.vue` avec un identifiant unique :

```js
{
  id: 4,
  name: 'Salade',
  description: 'Salade fraîche de saison.',
  time: 15,
  difficulty: 'Facile',
  emoji: '🥗',
  persons: 2
}
```

La boucle crée la quatrième carte automatiquement. Vérifier aussi le compteur `recipes.length`.

### Exercice 2 : afficher le nombre de personnes

Ajouter `persons` aux objets du tableau, puis dans `RecipeCard.vue` :

```html
<p>{{ recipe.persons }} personnes</p>
```

`persons` est une propriété de l'objet `recipe`. Il n'est pas nécessaire de créer une nouvelle prop indépendante tant que l'objet entier est transmis. Ajouter la propriété à toutes les recettes évite les affichages incomplets.

### Exercice 3 : extraire le pied de page

1. Créer `RecipeFooter.vue` avec le HTML du pied de page.
2. Déplacer son CSS si nécessaire.
3. L'importer dans `App.vue`.
4. Remplacer le footer existant par `<RecipeFooter />`.

### Exercice 4 : modifier le design

| Modification demandée | Où intervenir |
|---|---|
| Fond de la carte | `background` de `.recipe-card` dans `RecipeCard.vue` |
| Coins plus arrondis | `border-radius` de `.recipe-card` |
| Taille des emojis | `font-size` de `.recipe-image` |
| Couleur de difficulté | Classe `.difficile` dans `RecipeCard.vue` |
| Espace entre les cartes | `gap` de `.recipe-grid` dans `RecipeList.vue` |

Le cours utilise `:class="recipe.difficulty.toLowerCase()"`. La valeur `'Difficile'` active donc la classe `difficile`. Ajouter une recette avec cette difficulté permet de vérifier le nouveau style.

### Exercice 5 : expliquer l'imbrication

`App` est parent de `RecipeList`. `RecipeList` est à la fois enfant de `App` et parent des cartes. Le tableau appartient à `App`. Il descend vers `RecipeList` via la prop `recipes`, puis chaque objet descend vers une carte via la prop `recipe`.

## 9. CSS

Le fichier CSS fourni contient déjà des styles pour les formulaires, boutons, produits et panier. Il ne crée aucune fonctionnalité Vue : il met en forme les éléments et les classes présents dans le HTML.

Pour l'utiliser avec CDN, s'il est à côté du HTML :

```html
<link rel="stylesheet" href="style.css">
```

Avec un projet dont le fichier est dans `src`, l'importer depuis `src/main.js` :

```js
import './style.css'
```

| Classe du fichier fourni | Utilisation |
|---|---|
| `.product-card` | Conteneur d'un produit |
| `.available` / `.unavailable` | Disponibilité |
| `.cart` / `.cart-total` | Panier et total |
| `.message` + `.message-success` | Message de succès |
| `.message` + `.message-error` | Message d'erreur |
| `.message` + `.message-warning` | Avertissement |

```html
<div class="product-card">
  <h3>{{ product.name }}</h3>
  <p :class="product.stock > 0 ? 'available' : 'unavailable'">
    {{ product.stock > 0 ? 'Disponible' : 'Indisponible' }}
  </p>
</div>
```

Les variables dans `:root`, telles que `--primary`, s'utilisent avec `var(--primary)`. Les règles `@media` adaptent la présentation à la largeur. Pour Cookbook, les cartes utilisent `.recipe-card` : les styles `.product-card` ne s'y appliquent pas automatiquement.

## 10. Erreurs fréquentes et vérifications

| Problème | Réflexe |
|---|---|
| `{{ message }}` reste affiché tel quel | Vérifier chargement de Vue, montage, console et emplacement dans `#app` |
| Composant inconnu ou invisible | Vérifier fichier, import, enregistrement si Options API, puis balise |
| Valeur absente | Vérifier orthographe et chemin : `recipe.name`, pas `recipes.name` |
| Une chaîne est reçue au lieu d'un objet | Vérifier le `:` de `:recipe="recipe"` |
| Le total ne s'affiche pas | Vérifier `return`, noms et utilisation sans `()` pour un computed |
| Un clic ne change rien | Vérifier `@click`, méthode et donnée modifiée |
| Une modification avec `<script setup>` n'actualise pas la page | Vérifier réactivité et `.value` dans le JavaScript des refs |
| Une route manque | Vérifier son `path`, la vue importée et `<router-view />` |
| `Missing script: dev` ou `serve` | Lire `scripts` dans `package.json` |
| npm ne trouve pas le projet | Se placer dans le dossier contenant `package.json` |
| Le CSS ne s'applique pas | Vérifier import, classe, portée et règles concurrentes |

Corrections de syntaxe de tes notes : `return { username: '' }`, `type="text"`, `value="be"`, `alert(...)`, et `this.clicks++` si la donnée est nommée `clicks`. JavaScript et Java sont deux langages différents ; ici on écrit du JavaScript.

### Démarche à réutiliser pour chaque exercice

1. Repérer le type de projet et la syntaxe déjà utilisés.
2. Décrire le résultat attendu : texte, liste, champ, action, nouveau composant ou nouvelle page ?
3. Identifier le fichier qui possède les données et celui qui affiche le résultat.
4. Choisir le mécanisme dans le tableau de la section 1.
5. Faire une modification ciblée et enregistrer.
6. Vérifier le navigateur et sa console, puis les cas utiles : liste vide, recherche sans résultat, stock nul, plusieurs éléments.

### Sources et périmètre

Fiche préparée à partir de tes notes collées, des cinq supports PDF fournis, de `src.zip`, de `Défi-03.txt`, des trois captures de défis et de `style.css`. Les extraits sont des exemples pour tes notes ; les projets sources n'ont pas été modifiés et les applications n'ont pas été exécutées. Certaines pages de la présentation sont sans texte extractible ; elles ne servent pas de base à des détails supplémentaires. Les corrections et compléments sont distingués des consignes d'exercices. La suite du cours pourra approfondir les événements enfant → parent, les API, Router et l'état partagé.
