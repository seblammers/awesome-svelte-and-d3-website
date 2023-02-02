<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';

	export let data;
	let taglist = [];

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
<p>Here are the three most recent projects:</p>

<section class="u-container">
	{#if $projectsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $projectsQueryStore.error}
		<p>Oopsie! {$projectsQueryStore.error.message}</p>
	{:else}
		<div class="u-grid">
			{#each $projectsQueryStore.data.projects.slice(0, 3) as p}
				<ProjectCard
					name={p.name}
					authors={p.authors}
					description={p.description}
					url={p.image[0].url}
					slug={p.slug}
					demo={p.demo}
					sourceCode={p.sourceCode}
					tags={p.tags}
					{taglist}
				/>
			{/each}
		</div>

		<a class="button" href="/projects">Show all projects</a>
	{/if}
</section>

<h2>Tutorials</h2>
<p>Here are the three most recent tutorials:</p>

<style>
	.button {
		position: relative;
		display: block;
		text-align: center;
		margin: var(--space-s) var(--space-l-xl);
	}
</style>
