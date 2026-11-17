# 4. computed – Calculer des valeurs automatiquement

`computed` est une propriété qui permet de définir de valeurs calculées à partir de données réactives.

La formule pour calculer automatiquement le montant total d'un produit est: 
Prix total = Prix unitaire > quantité

Quand `price` ou `quantity` change, Vue recalcul automatiquement `totalPrice` avec computed.

*Différence entre methods et computed:*
`methods` 
* Exécute une fonction
* Est appelé explicitement
* N'as pas de cache automatique
* Est idéal pour les actions.

`computed`
* Calcul une valeure
* Est utilisé comme une propriété
* A son résultat mis en cache
* Est idéal pour les calculs.
* il est recalculé selon que ses dépendances réactives changent.
