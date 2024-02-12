<script setup lang="ts">
definePageMeta({
	middleware: ['auth'],
});
//supabase
const router = useRouter();
const user = useSupabaseUser();
const client = useSupabaseClient();

//Assigning the username from the db to a variable
const { data } = await client
    .from('profiles')
    .select(`username, first_name, last_name, email`)
    .eq('id', user.value!.id)
    .single();

const username = data?.username as unknown as string;

async function logOut() {
	try {
    await client.auth.signOut();
		await router.push('/login');
	} catch (err: any) {
		console.log(err.message);
	}
}
</script>

<template>
	<UCard class="mx-auto max-w-[720px] max-h-[200px] mt-10">
		<div class="flex justify-between">
			<span v-if="username">Welcome {{ username }}</span>
      <div v-else class="flex flex-col gap-2 border-[1px] border-b-red-600">
        <span>Welcome {{ user?.email }}</span>
        <span>Update <NuxtLink to="/profile">Profile</NuxtLink></span>
      </div>
			<div>
				<UButton label="Log out" @click="logOut" />
			</div>
		</div>
	</UCard>
</template>
