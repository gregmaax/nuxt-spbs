<script setup lang="ts">
const isOpen = ref(false);
</script>

<template>
  <h1 class="text-center mt-[30px]">Restaurants page</h1>
  <div class="m-6 text-center">
    <UButton label="Add restaurant" @click="isOpen = true" />

    <UModal v-model="isOpen">
      <div class="p-4">
        <UCard class="max-w-[720px] mx-auto mt-[30px] mb-[10px] p-10">
          <RestaurantForm />
        </UCard>
      </div>
    </UModal>
  </div>
  <UCard class="max-w-[720px] mx-auto mb-[10px] p-10">
    <RestaurantList />
  </UCard>
</template>
