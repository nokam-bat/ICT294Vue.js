Créer un projet Vue à l'aide de CLI, L'interface en ligne de commande.

**Installer Vue CLi:** `npm i -g @vue/cli`
**Créer le projet:** `vue create real-world-vue`

Pui choisir manuellement selon les consignes du tuto.

Démarrer le serveur de développement:`cd real-world-vue` `npm run serve` 
Ce que ça fait: ça compile l'app et la sert sur un hôte local

Résultat: on a deux pages de base. Dans `views` on a `AbouttView.vue` (à propos) et `HomeView.vue` (accueil)

*interface graphique Vue* Vue propose une interface utilisateur graphique pour gérer les projets. pour l'ouvrir dans le navigateur: `vue ui`
=> cette interface permet de retrouver visuellement les outils de gestion du projet. (c'est en béta?)

**Arborescence du projet**
`node_modules` bibliothèques nécessaires à la cosntruction du projet Vue.
`public` Ressources statiques qui ne sont pas traités lors du build par Webpack.
`src` dossier qui contient le code principal de l'application.
`assets` dossier qui contient les ressources de types images et polices, susceptibles d'être optimisé par Webpack.
`router` configuration de Vue Router, utilisé pour la navigation entre les vues.
`store` emplacement du code Vuex qui gère l'état de l'application. on l'utilisera pas.
`views` composants des différentes vues chargées par Vue Router.
`App.vue` composant racie dans lequel s'imbrique les autres composants. 
`main.js` point d'entrée qui créer l'application et monte le composant racide dans le DOM
`.gitignore` liste des éléments que git doit ignorer
`babel.config.js` fichier de configuration Babel.
`package.json` décrit le projet et ses dépendances pour npm.

**COmment l'application est-elle chargée?**
dans main.js:
importer Vue et les éléments du projet, créer l'app, lui associer le routeur et le magasin, puis la monter dans le DOM.

*Extrait de code présenté dans le document source*
```bash
import {createAPP} fron "vue";
import App from "./App.vue";
import router from "./router";
import store from "./store";

createApp(App)
    .use(Router)
    .use(Store)
    .mount("#app");
```
=> c'est super différent de Vite.
**explication du processus du code**
*importer* la méthode de création de l'app, le composant racine App.vue, le router et le store.
*créer* la méthode createApp reçoit App, le composant racine qui contient tt l'app.
*enrengistrer les modules* les appels use() associent le router et le magasin à l'application
*monter*la méthode mount() indique l'emplacement dans le DOM où l'app doit être affichée. et le sélecteur #app correspond à l'élément html portant l'id app.
**public/index.html**
c'est là que l'app sera montée (le bordel, dans public carrément??)


**Défi 1**
Mettre mon nom avec un msg, i guess?
Dans HomeView.vue je remplace HelloWorld par Noor et je modifie le message.

Résultat
```bash
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <Noor msg="Welcome to Vue.js, Noor" />
  </div>
</template>

<script>
// @ is an alias to /src
import Noor from "@/components/HelloWorld.vue";

export default {
  name: "HomeView",
  components: {
    Noor,
  },
};
</script>

```

**Défi 2**
écrire A warm welcome to my page qui remplace tt dans Home.

j'ai tt supprimé dans `HomeView.vue` et j'ai juste écrit un template avec un titre <h1> qui mettait mon {{ms}}
et j'ai écrit un script setup où j'ai mis la const msg = "mon message"

```bash
<template>
  <div class="home">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script setup>
const msg = "a warm welcome to my page!";
</script>

```

=> c'est un enfer, juste un espace fait foirer la compilation
**Défi 3**
mettre un message, signé mon nom.
Le corrigé de la prof dit de créer un composant `NoorName` dans `components`.
dans mon `NoorName.vue` je mets juste:
```bash

<template>
    <p>Noor</p>
</template>
```
et je modifie `HomeView.vue en ajoutant la balise `<NoorName />` dans le template et je fais l'import dans le script.

*Dans NoorName.vue dans components:*
```bash
<template>
  <h1>Noor</h1>
</template>

```
*Dans HomeView.vue dans views:*
```bash
<template>
  <div class="home">
    <h1>{{ msg }}</h1>
    <NoorName />
  </div>
</template>

<script setup>
import NoorName from "@/components/NoorName.vue";
const msg = "a warm welcome to my page!";
</script>

```


**en cas de problème d'affichage, se mettre dans le bon dossier:** 
`cd real-world-vue`
et taper la commande de correction des espacements:
`npm run lint -- --fix`


**Mes exos étaient faux, je viens de trouver le dossier src du corrigé de la prof...*