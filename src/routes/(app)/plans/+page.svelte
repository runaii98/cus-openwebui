<script lang="ts">
	import PlansGrid from '$lib/components/plans/PlansGrid.svelte';
	import { user } from '$lib/stores';
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	onMount(() => {
		if (!$user) {
			goto('/auth');
		}
	});
</script>

<svelte:head>
	<title>Plans - AI Chat Platform</title>
</svelte:head>

<div class="flex flex-col h-full">
	<div class="px-6 py-8">
		<div class="max-w-6xl mx-auto">
			<div class="text-center mb-12">
				<h1 class="text-4xl font-bold text-gray-900 dark:text-white mb-4">
					Choose Your Plan
				</h1>
				<p class="text-xl text-gray-600 dark:text-gray-300">
					Select the perfect plan for your AI chat needs
				</p>
			</div>
			<PlansGrid />
		</div>
	</div>
</div>