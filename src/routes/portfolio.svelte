<script>
	import Hero from '$lib/components/Hero.svelte';
	import FeaturedWork from '$lib/components/FeaturedWork.svelte';
	import AIExperiments from '$lib/components/AIExperiments.svelte';
	import TechStack from '$lib/components/TechStack.svelte';
	import Contact from '$lib/components/Contact.svelte';
</script>

<svelte:head>
	<title>Volodymyr Voleniuk | Senior Full-Stack Engineer</title>
	<meta
		name="description"
		content="Portfolio of Volodymyr Voleniuk, Senior Full-Stack Engineer specializing in AI/LLM Integration and Platform Engineering."
	/>
</svelte:head>

<div class="bg-slate-950">
	<Hero />
	<FeaturedWork />
	<AIExperiments />
	<TechStack />
	<Contact />
</div>
