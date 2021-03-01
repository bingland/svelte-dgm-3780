<script>
	import { onMount } from 'svelte'
    import Answer from './Answer.svelte'

    const baseURL = 'https://jservice.io/api/'

    let categories = []
    let gameReady = false
    let answering = false
    let selectedClue = null;

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
                            answer: clue.answer,
                            category: data.title,
                            selectable: true
                        }
                    ))
                }]
            })
        }
    }

	onMount(async () => getData(calcCategories(5)))

    $: if (categories.length === 5 && !gameReady) {
        console.log('======= RETRYING =======')
        let clues = []
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

        categories = clues
        console.log(categories)

        if (recalc) {
            getData(indexes)
        }
        else {
            gameReady = true
        }
    }
    
    const disableTile = () => {
        answering = false
    }

    const selectTile = (clue) => {
        if (clue.selectable) {
            answering = true
            selectedClue = clue
            clue.selectable = false
            categories = categories
        } 
    }
    
</script>

{#if gameReady}
    <div class="Board"> 
        {#each categories as category, j}
            <div class="col">
                <div class="category tile" >{category.title}</div>
                {#each Array(5) as _, k}
                    <div class="tile {categories[j].clues[k].selectable ? 'enabled' : 'disabled'}" on:click={() => selectTile(categories[j].clues[k])} >{(k + 1) * 200}</div>
                {/each}
            </div>
        {/each}
    </div>
    {#if answering}
        <Answer on:click={disableTile} selectedClue={selectedClue} />
    {/if}
{/if}


<style>
    .Board {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        text-align: center;
        grid-gap: 7px;
    }
    .col { 
        display: grid;
        grid-template-columns: 1fr;
        grid-gap: 7px;
    }
    .tile {
        height: 100px;
        background-color: lightblue;
        border: 1px solid grey;
        display: grid;
        align-items: center;
        border-radius: 10px;
        font-size: 60px;
    }
    .category {
        font-size: 25px;
        text-transform: capitalize;
        background-color: lightsteelblue;
    }
    .enabled {
        cursor: pointer;
    }
    .enabled:hover {
        background-color: white;
    }
    .disabled {
        background-color: white;
        cursor: default;
    }
    
</style>