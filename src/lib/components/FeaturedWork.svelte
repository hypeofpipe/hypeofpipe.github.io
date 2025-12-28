<script>
	const works = [
		{
			title: 'Backstage.io @ Novo Nordisk',
			desc: 'Building the most adopted plugin for our internal developer platform.',
			tags: ['React', 'Node.js', 'AI Workflows', 'K8s', 'CI/CD'],
			link: '#', // Placeholder or specific link if available
			cta: 'Read more',
			isPublic: false
		},
		{
			title: 'ADN Anime Platform Migration',
			desc: '300k daily users • 40% faster. Migrated legacy PHP to Node.js microservices.',
			tags: ['Node.js', 'AWS', 'TDD', 'TypeScript'],
			link: '#',
			cta: 'Read case study',
			isPublic: true
		}
	];
</script>

<section id="work" class="py-24 sm:py-32">
	<div class="mx-auto max-w-7xl px-6 lg:px-8">
		<div class="mx-auto max-w-2xl text-center">
			<h2 class="text-3xl font-bold tracking-tight text-white sm:text-4xl">Featured Work</h2>
			<p class="mt-2 text-lg leading-8 text-slate-400">The Specialist Side</p>
		</div>

		<div
			class="mx-auto mt-16 grid max-w-2xl grid-cols-1 gap-x-8 gap-y-20 lg:mx-0 lg:max-w-none lg:grid-cols-2"
		>
			{#each works as work}
				<article
					class="flex flex-col items-start justify-between bg-slate-900/50 p-8 rounded-2xl border border-white/5 hover:border-indigo-500/30 transition-all hover:shadow-2xl hover:shadow-indigo-500/10 group"
				>
					<div class="flex items-center gap-x-4 text-xs">
						{#each work.tags as tag}
							<span
								class="relative z-10 rounded-full bg-slate-800 px-3 py-1.5 font-medium text-slate-300 hover:bg-slate-700"
								>{tag}</span
							>
						{/each}
					</div>
					<div class="group relative mt-6">
						<h3
							class="text-2xl font-semibold leading-6 text-white group-hover:text-indigo-400 transition-colors"
						>
							<a href={work.link}>
								<span class="absolute inset-0"></span>
								{work.title}
							</a>
						</h3>
						<p class="mt-5 line-clamp-3 text-sm leading-6 text-slate-400">{work.desc}</p>
					</div>
					<div class="relative mt-8 flex items-center gap-x-4">
						<a
							href={work.link}
							class="text-sm font-semibold leading-6 text-indigo-400 hover:text-indigo-300"
						>
							{work.cta} <span aria-hidden="true">→</span>
						</a>
						{#if !work.isPublic}
							<span class="text-xs text-slate-500 italic">(Internal/Restricted)</span>
						{/if}
					</div>
				</article>
			{/each}
		</div>
	</div>
</section>
