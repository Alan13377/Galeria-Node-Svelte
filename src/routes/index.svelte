<script>
	import { onMount } from 'svelte';
	import auth from '../utils/authService';
	import { isAuthenticated, user } from '../utils/store';
	import CrudGallery from '../components/Crud-gallery.svelte';

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

<div>
	<div>
		{#if $isAuthenticated}
			<h2>Hey {$user.name}!</h2>
			{#if $user.picture}
				<CrudGallery />
			{:else}
				<img src="https://source.unsplash.com/random/400x300" alt="Random Photo" />
			{/if}
		{:else}
			<div class="index">
				<h3 class="text_pop">
					<span>S</span>
					<span>v</span>
					<span>e</span>
					<span>l</span>
					<span>t</span>
					<span>e</span>
					<br />
					<span>G</span>
					<span>a</span>
					<span>l</span>
					<span>l</span>
					<span>e</span>
					<span>r</span>
					<span>y</span>
				</h3>
				<div>
					<img src="pictures.png" alt="" />
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.index {
		margin: 40px;
		display: flex;
		gap: 20px;
	}
	@media screen and (max-width: 480px) {
		.index {
			flex-direction: column;
		}
		img {
			max-width: 100%;
		}
		h3 {
			font-size: 80px;
			letter-spacing: -10px;
		}
	}
	img {
		width: 600px;
		height: auto;
	}

	.text_pop span {
		font-size: 100px;
		animation: in 0.5s forwards;
	}
	.text_pop span:hover {
		animation: out 0.5s forwards;
	}

	@keyframes out {
		0% {
			text-shadow: -4px 4px 0px #ef476f;
		}
		50% {
			text-shadow: -4px 4px 0px #ef476f, -8px 8px 0px #a23450;
		}
		100% {
			text-shadow: -4px 4px 0px #ef476f, -8px 8px 0px #a23450, -12px 12px 0px #4d1a26;
			transform: translate(12px, -12px);
		}
	}

	@keyframes in {
		0% {
			text-shadow: -4px 4px 0px #ef476f, -8px 8px 0px #a23450, -12px 12px 0px #4d1a26;
			transform: translate(12px, -12px);
		}
		33% {
			text-shadow: -4px 4px 0px #ef476f, -8px 8px 0px #a23450;
		}
		66% {
			text-shadow: -4px 4px 0px #ef476f;
		}
		100% {
			text-shadow: -4px 4px 0px #ef476f;
		}
	}
</style>
