<script setup lang="ts">
  const links = [{
    label: 'Home',
    to: '/'
  },{
    label: 'Items',
    to: '/items'
  },{
    label: 'About',
    to: '/about'
  },{
    label: 'Stephen A',
    avatar: {
      src: 'https://www.caa.com/caaspeakers/partyHeadshot/stephen-smith.png'
    }
  }]
</script>

<template>
  <header>
    <UHorizontalNavigation :links="links" class="flex gap-2 justify-center"/>
  </header>
</template>