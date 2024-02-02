<script lang="ts">
	import { Move, checkWinner, State } from '$lib/utils';
    import Icon from '$lib/Icon.svelte';
    import EmptyCell from '$lib/EmptyCell.svelte';
    import { tick } from 'svelte';
    import { goto } from '$app/navigation';
    import { redirect } from '@sveltejs/kit';

	let board = getEmptyBoard();

	let turn = Move.O;
	let state = State.Playing;
	let boardErrasdsa: HTMLElement, statusEl: HTMLElement;

	function place(row: number, col: number) {
		board[row][col] = turn;
		turn = turn === Move.O ? Move.X : Move.O;
		tick().then(focusNextAvailableTile);
	}

	function focusNextAvailableTile() {
		const nextTile = boardErrasdsa.querySelector('button:not(:disabled)');
		if (nextTile) {
			(nextTile as HTMLElement).focus();
		} else {
			statusEl.focus();
		}
	}

	$: winner = checkWinner(board);
	$: state = getGameState(winner, board);

	function reset() {
		board = getEmptyBoard();
		tick().then(focusNextAvailableTile);
	}

	function getEmptyBoard() {
		return [
			[Move.Empty, Move.Empty, Move.Empty],
			[Move.Empty, Move.Empty, Move.Empty],
			[Move.Empty, Move.Empty, Move.Empty]
		];
	}

	function getGameState(winner: Move | undefined, board: Move[][]) {
		if (winner) {
            
			return State.Won;
		} else if (board.every((row) => row.every((col) => col !== Move.Empty))) {
			return State.Draw;
		} else {
			return State.Playing;
		}
	}
</script>

<div class="flex flex-col min-h-screen w-full justify-center items-center relative">
    <div class="board" bind:this={boardErrasdsa}>
        {#each board as row, r}
            {#each row as col, c}
                <div class="bg-slate-900 border-4 border-slate-900 hover:border-blue-950 transition-colors h-[100px] flex flex-col justify-center items-center rounded-md" >
                    {#if col !== Move.Empty}
                        <Icon move={col}  />
                    {:else}
                        <EmptyCell on:click={() => place(r, c)} disabled={state !== State.Playing}>
                            <span class="hidden">{r + 1} {c + 1}</span>
                        </EmptyCell>
                    {/if}
                </div>
            {/each}
        {/each}
    </div>


    <div class="text-4xl font-bold absolute top-[270px] ">
        {#if state === State.Playing}
            It's <span class="text-slate-200">{turn}</span> turn
        {/if}
    </div>

    
        {#if state !== State.Playing}
        <div class="absolute bg-slate-950 px-5 py-3 rounded text-2xl">
            <div class="text-xl font-bold">
                {#if state === State.Won}
                <p class="text-3xl text-center text-slate-300">{winner} </p>
                <p class="font-bold text-center">Is a winner</p>
                {:else if state === State.Draw}
                <p class="text-3xl text-center text-slate-300">DRAW </p>
                {:else}
                    Turn: {turn}
                {/if}
            </div>
            <button on:click={reset} class="bg-slate-900 px-6 py-2 rounded-lg mt-3 uppercase border-2 border-slate-900 font-bold hover:border-blue-950 transition-colors">Play <span class="text-slate-300">again</span></button>
        </div>

        {/if}
</div>

<style>
	.board {
		display: grid;
		grid-template-columns: repeat(3, minmax(auto, 100px));
		grid-template-rows: repeat(3, auto);
		gap: 0.25rem;
		background: var(--gray-4);
	}
</style>