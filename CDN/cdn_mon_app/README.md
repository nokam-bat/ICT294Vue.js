# Ma boutique Vue.js — guide de révision

Cet exercice crée une petite boutique avec **Vue 3 chargé par CDN**. Le but est de comprendre comment les données JavaScript, le HTML et les actions de l’utilisateur sont reliés.

## 1. Ouvrir le projet

1. Ouvrir `index.html` dans un navigateur, ou utiliser Live Server dans VS Code.
2. Garder une connexion Internet : Vue est téléchargé depuis `unpkg.com`.
3. Tester la recherche et les boutons `+` et `-`.

Il n’y a pas de dépendances à installer ni de commande `npm` à lancer.

| Fichier | Rôle |
| --- | --- |
| `index.html` | Structure de la page, directives Vue et logique JavaScript |
| `style.css` | Apparence : couleurs, cartes, boutons et adaptation au mobile |
| `README.md` | Explications pour relire et comprendre l’exercice |

## 2. Vue prend le contrôle d’une partie de la page

Le HTML contient une zone racine :

```html
<div id="app">
  <h1>{{ title }}</h1>
</div>
```

En bas du fichier, le premier script charge Vue. Le second crée l’application :

```js
const { createApp } = Vue

createApp({
  data() {
    return { /* les données de l’application */ }
  },
  methods: { /* les actions */ },
  computed: { /* les valeurs calculées */ }
}).mount('#app')
```

- `const { createApp } = Vue` récupère la fonction `createApp` fournie par Vue.
- `createApp(...)` définit l’application.
- `.mount('#app')` la relie à l’élément qui possède `id="app"`.
- `{{ title }}` affiche la valeur de `title` dans le HTML. Cette syntaxe s’appelle l’interpolation.

**La réactivité :** quand une donnée change, Vue met automatiquement à jour les parties de la page qui en dépendent.

## 3. `data()` : les données de départ

`data()` retourne un objet contenant l’état de l’application :

```js
title: "Ma boutique à moi",
products: [
  { id: 0, name: "Chat adulte", price: 100, stock: 0, quantity: 0 },
  { id: 1, name: "Chaton", price: 150, stock: 3, quantity: 0 },
  { id: 2, name: "Croquettes chat adulte", price: 200, stock: 1, quantity: 0 },
  { id: 3, name: "Croquettes chaton", price: 250, stock: 2, quantity: 0 }
],
search: ""
```

Chaque objet du tableau `products` représente un produit :

| Propriété | Signification |
| --- | --- |
| `id` | Identifiant unique du produit |
| `name` | Nom affiché |
| `price` | Prix d’une unité, en euros |
| `stock` | Quantité maximale que l’on peut sélectionner |
| `quantity` | Quantité actuellement choisie dans le panier |

**Attention : `stock` et `quantity` sont différents.** Dans cet exercice, ajouter un produit augmente `quantity`, mais ne diminue pas `stock`.

## 4. `v-model` : relier la recherche à une donnée

```html
<label for="recherche">Recherche</label>
<input id="recherche" type="text" v-model="search">
<p>Vous recherchez : {{ search }}</p>
```

Deux liens différents existent ici :

- `for="recherche"` et `id="recherche"` associent le label au champ. Cliquer sur le label place le curseur dans le champ.
- `v-model="search"` relie la valeur du champ à la donnée Vue `search`, dans les deux sens.

Quand on écrit `chaton`, `search` devient `"chaton"`. Vue actualise le paragraphe et la liste filtrée. Si le code change `search`, le champ est également mis à jour.

## 5. `computed` : filtrer les produits

Une propriété calculée sert à obtenir une valeur à partir d’autres données. Ici, `productsFilter` dépend de `products` et de `search` :

```js
productsFilter() {
  return this.products.filter(product => {
    return product.name.toLowerCase().includes(this.search.toLowerCase())
  })
}
```

Pour lire cette expression, partir de l’intérieur :

1. `this.products` récupère le tableau de l’application.
2. `filter(...)` examine chaque produit et renvoie un nouveau tableau contenant ceux pour lesquels le test vaut `true`.
3. `product` est le nom donné au produit examiné à ce moment-là. Il est déclaré comme paramètre de la fonction fléchée `product => { ... }`.
4. `product.name.toLowerCase()` transforme le nom en minuscules.
5. `this.search.toLowerCase()` transforme aussi la recherche en minuscules.
6. `includes(...)` renvoie `true` si le nom contient le texte recherché, sinon `false`.

