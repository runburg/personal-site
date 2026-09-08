<script lang="ts">
	let { size = 24, active = false }: { size?: number; active?: boolean } = $props();
</script>

<svg viewBox="0 0 100 100" width={size} height={size} role="img" aria-label="Skills">
	<g class="gear" class:active>
		<circle cx="50" cy="50" r="16" fill="none" stroke="currentColor" stroke-width="6" />
		{#each Array(8) as _, i}
			<rect
				x="47"
				y="6"
				width="6"
				height="14"
				rx="1.5"
				fill="currentColor"
				transform="rotate({i * 45} 50 50)"
			/>
		{/each}
	</g>
</svg>

<style>
	svg {
		display: block;
		color: var(--color-text-muted);
	}

	.gear {
		transform-origin: 50px 50px;
		transition: transform 0.6s ease;
	}

	:global(button:hover) .gear,
	.gear.active {
		transform: rotate(50deg);
		color: var(--color-sun);
	}
</style>
