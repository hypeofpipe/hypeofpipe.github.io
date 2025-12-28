<script>
	const experiments = [
		{
			title: 'Nano-LLM',
			desc: 'Trained from scratch on limited data.',
			tech: ['PyTorch', 'Transformers', 'CUDA'],
			link: '#',
			color: 'from-pink-500 to-rose-500'
		},
		{
			title: 'RAG Apps',
			desc: 'OpenAI tutorials + OSS models.',
			tech: ['LangChain', 'OpenAI', 'Pinecone'],
			link: '#',
			color: 'from-blue-500 to-indigo-500'
		},
		{
			title: 'Local AI',
			desc: 'Ollama, SD, voice cloning experiments.',
			tech: ['Llama 2', 'Whisper', 'Stable Diffusion'],
			link: '#',
			color: 'from-emerald-500 to-teal-500'
		}
	];
</script>

<section id="experiments" class="py-24 sm:py-32 bg-slate-900/40 relative overflow-hidden">
	<div
		class="absolute inset-0 bg-[url('/grid.svg')] bg-center [mask-image:linear-gradient(180deg,white,rgba(255,255,255,0))]"
	></div>

	<div class="mx-auto max-w-7xl px-6 lg:px-8 relative">
		<div class="mx-auto max-w-2xl text-center">
			<h2 class="text-3xl font-bold tracking-tight text-white sm:text-4xl">AI/LLM Experiments</h2>
			<p class="mt-2 text-lg leading-8 text-slate-400">The Builder Side</p>
		</div>

		<div
			class="mx-auto mt-16 grid max-w-2xl grid-cols-1 gap-6 sm:grid-cols-2 lg:mx-0 lg:max-w-none lg:grid-cols-3"
		>
			{#each experiments as exp}
				<div
					class="relative overflow-hidden rounded-2xl bg-slate-800 px-6 pb-20 pt-10 shadow-xl ring-1 ring-inset ring-white/10 hover:ring-indigo-500/50 transition-all hover:scale-[1.02] hover:-translate-y-1"
				>
					<div
						class={`absolute inset-0 bg-gradient-to-br ${exp.color} opacity-0 group-hover:opacity-10 transition-opacity`}
					></div>
					<div class="relative">
						<h3
							class="mt-2 text-lg font-bold text-white group-hover:text-indigo-300 transition-colors"
						>
							{exp.title}
						</h3>
						<p class="mt-4 text-sm leading-6 text-slate-300">{exp.desc}</p>
						<div class="mt-6 flex flex-wrap gap-2">
							{#each exp.tech as t}
								<span
									class="inline-flex items-center rounded-md bg-slate-700/50 px-2 py-1 text-xs font-medium text-slate-300 ring-1 ring-inset ring-white/10"
									>{t}</span
								>
							{/each}
						</div>
						<a href={exp.link} class="absolute inset-0">
							<span class="sr-only">View {exp.title}</span>
						</a>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>
