<!-- src/lib/components/PaymentForm.svelte -->
<script>
  import { onMount } from 'svelte';
  import Stripe from '@stripe/stripe-js';
  let stripe, elements, card;

  onMount(async () => {
    stripe = Stripe('your-stripe-public-key');
    elements = stripe.elements();
    card = elements.create('card');
    card.mount('#card-element');
  });

  const handlePayment = async () => {
    const { token } = await stripe.createToken(card);
    // Send token to your server to process payment
  };
</script>

<form on:submit|preventDefault={handlePayment}>
  <div id="card-element" class="my-4"></div>
  <button type="submit" class="bg-blue-500 text-white py-2 px-4 rounded">Pay</button>
</form>

<style>
  #card-element {
    border: 1px solid #ccc;
    padding: 10px;
    border-radius: 5px;
  }
</style>
