<script>
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	import { slide } from 'svelte/transition';
	let movieData = '';
	let show = '';
	console.log(show);
	onMount(() => {
		async function details() {
			const res = await fetch(
				`https://api.themoviedb.org/3/search/movie?api_key=7ad8f69f01d2db4addc11c38673821da&query=${$page.params.slug}`
			);
			const data = await res.json();
			console.log(data);
			movieData = data.results.filter((res) => res.backdrop_path !== null);
		}
		details();
	});
</script>

<div class="grid grid-flow-col gap-8 px-16 py-8">
	{#each movieData as movie}
		<div class="w-80 bg-violet-300 p-2 pb-1 border border-stone-500 rounded-xl overflow-y-hidden">
			<img
				class="w-full rounded-lg"
				src="https://image.tmdb.org/t/p/w500/{movie.backdrop_path}"
				alt=""
			/>
			<div
				class="my-2 p-2 rounded-lg border bg-violet-200 hover:bg-violet-300 transition-all duration-500 space-y-2"
			>
				<h1 class="text-lg text-slate-500 flex justify-between">
					Movie Title: <span class="text-slate-700 font-medium">
						{movie.title}
					</span>
				</h1>
				<p class="text-lg text-slate-500 flex justify-between">
					Rating: <span class="text-yellow-500 bg-yellow-100 rounded-full px-1 font-medium"
						>{movie.vote_average}</span
					>
				</p>
				{#if show === movie.id}
					<div transition:slide class="space-y-2">
						<p class="text-lg text-slate-500 flex justify-between">
							Release date: <span class="text-slate-700 font-medium">{movie.release_date}</span>
						</p>
						<p class="text-lg text-slate-500 flex justify-between">
							Original Language: <span class="text-slate-700 font-medium"
								>{movie.original_language}</span
							>
						</p>
						<div class="text-lg text-slate-500 flex justify-between items-center">
							<div>
								<h1>Movie description:</h1>
								<img
									class="h-48 rounded-xl"
									src="https://image.tmdb.org/t/p/w500/{movie.poster_path}"
									alt=""
								/>
							</div>
							<span class="text-slate-700 w-1/2 text-justify font-medium text-sm"
								>{movie.overview}</span
							>
						</div>
					</div>
				{/if}
			</div>
			<div
				on:mousedown={() => (show = movie.id)}
				class="flex bg-violet-900 rounded-b-xl h-4 justify-center items-center relative w-full bottom-0 z-10"
			>
				<img
					src="/images/downArrow.svg"
					alt="downarrow"
					class="w-3 h-3 transition-all duration-500 {show ? 'rotate-180' : 'rotate-0'}"
				/>
			</div>
		</div>
	{/each}
</div>
