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
					dropdownTags
					demo
					sourceCode
					featured
					image {
						url(transformation: { image: { resize: { width: 800, height: 800, fit: crop } } })
					}
				}
			}
		`
	});
	const tutorialsQueryStore = queryStore({
		client: getContextClient(),
		query: gql`
			{
				tutorials {
					name
					authors
					slug
					description
					dropdownTags
					demo
					type
					featured
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

<section>
	<h1>Welcome to Awesome Svelte & D3</h1>

	<h2>In the wild</h2>
	<p />
	<p>Here are our three featured projects:</p>

	{#if $projectsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $projectsQueryStore.error}
		<p>Oopsie! {$projectsQueryStore.error.message}</p>
	{:else}
		<div class="u-grid">
			{#each $projectsQueryStore.data.projects.filter((x) => x.featured) as p}
				<ProjectCard
					name={p.name}
					authors={p.authors}
					description={p.description}
					url={p.image[0].url}
					slug={p.slug}
					demo={p.demo}
					sourceCode={p.sourceCode}
					tags={p.dropdownTags}
					{taglist}
				/>
			{/each}
		</div>

		<a class="button-standard align-right" href="/projects"> Show all projects </a>
	{/if}
</section>

<br />
<br />

<section>
	<h2>Tutorials</h2>
	<p>Here are our three featured tutorials:</p>

	{#if $tutorialsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $tutorialsQueryStore.error}
		<p>Oopsie! {$tutorialsQueryStore.error.message}</p>
	{:else}
		<div class="u-grid">
			{#each $tutorialsQueryStore.data.tutorials.filter((x) => x.featured) as p}
				<ProjectCard
					name={p.name}
					authors={p.authors}
					description={p.description}
					url={p.image[0].url}
					slug={p.slug}
					demo={p.demo}
					type={p.type}
					tags={p.dropdownTags}
					{taglist}
				/>
			{/each}
		</div>

		<a class="button-standard align-right" href="/learning"> Show all tutorials </a>
	{/if}
</section>
