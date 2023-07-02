<script>
	import Sidebar from '$lib/Components/Sidebar.svelte';
	import '../theme.postcss';
	import '@skeletonlabs/skeleton/themes/theme-skeleton.css';
	import '@skeletonlabs/skeleton/styles/skeleton.css';

	import '../app.postcss';
</script>

<div class=" w-full">
	<nav class="w-full h-20 border-slate-500 border shadow-lg flex items-center">
		<h1 class="text-xl font-semibold text-slate-700 px-8 underline-offset-8 underline">
			Nerve Sparks
		</h1>
	</nav>
	<div class="flex">
		<div class="w-64 border-b-0 border border-t-0 border-slate-500 py-8 px-4">
			<Sidebar />
		</div>
		<div class="w-3/4">
			<slot />
		</div>
	</div>
</div>
