<script>
	let letter = '';
  let excludedLetters = [];

  // @ts-ignore
  function updateLetter(e) {
    if (e && e.data) letter = e.data
	}

  // @ts-ignore
  function handleEnter(e) {
    if (e && e.key && e.key === 'Enter') {
      excludedLetters.push(letter);
      letter = '';
    }
  }
</script>

<div class="wrapper">
	<input on:keypress={e => handleEnter(e)} on:input={(e) => updateLetter(e)} value={letter} type='text' class='input' />
</div>

<style>
	.wrapper {
		display: flex;
    width: 40px;
	}

  .input {
    height: 32px;
    margin-top: 12px;
    text-align: center;
    text-transform: uppercase;
    width: 100%;
  }
</style>
