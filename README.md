# Vue.js — Mes notes ICT294

Cette fiche suit la progression des tutoriels : d’abord Vue dans une page HTML avec un CDN, ensuite un projet Vite avec un premier composant, puis Cookbook. Chaque partie garde la syntaxe du support correspondant.

## Sommaire

1. [CDN : découvrir les bases](#1-cdn--découvrir-les-bases)
2. [Vite : afficher un composant dans un autre](#2-vite--afficher-un-composant-dans-un-autre)
3. [Cookbook : transmettre des données et répéter les cartes](#3-cookbook--transmettre-des-données-et-répéter-les-cartes)

Le support Vue CLI et les défis du ZIP sont dans [une annexe séparée](ANNEXE-CLI-DEFIS.md).

## 1. CDN : découvrir les bases

### 1.1. Le point de départ : un fichier HTML

Un CDN permet ici de charger Vue depuis une adresse Internet. On crée `index.html`, puis on l’ouvre dans le navigateur. Pas de projet npm à créer pour cette étape.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Apprendre Vue</title>
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
        return {
          message: 'Bonjour Vue.js !'
        }
      }
    }).mount('#app')
  </script>
</body>
</html>
```

Lecture dans l’ordre :

1. Le navigateur trouve `<div id="app">` : c’est la zone dans laquelle on veut utiliser Vue.
2. Le premier `<script>` charge Vue.
3. `const { createApp } = Vue` récupère la fonction `createApp` fournie par Vue.
4. `createApp({...})` crée l’application avec les données et les fonctions qu’on lui donne.
5. `.mount('#app')` relie l’application à la zone HTML portant `id="app"`.
6. `{{ message }}` affiche la valeur de `message` : **Bonjour Vue.js !**

**Pour les exemples suivants**, on garde cette structure. Le HTML va dans `<div id="app">`. Les données, méthodes et calculs vont dans l’objet de `createApp`. On complète les blocs existants ; on ne crée pas une nouvelle application pour chaque ajout.

### 1.2. `data()` : les données de l’application

Dans `createApp`, remplacer le bloc `data()` par :

```js
data() {
  return {
    product: 'Chaussettes',
    price: 12,
    quantity: 2
  }
}
```

Dans la zone HTML contrôlée par Vue :

```html
<h1>{{ product }}</h1>
<p>Prix : {{ price }} CHF</p>
<p>Quantité : {{ quantity }}</p>
```

`data()` retourne un objet. Chaque propriété possède un nom et une valeur : ici `quantity` vaut `2`.

Ces données sont **réactives** : quand une donnée change dans l’application, Vue actualise l’affichage qui l’utilise.

### 1.3. `methods` : effectuer une action

On garde les données précédentes et on ajoute `methods` juste après `data()`. La virgule entre les deux blocs est nécessaire.

```js
createApp({
  data() {
    return {
      product: 'Chaussettes',
      price: 12,
      quantity: 2
    }
  },
  methods: {
    increaseQuantity() {
      this.quantity++
    },
    decreaseQuantity() {
      this.quantity--
    }
  }
}).mount('#app')
```

Ajouter dans le HTML :

```html
<button @click="decreaseQuantity">−</button>
<button @click="increaseQuantity">+</button>
```

Au clic sur `+`, Vue appelle `increaseQuantity`. `this.quantity++` ajoute 1 à la quantité. Le paragraphe qui affiche `quantity` se met à jour.

Dans ces méthodes, `this.quantity` désigne la donnée de l’application. Dans le HTML, on écrit directement `quantity`.

L’exemple du cours permet de descendre sous zéro. Si l’exercice demande de l’empêcher, remplacer le contenu de `decreaseQuantity` par :

```js
if (this.quantity > 0) {
  this.quantity--
}
```

### 1.4. `computed` : obtenir une valeur calculée

Le total dépend du prix et de la quantité. Ajouter ce bloc après `methods`, avec une virgule entre les blocs :

```js
computed: {
  totalPrice() {
    return this.price * this.quantity
  }
}
```

Dans le HTML :

```html
<p>Total : {{ totalPrice }} CHF</p>
```

Avec un prix de 12 et une quantité de 2, le total vaut 24. Après un clic sur `+`, il vaut 36.

| Bloc | Question à se poser | Exemple |
| --- | --- | --- |
| `data()` | Quelle valeur faut-il mémoriser ? | La quantité |
| `methods` | Quelle action faut-il effectuer ? | Augmenter la quantité |
| `computed` | Quelle valeur faut-il déduire des données ? | Prix × quantité |

Le calcul doit retourner son résultat avec `return`. Dans le HTML, on lit `totalPrice` sans parenthèses.

### 1.5. `v-if` : choisir ce qui apparaît

Ajouter `stock: 3` aux propriétés retournées par `data()`.

```html
<p v-if="stock > 10">Stock suffisant</p>
<p v-else-if="stock > 0">Stock limité</p>
<p v-else>Rupture de stock</p>
```

Pour `stock: 3`, seul **Stock limité** apparaît. Pour `stock: 0`, c’est **Rupture de stock**.

`v-if` teste une condition. `v-else-if` teste la suivante si la première est fausse. `v-else` couvre le cas restant et n’a pas de condition. Ces blocs doivent se suivre.

Le support mentionne aussi `v-show` : il masque l’élément avec du CSS. `v-if`, lui, ajoute ou retire l’élément.

### 1.6. `v-for` : afficher une liste

Ajouter ce tableau dans les données :

```js
products: [
  { id: 1, name: 'Chaussettes', price: 12 },
  { id: 2, name: 'Chaussures', price: 80 },
  { id: 3, name: 'T-shirt', price: 25 }
]
```

Dans le HTML :

```html
<ul>
  <li v-for="product in products" :key="product.id">
    {{ product.name }} : {{ product.price }} CHF
  </li>
