<script>
	import { Navbar, NavbarBrand } from 'sveltestrap';
	import InitialScreen from './InitialScreen.svelte';
	import Trivia from './Trivia.svelte';

	let category;
	let difficulty;
    $: console.log(difficulty)
	$: console.log(category)
	let showInitial = true;
	const screen = (e) => {
		console.log('clicked');
		const params = e.detail
		category = params.category;
		difficulty = params.difficulty;
		showInitial = false;
	}
</script>

<main>
	<Navbar color="info" light="primary">
		<NavbarBrand href="/">Trivia Game</NavbarBrand>
	</Navbar>
	<div class="content">
		{#if showInitial}
			<InitialScreen on:continue={screen}></InitialScreen>
		{:else}
			<Trivia bind:category={category} bind:difficulty={difficulty}></Trivia>
		{/if}
	</div>
</main>


<style>
	main {
		text-align: left;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	.content {
		margin-top: 200px;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>