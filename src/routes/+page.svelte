<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';

	export let data;

	const projectsQueryStore = queryStore({
		client: getContextClient(),
		query: gql`
			{
				projects {
					name
					slug
					description
					tags
					demo
					sourceCode
					image {
						url(transformation: { image: { resize: { width: 800, height: 800, fit: crop } } })
					}
				}
			}
		`
	});
</script>

<svelte:head>
	<title>Awesome Svelte & D3 | Home</title>
</svelte:head>

<h1>Welcome to Awesome Svelte & D3</h1>

<h2>In the wild</h2>
<p>A growing list of awesome Projects:</p>

<div class="container projects">
	{#if $projectsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $projectsQueryStore.error}
		<p>Oopsie! {$projectsQueryStore.error.message}</p>
	{:else}
		{#each $projectsQueryStore.data.projects as p}
			<ProjectCard name={p.name} description={p.description} url={p.image[0].url} slug={p.slug} />
		{/each}
	{/if}
</div>
