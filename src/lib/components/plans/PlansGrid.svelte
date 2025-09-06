<script lang="ts">
	import PlanCard from './PlanCard.svelte';
	import { user } from '$lib/stores';

	const plans = [
		{
			id: 'basic',
			name: 'Basic',
			price: 999, // ₹9.99 in paise
			currency: 'INR',
			features: [
				'1,000 tokens/month',
				'GPT-3.5 Turbo access',
				'Basic support',
				'Web interface access',
				'Chat history (7 days)'
			],
			razorpayPlanId: 'plan_basic_monthly',
			tokensPerMonth: 1000,
			modelAccess: ['gpt-3.5-turbo']
		},
		{
			id: 'pro',
			name: 'Pro',
			price: 1999, // ₹19.99 in paise
			currency: 'INR',
			features: [
				'10,000 tokens/month',
				'GPT-4 & GPT-3.5 access',
				'Priority support',
				'Advanced features',
				'Chat history (30 days)',
				'API access'
			],
			razorpayPlanId: 'plan_pro_monthly',
			tokensPerMonth: 10000,
			modelAccess: ['gpt-4', 'gpt-3.5-turbo'],
			popular: true
		},
		{
			id: 'enterprise',
			name: 'Enterprise',
			price: 4999, // ₹49.99 in paise
			currency: 'INR',
			features: [
				'Unlimited tokens',
				'All AI models access',
				'24/7 dedicated support',
				'Custom integrations',
				'Unlimited chat history',
				'Full API access',
				'Team collaboration'
			],
			razorpayPlanId: 'plan_enterprise_monthly',
			tokensPerMonth: -1, // Unlimited
			modelAccess: ['gpt-4', 'gpt-3.5-turbo', 'claude-3', 'gemini-pro']
		}
	];
</script>

<div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-12">
	{#each plans as plan}
		<PlanCard {plan} currentUser={$user} />
	{/each}
</div>

<div class="text-center text-sm text-gray-500 dark:text-gray-400">
	<p>All plans include secure data handling and GDPR compliance.</p>
	<p>Cancel anytime. No hidden fees. 7-day money-back guarantee.</p>
</div>