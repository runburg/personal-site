<script lang="ts">
	const groups: { heading: string; tags: string[] }[] = [
		{
			heading: 'Languages',
			tags: ['Python', 'Fortran', 'C', 'C++', 'TypeScript', 'SQL', 'Bash', 'Rust (learning)']
		},
		{
			heading: 'Web & UI',
			tags: ['Svelte / SvelteKit', 'Flask', 'Multi-device real-time UIs', 'HTML / CSS']
		},
		{
			heading: 'Robotics & systems',
			tags: [
				'ROS2',
				'Behavior trees',
				'Hardware drivers & control logic',
				'Sensor fusion (GNSS, IMU)',
				'State estimation'
			]
		},
		{
			heading: 'Scientific computing',
			tags: [
				'Finite-difference PDE solvers',
				'Cable & marine dynamics modeling',
				'Numerical optimization & profiling',
				'Simulation validation & benchmarking'
			]
		},
		{
			heading: 'HPC & data',
			tags: [
				'SLURM batch pipelines',
				'Distributed HPC analyses',
				'Large astronomy datasets over SQL',
				'Data visualization (Matplotlib)'
			]
		},
		{
			heading: 'DevOps',
			tags: [
				'GitHub Enterprise Server admin',
				'GitHub Actions CI/CD',
				'Self-hosted runners',
				'CMake cross-platform builds',
				'SVN → Git migration',
				'Linux'
			]
		},
		{
			heading: 'Working with people',
			tags: [
				'Technical communication & outreach',
				'Documentation & user guides',
				'Project coordination',
				'Mentorship',
				'Team management'
			]
		}
	];
</script>

<section>
	<h2>Technical skills</h2>
	<p>
		Software developer and physicist — data analysis, web applications, DevOps, simulation, and
		high-performance scientific computing. Comfortable from hardware drivers up through the UI, and
		from a first literature review through to a shipped result.
	</p>
	<div class="groups">
		{#each groups as group (group.heading)}
			<div class="group">
				<h3>{group.heading}</h3>
				<div class="tags">
					{#each group.tags as tag (tag)}
						<span>{tag}</span>
					{/each}
				</div>
			</div>
		{/each}
	</div>
</section>

<style>
	.groups {
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
		margin-top: 1.5rem;
	}

	h3 {
		font-family: var(--font-body);
		font-size: 0.85rem;
		text-transform: none;
		color: var(--color-text-muted);
		font-weight: 500;
	}

	.tags {
		display: flex;
		flex-wrap: wrap;
		gap: 0.5rem;
	}

	.tags span {
		background: var(--color-panel);
		border: 1px solid var(--color-border);
		border-radius: 0.4rem;
		padding: 0.35rem 0.7rem;
		font-size: 0.85rem;
		color: var(--color-text);
	}
</style>
