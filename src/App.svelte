<script>
	import Content from './Content.svelte';
	import Modal from './Modal.svelte';	
					
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto; 
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		padding-bottom: 1em;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>

<svelte:head>
	<title>Quarantine Notes</title>
</svelte:head>

<main>
	<h1>Quarantine Notes</h1>
	<Modal>
		<Content/>
	</Modal>
</main>
