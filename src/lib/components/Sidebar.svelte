<script lang="ts">
	import OrbitLogo from './icons/OrbitLogo.svelte';
	import AtomIcon from './icons/AtomIcon.svelte';
	import GearIcon from './icons/GearIcon.svelte';
	import BirdIcon from './icons/BirdIcon.svelte';

	type Section = 'research' | 'skills' | 'hobbies';

	let {
		active = $bindable('research'),
		onnavigate
	}: { active?: Section; onnavigate?: (s: Section) => void } = $props();

	let expanded = $state(false);

	const items: { id: Section; label: string }[] = [
		{ id: 'research', label: 'Research' },
		{ id: 'skills', label: 'Skills' },
		{ id: 'hobbies', label: 'Hobbies' }
	];

	function select(id: Section) {
		active = id;
		onnavigate?.(id);
	}
</script>

<nav
	class="sidebar"
	class:expanded
	onmouseenter={() => (expanded = true)}
	onmouseleave={() => (expanded = false)}
>
	<div class="brand">
		<OrbitLogo size={36} />
		{#if expanded}
			<span class="name">Jack</span>
		{/if}
	</div>

	<ul>
		{#each items as item (item.id)}
			<li>
				<button
					class:active={active === item.id}
					onclick={() => select(item.id)}
					aria-current={active === item.id}
				>
					<span class="icon">
						{#if item.id === 'research'}
							<AtomIcon />
						{:else if item.id === 'skills'}
							<GearIcon active={active === 'skills'} />
						{:else if item.id === 'hobbies'}
							<BirdIcon playKey={active === 'hobbies' ? 1 : 0} />
						{/if}
					</span>
					{#if expanded}
						<span class="label">{item.label}</span>
					{/if}
				</button>
			</li>
		{/each}
	</ul>
</nav>

<style>
	.sidebar {
		position: fixed;
		top: 0;
		left: 0;
		bottom: 0;
		width: var(--sidebar-collapsed);
		background: var(--color-panel);
		border-right: 1px solid var(--color-border);
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		padding: 1.25rem 0.75rem;
		transition: width 0.25s ease;
		overflow: hidden;
		z-index: 10;
	}

	.sidebar.expanded {
		width: var(--sidebar-expanded);
	}

	.brand {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		padding-left: 0.15rem;
		min-height: 36px;
	}

	.name {
		font-family: var(--font-display);
		font-size: 1.15rem;
		white-space: nowrap;
	}

	ul {
		list-style: none;
		margin: 0;
		padding: 0;
		display: flex;
		flex-direction: column;
		gap: 0.4rem;
	}

	button {
		display: flex;
		align-items: center;
		gap: 0.9rem;
		width: 100%;
		background: none;
		border: none;
		border-radius: 0.6rem;
		padding: 0.6rem 0.65rem;
		cursor: pointer;
		color: var(--color-text-muted);
		transition: background 0.2s ease;
	}

	button:hover {
		background: var(--color-panel-hover);
	}

	button.active {
		background: var(--color-panel-hover);
		color: var(--color-text);
	}

	.icon {
		flex: 0 0 auto;
		display: flex;
	}

	.label {
		font-size: 0.95rem;
		white-space: nowrap;
	}
</style>
