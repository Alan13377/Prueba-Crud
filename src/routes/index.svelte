<script>
	import { supabase } from '$lib/supabaseClient';

	import { onMount } from 'svelte';

	let productos = [];
	let id;
	let nombre;
	let precio;
	let sku;
	let temporada;
	let cantidad;
	let categoria;

	const getData = async () => {
		let { data } = await supabase.from('Productos').select('*');
		productos = data;
	};

	async function SaveProduct(e) {
		e.preventDefault();
		const { data, error } = await supabase.from('Productos').insert([
			{
				nombre: nombre,
				precio: precio,
				sku: sku,
				temporada: temporada,
				cantidad: cantidad,
				categoria: categoria
			}
		]);
		getData();
	}

	async function deleteProduct(id) {
		const { data, error } = await supabase.from('Productos').delete().match({ id: id });
		location.reload();
		getData();
	}

	getData();
</script>

<div class="container">
	<h1>Svelte JS CRUD</h1>
	<h2>Productos</h2>
	<div class="contenedor-productos">
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
				<input bind:value={sku} type="number" name="sku" />
			</div>
			<div>
				<label for="temporada">En temporada</label>
				<select name="temporada" bind:value={temporada}>
					<option value={true}>Verdadero</option>
					<option value={false}>Falso</option>
				</select>
			</div>
			<div>
				<label for="cantidad">Cantidad</label>
				<input
					bind:value={cantidad}
					type="text"
					name="cantidad"
					placeholder="Cantidad del Producto"
				/>
			</div>
			<div>
				<label for="categoria">Categoria</label>
				<select name="categoria" bind:value={categoria}>
					<option value={'Verdura'}>Verdura</option>
					<option value={'Fruta'}>Fruta</option>
				</select>
			</div>
			<input type="submit" value="Enviar" on:click={SaveProduct} />
		</form>
		<div class="table">
			<table>
				<thead>
					<tr>
						<th>ID</th>
						<th>Nombre</th>
						<th>Precio</th>
						<th>SKU </th>
						<th>Temporada</th>
						<th>imagen</th>
						<th>Cantidad</th>
						<th>Categoria</th>
						<th>Acciones</th>
					</tr>
				</thead>
				<tbody>
					{#each productos as producto}
						<tr>
							<td>{producto.id}</td>
							<td>{producto.nombre}</td>
							<td>{producto.precio}</td>
							<td>{producto.sku}</td>
							<td>{producto.temporada}</td>
							<td><img src={producto.imagen} alt={producto.nombre} /></td>
							<td>{producto.cantidad}</td>
							<td>{producto.categoria}</td>
							<td>
								<input type="submit" value="Borrar" on:click={deleteProduct(producto.id)} />
								<input type="submit" value="Editar" />
							</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
	</div>
</div>

<style>
	img {
		width: 100px;
	}

	.container {
		max-width: 1200px;
		margin: 0 auto;
	}
	.contenedor-productos {
		display: flex;

		justify-content: space-around;
	}
</style>
