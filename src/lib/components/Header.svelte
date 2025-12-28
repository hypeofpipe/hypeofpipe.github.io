<script>
	let isMenuOpen = false;

	const navItems = [
		{ label: 'Work', href: '/#work' },
		{ label: 'Experiments', href: '/#experiments' },
		{ label: 'Skills', href: '/#skills' },
		{ label: 'Contact', href: '/#contact' }
	];
</script>

<header
	class="fixed top-0 left-0 right-0 z-50 border-b border-indigo-500/10 bg-slate-950/80 backdrop-blur-md"
>
	<div class="mx-auto flex h-16 max-w-7xl items-center justify-between px-6 lg:px-8">
		<a
			href="/"
			class="text-xl font-bold tracking-tight text-white hover:text-indigo-400 transition-colors"
		>
			VV<span class="text-indigo-500">.</span>
		</a>

		<!-- Desktop Nav -->
		<nav class="hidden md:flex gap-8">
			{#each navItems as item}
				<a
					href={item.href}
					class="text-sm font-medium text-slate-400 hover:text-white transition-colors"
				>
					{item.label}
				</a>
			{/each}
			<a
				href="/resume.pdf"
				class="rounded-full bg-indigo-600 px-4 py-1.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 transition-all"
				download
			>
				Resume
			</a>
		</nav>

		<!-- Mobile Menu Button -->
		<button
			class="md:hidden text-slate-400 hover:text-white"
			on:click={() => (isMenuOpen = !isMenuOpen)}
		>
			<span class="sr-only">Open menu</span>
			<svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"
				/>
			</svg>
		</button>
	</div>
</header>
