<script>
	import { spring } from 'svelte/motion';

	const allLetters = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z'];
	let index = 0;
	let letters = allLetters.sort(() => Math.random() - 0.5);
	/**
	 * @type {any[]}
	*/
	let alreadySeen = [];
	let alreadySeenString = '';
	let disableButtons = false;
	let firstRun = true;
	let currentLetter = '';
	let alreadyUsed = false;

	export
	/**
	 * @type {any}
	*/ let shouldRepeat;
	 export
	/**
	 * @type {any}
	*/ let startTimer;

	const displayed_index = spring();
	$: displayed_index.set(index);
	$: offset = modulo($displayed_index, 1);

	/**
	 * @param {number} n
	 * @param {number} m
	 */
	function modulo(n, m) {
		return ((n % m) + m) % m;
	}

	function initializeSelector() {
		if (!shouldRepeat) {
			letters = letters.filter(letter => letter !== currentLetter)
		}
		firstRun = false;
		alreadyUsed = false;
		index = Math.floor(Math.random()*letters.length);
		disableButtons = true;
		setTimeout(() => {
			disableButtons = false
			currentLetter = letters[index]
		}, 500)
	}

	function useLetter() {
		alreadySeen.push(currentLetter)
		alreadySeenString = alreadySeen.join(' ')
		alreadyUsed = true;
		startTimer();
	}
</script>

<div class="wrapper">
	<div class="counter">
		<div class="counter-viewport">
			<div class="counter-digits" style="transform: translate(0, {100 * offset}%)">
				{#each letters as letter}
					<strong class="hidden" aria-hidden="true">{letter}</strong>
				{/each}
				<strong>{firstRun ? '?' : letters[index]}</strong>
			</div>
		</div>
	</div>
	<div class='actions'>
		<button disabled={disableButtons} on:click={initializeSelector}>
			Nueva Letra
		</button>
		<button disabled={disableButtons || firstRun || alreadyUsed} on:click={useLetter}>
			Usar
		</button>
	</div>
	<div class='letters'>
		<div class='letters-title'>Letras que salieron:</div>
		<div class='letters-content'>{alreadySeenString}</div>
	</div>
</div>

<style>
	.wrapper {
		align-items: center;
		display: flex;
		flex-direction: column;
		justify-content: center;
		width: 256px;
	}
	.counter {
		align-items: center;
		border-bottom: 1px solid rgba(0, 0, 0, 0.1);
		border-top: 1px solid rgba(0, 0, 0, 0.1);
		display: flex;
		justify-content: center;
		margin: 1rem 0;
		width: 100%;
	}
	.actions {
		display: flex;
		justify-content: space-between;
		width: 100%;
	}
	.actions button {
		height: 32px;
		width: 48%;
		border: none;
		cursor: pointer;
		border-radius: 20px;
	}
	.counter-viewport {
		width: 8em;
		height: 4em;
		overflow: hidden;
		text-align: center;
		position: relative;
	}

	.counter-viewport strong {
		position: absolute;
		display: flex;
		width: 100%;
		height: 100%;
		font-weight: 400;
		color: var(--accent-color);
		font-size: 4rem;
		align-items: center;
		justify-content: center;
	}
	.counter-digits {
		position: absolute;
		width: 100%;
		height: 100%;
	}
	.hidden {
		top: -100%;
		user-select: none;
	}
	.letters {
		margin-top: 32px;
		width: 100%;
	}
	.letters-title {
		width: 100%;
	}
	.letters-content {
		font-size: 14px;
		font-weight: 600;
		margin-top: 8px;
	}
</style>
