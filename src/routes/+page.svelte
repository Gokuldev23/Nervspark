<script>
	let movies;
	let filteredMovie;
	let name = '';
	import { clicked } from '$lib/Components/utils';
	async function handle() {
		const res = await fetch(
			`https://api.themoviedb.org/3/search/person?query=${$clicked}&api_key=7ad8f69f01d2db4addc11c38673821da`
		);
		const data = await res.json();
		const response = await fetch(
			`https://api.themoviedb.org/3/discover/movie?api_key=7ad8f69f01d2db4addc11c38673821da&with_cast=${
				$clicked ? data.results[0].id : ''
			}&sort_by=popularity.desc&page=1`
		);
		const datas = await response.json();

		movies = datas.results.filter((res) => res.backdrop_path !== null);
		filteredMovie = [...movies];
	}
	$: {
		$clicked;
		handle();
	}
	function filtered() {
		const search = name.toLowerCase();
		if (!search) {
			filteredMovie = [...movies];
		}
		const matchingMovies = movies.filter((movie) => movie.title.toLowerCase().includes(search));
		const remainingMovies = movies.filter((movie) => !movie.title.toLowerCase().includes(search));
		filteredMovie = [...matchingMovies, ...remainingMovies];
	}
</script>

<div>
	<input
		type="text"
		class="border w-1/2 ml-4 mt-6 input border-violet-700 focus:bg-blue-200 rounded-xl p-2 focus:outline-blue-700 focus:shadow-xl focus:shadow-blue-800"
		bind:value={name}
		on:input={filtered}
	/>
	<button class="btn variant-ghost-secondary font-medium text-violet-800" on:click={handle}
		>Search</button
	>
	<div class="grid grid-cols-4 grid-rows-4 gap-4 px-4 py-8 overflow-y-scroll">
		{#if movies}
			{#each filteredMovie as item}
				<a href="/movie/{item.title}">
					<div
						class="p-2 rounded-xl border border-[#ECECEC] shadow transition-all duration-300 hover:shadow-2xl overflow-hidden"
					>
						<img
							class="w-full rounded-lg hover:scale-[1.1] transition-all duration-300"
							src="https://image.tmdb.org/t/p/w500/{item.backdrop_path}"
							alt={item.title}
						/>
						<h1 class="text-base/5 font-medium text-stone-700 my-4">{item.title}</h1>
					</div>
				</a>
			{/each}
		{/if}
	</div>
</div>
