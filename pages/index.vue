<script setup lang="ts">
definePageMeta({
	middleware: ['auth'],
});
//supabase
const router = useRouter();
const user = useSupabaseUser();
const client = useSupabaseClient();

const { data } = await client
    .from('profiles')
    .select(`username, first_name, last_name, email`)
    .eq('id', user.value!.id)
    .single();

const username = data?.username as unknown as string;

async function logOut() {
	try {
		const { error } = await client.auth.signOut();
		if (error) throw error;
		await router.push('/login');
	} catch (err) {
		console.log(err);
	}
}
</script>

<template>
	<UCard class="mx-auto max-w-[720px] max-h-[100px] mt-10">
		<div class="flex justify-between">
			<span v-if="username">Welcome {{ username }}</span>
      <span v-else>Welcome {{ user?.email }}</span>
			<div>
				<UButton label="Log out" @click="logOut" />
			</div>
		</div>
	</UCard>
</template>