Exemple : avec `CHATON`, le test compare les noms à `chaton` et conserve « Chaton » et « Croquettes chaton ».

Avec une recherche vide, tous les noms contiennent la chaîne vide : tous les produits restent visibles. `filter()` ne supprime rien du tableau `products`.

### Pourquoi `this` ?

Dans les méthodes et propriétés calculées de cette application, `this` permet d’accéder aux données de l’application : `this.products`, `this.search`.

Dans le template HTML, on écrit directement `products` ou `search`, sans `this`.

### Pourquoi `computed` ?

La liste filtrée est un résultat dérivé des données. Vue la recalcule lorsque ses dépendances réactives changent et conserve le résultat en cache tant qu’elles ne changent pas.

Dans le HTML, on utilise `productsFilter`, **sans parenthèses**, même si sa définition ressemble à une fonction.

## 6. `v-for` : afficher une carte par produit

```html
<div class="product-card"
     v-for="product in productsFilter"
     :key="product.id">
  {{ product.name }} - {{ product.price }} € - {{ product.stock }}
  <p>Quantité : {{ product.quantity }}</p>
</div>
```

On écrit **un seul bloc de produit**. Vue le répète pour chaque élément de `productsFilter`.

- `productsFilter` est la liste à parcourir.
- `product` représente le produit courant dans ce bloc.
- `product.name` lit la propriété `name` de cet objet.
- `:key="product.id"` donne à Vue un identifiant stable et unique pour suivre chaque carte quand la liste change.

Le `:` est le raccourci de `v-bind`. Ici, la clé reçoit la valeur JavaScript de `product.id`.

## 7. `v-if` et `v-else` : afficher la disponibilité

```html
<p class="available" v-if="product.stock > 0">Produit disponible</p>
<p class="unavailable" v-else>Produit indisponible</p>
```

Si le stock est supérieur à zéro, Vue affiche le premier paragraphe. Sinon, il affiche le second. Le `v-else` doit suivre directement le bloc `v-if` correspondant.

La condition utilise le **stock**, pas la quantité restante à sélectionner. Un produit de stock 3 reste donc marqué disponible même si on en a déjà sélectionné 3.

## 8. `methods` et `@click` : modifier les quantités

```html
<button @click="increaseQuantity(product)">+</button>
<button @click="decreaseQuantity(product)">-</button>
```

`@click` est le raccourci de `v-on:click` : il exécute l’expression au clic. On transmet le produit de la carte à la méthode pour modifier le bon objet.

```js
increaseQuantity(product) {
  if (product.quantity < product.stock) {
    product.quantity++
  }
},
decreaseQuantity(product) {
  if (product.quantity > 0) {
    product.quantity--
  }
}
```

- `++` ajoute 1 et `--` retire 1.
- Le premier `if` empêche de dépasser le stock.
- Le second empêche de descendre sous zéro.
- Les boutons restent cliquables aux limites, mais la méthode ne modifie plus la quantité.

Le produit reçu est l’objet du tableau d’origine. Modifier sa quantité met donc aussi à jour le panier et le total.

## 9. Le panier : afficher uniquement les quantités choisies

```html
<div v-for="product in products" :key="product.id">
  <p v-if="product.quantity > 0">
    {{ product.name }} - {{ product.quantity }}
  </p>
</div>
```

Le panier parcourt `products`, puis affiche un paragraphe seulement si la quantité est supérieure à zéro.

**Pourquoi `products` et pas `productsFilter` ?** La recherche filtre les cartes de la boutique. Elle ne doit pas faire disparaître du panier les produits déjà choisis.

Il n’y a pas de tableau séparé pour le panier : ce sont les propriétés `quantity` des produits qui mémorisent la sélection.

## 10. `totalPrice` : calculer le montant du panier

```js
totalPrice() {
  let total = 0
  for (const product of this.products) {
    total = total + (product.price * product.quantity)
  }
  return total
}
```

1. `let total = 0` initialise le montant.
2. `for...of` parcourt les valeurs du tableau `this.products`.
3. `const product` déclare la variable qui représente le produit courant à chaque tour.
4. `price * quantity` donne le sous-total de ce produit.
5. On ajoute ce sous-total à `total`.
6. `return total` fournit le résultat à afficher avec `{{ totalPrice }}`.

