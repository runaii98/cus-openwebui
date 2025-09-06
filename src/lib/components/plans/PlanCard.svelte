<script lang="ts">
	import PaymentButton from './PaymentButton.svelte';

	export let plan: {
		id: string;
		name: string;
		price: number;
		currency: string;
		features: string[];
		razorpayPlanId: string;
		tokensPerMonth: number;
		modelAccess: string[];
		popular?: boolean;
	};
	export let currentUser: any;

	$: isCurrentPlan = currentUser?.subscription?.plan_id === plan.id;
</script>

<div class="relative bg-white dark:bg-gray-800 rounded-xl shadow-xl p-8 transition-all duration-300 hover:shadow-2xl {plan.popular ? 'ring-2 ring-blue-500 scale-105' : 'hover:scale-105'}">
	{#if plan.popular}
		<div class="absolute -top-4 left-1/2 transform -translate-x-1/2">
			<span class="bg-gradient-to-r from-blue-500 to-purple-600 text-white text-sm font-semibold px-4 py-2 rounded-full shadow-lg">
				Most Popular
			</span>
		</div>
	{/if}

	{#if isCurrentPlan}
		<div class="absolute -top-4 right-4">
			<span class="bg-green-500 text-white text-sm font-semibold px-3 py-1 rounded-full">
				Current Plan
			</span>
		</div>
	{/if}

	<div class="text-center mb-8">
		<h3 class="text-2xl font-bold text-gray-900 dark:text-white mb-2">{plan.name}</h3>
		<div class="text-5xl font-bold text-gray-900 dark:text-white mb-2">
			₹{(plan.price / 100).toFixed(2)}
		</div>
		<span class="text-gray-500 dark:text-gray-400">/month</span>
	</div>

	<ul class="space-y-4 mb-8">
		{#each plan.features as feature}
			<li class="flex items-start">
				<svg class="w-5 h-5 text-green-500 mr-3 mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
					<path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
				</svg>
				<span class="text-gray-700 dark:text-gray-300">{feature}</span>
			</li>
		{/each}
	</ul>

	<div class="mt-auto">
		{#if isCurrentPlan}
			<button disabled class="w-full bg-gray-300 text-gray-600 font-semibold py-3 px-6 rounded-lg cursor-not-allowed">
				Current Plan
			</button>
		{:else}
			<PaymentButton {plan} />
		{/if}
	</div>
</div>