<script lang="ts">
	import { onMount } from 'svelte';

	const name = 'Jack Runburg';
	const letters = name.split('');

	let nameEl: HTMLElement;
	let ready = $state(false);
	let flying = $state(false);
	let reduced = $state(false);
	let facing = $state<1 | -1>(1);
	let turnAnim = $state<'none' | 'l2r' | 'r2l'>('none');

	// Perch positions (top-centre of each non-space letter), in .name-local px.
	let perches: { x: number; y: number }[] = [];
	let curPerch = 0;
	let hopTimer: ReturnType<typeof setTimeout>;
	let stepTimer: ReturnType<typeof setTimeout>;
	let turnTimer: ReturnType<typeof setTimeout>;
	let resizeRaf = 0;

	// bird box size — keep in sync with .bird's width/height below
	const BW = 46;
	const BH = 26;
	// how far the feet sink into a letter's calibrated landing point, so it
	// reads as standing on the glyph rather than hovering above it
	const PERCH_SINK = 0;

	// Where a glyph visually starts, as a fraction down the letter's line box —
	// caps/ascenders start near the top, x-height letters start lower. These
	// are calibrated by eye against the display font; nudge if a perch looks off.
	const ASCENDERS = new Set(['b', 'd', 'f', 'h', 'k', 'l', 't']);
	const CAP_TOP = 0.14;
	const X_HEIGHT_TOP = 0.46;
	function topFrac(ch: string) {
		return /[A-Z]/.test(ch) || ASCENDERS.has(ch) ? CAP_TOP : X_HEIGHT_TOP;
	}

	function measure() {
		if (!nameEl) return;
		const spans = Array.from(nameEl.querySelectorAll<HTMLElement>('.ltr:not(.space)'));
		const box = nameEl.getBoundingClientRect();
		perches = spans.map((s) => {
			const r = s.getBoundingClientRect();
			return {
				x: r.left - box.left + r.width / 2,
				y: r.top - box.top + r.height * topFrac(s.textContent ?? '')
			};
		});
	}

	function place(i: number, animate: boolean) {
		if (!perches.length || !nameEl) return;
		curPerch = i;
		const p = perches[i];
		const targetX = p.x - BW / 2;
		const prevRaw = nameEl.style.getPropertyValue('--bx');
		const prevX = prevRaw ? parseFloat(prevRaw) : targetX;
		const dx = targetX - prevX;

		if (Math.abs(dx) > 1) {
			const newFacing: 1 | -1 = dx < 0 ? -1 : 1;
			if (newFacing !== facing) {
				turnAnim = facing === -1 ? 'l2r' : 'r2l';
				facing = newFacing;
				clearTimeout(turnTimer);
				turnTimer = setTimeout(() => (turnAnim = 'none'), 360);
			}
		}

		nameEl.style.setProperty('--bx', `${targetX}`);
		nameEl.style.setProperty('--by', `${p.y - BH + PERCH_SINK}`);

		if (animate) {
			flying = true;
			clearTimeout(hopTimer);
			hopTimer = setTimeout(() => (flying = false), 780);
		}
	}

	function step() {
		// flit to a nearby-ish letter, favouring short hops
		const n = perches.length;
		let next = curPerch;
		const span = 4;
		while (next === curPerch) {
			const off = Math.floor(Math.random() * (span * 2 + 1)) - span;
			next = Math.min(n - 1, Math.max(0, curPerch + (off || 1)));
		}
		place(next, true);
		stepTimer = setTimeout(step, 1100 + Math.random() * 1700);
	}

	function onResize() {
		cancelAnimationFrame(resizeRaf);
		resizeRaf = requestAnimationFrame(() => {
			measure();
			place(curPerch, false);
		});
	}

	onMount(() => {
		reduced = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

		const start = () => {
			measure();
			if (!perches.length) return;
			const first = reduced ? perches.length - 1 : Math.floor(perches.length / 2);
			place(first, false);
			requestAnimationFrame(() => (ready = true));
			if (!reduced) stepTimer = setTimeout(step, 900);
		};

		if (document.fonts && document.fonts.status !== 'loaded') {
			document.fonts.ready.then(start);
		} else {
			start();
		}

		window.addEventListener('resize', onResize);
		return () => {
			window.removeEventListener('resize', onResize);
			clearTimeout(hopTimer);
			clearTimeout(stepTimer);
			clearTimeout(turnTimer);
			cancelAnimationFrame(resizeRaf);
		};
	});
