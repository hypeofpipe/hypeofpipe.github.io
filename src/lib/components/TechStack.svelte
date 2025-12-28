<script>
	const stack = [
		{
			category: 'AI/LLMs',
			skills: ['GPT-4o', 'Claude 4', 'Gemini 2.5', 'Prompt Engineering', 'RAG']
		},
		{ category: 'Backend', skills: ['Node.js', 'TypeScript', 'Nest.js', 'PostgreSQL', 'RabbitMQ'] },
		{ category: 'DevOps', skills: ['K8s (CKAD)', 'AWS', 'Docker', 'CI/CD'] },
		{ category: 'Frontend', skills: ['React', 'Redux', 'Svelte/SvelteKit', 'TailwindCSS'] }
	];
</script>

<section id="skills" class="py-24 sm:py-32">
	<div class="mx-auto max-w-7xl px-6 lg:px-8">
		<div class="mx-auto max-w-2xl text-center mb-16">
			<h2 class="text-3xl font-bold tracking-tight text-white sm:text-4xl">
				Tech Stack & Expertise
			</h2>
		</div>

		<div class="grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-4">
			{#each stack as category}
				<div class="border-l-2 border-indigo-500 pl-6 py-2">
					<h3 class="text-lg font-semibold text-white mb-4">{category.category}</h3>
					<ul class="space-y-2">
						{#each category.skills as skill}
							<li class="text-slate-400 text-sm hover:text-white transition-colors cursor-default">
								{skill}
							</li>
						{/each}
					</ul>
				</div>
			{/each}
		</div>
	</div>
</section>
