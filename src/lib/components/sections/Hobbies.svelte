<script lang="ts">
	const hobbies: { name: string; note: string }[] = [
		{
			name: 'Plants',
			note: 'Growing a small jungle at home and learning what actually thrives in this climate.'
		},
		{
			name: 'Surfing',
			note: 'The reason a lot of weekends start before sunrise.'
		},
		{
			name: 'Weaving',
			note: 'Working a loom by hand — a slow, tactile counterweight to screen time.'
		},
		{
			name: 'Mālama ʻāina',
			note: 'Monthly community work days with Kauluakalana in Kailua for several years, helping restore Kawainui — the marsh and loko iʻa (fishpond) — alongside broader conservation and cultural-preservation efforts across Oʻahu.'
		},
		{
			name: 'Energy & food sovereignty',
			note: 'Advocating for ocean and clean energy, driving electric and pushing for better charging access, and generally caring about reconnecting with land and where things come from in Hawaiʻi.'
		}
	];
</script>

<section>
	<h2>Hobbies</h2>
	<p>Away from the keyboard, mostly outside or making something with my hands.</p>
	<ul class="list">
		{#each hobbies as hobby (hobby.name)}
			<li>
				<span class="name">{hobby.name}</span>
				<span class="note">{hobby.note}</span>
			</li>
		{/each}
	</ul>
</section>

<style>
	.list {
		list-style: none;
		padding: 0;
		margin: 1.5rem 0 0 0;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.list li {
		display: flex;
		flex-direction: column;
		gap: 0.2rem;
		border-left: 2px solid var(--color-border);
		padding-left: 1rem;
	}

	.name {
		color: var(--color-text);
		font-weight: 500;
	}

	.note {
		color: var(--color-text-muted);
		font-size: 0.9rem;
		line-height: 1.55;
	}
</style>
