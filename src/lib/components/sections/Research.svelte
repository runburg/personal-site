<script lang="ts">
	type Paper = {
		title: string;
		authors: string;
		venue: string;
		arxiv: string;
	};

	const darkMatter: Paper[] = [
		{
			title:
				'The velocity-dependent J-factor of the Milky Way halo: does what happens in the galactic bulge stay in the galactic bulge?',
			authors: 'K. Kiriu, J. Kumar, J. Runburg',
			venue: 'JCAP 11 (2022) 030',
			arxiv: '2208.14002'
		},
		{
			title:
				'Searching for velocity-dependent dark matter annihilation signals from extragalactic halos',
			authors: 'E. J. Baxter, J. Kumar, A. D. Paul, J. Runburg',
			venue: 'JCAP 09 (2022) 026',
			arxiv: '2205.02386'
		},
		{
			title: 'Sensitivity to dark sector scales from gravitational wave signatures',
			authors: 'J. B. Dent, B. Dutta, S. Ghosh, J. Kumar, J. Runburg',
			venue: 'JHEP 08 (2022) 300',
			arxiv: '2203.11736'
		},
		{
			title: 'J-factors for velocity-dependent dark matter',
			authors: 'B. Boucher, J. Kumar, V. B. Le, J. Runburg',
			venue: 'Phys. Rev. D 106, 023025 (2022)',
			arxiv: '2110.09653'
		},
		{
			title: 'Constraining dark matter microphysics with the annihilation signal from subhalos',
			authors: 'J. Runburg, E. J. Baxter, J. Kumar',
			venue: 'JCAP 06 (2022) 023',
			arxiv: '2106.10399'
		},
		{
			title:
				'Prospects for measuring dark matter microphysics with observations of dwarf spheroidal galaxies',
			authors: 'E. J. Baxter, J. Kumar, A. B. Pace, J. Runburg',
			venue: 'JCAP 07 (2021) 030',
			arxiv: '2103.11646'
		},
		{
			title:
				'Effective J-factors for Milky Way dwarf spheroidal galaxies with velocity-dependent annihilation',
			authors: 'K. K. Boddy, J. Kumar, A. B. Pace, J. Runburg, L. E. Strigari',
			venue: 'Phys. Rev. D 102, 023029 (2020)',
			arxiv: '1909.13197'
		},
		{
			title:
				'Angular distribution of gamma-ray emission from velocity-dependent dark matter annihilation in subhalos',
			authors: 'K. K. Boddy, J. Kumar, J. Runburg, L. E. Strigari',
			venue: 'Phys. Rev. D 100, 063019 (2019)',
			arxiv: '1905.03431'
		}
	];

	const galaxies: Paper[] = [
		{
			title:
				'Stellar and black hole assembly in z < 0.3 infrared-luminous mergers: intermittent starbursts vs. super-Eddington accretion',
			authors: 'D. Farrah, … J. Runburg, et al.',
			venue: 'MNRAS 514, 3061 (2022)',
			arxiv: '2205.00037'
		},
		{
			title:
				'Consistent analysis of the AGN luminosity function in X-ray and mid-infrared in the XMM-LSS field',
			authors: 'J. Runburg, D. Farrah, A. Sajina, et al.',
			venue: 'ApJ 927, 214 (2022)',
			arxiv: '2112.00082'
		}
	];
</script>

<section>
	<h2>Research</h2>
	<p>
		PhD in physics (University of Hawaiʻi at Mānoa, 2022) in computational and theoretical dark
		matter phenomenology. My work centered on velocity-dependent dark matter annihilation — deriving
		effective J-factors for dwarf spheroidal galaxies, the Milky Way halo, and subhalo populations,
		and forecasting how well next-generation gamma-ray observations could pin down the underlying
		particle physics. Analyses ran end to end: simulating and querying large astronomy datasets over
		SQL, batching pipelines across an HPC cluster with SLURM, and turning the results into figures
		built to communicate. A second thread covered AGN luminosity functions and infrared-luminous
		galaxy mergers.
	</p>
	<p>
		Full list on
		<a
			href="https://arxiv.org/search/?searchtype=author&amp;query=Jack+Runburg"
			target="_blank"
			rel="noopener">arXiv</a
		>.
	</p>

	<h3 class="group-heading">Dark matter phenomenology</h3>
	<ul class="papers">
		{#each darkMatter as paper (paper.arxiv)}
			<li>
				<a class="title" href="https://arxiv.org/abs/{paper.arxiv}" target="_blank" rel="noopener">
					{paper.title}
				</a>
				<span class="meta">{paper.authors}</span>
				<span class="meta">{paper.venue} · arXiv:{paper.arxiv}</span>
			</li>
		{/each}
	</ul>

	<h3 class="group-heading">Galaxies &amp; active galactic nuclei</h3>
	<ul class="papers">
		{#each galaxies as paper (paper.arxiv)}
			<li>
				<a class="title" href="https://arxiv.org/abs/{paper.arxiv}" target="_blank" rel="noopener">
					{paper.title}
				</a>
				<span class="meta">{paper.authors}</span>
				<span class="meta">{paper.venue} · arXiv:{paper.arxiv}</span>
			</li>
		{/each}
	</ul>
</section>

<style>
	.group-heading {
		margin-top: 2rem;
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		color: var(--color-tide);
	}

	.papers {
		list-style: none;
		padding: 0;
		margin: 1rem 0 0 0;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.papers li {
		display: flex;
		flex-direction: column;
		gap: 0.2rem;
		border-left: 2px solid var(--color-border);
		padding-left: 1rem;
	}

	.title {
		color: var(--color-text);
		font-weight: 500;
		text-decoration: none;
	}

	.title:hover {
		color: var(--color-tide);
	}

	.meta {
		color: var(--color-text-muted);
		font-size: 0.85rem;
	}
</style>
