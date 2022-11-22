<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';
	import Tags from 'svelte-tags-input';

	// see https://dev.to/tiim/sveltekit-server-side-rendering-ssr-with-urqlsvelte-534k
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

	// $: tags_all = [...new Set($projectsQueryStore.data.projects.map((item) => item.tags))].reduce(
	// 	(accumulator, currentValue) => accumulator.concat(currentValue),
	// 	[]
	// );

	// //$: console.log(tags_all);

	// let tags_all_lower = [];

	// $: {
	// 	tags_all.forEach((element) => {
	// 		tags_all_lower.push(element.toLowerCase());
	// 	});
	// 	// for (const tag of tags_all) {
	// 	// 	console.log(tag.toLowerCase());
	// 	// 	tags_all_lower.push(tag.toLowerCase());
	// 	// }
	// }

	// $: console.log(tags_all_lower);

	// $: tags_unique = [...new Set(tags_all_lower)];

	// $: console.log(tags_unique);

	// If on:tags is defined
	let taglist = '';

	function handleTags(event) {
		taglist = event.detail.tags;
	}

	let tags_unique = [
		'scrollytelling',
		'scatter',
		'line',
		'bar',
		'map',
		'bump',
		'sankey',
		'machineLearning',
		'statistics',
		'interactive',
		'food'
	];
</script>

<svelte:head>
	<title>Awesome Svelte & D3 | Projects</title>
</svelte:head>

<h2>Projects in the wild</h2>

<h4>Use tags to filter out specific projects:</h4>
<Tags
	on:tags={handleTags}
	placeholder={'Enter a tag...'}
	autoComplete={tags_unique}
	name={'selected-tags'}
	id={'tag-selector'}
/>

<div class="container grid">
	{#if $projectsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $projectsQueryStore.error}
		<p>Oopsie! {$projectsQueryStore.error.message}</p>
	{:else}
		{#each $projectsQueryStore.data.projects as p}
			{#key taglist}
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
			{/key}
		{/each}
	{/if}
</div>
