<script>
	import '@fontsource/clear-sans';
	import '$lib/styles/global.scss';
	import Meta from '$lib/components/Meta.svelte';
	import Nav from '$lib/components/Nav.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import { createClient, setContextClient } from '@urql/svelte';

	const client = createClient({
		url: `https://api-eu-central-1.hygraph.com/v2/cl0z9knhb4qnk01z38ju48cfr/master`
	});

	setContextClient(client);

	export let data;
</script>

<Meta />

<div class="layout">
	<Nav />

	{#key data.currentRoute}
		<main class="flow">
			<slot />
		</main>
	{/key}

	<Footer />
</div>
