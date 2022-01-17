<script>
	export let data = [];
	let btnUpdateActive = true;
	let btnResetActive = false;
	let datosApi = {
		id: null,
		image: '',
		title: '',
		desc: ''
	};

	const url = 'https://backend-node-alan13377.vercel.app/api/galeria';
	const getImages = async () => {
		const response = await fetch(url);
		data = await response.json();
		btnUpdateActive = true;
	};

	//*Insertar

	const addImage = async () => {
		const newImage = {
			id: datosApi.id,
			image: datosApi.image,
			title: datosApi.title,
			desc: datosApi.desc
		};
		//Insertar en la API
		const response = await fetch(url, {
			method: 'POST',
			body: JSON.stringify(newImage),
			headers: {
				'Content-Type': 'application/json'
			}
		});
		let promise = getImages();
	};

	//*Eliminar
	const deleteImage = async (id) => {
		const response = await fetch(url + `/${id}`, {
			method: 'DELETE'
		});
		getImages();
	};

	//*Editar
	let editar = (datos) => {
		datosApi = datos;
		btnResetActive = true;
		btnUpdateActive = false;
	};

	//*Actualizar
	const updateImage = async (id) => {
		let newImage = {
			id: datosApi.id,
			image: datosApi.image,
			title: datosApi.title,
			desc: datosApi.desc
		};
		let response = await fetch(url + `/${id}`, {
			method: 'PUT',
			body: JSON.stringify(newImage),
			headers: {
				'Content-Type': 'application/json'
			}
		});
		getImages();
	};

	const clearInputs = () => {
		let id = (document.getElementById('id').value = '');
		let image = (document.getElementById('image').value = '');
		let title = (document.getElementById('title').value = '');
		let desc = (document.getElementById('desc').value = '');
		btnUpdateActive = true;
		btnResetActive = false;
	};
	getImages();
</script>

<section>
	<div>
		<form action="" class="formulario  ">
			<fieldset>
				<legend>Ingrese Los Datos</legend>
				<div>
					<label for="id">ID:</label>
					<input type="text" name="id" id="id" bind:value={datosApi.id} disabled />
				</div>
				<div>
					<label for="image">Imagen:</label>
					<input type="text" name="image" id="image" bind:value={datosApi.image} />
				</div>
				<div>
					<label for="title">Titulo:</label>
					<input type="text" name="tilte" id="title" bind:value={datosApi.title} />
				</div>
				<div>
					<label for="desc">Descripcion:</label>
					<input type="text" name="desc" id="desc" bind:value={datosApi.desc} />
				</div>
				<div class="Enviar">
					<button
						type="submit"
						class="btn btn-agregar"
						on:click|preventDefault={addImage}
						disabled={btnResetActive}>Agregar</button
					>
					<button
						type="submit"
						class="btn btn-update"
						on:click|preventDefault={updateImage(datosApi._id)}
						disabled={btnUpdateActive}>Actualizar</button
					>
					<button type="button" class="btn btn-cancelar" on:click={clearInputs}>Cancelar</button>
				</div>
			</fieldset>
		</form>
	</div>

	<div>
		<div class="contenedor">
			<div class="galeria">
				{#each data as datos}
					<div>
						<img src={datos.image} />

						<button type="submit" class="btn btn-danger" on:click={deleteImage(datos._id)}
							>Eliminar</button
						>
						<button type="submit" class="btn btn-editar" on:click={editar(datos)}>Editar</button>
					</div>
				{/each}
			</div>
		</div>
	</div>
</section>

<style>
	img {
		max-width: 100%;
		height: 60%;
		display: block;
	}
	fieldset {
		border: 1px solid #333;
		padding: 30px;
		margin: 10px;
	}
	.formulario {
		margin: 20px 0 0 0;
	}
	legend {
		font-size: 1.5em;
		font-weight: bold;
		color: #000;
	}

	label {
		font-weight: bold;
		text-transform: uppercase;
		display: block;
	}
	input {
		padding: 10px;
		display: block;
		width: 100%;
		background-color: white;
		border: 1px solid #ccc;
		border-radius: 10px;
		margin: 0;
		margin-bottom: 20px;
	}

	.btn:hover {
		cursor: pointer;
	}

	.btn-update {
		box-shadow: 0px 1px 0px 0px #fff6af;
		text-shadow: 0px 1px 0px #ffee66;
		background: #ffab23;
	}

	.btn-cancelar {
		box-shadow: inset 0px 1px 0px 0px #d9fbbe;
		text-shadow: inset 0px 1px 0px #004fc5;
		background: linear-gradient(#0064f9, #0064f9);
	}
	.btn-agregar {
		box-shadow: inset 0px 1px 0px 0px #d9fbbe;
		text-shadow: 1px 1px 0px #2f6627;
		background: #44c767;
	}
	.btn-danger {
		box-shadow: inset 0px 39px 0px -24px #e67a73;
		text-shadow: inset 0px 1px 0px #b23e35;
		background: #e4685d;
	}
	.btn-editar {
		box-shadow: inset 0px 1px 0px 0px #5dac1c;
		text-shadow: inset 0px 1px 0px #86ae47;
		background: #47e247;
	}

	.btn {
		font-size: 15px;
		font-family: Arial;
		font-weight: bold;
		width: 140px;
		height: 50px;
		border-width: 1px;
		color: #fff;
		border-color: #fff;
		border-top-left-radius: 4px;
		border-top-right-radius: 4px;
		border-bottom-left-radius: 4px;
		border-bottom-right-radius: 4px;
		margin: 20px 5px;
		width: 100%;
	}

	@media (min-width: 768px) {
		.btn {
			font-size: 15px;
			font-family: Arial;
			font-weight: bold;
			width: 125px;
			height: 50px;
			border-width: 1px;
			color: #fff;
			border-color: #fff;
			border-top-left-radius: 4px;
			border-top-right-radius: 4px;
			border-bottom-left-radius: 4px;
			border-bottom-right-radius: 4px;
			margin: 20px 5px;
		}
	}
	@media (min-width: 768px) {
		.galeria {
			display: grid;
			grid-template-columns: repeat(3, 1fr);
			gap: 20px;
		}
	}
</style>
