<script>
  import Counter from '$lib/Counter.svelte';
  import Input from '$lib/input/Input.svelte';

  /**
	 * @type {any[]}
	*/
  let excludedLetters = [];
  let checkboxValue = false;
  let timerCheckbox = false;
  let timer = 0;
  let timeFormatted = '00:00';
  let timerRunning = false;
  // @ts-ignore
  let interval;
  
  // @ts-ignore
  function handleCheckbox(e) {
    checkboxValue = e.target.checked
  }

  function beep() {
    var snd = new Audio("data:audio/wav;base64,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qJrx4KOQjahZxWbcZgztj2c49nKmkId44S71j0c8eV9yDK6uPRzx5X18eDvjvQ6yKo9ZSS6l//8elePK/Lf//IInrOF/FvDoADYAGBMGb7FtErm5MXMlmPAJQVgWta7Zx2go+8xJ0UiCb8LHHdftWyLJE0QIAIsI+UbXu67dZMjmgDGCGl1H+vpF4NSDckSIkk7Vd+sxEhBQMRU8j/12UIRhzSaUdQ+rQU5kGeFxm+hb1oh6pWWmv3uvmReDl0UnvtapVaIzo1jZbf/pD6ElLqSX+rUmOQNpJFa/r+sa4e/pBlAABoAAAAA3CUgShLdGIxsY7AUABPRrgCABdDuQ5GC7DqPQCgbbJUAoRSUj+NIEig0YfyWUho1VBBBA//uQZB4ABZx5zfMakeAAAAmwAAAAF5F3P0w9GtAAACfAAAAAwLhMDmAYWMgVEG1U0FIGCBgXBXAtfMH10000EEEEEECUBYln03TTTdNBDZopopYvrTTdNa325mImNg3TTPV9q3pmY0xoO6bv3r00y+IDGid/9aaaZTGMuj9mpu9Mpio1dXrr5HERTZSmqU36A3CumzN/9Robv/Xx4v9ijkSRSNLQhAWumap82WRSBUqXStV/YcS+XVLnSS+WLDroqArFkMEsAS+eWmrUzrO0oEmE40RlMZ5+ODIkAyKAGUwZ3mVKmcamcJnMW26MRPgUw6j+LkhyHGVGYjSUUKNpuJUQoOIAyDvEyG8S5yfK6dhZc0Tx1KI/gviKL6qvvFs1+bWtaz58uUNnryq6kt5RzOCkPWlVqVX2a/EEBUdU1KrXLf40GoiiFXK///qpoiDXrOgqDR38JB0bw7SoL+ZB9o1RCkQjQ2CBYZKd/+VJxZRRZlqSkKiws0WFxUyCwsKiMy7hUVFhIaCrNQsKkTIsLivwKKigsj8XYlwt/WKi2N4d//uQRCSAAjURNIHpMZBGYiaQPSYyAAABLAAAAAAAACWAAAAApUF/Mg+0aohSIRobBAsMlO//Kk4soosy1JSFRYWaLC4qZBYWFRGZdwqKiwkNBVmoWFSJkWFxX4FFRQWR+LsS4W/rFRb/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////VEFHAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAU291bmRib3kuZGUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMjAwNGh0dHA6Ly93d3cuc291bmRib3kuZGUAAAAAAAAAACU=");  
    snd.play();
  }

  function resetExcluded() {
    excludedLetters = []
  }

  //@ts-ignore
  function addExcluded(newLetter) {
    excludedLetters = [...new Set([...excludedLetters, newLetter])]
  }

  // @ts-ignore
  function handleTimer(e) {
    timerCheckbox = e.target.checked
  }

  // @ts-ignore
  function formatTime() {
    const display = document.getElementById('countdown')

    // @ts-ignore
    let minutes = parseInt(Math.floor(timer / 3600), 10);
    // @ts-ignore
    let seconds = parseInt(timer % 60, 10);
    
    // @ts-ignore
    minutes = minutes < 10 ? "0" + minutes : minutes;
    // @ts-ignore
    seconds = seconds < 10 ? "0" + seconds : seconds;

    // @ts-ignore
    display.textContent = minutes + ":" + seconds;
  }

  // @ts-ignore
  function handleTimerChange(e) {
    timer = e.target.value;
    formatTime();
  }

  // @ts-ignore
  function startTimer() {
    formatTime()
    let duration = timer, minutes, seconds;
    const display = document.getElementById('countdown')
    timerRunning = true;
    interval = setInterval(function () {
      // @ts-ignore
      minutes = parseInt(duration / 3600, 10);
      // @ts-ignore
      seconds = parseInt(duration % 60, 10);
      minutes = minutes < 10 ? "0" + minutes : minutes;
      seconds = seconds < 10 ? "0" + seconds : seconds;
      // @ts-ignore
      display.textContent = minutes + ":" + seconds;
      if (--duration < 0) {
        duration = timer;
        timerRunning = false;
        beep();
        resetInterval()
      }
    }, 1000);
  }

  function resetInterval() {
    timerRunning = false;
    // @ts-ignore
    clearInterval(interval)
    formatTime()
  }

  //@ts-ignore
  function removeChar(char) {
    const newArray = excludedLetters.filter(l => l !== char);
    excludedLetters = newArray;
  }
