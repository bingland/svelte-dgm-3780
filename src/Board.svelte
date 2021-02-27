<script>
	import { onMount } from 'svelte'

    const baseURL = 'https://jservice.io/api/'

    let categories = []

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

{#if categories.length === 5}
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
    }
    .col {
        
    }
    .tile {
        height: 100px;
        background-color: lightblue;
        border: 1px solid grey;
    }
    .category {
        font-size: 30px;
    }
    .score {
        font-size: 70px;
    }
    
</style>