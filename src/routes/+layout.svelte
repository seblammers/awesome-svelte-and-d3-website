<script>
	import '$lib/styles/global.scss';
	import Nav from '$lib/components/Nav.svelte';
	import { createClient, setContextClient } from '@urql/svelte';

	const client = createClient({
		url: `https://api-eu-central-1.hygraph.com/v2/cl0z9knhb4qnk01z38ju48cfr/master`
	});

	setContextClient(client);

	export let data;
</script>

<div class="layout">
	<Nav />

	{#key data.currentRoute}
		<main class="flow">
			<slot />
		</main>
	{/key}
</div>
