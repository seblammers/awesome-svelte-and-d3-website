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
					authors
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
<p>A curated list of awesome Projects:</p>

<div class="container grid">
	{#if $projectsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $projectsQueryStore.error}
		<p>Oopsie! {$projectsQueryStore.error.message}</p>
	{:else}
		{#each $projectsQueryStore.data.projects as p}
			<ProjectCard
				name={p.name}
				authors={p.authors}
				description={p.description}
				url={p.image[0].url}
				slug={p.slug}
				demo={p.demo}
				sourceCode={p.sourceCode}
				tags={p.tags}
			/>
		{/each}
	{/if}
</div>
