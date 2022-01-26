<template>
    <div class="gameBoard" ref="gameBoard">
        <div class="controls">
            <div class="moves">
                <span>Moves:</span>
                <span>{{ moves }}</span>
            </div>
            <div class="matches">
                <span>Matches:</span>
                <span>{{ matches }}</span>
            </div>
            <button class="reset" type="button" @click.prevent="resetGame">
                <span>New Game</span>
            </button>
        </div>
        <div class="container">
            <Card
                v-for="({ val, hidden, selected }, index) in cards"
                :key="index"
                :value="val"
                :hidden="hidden"
                :selected="selected"
                :index="index"
                @select-card="selectCard"
            />
        </div>
    </div>
</template>

<script>
import { ref } from 'vue'
import Card from './Card.vue'

const shuffle = (array) => {
    let currentIndex = array.length, randomIndex;

    // While there remain elements to shuffle...
    while (currentIndex != 0) {

        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [array[currentIndex], array[randomIndex]] = [
            array[randomIndex], array[currentIndex]];
    }

    return array;
}

const start = () => {
    const val = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
    const random = shuffle([...val, ...val])
    const final = random.map((val) => {
        return {
            val,
            hidden: false,
            selected: false
        }
    })

    return final
}

export default {
    components: {
        Card
    },
    setup() {
        const cards = ref([...start()])
        const moves = ref(0)
        const matches = ref(0)
        const gameBoard = ref(null)

        let selectedCards = []

        const selectCard = (index) => {
            cards.value[index].selected = true
            selectedCards = [...selectedCards, index]

            if (selectedCards.length === 2) {
                gameBoard.value.style.pointerEvents = 'none'
                setTimeout(() => {
                    moves.value++
                    if (cards.value[selectedCards[0]].val === cards.value[selectedCards[1]].val) {
                        matches.value++
                        cards.value[selectedCards[0]].hidden = true
                        cards.value[selectedCards[1]].hidden = true
                    } else {
                        cards.value[selectedCards[0]].selected = false
                        cards.value[selectedCards[1]].selected = false
                    }
                    selectedCards = []
                    gameBoard.value.style.pointerEvents = 'all'
                }, 1000)
            }
        }

        const resetGame = () => {
            cards.value = [...start()]
            moves.value = 0
            matches.value = 0
        }

        return {
            cards,
            selectCard,
            moves,
            matches,
            resetGame,
            gameBoard
        }
    }
}
</script>

<style lang="scss" scoped>
.gameBoard {
    min-height: 100vh;
    background: #000;
    display: grid;
    place-content: center;
    padding-bottom: 2rem;
}

.controls {
    margin: 2rem auto;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    max-width: 1000px;
    width: 80%;

    .moves,
    .matches {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 1rem 2rem;
        border-radius: 50px;
        background: linear-gradient(90deg, #7b5bc7 0%, #ea388d 100%);
        color: #f8fafc;
        font-family: "Montserrat", sans-serif;
        font-weight: 700;
        font-size: 14px;
        text-transform: uppercase;

        span:first-child {
            display: inline-block;
            margin-right: 8px;
        }
    }
    .reset {
        appearance: none;
        -webkit-appearance: none;
        cursor: pointer;
        border: none;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 1rem 2rem;
        border-radius: 50px;
        background: linear-gradient(90deg, #7b5bc7 0%, #ea388d 100%);
        color: #f8fafc;
        font-family: "Montserrat", sans-serif;
        font-weight: 700;
        font-size: 14px;
        text-transform: uppercase;
    }
}

.container {
    max-width: 1000px;
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    gap: 2rem;
    perspective: 1000px;
}
</style>