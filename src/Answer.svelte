<script>
    export let selectedClue;
    let {question, answer, value} = selectedClue

    let result = null
    let showAnswer = false
    let userAnswer = ''

    const submitAnswer = () => {
        showAnswer = true
        result = userAnswer == answer
    }
</script>

<div class="Answer">
    <div class="textWrapper">
        <h1 class="questionText">{question}</h1>
        {#if showAnswer}
            <h2 class="answerText">{result === true ? 'Correct! ' : 'Incorrect! '}Answer was {answer}</h2>
        {/if}
        {#if !showAnswer}
            <div class="answerArea">
                <input bind:value={userAnswer} type="text" placeholder="Type answer here...">
                <button on:click={submitAnswer}>Submit</button>
                <button on:click={submitAnswer}>I don't know</button>
            </div>
        {/if}
        {#if showAnswer}
        <div class="answerArea">
            <button on:click>Return to Board</button>
        </div>
        {/if}
    </div>
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
    .textWrapper {
        max-width: 1200px;
        height: 100%;
        margin: 0 auto;
        text-align: center;
        display: grid;
        align-content: center;
    }
    .questionText {
        font-size: 60px;
        padding-bottom: 30px;
    }
    .answerText {
        font-size: 30px;
    }

    .answerArea {
        display: flex;
        justify-content: center;
        font-size: 300px;
    }
    input, button {
        font-size: 30px;
        padding: 10px;
    }

    
</style>