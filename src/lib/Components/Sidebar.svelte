<script>
	import { slide } from 'svelte/transition';
	import { clicked } from './utils';
	let celebArray = [
		'Lokesh Kanagaraj',
		'Johnny depp',
		'Christopher Nolan',
		'Rajini',
		'Leonardo Dicaprio'
	];
	let show = false;
	function update(e) {
		clicked.update((prev) => (prev = e.target.innerHTML));
		console.log(e.target.innerHTML);
	}
	function add(e) {
		if (e.key == 'Enter') {
			let celebName = e.target.value;
			celebArray = [...celebArray, celebName];
			e.target.value = '';
		}
	}
</script>

<div class="w-full">
	<h1 class="card text-center py-2 mb-4 font-semibold text-xl">Celebrities</h1>
	{#each celebArray as celeb}
		<button
			on:click={update}
			class="mx-auto font-semibold rounded-md text-sm block variant-filled-secondary my-1 btn w-full"
			>{celeb}</button
		>
	{/each}
	{#if show}
		<input
			transition:slide
			class="w-full py-1 rounded-md border focus:bg-blue-200 border-violet-500 px-4 text-xl input focus:outline-blue-700 focus:shadow-xl focus:shadow-blue-800"
			type="text"
			name=""
			id=""
			on:keypress={add}
		/>
	{/if}
	<button
		on:click={() => (show = !show)}
		class="btn variant-outline-primary rounded-md mx-auto block my-6 w-full text-green-500 font-medium"
		>+ Add yours</button
	>
</div>
