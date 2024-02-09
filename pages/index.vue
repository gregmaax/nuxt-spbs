<script setup lang="ts">
const router = useRouter();
const user = useSupabaseUser();
const client = useSupabaseClient();

async function logOut(){
  try {
    const {error} = await client.auth.signOut();
    if(error) throw error
    await router.push("/login");
  } catch (err){
    console.log(err);
  }
}
</script>

<template>
  <h1>Home component</h1>
  <Alert />
  <UCard class="mx-auto max-w-[720px] max-h-[100px]">
      <div>
        Email : {{user?.email}}
      </div>
    <UButton label="Log out" @click="logOut"/>
  </UCard>

</template>