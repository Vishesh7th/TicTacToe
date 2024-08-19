<template>
    <div class="tic-tac-toe">
        <div class="board">
            <q-btn v-for="(cell, index) in board" :key="index" class="cell" :disable="!!cell || !!winner"
                @click="makeMove(index)" color="black" unelevated>
                {{ cell }}
            </q-btn>
        </div>
        <div class="status">{{ statusMessage }}</div>
        <q-btn @click="resetGame" color="secondary">Reset</q-btn>
    </div>
</template>
<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
export default defineComponent({
    name: 'TicTacToe',
    setup() {
        const board = ref<Array<string | null>>(Array(9).fill(null));
        const currentPlayer = ref<'X' | 'O'>('X');
        const winner = ref<string | null>(null);
        const winningCombinations = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6],
        ]
        const checkWinner = () => {
            for (const combination of winningCombinations) {
                const [a, b, c] = combination;
                if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
                    winner.value = board.value[a];
                    return;
                }
            }
            if (!board.value.includes(null)) {
                winner.value = 'Draw';
            }
        }
        const makeMove = (index: number) => {
            if (!board.value[index] && !winner.value) {
                board.value[index] = currentPlayer.value;
                checkWinner();
                if (!winner.value) {
                    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
                }
            }
        }
        const resetGame = () => {
            board.value = Array(9).fill(null);
            currentPlayer.value = 'X';
            winner.value = null;
        }
        const statusMessage = computed(() => { if (winner.value) { return winner.value === 'Draw' ? "It's a draw!" : `Player ${winner.value} wins!`; } else { return `Player ${currentPlayer.value}'s turn`; } })
        return {
            board,
            currentPlayer,
            winner,
            makeMove,
            resetGame,
            statusMessage
        }
    }
})
</script>
<style scoped>
.tic-tac-toe {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.board {
    display: grid;
    grid-template-columns: repeat(3, 100px);
    grid-template-rows: repeat(3, 100px);
    gap: 10px;
    margin-bottom: 20px;
}

.cell {
    width: 100px;
    height: 100px;
    font-size: 2em;
    text-align: center;
}

.status {
    margin-bottom: 20px;
    font-size: 1.2em;
}
</style>