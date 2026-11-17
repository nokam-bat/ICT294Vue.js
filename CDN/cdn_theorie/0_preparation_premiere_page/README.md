# 0. Préparation : votre première page Vue.js
Créez un fichier index.html et copiez le code suivant :

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Apprendre Vue.js</title>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
    </div>
    <!-- Chargement de Vue.js 3 depuis le CDN -->
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

Ouvrez le fichier dans votre navigateur.

Vous venez de créer votre première application Vue.js !