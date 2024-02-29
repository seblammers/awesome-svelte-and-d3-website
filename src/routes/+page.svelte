<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';

	export let data;
	let taglist = [];

	let showTags = false;
	const projectsQueryStore = queryStore({
		client: getContextClient(),
		query: gql`
			{
				projects(orderBy: lastCommit_DESC) {
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
	<a rel="me" href="https://vis.social/@seblammers" />
</svelte:head>

<section>
	<h1>Welcome to Awesome Svelte & D3</h1>
	<h3>What's this?</h3>
	<p>
		Here you will find awesome data visualization projects that were created with <a
			href="https://svelte.dev/">Svelte</a
		>
		& <a href="https://d3js.org/">D3</a>.
	</p>
	<p>
		Check out the <a href="/projects">Projects</a> and dive into the code to learn how it works.
	</p>

	<h3>Start learning!</h3>
	<p>
		If you want to learn more about how to create such projects with Svelte and D3, check out the
		selected <a href="/learning">Learning resources</a>.
	</p>

	<h2>Projects in the wild</h2>

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
					{showTags}
				/>
			{/each}
		</div>

		<a class="button-standard align-right" href="/projects"> Show all projects </a>
	{/if}
</section>

<br />
<br />

<section>
	<h2>Tutorials (Text or Video)</h2>

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
					{showTags}
				/>
			{/each}
		</div>

		<a class="button-standard align-right" href="/learning"> Show all tutorials </a>
	{/if}
</section>
