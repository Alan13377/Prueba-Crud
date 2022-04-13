<script>
	import Navbar from '../components/Navbar.svelte';
</script>

<main class="container">
	<Navbar />
	<slot />
</main>

<style>
	.container {
		max-width: 1200px;
		margin: 0 auto;
		font-family: 'Lato', sans-serif;
	}
</style>
