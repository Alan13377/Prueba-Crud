<script>
	import { supabase } from '$lib/supabaseClient';
	import Swal from 'sweetalert2';
	import 'sweetalert2/dist/sweetalert2.css';

	let productos = [];
	let id = '';
	let nombre;
	let precio;
	let sku;
	let temporada;
	let cantidad;
	let categoria;
	let filename = '';
	let file = null;

	const getData = async () => {
		let { data } = await supabase.from('Productos').select('*');
		productos = data;
	};

	async function SaveProduct(e) {
		e.preventDefault();
		const filename = 'imagen' + '/' + Date.now();
		const image = await supabase.storage.from('imagenes').upload(filename, file, {
			cacheControl: '3600',
			upsert: false
		});
		const { publicURL } = await supabase.storage.from('imagenes').getPublicUrl(filename);
		const newProduct = await supabase.from('Productos').insert([
			{
				nombre: nombre,
				precio: precio,
				sku: sku,
				temporada: temporada,
				imagen: publicURL,
				filename: filename,
				cantidad: cantidad,
				categoria: categoria
			}
		]);

		if (newProduct.status === 201) {
			nombre = '';
			precio = '';
			sku = '';
			temporada = '';
			cantidad = '';
			categoria = '';
			Swal.fire({
				position: 'top-end',
				icon: 'success',
				title: 'Producto Guardado exitosamente',
				showConfirmButton: false,
				timer: 1500
			});
		} else {
			Swal.fire('No se guardo Correctamente', 'Asegurate de llenar todos los campos', 'error');
		}

		getData();
	}

	async function editProduct(producto) {
		id = producto.id;
		nombre = producto.nombre;
		precio = producto.precio;
		sku = producto.sku;
		temporada = producto.temporada;
		cantidad = producto.cantidad;
		categoria = producto.categoria;
	}
	async function updateProduct(e) {
		e.preventDefault();

		if (file == null) {
			const edit = await supabase
				.from('Productos')
				.update({
					nombre: nombre,
					precio: precio,
					sku: sku,
					temporada: temporada,
					cantidad: cantidad,
					categoria: categoria
				})
				.match({ id: id });
		} else {
			const filename = 'imagen' + '/' + Date.now();
			const image = await supabase.storage.from('imagenes').upload(filename, file, {
				cacheControl: '3600',
				upsert: false
			});
			const { publicURL } = await supabase.storage.from('imagenes').getPublicUrl(filename);
			const update = await supabase
				.from('Productos')
				.update({
					nombre: nombre,
					precio: precio,
					sku: sku,
					temporada: temporada,
					imagen: publicURL,
					cantidad: cantidad,
					categoria: categoria,
					filename: filename
				})
				.match({ id: id });
		}

		getData();
	}

	function alertDelete(id) {
		Swal.fire({
			title: 'Estas seguro de eliminar el Producto?',
			text: 'La accion no se puede revertir!',
			icon: 'warning',
			showCancelButton: true,
			confirmButtonColor: '#3085d6',
			cancelButtonColor: '#d33',
			confirmButtonText: 'Si,eliminar!'
		}).then((result) => {
			if (result.isConfirmed) {
				deleteProduct(id);
			}
		});
	}
	async function deleteProduct(id) {
		await supabase.from('Productos').delete().match({ id: id });
		location.reload();

		getData();
	}
	function getFile(e) {
		file = !!e.target.files.length && e.target.files[0];
	}

	const cancelar = () => {
		id = '';
		nombre = '';
		precio = '';
		sku = '';
		temporada = '';
		cantidad = '';
		categoria = '';
	};
	getData();
</script>

<svelte:head>
	<title>Crud</title>
</svelte:head>

