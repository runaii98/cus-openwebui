<script lang="ts">
	import { toast } from 'svelte-sonner';
	import { user } from '$lib/stores';
	import { goto } from '$app/navigation';

	export let plan: any;

	let processing = false;

	const handlePayment = async () => {
		if (!$user) {
			toast.error('Please login to subscribe');
			goto('/auth');
			return;
		}

		processing = true;

		try {
			// Load Razorpay script dynamically
			if (!window.Razorpay) {
				await loadRazorpayScript();
			}

			// Create order (this would be an API call to your backend)
			const order = await createRazorpayOrder(plan);

			const options = {
				key: 'rzp_test_your_key_id', // Replace with your actual Razorpay key
				amount: plan.price,
				currency: plan.currency,
				name: 'AI Chat Platform',
				description: `${plan.name} Plan Subscription`,
				order_id: order.id,
				handler: function (response) {
					handlePaymentSuccess(response);
				},
				prefill: {
					name: $user.name,
					email: $user.email
				},
				theme: {
					color: '#3B82F6'
				},
				modal: {
					ondismiss: function() {
						processing = false;
					}
				}
			};

			const rzp = new window.Razorpay(options);
			rzp.open();

		} catch (error) {
			console.error('Payment initialization failed:', error);
			toast.error('Payment initialization failed. Please try again.');
			processing = false;
		}
	};

	const loadRazorpayScript = () => {
		return new Promise((resolve, reject) => {
			const script = document.createElement('script');
			script.src = 'https://checkout.razorpay.com/v1/checkout.js';
			script.onload = resolve;
			script.onerror = reject;
			document.head.appendChild(script);
		});
	};

	const createRazorpayOrder = async (plan) => {
		// This would be an API call to your backend to create Razorpay order
		const response = await fetch('/api/payment/create-order', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				'Authorization': `Bearer ${localStorage.token}`
			},
			body: JSON.stringify({
				planId: plan.id,
				amount: plan.price,
				currency: plan.currency
			})
		});

		if (!response.ok) {
			throw new Error('Failed to create payment order');
		}

		return response.json();
	};

	const handlePaymentSuccess = async (response) => {
		try {
			// Verify payment on backend
			const verification = await fetch('/api/payment/verify', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
					'Authorization': `Bearer ${localStorage.token}`
				},
				body: JSON.stringify({
					razorpay_order_id: response.razorpay_order_id,
					razorpay_payment_id: response.razorpay_payment_id,
					razorpay_signature: response.razorpay_signature,
					plan_id: plan.id
				})
			});

			if (verification.ok) {
				toast.success('Payment successful! Your plan has been activated.');
				// Redirect to dashboard or refresh user data
				setTimeout(() => {
					window.location.href = '/';
				}, 2000);
			} else {
				throw new Error('Payment verification failed');
			}
		} catch (error) {
			console.error('Payment verification failed:', error);
			toast.error('Payment verification failed. Please contact support.');
		} finally {
			processing = false;
		}
	};
</script>

<button
	on:click={handlePayment}
	disabled={processing}
	class="w-full bg-gradient-to-r from-blue-600 to-purple-600 hover:from-blue-700 hover:to-purple-700 text-white font-semibold py-3 px-6 rounded-lg transition-all duration-300 transform hover:scale-105 {processing ? 'opacity-50 cursor-not-allowed' : 'hover:shadow-lg'}"
>
	{#if processing}
		<span class="flex items-center justify-center">
			<svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
				<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
				<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
			</svg>
			Processing...
		</span>
	{:else}
		Subscribe to {plan.name}
	{/if}
</button>