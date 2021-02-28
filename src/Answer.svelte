<script>
    export let selectedClue;
    let {question, answer, value} = selectedClue
</script>

<div class="Answer">
    <p on:click>Click to close. {question} {answer} {value}</p>
</div>

<style>
    .Answer {
        background-color: lightcoral;
        height: 100%;
        width: 100%;
        position: absolute;
        top: 0; 
        left: 0;
    }
</style>