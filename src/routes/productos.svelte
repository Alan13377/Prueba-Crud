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

<div class="contenedor-frutas">
	{#each productos as { id, nombre, precio, sku, temporada, imagen }}
		<div class="card">
			<div class="card__cover"><img src={imagen} alt="" /></div>

			<div class="card__content">
				<h3>Producto: {nombre}</h3>
				<p><span class="descripcion">SKU:</span> {sku}</p>
			</div>
			<p><span class="descripcion">En temporada:</span>{temporada}</p>
			<p><span class="descripcion">Precio:$ </span>{precio}</p>
		</div>
	{/each}
</div>

<style>
	img {
		max-width: 100%;
		max-height: 100%;
	}

	.card {
		border: 1px solid #ccc;
		border-radius: 10px;
		padding: 20px;
	}
	.card__content {
		text-align: center;
	}
	.descripcion {
		font-size: 15px;
		font-weight: bold;
	}
	@media (min-width: 768px) {
		.contenedor-frutas {
			display: grid;
			grid-template-columns: repeat(4, 1fr);
			gap: 40px;
		}
	}
</style>
