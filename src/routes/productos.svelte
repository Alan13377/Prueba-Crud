<script>
	import { supabase } from '$lib/supabaseClient';
	let productos = [];

	const getData = async () => {
		let { data } = await supabase.from('Productos').select('*');
		productos = data;
	};

	getData();
</script>

<h2>Productos</h2>

<div>
	{#each productos as { id, nombre, precio, sku, temporada, imagen }}
		<div class="contenedor-frutas">
			<img src={imagen} alt="" />
			<p>{sku}</p>
			<h3>{nombre}</h3>
			<p>{precio}</p>
			<p>{temporada}</p>
		</div>
	{/each}
</div>

<style>
	.contenedor-frutas {
		display: grid;
	}
</style>
