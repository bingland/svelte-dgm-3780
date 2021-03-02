<script>
    export let cash, correctAnswers, totalAnswers

</script>

<div class="Scoreboard">
	<div class="">Points: ${cash}</div>
    <div class="">{correctAnswers}/{totalAnswers}</div>
</div>

<style>
    .Scoreboard {
        background-color: lightsteelblue;
        padding: 10px;
        border-radius: 0 0 5px 5px;
        margin-bottom: 10px;
        border: 1px solid grey;
        font-size: 20px;
        display: grid;
        grid-template-columns: 1fr 1fr;
        justify-items: center;
    }
</style>