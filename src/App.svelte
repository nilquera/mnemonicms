<script>
	import { select_options } from "svelte/internal";
	import {Spinner, Options} from "./components"
	const CORS_URL = "https://pacific-castle-70205.herokuapp.com";
	const BASE_URL = "https://pinfruit.com/mnemonics";
	let value = null; // value shown in the input field
	$: valueSearched = value; // value actually fetched
	let options = [];
	let selected = [];
	let loading = false;
	let encoded = false;
	
	const handleInput = (evt) => {
		value = evt.target.value;
		selected = []
		options = []
	};

	$: if (valueSearched !== null) {
		loading = true
		fetch(`${CORS_URL}/${BASE_URL}/${valueSearched}`, { referrerPolicy: "origin" })
			.then((res) => res.json())
			.then((json) => {
				options = json;
				loading = false;
			});
	}

	const handleSelectOption = ([word, numbers, extra]) => {
		if (!valueSearched) return
		selected = [...selected, {word, numbers}];
		const numOfDigits = numbers.toString().length
		const trimmedString = valueSearched.toString().slice(numOfDigits)
		const trimmedNumber = trimmedString === "" ? null : Number(trimmedString)

		valueSearched = trimmedNumber;

		if (!trimmedNumber) encoded = true;
	};

	const handleRemoveSelection = () => {
		const [selectionRemoved] = selected.slice(-1)
		const valueSearchedBefore = valueSearched
		let newValueSearched = selectionRemoved.numbers.toString()

		if (valueSearchedBefore !== null) newValueSearched += valueSearchedBefore.toString()

		valueSearched = Number(newValueSearched);
		
		selected.pop()
		selected = selected;	
		
		
	}
</script>

<main>
	
	<h1>Mnemonic Major System!</h1>
	<input {value} placeholder="Write a number..." type="number" on:input={handleInput} />
	<div class="selection">
		{#each selected as {word, numbers}}
			<span>{word}</span>
		{/each}
		{#if selected.length > 0}
			<span class="arrow" on:click={handleRemoveSelection}>&#128281;</span>
		{/if}
	</div>
	{#if loading}
	<Spinner/>
	{:else if valueSearched}
	<Options {options} {handleSelectOption}/>
	<!-- <div class="options">
		{#each options as option}
		<span on:click={handleSelectOption(option)}>{option[0]}<sup>{option[1].toString().length}</sup></span>
		{/each}
	</div> -->
	{:else if encoded}
		<p>the number is fully encoded, use the blue arrow to backtrack
		</p>
	{/if}
</main>

<style>
	.selection {
		height: 50px;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		font-size: 30px;
	}

	.selection .arrow {
		cursor: pointer;
		font-size: 30px;
	}

	.selection span {
		display: inline-block;
		margin: 0px 5px;
	}

	.options {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
	.options span {
		margin: 0px 5px;
		cursor: pointer;
	}

	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
