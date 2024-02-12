<script setup lang="ts">
//supabase
import {RealtimeChannel} from "@supabase/realtime-js";

const restaurantsList = ref();

const client = useSupabaseClient();
let realtimeChannel: RealtimeChannel;

// Fetch restaurants and get the refresh method provided by useAsyncData
const { data: restaurants, refresh: refreshRestaurants } = await useAsyncData('restaurants', async () => {
  const { data } = await client.from('restaurants').select('name, city, rating, hasEaten')
  return restaurantsList.value = data
})

// Once page is mounted, listen to changes on the `restaurants` table and refresh collaborators when receiving event
onMounted(() => {
  // Real time listener for new workouts
  realtimeChannel = client.channel('public:restaurants').on(
      'postgres_changes',
      { event: '*', schema: 'public', table: 'restaurants' },
      () => refreshRestaurants()
  )

  realtimeChannel.subscribe()
})

// Don't forget to unsubscribe when user left the page
onUnmounted(() => {
  client.removeChannel(realtimeChannel)
})

/*
const { data, error } = await client
    .from('restaurants').select('name, city, rating, hasEaten')
*/

</script>

<template>
  <UTable :rows="restaurantsList" class="mt-4"/>
</template>