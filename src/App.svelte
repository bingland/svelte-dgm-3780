<script>
	import Board from './Board.svelte'
	import Scoreboard from './Scoreboard.svelte'

	// game logic
	let cash = 0
	let correctAnswers = 0
	let totalAnswers = 0

	const wrongAnswer = () => {
		console.log('WRONG ANSWER')
		totalAnswers += 1
		console.log(`${cash} ${correctAnswers} ${totalAnswers}`)
	}
	const correctAnswer = (value) => {
		console.log('CORRECT ANSWER')
		totalAnswers += 1
		correctAnswers += 1
		cash += value
		console.log(`${cash} ${correctAnswers} ${totalAnswers}`)

	}
</script>

<div class="App">
	<Scoreboard cash={cash} correctAnswers={correctAnswers} totalAnswers={totalAnswers} />
	<Board wrongAnswer={wrongAnswer} correctAnswer={correctAnswer} />
</div>

<style>
	.App {
		/* text-align: center; */
		max-width: 1200px;
		margin: 0 auto;
		font-family: 'Lato', sans-serif;
	}
</style>