</ul>
```

`products` est le tableau complet. `product` est le nom choisi pour désigner un élément pendant la boucle. Vue crée un `<li>` par produit.

`product.name` accède au nom de cet élément. `:key="product.id"` donne à Vue un identifiant unique et stable pour chaque élément.

### 1.7. `v-model` : relier un champ à une donnée

Ajouter `username: ''` dans les données.

```html
<label>Votre nom :</label>
<input type="text" v-model="username">
<p>Bonjour {{ username }} !</p>
```

Quand on écrit dans le champ, `username` change et le paragraphe s’actualise. La liaison fonctionne aussi dans l’autre sens : si le code change `username`, le champ reflète cette valeur.

Le support montre aussi les cases à cocher, les listes déroulantes et les zones de texte. Le principe reste le même : le champ est lié à une donnée déclarée dans `data()`.

### 1.8. Les événements : déclencher une méthode

On a déjà utilisé `@click`. Le symbole `@` est le raccourci de `v-on:` :

```html
<button @click="increaseQuantity">+</button>
<!-- Même comportement : -->
<button v-on:click="increaseQuantity">+</button>
```

Pour transmettre une information à une méthode :

```html
<button @click="sayHello('Noor')">Dire bonjour</button>
```

Ajouter dans le bloc `methods` :

```js
sayHello(name) {
  alert('Bonjour ' + name)
}
```

| Écriture | Action observée |
| --- | --- |
| `@click` | Clic |
| `@input` | Saisie dans un champ |
| `@change` | Changement validé, selon le champ |
| `@submit` | Soumission d’un formulaire |
| `@keyup` | Relâchement d’une touche |
| `@mouseover` | Passage du pointeur |

### 1.9. Ce qu’il faut comprendre avant de passer à Vite

Tu dois pouvoir expliquer le trajet suivant : **une action change une donnée → Vue actualise l’affichage et les valeurs calculées qui en dépendent**.

Petit contrôle : afficher une quantité, l’augmenter avec un bouton et afficher un total calculé. Ces trois éléments utilisent respectivement `data()`, `methods` et `computed`.

Le tutoriel propose ensuite une boutique pour réutiliser ces bases ensemble. C’est une application des notions précédentes, toujours dans le parcours CDN.

Pour charger le `style.css` fourni, s’il est à côté de `index.html`, ajouter dans `<head>` :

```html
<link rel="stylesheet" href="style.css">
```

Le CSS met en forme la page ; les comportements viennent du code Vue.

## 2. Vite : afficher un composant dans un autre

On change maintenant de façon d’organiser le travail : le tutoriel utilise un projet avec des fichiers `.vue` et un serveur de développement. **Le premier objectif est seulement de créer un composant et de l’afficher dans un autre.**

### 2.1. Créer et lancer le projet

Les commandes du tutoriel :

```sh
npm create vite@latest mon-projet-vue -- --template vue
cd mon-projet-vue
npm install
npm run dev
```

| Commande | Ce qu’elle fait |
| --- | --- |
| `npm create vite…` | Crée le projet à partir du modèle Vue |
| `cd mon-projet-vue` | Entre dans son dossier |
| `npm install` | Installe ses dépendances |
| `npm run dev` | Lance le serveur de développement |

Ouvrir l’adresse affichée par le terminal. Pour reprendre le projet plus tard, se placer dans son dossier et lancer `npm run dev`. On réinstalle les dépendances si elles sont absentes ou ont changé. `Ctrl+C` arrête le serveur.

### 2.2. Les fichiers à repérer

```text
src/
├── main.js
├── App.vue
└── components/
    └── Bonjour.vue   ← à créer