`const` empêche de réassigner la variable `product` ; cela n’empêche pas de modifier les propriétés de l’objet qu’elle désigne.

Exemple : 2 chatons à 150 € et 1 paquet de croquettes chaton à 250 € donnent **2 × 150 + 1 × 250 = 550 €**. Un produit de quantité 0 ajoute 0 € au total.

## 11. Comprendre le chemin complet d’une action

**Quand je saisis une recherche :**

```text
Saisie dans le champ
→ v-model met à jour search
→ productsFilter calcule la liste correspondante
→ v-for actualise les cartes affichées
```

**Quand je clique sur `+` :**

```text
Clic sur le bouton d’une carte
→ increaseQuantity reçoit le produit
→ la méthode vérifie la limite de stock
→ quantity augmente si la limite le permet
→ Vue actualise la quantité, le panier et totalPrice
```

À retenir : `data` stocke l’état, `methods` le modifie, `computed` en déduit des résultats, et le template affiche le tout.

## 12. HTML et CSS : qui fait quoi ?

Le HTML donne la structure et le sens :

| Balise | Utilité |
| --- | --- |
| `h1` | Titre principal |
| `h2` | Titre de section, comme le panier |
| `h3` | Sous-titre possible, par exemple un nom de produit ; non utilisé actuellement |
| `p` | Paragraphe ou information textuelle |
| `label` | Nom d’un champ de formulaire |
| `input` | Champ de saisie |
| `button` | Action déclenchée par l’utilisateur |
| `div` | Regroupement d’éléments |

Le CSS applique l’apparence à partir des sélecteurs :

| Sélecteur | Élément concerné |
| --- | --- |
| `#app` | Conteneur principal identifié par `id="app"` |
| `.product-card` | Chaque carte produit |
| `.available` / `.unavailable` | Message de disponibilité en vert / rouge |
| `.cart` | Bloc du panier |
| `.cart-total` | Paragraphe du total |
| `button` | Tous les boutons |

Un `.` cible une classe et un `#` cible un identifiant. Une classe peut être réutilisée sur plusieurs éléments.

Le fichier CSS contient aussi des règles prévues pour d’autres éléments, comme des messages ou des listes. Elles s’appliquent seulement si les éléments correspondants existent dans le HTML. La règle `@media (max-width: 600px)` adapte notamment l’espace et les boutons aux petits écrans.

## 13. Vérifier que j’ai compris

| Manipulation | Résultat attendu |
| --- | --- |
| Ouvrir la page | 4 produits, panier sans ligne de produit, total à 0 € |
| Rechercher `CHATON` | 2 cartes : Chaton et Croquettes chaton |
| Vider la recherche | Les 4 cartes réapparaissent |
| Cliquer sur `+` pour Chat adulte | Quantité à 0, car son stock est nul |
| Cliquer 4 fois sur `+` pour Chaton depuis 0 | Quantité bloquée à 3, total à 450 € |
| Rechercher ensuite `adulte` | Le chaton reste dans le panier et le total reste à 450 € |
| Vider la recherche et retirer les 3 chatons | Quantité à 0, ligne retirée du panier, total à 0 € |
| Cliquer encore sur `-` | Quantité toujours à 0 |

La sélection est gardée uniquement en mémoire : recharger la page réinitialise les quantités et la recherche.

## 14. Fiche mémo

| Syntaxe / notion | À retenir |
| --- | --- |
| `{{ valeur }}` | Afficher une valeur dans le texte |
| `v-model="search"` | Synchroniser un champ et une donnée |
| `v-for="product in productsFilter"` | Répéter un bloc pour chaque produit filtré |
| `:key="product.id"` | Identifier chaque élément de la liste |
| `v-if` / `v-else` | Choisir un affichage selon une condition |
| `@click="methode(product)"` | Appeler une méthode au clic avec le produit concerné |
| `data()` | Déclarer les données initiales |
| `methods` | Définir les actions |
| `computed` | Définir les valeurs calculées à partir des données |
| `this.products` | Accéder aux produits dans la logique de l’application |
| `filter()` | Construire un tableau avec les éléments qui passent un test |
| `includes()` | Tester si un texte contient un autre texte |
| `toLowerCase()` | Passer un texte en minuscules |