</script>

<div>
  <div class='container'>
    <h2 class='wrapper-small'>
      <span class='title'>Repetir letras</span>
      <label class="switch">
         <input type="checkbox" on:input={handleCheckbox}>
        <span class="slider round"></span>
      </label>
    </h2>
    <h2 class='wrapper-small'>
      <span class='title'>Timer</span>
      <label class="switch">
         <input type="checkbox" on:input={handleTimer}>
        <span class="slider round"></span>
      </label>
    </h2>
  </div>
  <h2 class='wrapper'>
    <div class='timer-wrapper'>
      <input on:input={handleTimerChange} disabled={!timerCheckbox} type="number" class="timer" />
      <span class="ms">seg</span>
      <span class={`seg ${!timerCheckbox ? 'hidden' : ''}`}>{`${(timer/3600.0).toFixed(2)} minutos`}</span>
    </div>
    <div id="countdown">
      {timeFormatted}
    </div>
    <button class={`timer-button ${!timerRunning ? 'none' : ''}`} on:click={resetInterval}>Reiniciar</button>
    <button disabled={!timerCheckbox} class={`timer-button ${timerRunning ? 'none' : ''}`} on:click={startTimer}>Iniciar</button>
  </h2>
  <div class='excluded'>
    <Input addExcluded={addExcluded} resetExcluded={resetExcluded} />
    <div class="tags">
      {#each excludedLetters as character}
        <div on:click={() => removeChar(character)}>{character}</div>
      {/each}
    </div>
  </div>
  <Counter excludedLetters={excludedLetters} timerActive={timerCheckbox} startTimer={startTimer} shouldRepeat={checkboxValue} />
</div>

<style>
  .container {
    display: flex;
    justify-content: space-between;
    width: 256px;
  }
  h2 {
		margin: 24px 0;
		width: 256px;
	}
  h2 .title {
    font-size: 14px;
  }
  .seg {
    bottom: -16px;
    color: #877e7e;
    font-size: 12px;
    left: 8px;
    position: absolute;
  }
  .timer-button {
		border-radius: 20px;
		cursor: pointer;
    border: none;
    font-size: 14px;
    height: 34px;
    width: 78px;
  }
  .hidden {
    visibility: hidden;
  }
  .none {
    display: none;
  }
  .ms {
    color: #877e7e;
    font-size: 14px;
    position: absolute;
    right: 8px;
    top: 8px;
  }
  .timer-wrapper {
    position: relative
  }
  .timer {
    padding: 6px 24px 6px 6px;
    width: 80px;
  }
	.wrapper {
		align-items: center;
		display: flex;
		justify-content: space-between;
	}
  .wrapper-small {
    align-items: center;
		display: flex;
		justify-content: space-between;
    width: 45%;
  }
	.switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
  }
  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    -webkit-transition: .4s;
    transition: .4s;
    width: 60px;
  }
  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    -webkit-transition: .4s;
    transition: .4s;
  }
  input:checked + .slider {
    background-color: #2196F3;
  }
  input:focus + .slider {
    box-shadow: 0 0 1px #2196F3;
  }
  input:checked + .slider:before {
    -webkit-transform: translateX(26px);
    -ms-transform: translateX(26px);
    transform: translateX(26px);
  }

  .slider.round {
    border-radius: 34px;
  }
  .slider.round:before {
    border-radius: 50%;
  }
  .excluded {
    position: relative;
  }
  .tags {
    bottom: -30px;
    height: 20px;
    position: absolute;
    width: 256px;
    display: flex;
  }
  .tags div {
    background-color: #ccc;
    border-radius: 20px;
    height: 16px;
    margin-left: 8px;
    padding: 4px 6px;
  }
</style>
