<script>
	import Board from './Board.svelte'
</script>

<div class="App">
	<nav>Score: 0. High score?</nav>
	<Board />
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