```

`main.js` démarre l’application. `App.vue` est le composant principal. `components/` contient les composants qu’on crée.

Un **composant** est un morceau d’interface placé dans son propre fichier et utilisable dans un autre composant.

### 2.3. Créer l’enfant : `src/components/Bonjour.vue`

```vue
<template>
  <div>
    <h2>Bonjour !</h2>
    <p>Je suis le composant enfant.</p>
  </div>
</template>
```

`<template>` contient ce que le composant affiche. Ici, un titre et un paragraphe suffisent : il n’y a pas de logique JavaScript à ajouter.

### 2.4. Afficher cet enfant dans `src/App.vue`

```vue
<script setup>
import Bonjour from './components/Bonjour.vue'
</script>

<template>
  <div>
    <h1>Mon application Vue.js</h1>
    <Bonjour />
  </div>
</template>
```

Deux lignes font le lien :

- `import Bonjour from './components/Bonjour.vue'` charge le composant depuis son fichier. Le chemin part du dossier contenant `App.vue`.
- `<Bonjour />` l’affiche à cet endroit du template.

Avec le `<script setup>` de ce tutoriel, l’import suffit pour rendre le composant utilisable dans le template.

Résultat :

```text
Mon application Vue.js       ← App.vue
Bonjour !                    ← Bonjour.vue
Je suis le composant enfant. ← Bonjour.vue
```

### 2.5. Comprendre l’imbrication

```text
App.vue (parent)
└── Bonjour.vue (enfant)
```

Le parent affiche l’enfant. Si on modifie le paragraphe dans `Bonjour.vue`, c’est cette partie de la page qui change.

**Exercice du support :** remplacer le texte de l’enfant par « Bienvenue sur mon site ! » et « Mon premier composant Vue.js fonctionne. », enregistrer et observer le navigateur.

À cette étape, il faut savoir créer le fichier, l’importer et utiliser sa balise. Les données transmises entre composants arrivent dans la partie suivante.

## 3. Cookbook : transmettre des données et répéter les cartes

Cookbook reprend l’imbrication et ajoute deux besoins : afficher une recette à partir de données reçues, puis afficher une carte pour chaque recette.

### 3.1. Préparer le projet Cookbook

Pour créer le projet séparé du tutoriel :

```sh
npm create vite@latest cookbook -- --template vue
cd cookbook
npm install
npm run dev
```

Si ce projet existe déjà, le reprendre dans son dossier.

### 3.2. Commencer par un composant simple : l’en-tête

Le tutoriel commence par `src/components/AppHeader.vue`, puis l’affiche dans `App.vue`. C’est le même principe que `Bonjour.vue` :

```vue
<script setup>
import AppHeader from './components/AppHeader.vue'
</script>

