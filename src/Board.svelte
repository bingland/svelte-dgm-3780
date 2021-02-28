<script>
	import { onMount } from 'svelte'

    const baseURL = 'https://jservice.io/api/'

    let categories = []
    let clues = []
    let gameReady = false

    const calcCategories = (quantity) => {
        let results = []
        for (let i = 0; i < quantity; i++) {
            results.push(Math.floor(Math.random() * 17000) + 1)
        }
        return results
    }

    const getData = (ids) => {
        console.log(ids)

        categories = []

        for (let id of ids) {
            fetch(baseURL + `category?id=${id}`)
            .then(response => response.json())
            .then(data => {
                categories = [...categories, {
                    title: data.title,
                    id: data.id,
                    clues: data.clues.map(clue => ({
                            id: clue.id,
                            value: clue.value,
                            question: clue.question,
                            answer: clue.answer
                        }
                    ))
                }]
            })
        }
    }

	onMount(async () => getData(calcCategories(5)))

    $: if (categories.length === 5) {
        console.log('======= RETRYING =======')
        clues = []
        let indexes = categories.map(category => category.id)
        console.log(indexes)
        let recalc = false
        // narrow down to 5 clues with values from 200 to 1000
        for (let category of categories) {
            let catResults = []
            for (let i = 200; i <= 1000; i += 200) {
                let candidates = []
                candidates = category.clues.filter(question => question.value === i)
                if (candidates.length === 1) {
                    catResults = [...catResults, candidates[0]]
                }
                else if (candidates.length > 1) {
                    catResults = [...catResults, candidates[Math.floor(Math.random() * candidates.length)]]
                }
                else {
                    // No clues match the value, get new category
                    console.log(`No candidates found for ${i} in ${category.title}`)
                    console.log('Replacing index ' + indexes.indexOf(category.id))
                    indexes.splice(indexes.indexOf(category.id), 1, calcCategories(1)[0])
                    recalc = true
                    break;
                }
            }

            clues = [...clues, {title: category.title, clues: catResults}]

        }  

        console.log(clues)

        if (recalc) {
            getData(indexes)
        }
        else {
            gameReady = true
        }
    }
    

    
</script>

{#if gameReady}
    <div class="Board"> 
        {#each categories as category, i}
            <div class="col">
                <div class="category tile">{category.title}</div>
                {#each Array(5) as _, i}
                    <div class="score tile">{(i + 1) * 200}</div>
                {/each}
            </div>
        {/each}
    </div>
{/if}


<style>
    .Board {
        border: 1px solid black;
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        text-align: center;
    }
    .col {
        
    }
    .tile {
        height: 100px;
        background-color: lightblue;
        border: 1px solid grey;
        display: grid;
        align-items: center;
    }
    .category {
        font-size: 30px;
        /* text-transform: uppercase; */
    }
    .score {
        font-size: 70px;
    }
    .score:hover {
        background-color: white;
        cursor: pointer;
    }
    
</style>