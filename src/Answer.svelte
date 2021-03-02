<script>
    export let selectedClue
    export let wrongAnswer, correctAnswer

    let {question, answer, value, category} = selectedClue

    let result = null
    let showAnswer = false
    let userAnswer = ''

    const submitAnswer = () => {
        showAnswer = true
        result = false
        // check answer; check if difference between answer is just an article
        if (userAnswer.toLowerCase() == answer.toLowerCase() 
        || (answer.toLowerCase().includes('the ', 0) && answer.toLowerCase().substr(4) == answer.toLowerCase()) 
        || (answer.toLowerCase().includes('a ', 0) && answer.toLowerCase().substr(2) == answer.toLowerCase())
        || (answer.toLowerCase().includes('an ', 0) && answer.toLowerCase().substr(3) == answer.toLowerCase())
        || (answer.toLowerCase().includes('to ', 0) && answer.toLowerCase().substr(3) == answer.toLowerCase())) {
            result = true
            correctAnswer(value)
        } else {
            wrongAnswer()
        }
    }
</script>

<div class="Answer">
    <div class="textWrapper">
        <h2 class="categoryText">{category}</h2>
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
    @keyframes fadeIn {
        0% { opacity: 0; }
        100% { opacity: 1; }
    }

    .Answer {
        background-color: white;
        height: 100%;
        width: 100%;
        position: absolute;
        top: 0; 
        left: 0;
        animation: fadeIn ease 0.25s;
    }
    .textWrapper {
        max-width: 1200px;
        height: 100%;
        margin: 0 auto;
        text-align: center;
        display: grid;
        align-content: center;
    }
    .categoryText {
        font-size: 30px;
        text-transform: capitalize;
    }
    .questionText {
        font-size: 60px;
        padding: 40px 20px 50px 20px;
    }
    .answerText {
        font-size: 30px;
        padding-bottom: 30px;
    }

    .answerArea {
        display: flex;
        justify-content: center;
        font-size: 300px;
    }
    input, button {
        font-size: 25px;
        padding: 10px;
        border-radius: 5px;
        border: 1px solid rgb(85, 85, 85);
        margin-right: 5px;
    }
    input {
    }

    
</style>