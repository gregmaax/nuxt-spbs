<script setup lang="ts">
import { z} from "zod";
import type {FormSubmitEvent} from "#ui/types";

//supabase
const client = useSupabaseClient();
const user = useSupabaseUser();

const schema = z.object({
  name: z.string(),
  city: z.string().optional(),
  rating: z.number().multipleOf(0.01)
      .min(0, "Rating must be between 0 and 10")
      .max(10, "Rating must be between 0 and 10")
      .optional(),
  hasEaten: z.boolean().optional()
})

type Schema = z.output<typeof schema>

const state = reactive({
  name: undefined,
  city: undefined,
  rating: undefined,
  hasEaten: undefined,
})

async function onSubmit (event: FormSubmitEvent<Schema>) {
  //add restaurant
  const insert = {
    name: event.data.name,
    city: event.data.city,
    rating: event.data.rating,
    hasEaten: event.data.hasEaten,
  }
  try {
    const { data, error } = await client
        .from('restaurants')
        .insert(insert as never)
        .select('*')
    console.log(data)
  } catch (error: any){
    console.log(error.message);
  }
}
</script>

<template>
  <UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
    <UFormGroup label="Name" name="name">
      <UInput v-model="state.name" type="text" />
    </UFormGroup>

    <UFormGroup label="City" name="city">
      <UInput v-model="state.city" type="text" />
    </UFormGroup>

    <UFormGroup label="Rating" name="rating">
      <UInput v-model="state.rating" type="number" step=".1"/>
    </UFormGroup>

    <UFormGroup label="Ate there ?" name="hasEaten">
      <UToggle v-model="state.hasEaten" />
    </UFormGroup>

    <UButton type="submit">
      Add restaurant
    </UButton>
  </UForm>
</template>

<style scoped>

</style>