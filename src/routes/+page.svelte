<script>
	import Input from '$lib/input/Input.svelte';
	import Switch from '$lib/switch/Switch.svelte';
</script>

<svelte:head>
	<title>Random Letters</title>
	<meta name="description" content="Random Letters" />
</svelte:head>

<section>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset="svelte-welcome.webp" type="image/webp" />
				<img src="svelte-welcome.png" alt="Welcome" />
			</picture>
		</span>
		<strong>Random Letters</strong> sirve para seleccionar letras de manera aleatoria
	</h1>
	<Switch />
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}
	h1 {
		width: 100%;
	}
	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}
	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
