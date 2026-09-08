<script lang="ts">
	let { size = 40 }: { size?: number } = $props();
	let hovered = $state(false);
</script>

<!--
	The one continuous, always-on animation on the page. Everything else
	is quiet by comparison and only moves in response to hover/open.
-->
<svg
	viewBox="0 0 100 100"
	width={size}
	height={size}
	role="img"
	aria-label="Orbiting planet logo"
	class:hovered
	onmouseenter={() => (hovered = true)}
	onmouseleave={() => (hovered = false)}
>
	<circle cx="50" cy="50" r="12" fill="var(--color-sun)" class="sun" />
	<circle
		cx="50"
		cy="50"
		r="34"
		fill="none"
		stroke="var(--color-border)"
		stroke-width="1"
		stroke-dasharray="2 4"
	/>
	<g class="orbit">
		<circle cx="50" cy="16" r="5" fill="var(--color-tide)" />
	</g>
</svg>

<style>
	svg {
		display: block;
		overflow: visible;
	}

	.sun {
		transform-origin: 50px 50px;
		transition: transform 0.4s ease;
	}

	.hovered .sun {
		transform: scale(1.12);
	}

	.orbit {
		transform-origin: 50px 50px;
		animation: spin 7s linear infinite;
	}

	.hovered .orbit {
		animation-duration: 2.2s;
	}

	@keyframes spin {
		from {
			transform: rotate(0deg);
		}
		to {
			transform: rotate(360deg);
		}
	}
</style>
