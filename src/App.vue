<script setup>
import { ref, computed } from "vue";
const player = ref("X");
const board = ref([
	["", "", ""],
	["", "", ""],
	["", "", ""],
]);
const CalculateWinner = (squares) => {
	const lines = [
		["0", "1", "2"],
		["3", "4", "5"],
		["6", "7", "8"],
		["0", "3", "6"],
		["1", "4", "7"],
		["2", "5", "8"],
		["0", "4", "8"],
		["2", "4", "6"],
	];
	for (let i = 0; i < lines.length; i++) {
		const [a, b, c] = lines[i];
		if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
			return squares[a];
	}
	return null;
};
const winner = computed(() => CalculateWinner(board.value.flat()));
const MakeMove = (x, y) => {
	if (winner.value) return;
	if (board.value[x][y] !== "") return;

	board.value[x][y] = player.value;

	player.value = player.value === "X" ? "O" : "X";
};
const ResetGame = () => {
	board.value = [
		["", "", ""],
		["", "", ""],
		["", "", ""],
	];
	player.value = "X";
};
</script>

<template>
	<main
		class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-neutral-200"
	>
		<h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>
		<h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

		<div class="flex flex-col items-center mb-8">
			<div v-for="(row, x) in board" :key="x" class="flex">
				<div
					v-for="(cell, y) in row"
					:key="y"
					@click="MakeMove(x, y)"
					:class="`select-none w-24 h-24 border border-sky-100 hover:bg-cyan-900/80 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer font-extrabold ${
						cell === 'X' ? 'text-fuchsia-600' : 'text-teal-600'
					}`"
				>
					<!-- {{ board[x][y] }} -->
					{{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
				</div>
			</div>
		</div>

		<h2 v-if="winner" class="text-6xl font-bold my-10">
			Player '{{ winner }}' wins!!
		</h2>

		<button
			@click="ResetGame"
			class="px-4 py-2 bg-fuchsia-700 rounded uppercase font-bold hover:bg-fuchsia-700/80 duration-300"
		>
			Reset Game
		</button>
	</main>
</template>
