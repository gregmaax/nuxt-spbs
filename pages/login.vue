<script setup lang="ts">
import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'

const client = useSupabaseClient();
const router = useRouter();
const errorMsg = ref<string | null>(null);


const schema = z.object({
  email: z.string().email('Invalid email'),
  password: z.string().min(8, 'Must be at least 8 characters')
})

type Schema = z.output<typeof schema>

const state = reactive({
  email: undefined,
  password: undefined
})

async function onSubmit (event: FormSubmitEvent<Schema>) {
  try {
    const {data, error} = await client.auth.signInWithPassword({
      email: event.data.email,
      password: event.data.password,
    });
    console.log(data);
    console.log(event.data);
    if(error) throw error;
    router.push("/");
  } catch (error){
    errorMsg.value = "Invalid credentials. Try again";
    console.log(error);
  }
}
</script>

<template>
  <UCard class="max-w-[500px] mx-auto mt-[100px]">
    <template #header>
      <h1>Sign in</h1>
    </template>
    <UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
      <UFormGroup label="Email" name="email">
        <UInput v-model="state.email" />
      </UFormGroup>

      <UFormGroup label="Password" name="password">
        <UInput v-model="state.password" type="password" />
      </UFormGroup>

      <div v-if="errorMsg" class="text-red-700">{{errorMsg}}</div>

      <UButton type="submit">
        Sign in
      </UButton>
    </UForm>
    <template #footer>
      <p>Don't have an account yet ? <ULink to="/register" inactive-class="text-primary">Register</ULink></p>
    </template>
  </UCard>

</template>