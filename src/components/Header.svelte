<script>
	import { onMount } from 'svelte';
	import auth from '../utils/authService';
	import { isAuthenticated, user } from '../utils/store';

	let auth0Client;

	onMount(async () => {
		auth0Client = await auth.createClient();

		isAuthenticated.set(await auth0Client.isAuthenticated());
		user.set(await auth0Client.getUser());
	});

	function login() {
		auth.loginWithPopup(auth0Client);
	}

	function logout() {
		auth.logout(auth0Client);
	}
</script>

<div class="Header">
	<div class="contenedor">
		<div class="barra-navegacion">
			<a href="/" class="logo"><h1>Galeria</h1></a>
			<nav class="navegacion">
				<a class="navegacion__enlace" sveltekit:prefetch href="/">Home</a>
				<a class="navegacion__enlace" sveltekit:prefetch href="/Gallery">Galeria</a>
				{#if $isAuthenticated}
					<a class="nav-link" href="/#" on:click={logout}>Log Out</a>
				{:else}
					<a class="nav-link" href="/#" on:click={login}>Log In</a>
				{/if}
			</nav>
		</div>
	</div>
</div>

<style>
	a {
		text-decoration: none;
	}

	.contenedor {
		width: min(80%, 120rem);
		margin: 0 auto; /**Centramos*/
	}
	@media (min-width: 768px) {
		.barra-navegacion {
			display: flex;
			justify-content: space-between;
			align-items: center;
		}
	}

	@media (min-width: 768px) {
		.navegacion {
			display: flex;
			gap: 20px;
		}
	}

	.navegacion__enlace {
		display: block;
		text-align: center;
	}
	.logo {
		font-weight: bold;
		text-align: center;
	}
</style>
