<script>
	import { onMount } from 'svelte'

	const baseURL = 'https://jservice.io/api/'

	$: categories = []

	const calcCategories = (quantity) => {
		let results = []
		for (let i = 0; i < quantity; i++) {
			results.push(Math.floor(Math.random() * Math.floor(17000)) + 1)
		}
		return results
	}

	const getData = (ids) => {
		console.log(ids)
		for (let id of ids) {
			console.log(id)
			fetch(baseURL + `category?id=${id}`)
			.then(response => response.json())
			.then(data => {
				categories = [...categories, {
					title: data.title,
					id: data.id,
					questions: data.clues.map(clue => ({
							id: clue.id,
							value: clue.value,
							question: clue.question,
							answer: clue.answer
						}
					))
				}]
			})
		}

		console.log(categories)
		
	}

	onMount(async () => getData(calcCategories(5)))

</script>

<div class="App">
	<nav>Score: 0. High score?</nav>
	<div class="tileArea">
		{#if categories.length === 5}
			{#each categories as category, i}
				<div class="tile">
					{category.title}
				</div>
			{/each}
		{/if}
	</div>
</div>

<style>
	.App {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		.App {
			max-width: none;
		} 
	}
</style>