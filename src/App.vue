<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <main>
    <RouterView />
  </main>
</template>

<style scoped>
.links {
  padding: 20px;
}

.active {
  font-weight: bold;
  color: blue;
}
</style>
