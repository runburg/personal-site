<script lang="ts">
	import { fly } from 'svelte/transition';
	import Sidebar from '$lib/components/Sidebar.svelte';
	import Home from '$lib/components/sections/Home.svelte';
	import Research from '$lib/components/sections/Research.svelte';
	import Skills from '$lib/components/sections/Skills.svelte';
	import Hobbies from '$lib/components/sections/Hobbies.svelte';

	type Section = 'home' | 'research' | 'skills' | 'hobbies';
	let active: Section = $state('home');
</script>

<svelte:head>
	<title>Jack Runburg</title>
	<meta
		name="description"
		content="Jack Runburg — software developer and physics PhD working on scientific computing, simulation, and web applications in Honolulu."
	/>
</svelte:head>

<Sidebar bind:active />

<main>
	{#key active}
		<div class="panel" in:fly={{ y: 12, duration: 260 }}>
			{#if active === 'home'}
				<Home />
			{:else if active === 'research'}
				<Research />
			{:else if active === 'skills'}
				<Skills />
			{:else if active === 'hobbies'}
				<Hobbies />
			{/if}
		</div>
	{/key}
</main>

<style>
	main {
		margin-left: var(--sidebar-collapsed);
		min-height: 100vh;
		padding: 4rem 3rem;
		max-width: 48rem;
	}

	.panel {
		width: 100%;
	}
</style>
