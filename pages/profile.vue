<script setup lang="ts">
import {z} from "zod";
import type {FormSubmitEvent} from "#ui/types";
import {AuthApiError, AuthError} from "@supabase/gotrue-js";

//supabase
const user = useSupabaseUser();
const client = useSupabaseClient();

const { data } = await client
    .from('profiles')
    .select(`username, first_name, last_name, email`)
    .eq('id', user.value!.id)
    .single()

console.log(data?.email)

//form schema
const schema = z.object({
  email: z.string().email('Invalid email'),
  username: z.string().min(3, "Must be at least 3 characters"),
  first_name: z.string().optional(),
  last_name: z.string().optional()
})

type Schema = z.output<typeof schema>

const state = reactive({
  username: data?.username,
  first_name: data?.first_name,
  last_name: data?.last_name,
  email: user.value?.email,
})

//logic
async function updateProfile() {


}
async function onSubmit (event: FormSubmitEvent<Schema>) {
  try {
    //loading.value = true

    const updates = {
      id: user.value?.id,
      updated_at: new Date(),
      username: event.data.username,
      first_name: event.data.first_name,
      last_name: event.data.last_name,
      email: user.value?.email,
  }

    const { error } = await client.from('profiles').upsert(updates as never)
    if (error) throw error
  } catch (error: any) {
    console.log(error.message)
  } finally {
    //loading.value = false
    console.log("donerrr")
  }
}
</script>

<template>
  <UCard class="max-w-[720px] mx-auto mt-[100px] p-10">
    <UForm :schema="schema" :state="state" class="space-y-4 flex justify-between" @submit="onSubmit">
      <div class="border-r-green-500 flex flex-col gap-4 max-w-[600px]">
        <UFormGroup label="Email" name="email">
          <UInput v-model="state.email" />
        </UFormGroup>
        <UFormGroup label="Username" name="username">
          <UInput v-model="state.username" />
        </UFormGroup>
        <UFormGroup label="First name" name="first_name">
          <UInput v-model="state.first_name" />
        </UFormGroup>
        <UFormGroup label="Last name" name="last_name">
          <UInput v-model="state.last_name" />
        </UFormGroup>
      </div>
      <div>
        <UButton label="Update profile" type="submit"/>
      </div>
    </UForm>
  </UCard>
</template>