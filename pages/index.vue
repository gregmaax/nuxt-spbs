<script setup lang="ts">
definePageMeta({
	middleware: ['auth'],
});
//supabase
const router = useRouter();
const user = useSupabaseUser();
const client = useSupabaseClient();

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
			<span>Email : {{ user?.email }}</span>
			<div>
				<UButton label="Log out" @click="logOut" />
			</div>
		</div>
	</UCard>
</template>
