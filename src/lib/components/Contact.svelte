<!-- src/lib/components/Contact.svelte -->
<script lang="ts">
  let name = '';
  let email = '';
  let message = '';
  let success = false;
  let error = '';

  const handleSubmit = async () => {
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ name, email, message })
      });

      if (response.ok) {
        success = true;
        name = '';
        email = '';
        message = '';
        error = '';
      } else {
        const result = await response.json();
        error = result.error || 'An error occurred while sending your message.';
      }
    } catch (err) {
      error = 'An error occurred while sending your message.';
      console.error('Error sending message:', err);
    }
  };
</script>

<div class="contact-form">
  <h2>Contact Us</h2>
  {#if success}
    <p class="text-green-500 mb-6">Your message has been sent successfully!</p>
  {/if}
  {#if error}
    <p class="text-red-500 mb-6">{error}</p>
  {/if}
  <form on:submit|preventDefault={handleSubmit} class="w-full">
    <div class="mb-4">
      <label for="name">Name</label>
      <input type="text" id="name" bind:value={name} required />
    </div>
    <div class="mb-4">
      <label for="email">Email</label>
      <input type="email" id="email" bind:value={email} required />
    </div>
    <div class="mb-4">
      <label for="message">Message</label>
      <textarea id="message" bind:value={message} required rows="4"></textarea>
    </div>
    <button type="submit">Send Message</button>
  </form>
</div>