<div class="contenedor-productos">
	<form class="formulario" enctype="multipart/form-data">
		<fieldset>
			<div>
				<label for="nombre">Nombre</label>
				<input bind:value={nombre} type="text" name="nombre" placeholder="Nombre del Producto" />
			</div>
			<div>
				<label for="precio">Precio</label>
				<input bind:value={precio} type="number" name="precio" placeholder="Precio" />
			</div>
			<div>
				<label for="sku">SKU</label>
				<input bind:value={sku} type="number" name="sku" />
			</div>
			<div class="select select-categoria">
				<label for="temporada">En temporada</label>
				<select name="temporada" bind:value={temporada}>
					<option value="" disabled>--Seleccione--</option>
					<option value={true}>Verdadero</option>
					<option value={false}>Falso</option>
				</select>
			</div>
			<div>
				<label for="imagen">Imagen</label>
				<input type="file" on:change={getFile} />
				<input type="hidden" bind:value={id} />
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
			<div class="select select-categoria">
				<label for="categoria">Categoria</label>
				<select name="categoria" bind:value={categoria}>
					<option value="" disabled>--Seleccione--</option>
					<option value="Verdura">Verdura</option>
					<option value="Fruta">Fruta</option>
				</select>
			</div>
			<input type="submit" class="btn btn-submit" value="Enviar" on:click={SaveProduct} />
			<button class="btn btn-update" value="Actualizar" on:click={updateProduct}>Actualizar</button>
			<button class="btn btn-cancelar" on:click|preventDefault={cancelar}> Cancelar </button>
		</fieldset>
	</form>
	<div class="tabla">
		<table id="table" class="contenido-tabla">
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
							<button class="btn btn-danger" on:click={alertDelete(producto.id)}>Eliminar</button>

							<button class="btn btn-edit" on:click={editProduct(producto)}> Editar</button>
						</td>
					</tr>
				{/each}
			</tbody>
		</table>
	</div>
</div>

<style>
	img {
		width: 100px;
	}

	@media (min-width: 768px) {
		.contenedor-productos {
			display: grid;
			grid-template-columns: 1fr 1fr;
			grid-gap: 20px;
		}
	}
	.tabla {
		margin-top: 20px;
		display: flex;
		justify-content: center;
	}
	.contenido-tabla {
		border-collapse: collapse;
	}

	@media (max-width: 768px) {
		table {
			width: 100%;
		}
		.contenido-tabla thead tr {
			display: none;
		}
		.contenido-tabla tbody td {
			display: block;
			padding: 0;
			border-bottom: 1px solid #ddd;
			text-align: center;
		}
	}
	.contenido-tabla thead tr {
		background-color: #1395bf;
		color: #fff;
		text-align: center;
	}
	.contenido-tabla tr,
	.contenido-tabla th {
		text-align: center;
		padding: 15px 10px;
	}

	.contenido-tabla tbody tr {
		border-bottom: 1px solid #1395bf;
	}
	.contenido-tabla tbody tr:nth-of-type(even) {
		background-color: #ececec;
	}

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
	label {
		font-size: 15px;
		display: block;

		font-weight: bold;
	}

	.select select {
		height: 40px;
		padding: 10px 15px;
		font-size: 15px;
		border: 1px solid #ccc;
		border-radius: 5px;
	}

	.btn {
		margin: 3px;
		padding: 10px;
		border-radius: 10%;
		border: none;
		display: inline-block;
	}

	.btn-danger {
		width: 100%;
		background-color: #ff0000;
		color: #fff;
		font-weight: bold;
	}

	.btn-danger:hover {
		cursor: pointer;
		background-color: #a70505;
	}
	.btn-edit {
		width: 100%;
		background-color: #39925a;
		color: #fff;
		font-weight: bold;
	}

	.btn-edit:hover {
		cursor: pointer;
		background-color: #225535;
	}

	.btn-submit {
		max-width: 100%;
		background-color: #00779e;
		color: #fff;
		font-weight: bold;
	}

	.btn-submit:hover {
		cursor: pointer;
		background-color: #005580;
	}
	.btn-update {
		max-width: 100%;
		background-color: #c67a02;
		color: #fff;
		font-weight: bold;
	}
	.btn-update:hover {
		cursor: pointer;
		background-color: #a15a02;
	}
	.btn-cancelar {
		max-width: 100%;
		background-color: #251414;
		color: #fff;
		font-weight: bold;
	}
	.btn-cancelar:hover {
		cursor: pointer;
		background-color: rgb(44, 33, 19);
	}
</style>
