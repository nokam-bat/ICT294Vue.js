# 1. createApp() – Créer une application Vue

`createApp()` est la fonction qui permet de créer une nouvelle application Vue.js.

`Vue` est l'objet global fourni par le CDN.
`createApp()` crée une instance d'application Vue.
L'objet passé à `createApp()` contient la configuration de l'application.
`.mount('#app')` indique à Vue où l'application doit être activée dans le HTML.
=> Vue prends donc le contrôle du contenu de l'élément `#app`.
=> `mount()`attache donc l'affichage à la page HTML.