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