<template>
  <AppHeader />
  <h1>Bienvenue dans ma cuisine !</h1>
</template>
```

Le fichier d’en-tête contient son HTML et son CSS. Dans un composant :

| Bloc | Contenu |
| --- | --- |
| `<template>` | Structure affichée |
| `<script setup>` | JavaScript et imports |
| `<style scoped>` | Styles ciblant ce composant |

Le CSS général peut toujours s’appliquer à la page. On peut omettre les blocs inutiles.

### 3.3. Une carte reçoit une recette : les props

Une **prop** est une donnée transmise par un composant parent à un composant enfant.

Avant de multiplier les cartes, voici un exemple minimal avec une seule recette. Cette étape intermédiaire sert à expliquer la transmission.

Dans `src/App.vue` :

```vue
<script setup>
import RecipeCard from './components/RecipeCard.vue'

const recipe = {
  id: 1,
  name: 'Spaghetti',
  time: 20
}
</script>

<template>
  <RecipeCard :recipe="recipe" />
</template>
```

Dans `src/components/RecipeCard.vue` :

```vue
<script setup>
defineProps({
  recipe: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <article>
    <h2>{{ recipe.name }}</h2>
    <p>{{ recipe.time }} minutes</p>
  </article>
</template>
```

Lecture de `<RecipeCard :recipe="recipe" />` :

- À gauche, `recipe` est le nom de la prop reçue par la carte.
- À droite, `recipe` est la donnée du parent qu’on lui transmet.
- Le `:` indique qu’on transmet la valeur JavaScript. Sans lui, `recipe="recipe"` transmettrait le texte « recipe ».

Dans l’enfant, `defineProps` déclare les données attendues : un objet nommé `recipe`, obligatoire. Dans ce `<script setup>`, `defineProps` ne nécessite pas d’import.

Puis `recipe.name` lit le nom dans l’objet reçu. La carte affiche **Spaghetti** et **20 minutes**.

### 3.4. Passer à plusieurs recettes : ajouter `RecipeList`

Le projet complet utilise cette organisation :

```text
App.vue — contient le tableau recipes
├── AppHeader.vue
└── RecipeList.vue — reçoit le tableau
    └── RecipeCard.vue — une carte par recette
```

Pour comprendre ce passage, voici une version minimale fonctionnelle avec les trois fichiers. Elle se concentre sur les données ; le tutoriel fournit le HTML et le CSS complets.

**`src/App.vue` :**

```vue
<script setup>
import RecipeList from './components/RecipeList.vue'

const recipes = [
  { id: 1, name: 'Spaghetti', time: 20 },
  { id: 2, name: 'Pizza', time: 35 },
  { id: 3, name: 'Gâteau', time: 45 }
]
</script>

<template>
  <h1>Mes recettes</h1>
  <RecipeList :recipes="recipes" />
</template>
```

**`src/components/RecipeList.vue` :**

```vue
<script setup>
import RecipeCard from './RecipeCard.vue'

defineProps({
  recipes: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section>
    <p>{{ recipes.length }} recettes disponibles</p>
    <RecipeCard
      v-for="recipe in recipes"
      :key="recipe.id"
      :recipe="recipe"
    />
  </section>
</template>
```

**`RecipeCard.vue` garde le code de l’étape précédente.**

La boucle se lit : « Pour chaque `recipe` dans le tableau `recipes`, afficher une carte et lui transmettre cette recette. » `recipes.length` donne le nombre de recettes.

Le tableau appartient à `App`. `RecipeList` le reçoit puis transmet chaque objet à une carte. La liste et la carte ne doivent pas recopier le tableau dans leur propre fichier.

Le tutoriel complet ajoute `description`, `difficulty` et `emoji` à chaque objet. La carte les affiche de la même façon que `name` et `time`.

### 3.5. Relier le CSS général

Le tutoriel Cookbook utilise ce `src/main.js` :

```js
import { createApp } from 'vue'
import App from './App.vue'
import './style.css'

createApp(App).mount('#app')
```

Il charge Vue, le composant principal et le CSS général, puis monte l’application dans `#app`.

Dans ce projet, le CSS général est dans `src/style.css`. Le CSS propre à une carte reste dans `RecipeCard.vue`. Le fichier CSS séparé fourni pour la boutique possède ses propres classes : son apparence ne s’applique que si le HTML utilise les classes correspondantes.

### 3.6. Les exercices Cookbook : où intervenir ?

Ces repères concernent le projet complet du tutoriel.

**1. Ajouter une recette**

Dans le tableau `recipes` de `App.vue`, ajouter un objet avec un identifiant unique :

```js
{
  id: 4,
  name: 'Salade',
  description: 'Salade fraîche de saison.',
  time: 15,
  difficulty: 'Facile',
  emoji: '🥗'
}
```

La boucle de `RecipeList` crée automatiquement une quatrième carte. Le compteur passe à quatre.

**2. Afficher le nombre de personnes**

Ajouter une propriété comme `persons: 2` à chaque recette du tableau. Dans le template de `RecipeCard.vue`, ajouter :

```html
<p>{{ recipe.persons }} personnes</p>
```

La carte reçoit déjà l’objet entier : elle peut donc lire sa nouvelle propriété. Pas besoin d’une prop supplémentaire pour `persons`.

**3. Créer `RecipeFooter.vue`**

Créer le fichier dans `src/components/`, y placer le footer avec le nom de l’application, une année et le copyright. Déplacer son CSS si nécessaire. Dans `App.vue`, importer le composant puis remplacer l’ancien footer par `<RecipeFooter />`.

**4. Modifier le design**

| Modification | Où intervenir dans le projet du tutoriel |
| --- | --- |
| Fond d’une carte | `background` de `.recipe-card` dans `RecipeCard.vue` |
| Arrondis | `border-radius` de `.recipe-card` |
| Taille des emojis | `font-size` de `.recipe-image` |
| Couleur de difficulté | Ajouter `.difficile` dans `RecipeCard.vue` |
| Espace entre les cartes | `gap` de `.recipe-grid` dans `RecipeList.vue` |

Le tutoriel utilise `:class="recipe.difficulty.toLowerCase()"`. Une difficulté `'Difficile'` ajoute donc la classe CSS `difficile`. Il faut une recette avec cette difficulté pour voir ce style.

**5. Expliquer l’imbrication**

`App` est le parent de `RecipeList`. `RecipeList` est l’enfant de `App` et le parent des cartes. Les données sont dans `App`, puis descendent par les props vers la liste et vers chaque carte.

### 3.7. Vérifier qu’on a compris

Tu dois pouvoir répondre à ces trois questions :

1. Pourquoi importer un composant ne suffit-il pas à le voir ? Parce qu’il faut aussi utiliser sa balise dans le template.
2. Pourquoi une quatrième recette crée-t-elle une quatrième carte ? Parce que `v-for` parcourt le tableau.
3. Pourquoi écrire `:recipe="recipe"` ? Pour transmettre l’objet courant à la carte via sa prop.

---

## Supports et limites de cette fiche

Progression fondée sur les supports locaux : *Tutoriel Vue js CDN*, *Tutoriel Vue.js avec Vite — Un composant dans un autre* et *Tutoriel Cookbook*. L’ordre proposé reprend leur progression pédagogique ; il ne prétend pas dater les séances du cours.

Les exemples simplifiés et les étapes intermédiaires sont des explications ajoutées pour cette fiche. Les consignes d’exercices sont signalées comme telles. Les instructions des documents ne sont pas des demandes d’exécuter ou d’installer ces projets sur ton ordinateur. Aucun projet source n’a été modifié et les exemples n’ont pas été exécutés dans un navigateur.