</script>

<section class="hero">
	<h1 class="name" class:ready class:flying class:reduced bind:this={nameEl} aria-label={name}>
		{#each letters as ch, i (i)}
			<span class="ltr" class:space={ch === ' '} aria-hidden="true">{ch === ' ' ? ' ' : ch}</span>
		{/each}

		<span class="bird" aria-hidden="true">
			<span class="bird-hop">
				<span
					class="bird-flip"
					class:turn-l2r={turnAnim === 'l2r'}
					class:turn-r2l={turnAnim === 'r2l'}
					style="--facing:{facing};"
				>
					<svg viewBox="0 0 84 48">
						<!-- tail -->
						<path class="body" d="M22 30 L3 22 L7 33 L20 36 Z" />
						<!-- body -->
						<ellipse class="body" cx="27" cy="28" rx="15" ry="12" />
						<!-- head -->
						<circle class="body" cx="41" cy="19" r="9" />
						<!-- decurved ʻiʻiwi bill -->
						<path
							class="beak"
							d="M49 16.3 C 57.8 13.2 64.7 15.7 69.8 24.1 C 64.2 17.7 56.9 16.5 49 20.8 Z"
						/>
						<!-- eye -->
						<circle class="eye" cx="43" cy="18" r="1.7" />
						<!-- legs -->
						<line class="leg" x1="24" y1="39" x2="24" y2="45" />
						<line class="leg" x1="31" y1="39" x2="31" y2="45" />
						<!-- wing -->
						<path class="wing" d="M24 24 Q14 20 8 27 Q18 33 27 30 Q27 26 24 24 Z" />
					</svg>
				</span>
			</span>
		</span>
	</h1>

	<p class="tagline">
		Software developer and physics PhD — scientific computing, simulation, robotics, and web
		applications. Based in Honolulu. Plant enthusiast, surfer, and weaver on the side.
	</p>

	<div class="links">
		<a href="https://github.com/runburg" target="_blank" rel="noopener">
			<svg class="ico" viewBox="0 0 16 16" aria-hidden="true">
				<path
					fill="currentColor"
					d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82a7.6 7.6 0 0 1 2-.27c.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.01 8.01 0 0 0 16 8c0-4.42-3.58-8-8-8z"
				/>
			</svg>
			<span>GitHub</span>
		</a>
		<a href="mailto:jack.runburg@gmail.com">
			<svg
				class="ico"
				viewBox="0 0 24 24"
				fill="none"
				stroke="currentColor"
				stroke-width="2"
				stroke-linecap="round"
				stroke-linejoin="round"
				aria-hidden="true"
			>
				<rect x="3" y="5" width="18" height="14" rx="2" />
				<path d="m3 7 9 6 9-6" />
			</svg>
			<span>Email</span>
		</a>
	</div>
</section>

<style>
	.hero {
		padding-top: 1rem;
	}

	.name {
		position: relative;
		margin: 0;
		font-family: var(--font-display);
		font-weight: 500;
		font-size: clamp(2.5rem, 8vw, 4.25rem);
		line-height: 1.1;
		white-space: nowrap;
	}

	.ltr {
		display: inline-block;
	}

	.ltr.space {
		width: 0.28em;
	}

	/* ---- bird ---- */
	.bird {
		position: absolute;
		left: 0;
		top: 0;
		width: 46px;
		height: 26px;
		opacity: 0;
		transform: translate(calc(var(--bx, 0) * 1px), calc(var(--by, 0) * 1px));
		transition:
			transform 0.75s cubic-bezier(0.38, 0.9, 0.3, 1.05),
			opacity 0.4s ease;
	}

	.name.ready .bird {
		opacity: 1;
	}

	.bird-hop {
		display: block;
	}

	.name.flying .bird-hop {
		animation: hop 0.78s ease-in-out;
	}

	/* facing flip lives on its own layer so it never blends with the
	   position transition (that's what made it look flat mid-flight) */
	.bird-flip {
		display: block;
		transform: scaleX(var(--facing, 1));
		transform-origin: 50% 85%;
	}

	.bird-flip.turn-l2r {
		animation: turn-l2r 0.34s ease-in-out;
	}

	.bird-flip.turn-r2l {
		animation: turn-r2l 0.34s ease-in-out;
	}

	.bird svg {
		display: block;
		width: 100%;
		height: 100%;
		overflow: visible;
	}

	.body {
		fill: #af0f2b;
	}

	.beak {
		fill: var(--color-sun);
	}

	.eye {
		fill: #200a0e;
	}

	.leg {
		stroke: var(--color-sun);
		stroke-width: 2;
		stroke-linecap: round;
	}

	.wing {
		fill: #870c22;
		transform-origin: 24px 24px;
		animation: flap 0.4s linear infinite;
	}

	.name.flying .wing {
		animation-duration: 0.16s;
	}

	.name.reduced .wing {
		animation: none;
	}

	/* four-pose flutter instead of a single back-and-forth tween */
	@keyframes flap {
		0% {
			transform: rotate(-4deg) scaleY(1);
		}
		25% {
			transform: rotate(-38deg) scaleY(0.92);
		}
		50% {
			transform: rotate(-54deg) scaleY(0.85);
		}
		75% {
			transform: rotate(-28deg) scaleY(0.95);
		}
		100% {
			transform: rotate(-4deg) scaleY(1);
		}
	}

	@keyframes hop {
		0% {
			transform: translateY(0);
		}
		45% {
			transform: translateY(-15px);
		}
		100% {
			transform: translateY(0);
		}
	}

	/* turnaround flourish: a quick pinch + wing-lift + pop, not a flat scaleX tween */
	@keyframes turn-l2r {
		0% {
			transform: scaleX(-1) rotate(0deg) translateY(0);
		}
		30% {
			transform: scaleX(-0.22) rotate(16deg) translateY(-3px);
		}
		55% {
			transform: scaleX(0.22) rotate(-12deg) translateY(-4px);
		}
		80% {
			transform: scaleX(1.1) rotate(4deg) translateY(-1px);
		}
		100% {
			transform: scaleX(1) rotate(0deg) translateY(0);
		}
	}

	@keyframes turn-r2l {
		0% {
			transform: scaleX(1) rotate(0deg) translateY(0);
		}
		30% {
			transform: scaleX(0.22) rotate(-16deg) translateY(-3px);
		}
		55% {
			transform: scaleX(-0.22) rotate(12deg) translateY(-4px);
		}
		80% {
			transform: scaleX(-1.1) rotate(-4deg) translateY(-1px);
		}
		100% {
			transform: scaleX(-1) rotate(0deg) translateY(0);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.bird {
			transition:
				opacity 0.4s ease,
				transform 0s;
		}
		.wing {
			animation: none;
		}
		.name.flying .bird-hop {
			animation: none;
		}
		.bird-flip.turn-l2r,
		.bird-flip.turn-r2l {
			animation: none;
		}
	}

	/* ---- text ---- */
	.tagline {
		font-size: 1.05rem;
		margin-top: 0.75rem;
	}

	.links {
		margin-top: 1.25rem;
		display: flex;
		gap: 1.1rem;
		font-size: 0.9rem;
	}

	.links a {
		display: inline-flex;
		align-items: center;
		gap: 0.4rem;
		color: var(--color-text-muted);
		text-decoration: none;
	}

	.links a:hover {
		color: var(--color-tide);
	}

	.ico {
		width: 1.15em;
		height: 1.15em;
		flex: 0 0 auto;
	}
</style>
