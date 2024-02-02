<script lang="ts">
	import { Move } from './utils';
    import { fade, fly } from 'svelte/transition';

	export let move: Move;
</script>


<div in:fly={{x: 100, y:100}}>

    {#if move === Move.X}
    <svg
    xmlns="http://www.w3.org/2000/svg"
    fill="none"
    viewBox="0 0 24 24"
    stroke-width="2.5"
    stroke="currentColor"
    class="w-12 h-12"
    >
    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
</svg>
{:else}
<svg
xmlns="http://www.w3.org/2000/svg"
fill="none"
viewBox="0 0 24 24"
stroke-width="2.5"
stroke="currentColor"
class="w-12 h-12"
>
<path stroke-linecap="round" stroke-linejoin="round" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
</svg>

{/if}
</div>
