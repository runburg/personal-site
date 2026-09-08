<script lang="ts">
	let { size = 24 }: { size?: number } = $props();
</script>

<svg viewBox="0 0 100 100" width={size} height={size} role="img" aria-label="Research">
	<circle cx="50" cy="50" r="7" fill="currentColor" />
	<ellipse
		cx="50"
		cy="50"
		rx="38"
		ry="16"
		fill="none"
		stroke="currentColor"
		stroke-width="4"
		opacity="0.5"
	/>
	<g class="electron">
		<circle cx="88" cy="50" r="5" fill="var(--color-tide)" />
	</g>
</svg>

<style>
	svg {
		display: block;
		color: var(--color-text-muted);
		overflow: visible;
	}

	:global(button:hover) svg,
	:global(.active) svg {
		color: var(--color-sun);
	}

	.electron {
		transform-origin: 50px 50px;
		opacity: 0;
		transition: opacity 0.3s ease;
	}

	:global(button:hover) .electron,
	:global(.active) .electron {
		opacity: 1;
		animation: orbit 1.4s linear infinite;
	}

	@keyframes orbit {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>
