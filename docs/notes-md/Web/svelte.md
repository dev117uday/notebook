# Svelte Notes

```js

<script>
	let name = "Uday Yadav";
	import Nested from './Nested.svelte';
	let count = 0;
	let string = `this string contains some <strong>HTML!!!</strong>`;

	$: double = count*2;

	function increment() {
		count = count+1;
	}

	# reactive statements
	$: console.log(`the count is ${count}`);

</script>

<h3>Hello : {name.toUpperCase()}</h3>

<img src={src} />

<p>{@html string}</p>

<button on:click={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>

<Nested answer={42} />


export makes it prop
<script>
	export let answer;
</script>

<p>The answer is {answer}</p>

{#if count%2 == 0} 
	<p>Only on even</p>
{:else}
	<p>Only on odd</p>
{/if}

const colors = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

{#each colors as color}
	<button
		aria-current={selected === color}
		aria-label={color}
		style="background: {color}"
		on:click={() => selected = color}>
	</button>
{/each}
	
{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

<button on:click|once={() => alert('clicked')}>
	Click me
</button>

<input bind:value={name} />

<h1>Hello {name}!</h1>


store : 
import { writable } from 'svelte/store';

function createCount() {
	const { subscribe, set, update } = writable(0);

	return {
		subscribe,
		increment: () => update((n) => n + 1),
		decrement: () => update((n) => n - 1),
		reset: () => set(0)
	};
}
export const count = createCount();


<script>
	import { count } from './stores.js';
</script>

<h1>The count is {$count}</h1>

<button on:click={count.increment}>+</button>
<button on:click={count.decrement}>-</button>
<button on:click={count.reset}>reset</button>
```