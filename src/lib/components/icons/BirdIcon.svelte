<script lang="ts">
	let { size = 24, playKey = 0 }: { size?: number; playKey?: number } = $props();
</script>

<svg viewBox="0 0 100 100" width={size} height={size} role="img" aria-label="Hobbies">
	<line x1="10" y1="72" x2="90" y2="72" stroke="var(--color-border)" stroke-width="3" />
	{#key playKey}
		<g class="bird">
			<path
				d="M50 70 C40 70 34 64 34 58 C40 60 46 60 50 56 C54 60 60 60 66 58 C66 64 60 70 50 70 Z"
				fill="currentColor"
			/>
			<path class="wing" d="M50 58 C46 48 38 44 30 46 C36 52 42 56 50 58 Z" fill="currentColor" />
			<path
				class="wing wing-r"
				d="M50 58 C54 48 62 44 70 46 C64 52 58 56 50 58 Z"
				fill="currentColor"
			/>
		</g>
	{/key}
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

	.bird {
		animation: land 0.6s ease-out;
		transform-origin: 50px 72px;
	}

	.wing {
		transform-origin: 50px 58px;
		animation: flap 0.35s ease-in-out 3;
	}

	.wing-r {
		animation-name: flap-r;
	}

	@keyframes land {
		0% {
			transform: translateY(-22px) translateX(10px) rotate(-8deg);
			opacity: 0;
		}
		70% {
			opacity: 1;
		}
		100% {
			transform: translateY(0) translateX(0) rotate(0deg);
			opacity: 1;
		}
	}

	@keyframes flap {
		0%,
		100% {
			transform: rotate(0deg);
		}
		50% {
			transform: rotate(-25deg);
		}
	}

	@keyframes flap-r {
		0%,
		100% {
			transform: rotate(0deg);
		}
		50% {
			transform: rotate(25deg);
		}
	}
</style>
