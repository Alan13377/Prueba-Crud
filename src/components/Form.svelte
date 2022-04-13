<script>
	import { supabase } from '$lib/supabaseClient';
	export let id;
	export let nombre;
	export let precio;
	export let sku;
	export let temporada;
	export let cantidad;
	export let categoria;

	async function SaveProduct() {
		const newProduct = await supabase.from('Productos').insert([
			{
				id: id,
				nombre: nombre,
				precio: precio
			}
		]);
		if (newProduct.status === 201) {
			nombre = '';
			precio = '';
		}
	}
</script>

<form class="formulario">
	<div>
		<label for="id">ID</label>
		<input bind:value={id} type="text" name="id" disabled />
	</div>
	<div>
		<label for="nombre">Nombre</label>
		<input bind:value={nombre} type="text" name="nombre" placeholder="Nombre del Producto" />
	</div>
	<div>
		<label for="precio">Precio</label>
		<input bind:value={precio} type="text" name="precio" placeholder="Precio" />
	</div>
	<div>
		<label for="sku">SKU</label>
		<input bind:value={sku} type="text" name="sku" placeholder="sku" />
	</div>
	<div>
		<label for="temporada">Temporada</label>
		<input bind:value={temporada} type="text" name="temporada" placeholder="En temporada" />
	</div>
	<div>
		<label for="imagen">Imagen</label>
		<input type="text" name="imagen" />
	</div>
	<div>
		<label for="cantidad">Cantidad</label>
		<input bind:value={cantidad} type="text" name="cantidad" placeholder="Cantidad" />
	</div>
	<div>
		<label for="categoria">Categoria</label>
		<select bind:value={categoria} name="categoria" id="">
			<option value="Verdura">Verdura</option>
			<option value="Fruta">Fruta</option>
		</select>
	</div>

	<button on:click={SaveProduct}>Guardar Post</button>
	<input type="submit" value="Modificar" />
</form>

<style>
	input:not([type='submit']) {
		padding: 10px;
		width: 80%;
		border: 2px solid #ccc;
		border-radius: 10px;
		background-color: white;
		margin-bottom: 15px;
		display: block;
	}
	.formulario {
		width: 100%;
	}
</style>
