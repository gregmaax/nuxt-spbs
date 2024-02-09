<script setup lang="ts">
import { z } from 'zod';
import type { FormSubmitEvent } from '#ui/types';

const client = useSupabaseClient();
const successMsg = ref<string | null>(null);

const schema = z.object({
	email: z.string().email('Invalid email'),
	password: z.string().min(8, 'Must be at least 8 characters'),
});

type Schema = z.output<typeof schema>;

const state = reactive({
	email: '',
	password: '',
});

async function onSubmit(event: FormSubmitEvent<Schema>) {
	try {
		const { data, error } = await client.auth.signUp({
			email: event.data.email,
			password: event.data.password,
		});
		console.log(event.data);
		if (error) throw error;
		successMsg.value = 'Check your email to confirm your account';
	} catch (error) {
		console.log(error);
	}
}
</script>

<template>
	<UCard class="max-w-[500px] mx-auto mt-[100px]">
		<template #header>
			<h1>Register</h1>
		</template>
		<UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
			<UFormGroup label="Email" name="email">
				<UInput v-model="state.email" />
			</UFormGroup>

			<UFormGroup label="Password" name="password">
				<UInput v-model="state.password" type="password" />
			</UFormGroup>

			<div v-if="successMsg" class="text-green-700">{{ successMsg }}</div>

			<UButton type="submit"> Register </UButton>
		</UForm>
		<template #footer>
			<p>
				Already registered ?
				<ULink to="/login" inactive-class="text-primary">Sign in</ULink>
			</p>
		</template>
	</UCard>
</template>
