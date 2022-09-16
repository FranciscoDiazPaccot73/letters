<script>
	let letter = '';
  export
	/**
	 * @type {any}
	*/ let resetExcluded;
	 export
	/**
	 * @type {any}
	*/ let addExcluded;

  // @ts-ignore
  function updateLetter(e) {
    if (e && e.data && onlyLetters(e.data)) {
      letter = e.data
      addExcluded(e.data.toUpperCase())
    }
	}

  // @ts-ignore
  function onlyLetters(str) {
    return /^[a-zA-Z]+$/.test(str);
  }

  function reset() {
    resetExcluded()
    letter = ''
  }
</script>

<div class="wrapper">
  <span class='title'>Excluir letras</span>
  <div class="content">
    <input
      on:input={(e) => updateLetter(e)}
      on:blur={() => letter = ''}
      value={letter}
      type='text'
      class='input'
      placeholder="ej: X,Y,Z"
    />
    <button on:click={reset}>Reiniciar</button>
  </div>
</div>

<style>
	.wrapper {
		display: flex;
    flex-direction: column;
    position: relative;
    width: 256px;
	}
  .title {
    font-size: 14px;
    margin-bottom: 10px;
  }
  .content {
    align-items: center;
    display: flex;
    justify-content: center;
  }
  .content button {
		border-radius: 20px;
		border: none;
		cursor: pointer;
    font-size: 14px;
    height: 32px;
  }
  .input {
    height: 32px;
    margin: 0 6px;
    text-transform: uppercase;
    width: 100%;
  }
</style>
