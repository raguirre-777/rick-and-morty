<script>
	import Card from './Card.svelte';
	let numeroAleatorio = '';
	let response = []
	let listReponse = []
	let loading = false
	let contador = 1

	function handleClick(){
		numeroAleatorio = Math.floor(Math.random()*(listReponse.info.count+1))
	}

	$:{
		fetch(`https://rickandmortyapi.com/api/character/${numeroAleatorio}`)
		.then(res => res.json())
		.then(apiResponse => {
			response = apiResponse
		})
	}

	$:{
		loading = true
		fetch(`https://rickandmortyapi.com/api/character/`)
		.then(res => res.json())
		.then(apiListResponse => {
			listReponse = apiListResponse
			loading = false
		})
	}
	$:{
	
	}

	function nextPage(){	
		if(contador > 33)
			{
				conador = 34
			}
		contador++
		fetch(`https://rickandmortyapi.com/api/character/?page=${contador}`)
		.then(res => res.json())
		.then(next => {
			listReponse = next
			loading = false
		})
		}
		
	function anteriorPage(){	
		if(contador < 2)
			{
				conador = 1
			}
		contador--
		fetch(`https://rickandmortyapi.com/api/character/?page=${contador}`)
		.then(res => res.json())
		.then(next => {
			listReponse = next
			loading = false
			
		})
		}
	
	
</script>

<main>
	<button on:click={handleClick}><h1> haz click para saber
		¿Qué personaje de Rick and Morty eres?
	</h1></button>
	<h2>{response.name == undefined ? '' : response.name  }</h2>
	<!-- svelte-ignore a11y-missing-attribute -->
	<img src={response.image}>
	<br>
	<br>
	<br>
	<h1>Lista de Cards de Todos los personajes </h1>
	{#if loading}
	<strong> Cargando.............</strong>
	{:else}
		{#each listReponse.results as personaje }
			<Card name={personaje.name} status={personaje.status} image={personaje.image} />
		{/each}
	{/if}
	<!-- svelte-ignore missing-declaration -->
	<button on:click={anteriorPage}> Anterior </button>
	<strong>page: {contador}</strong>
	<button on:click={nextPage}>Siguiente</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		background-color: steelblue;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>