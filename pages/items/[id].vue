<script setup lang="ts">
const route = useRoute();
console.log(route.params)
</script>

<template>
  <h1>Current item: {{route.params.id}}</h1>
</template>

<style scoped>

</style>