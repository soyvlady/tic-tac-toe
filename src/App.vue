<script setup lang="ts">
import { ref } from 'vue';
import Casilla from './components/Casilla.vue';

enum Player {
	X = '❌',
	O = '⭕'
}

const result = ref('');
const board = ref([
	["", "", ""],
	["", "", ""],
	["", "", ""]
]);

const restart = () => {
	board.value = [
		["", "", ""],
		["", "", ""],
		["", "", ""]
	];
	result.value = '';
};

const markCell = (row: number, col: number) => {
	if (board.value[row][col] === '' && result.value === '') {
		board.value[row][col] = board.value.flat().filter(cell => cell === '').length % 2 === 0 ? Player.X : Player.O;
	}
	if (checkWinner() !== '') {
		result.value = "Ganó el jugador " + checkWinner() + "! 🎉🎉🎉";
	}
	checkDraw();
};

const checkWinner = () => {
	// Check rows
	for (let i = 0; i < 3; i++) {
		if (board.value[i][0] === board.value[i][1] && board.value[i][1] === board.value[i][2] && board.value[i][0] !== '') {
			return board.value[i][0];
		}
	}
	// Check columns
	for (let i = 0; i < 3; i++) {
		if (board.value[0][i] === board.value[1][i] && board.value[1][i] === board.value[2][i] && board.value[0][i] !== '') {
			return board.value[0][i];
		}
	}
	// Check diagonals
	if (board.value[0][0] === board.value[1][1] && board.value[1][1] === board.value[2][2] && board.value[0][0] !== '') {
		return board.value[0][0];
	}
	if (board.value[0][2] === board.value[1][1] && board.value[1][1] === board.value[2][0] && board.value[0][2] !== '') {
		return board.value[0][2];
	}
	return '';
};

const checkDraw = () => {
	if (board.value.flat().filter(cell => cell === '').length === 0 && result.value === '') {
		result.value = "Es un empate! 🤝";
	}
};
</script>

<template>
	<div class="bg-gray-900 h-screen flex flex-col items-center">
		<h1 class="text-6xl my-16 font-extrabold text-rose-600">TIC TAC TOE</h1>
		<div class="grid grid-cols-3 gap-3">
			<div v-for="(row, i) in board" :key="i" class="grid gap-3">
				<Casilla
					v-for="(cell, j) in row"
					:key="j"
					:player="cell"
					@click="markCell(i, j)"
				/>
			</div>
		</div>
		<div>
			<button @click="restart" class="bg-rose-500 text-white px-5 py-2 mt-5 cursor-pointer">Reiniciar</button>
		</div>
		<div v-if="result === ''" class="mt-5">
			<h2 class="text-3xl font-bold text-rose-600">
				Turn del jugador:
				{{ board.flat().filter(cell => cell === '').length % 2 === 0 ? Player.X : Player.O }}
			</h2>
		</div>
		<div v-else class="mt-5">
			<h2 class="text-3xl font-bold text-rose-600 mt-5">{{ result }}</h2>
		</div>
	</div>
</template>

<style scoped>

</style>