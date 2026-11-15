# Annexe — Vue CLI et défis du ZIP

Cette annexe concerne le support **Vue - Creation_projet.pdf** et le code de **src.zip**. Les tutoriels Vite sont expliqués dans [la fiche principale](README.md).

## 1. Reconnaître ce parcours

Le support de création présente Vue CLI :

```sh
npm i -g @vue/cli
vue create real-world-vue
```

Puis, dans le dossier du projet :

```sh
npm run serve
```

`npm i -g @vue/cli` installe l’outil globalement ; on ne le relance pas à chaque ouverture du projet. `vue ui` ouvre son interface graphique.

Ces commandes sont celles de ce support. Elles ne sont pas les étapes à ajouter au projet Vite de la fiche principale. Les anciennes versions minimales de Node citées par le PDF ne servent pas de consigne pour installer les outils actuels.

Le ZIP contient seulement `src/`, sans `package.json` : il ne suffit pas, à lui seul, pour lancer un projet npm complet. Il faut le projet qui l’accompagne. Dans un projet complet, les scripts disponibles se lisent dans `package.json`.

## 2. Comprendre le projet fourni

Dans ce ZIP, l’application contient de la navigation :

```text
App.vue
└── router-view : affiche la vue choisie par l’URL
    ├── /         → HomeView.vue → HelloWorld.vue
    ├── /about    → AboutView.vue
    └── /violeta  → VioletaView.vue → VioletaPage.vue
```

`<router-link>` crée un lien de navigation. `<router-view />` affiche la vue correspondant à la route courante. La configuration est dans `src/router/index.js`.

Le dossier `store` contient une configuration Vuex vide. Il n’est pas nécessaire d’approfondir Vuex pour comprendre les modifications ci-dessous ; le support précise qu’il ne sera pas implémenté dans ce cours de base.

**Correction du support de création :** l’extrait final contient des différences de majuscules. Le code cohérent avec les imports est :

```js
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

createApp(App).use(router).use(store).mount('#app')
```

C’est une correction de l’extrait du PDF. Le `main.js` du ZIP utilise déjà des noms cohérents.

## 3. Défi 01 — Personnaliser l’accueil

La capture montre « Welcome to Vue.js, Violeta ».

Dans le ZIP, `HelloWorld.vue` affiche :

```html
<h1>{{ msg }}</h1>
```

Le texte vient de son parent, `src/views/HomeView.vue`. Dans ce fichier, remplacer la valeur transmise :

```html
<HelloWorld msg="Welcome to Vue.js, Violeta" />
```

Ici, `msg` reçoit un texte fixe. Le composant utilise cette prop pour afficher le titre.

## 4. Défi 02 — Afficher une page personnalisée

La capture montre « A warm welcome to my page! ».

Le ZIP contient déjà un composant `VioletaPage.vue` avec ce titre, à un espace près avant le point d’exclamation. Il est utilisé par `VioletaView.vue`, qui possède déjà la route `/violeta`.

Pour obtenir ce titre dans cette vue, le template du composant peut être :

```vue
<template>
  <h1>A warm welcome to my page!</h1>
</template>
```

Dans le projet complet en fonctionnement, accéder à `/violeta` permet d’afficher cette vue. Le menu du ZIP propose seulement Home et About. Si l’énoncé demande un accès par le menu, on peut y ajouter :

```html
<router-link to="/violeta">Violeta</router-link>
```

La capture seule ne précise pas s’il faut afficher ce contenu sur Home, About ou une nouvelle route. La route existante du ZIP est un repère, pas une preuve de la consigne exacte.

## 5. Défi 03 — Afficher un composant avec un nom

La capture ajoute « Violeta » sous le titre. Le fichier `Défi-03.txt` utilise `<VioletaName />`, mais il ne contient que le template. Le composant correspondant n’est pas présent dans le ZIP.

Créer `src/components/VioletaName.vue` :

```vue
<template>
  <h2>Violeta</h2>
</template>
```

Dans le fichier qui doit afficher le template du défi, utiliser la syntaxe déjà employée par les vues du ZIP :

```vue
<script>
import VioletaName from '../components/VioletaName.vue'

export default {
  components: {
    VioletaName
  }
}
</script>

<template>
  <div class="about">
    <h1>A warm welcome to my page!</h1>
    <VioletaName />
  </div>
</template>
```

Le chemin ci-dessus convient à un fichier situé dans `src/views/`. Si le parent se trouve dans `src/components/`, le chemin devient `./VioletaName.vue`.

Dans cette syntaxe du ZIP, on importe **et on enregistre** le composant dans `components`. Compléter les blocs existants si le fichier a déjà un `<script>` et un `export default`.

La classe CSS `about` ne choisit pas la route : elle sert à la mise en forme.

---

Sources : support de création Vue CLI, contenu de `src.zip`, `Défi-03.txt` et les trois captures. Les pistes ci-dessus décrivent comment obtenir les éléments visibles ; les captures ne fournissent pas un énoncé complet. Le projet n’a pas été exécuté ou modifié.
