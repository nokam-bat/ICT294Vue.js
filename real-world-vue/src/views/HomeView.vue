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
