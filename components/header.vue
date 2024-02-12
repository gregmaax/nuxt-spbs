<script setup lang="ts">
const user = useSupabaseUser();

const links = [
	{
		label: 'Home',
		to: '/',
	},
	{
		label: 'Restaurants',
		to: '/items',
	},
	{
		label: 'Profile',
		to: '/profile',
	},
];
</script>

<template>
	<header>
		<div class="flex justify-center">
			<UHorizontalNavigation
				:links="links"
				class="flex justify-center gap-2 max-w-[300px]"
			/>
		</div>
	</header>
</template>
