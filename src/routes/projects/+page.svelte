<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';
	import Tags from 'svelte-tags-input';
	import Accordion from '$lib/components/Accordion.svelte';

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

	// start with empty taglist
	// this is where tags will be added
	let taglist = [];

	// keep track of number of tags to display in filter box
	// results in "(0 tags active)" or "(1 tag active)" or "(23 tags active)" etc.
	$: n_tags = `(${taglist.length} ${taglist.length === 1 ? 'tag' : 'tags'} active)`;

	// this will add a tag to the filter-list when clicked on in the cards
	function addTag(event) {
		var newTag = event.detail.text;
		// but add the tag only if it is not already included
		if (!taglist.includes(newTag)) {
			taglist.push(newTag);
			taglist = taglist;
			// if the tag is already there, remove it
		} else if (taglist.includes(newTag)) {
			taglist = taglist.filter((item) => item !== newTag);
		}
	}

	// this is the native function provided by svelte-tags-input
	function handleTags(event) {
		taglist = event.detail.tags;
	}

	// WIP: a list of all (?) tags
	// used for autocomplete!
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

<Accordion summary="Filter Projects {n_tags}">
	<h4>Use tags to filter out specific projects:</h4>
	<p class="instruction">
		You can either start typing and use autocomplete or click on the tags within each Project-Card.
	</p>

	<div class="tag-wrapper">
		<Tags
			on:tags={handleTags}
			placeholder={'Enter a tag...'}
			autoComplete={tags_unique}
			onlyAutocomplete={true}
			onlyUnique={true}
			name={'selected-tags'}
			id={'tag-selector'}
			tags={taglist}
		/>
	</div>
</Accordion>

<section class="u-container">
	{#if $projectsQueryStore.fetching}
		<p>Loading...</p>
	{:else if $projectsQueryStore.error}
		<p>Oopsie! {$projectsQueryStore.error.message}</p>
	{:else}
		<div class="u-grid">
			{#each $projectsQueryStore.data.projects as p}
				{#key taglist}
					<ProjectCard
						on:addTag={addTag}
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
		</div>
	{/if}
</section>

<style lang="scss">
	.instruction {
		font-size: var(--step-0);
	}

	// the following had to be added in this manner to override the styles of the
	// svelte-tags-input component
	.tag-wrapper :global(.svelte-tags-input) {
		font-family: inherit;
		font-size: var(--step-0);
	}

	.tag-wrapper :global(.svelte-tags-input-matchs) {
		font-family: inherit;
		font-size: var(--step-0);
	}
	.tag-wrapper :global(.svelte-tags-input-tag) {
		font-family: inherit;
		font-size: var(--step-0);
	}

	.tag-wrapper :global(.svelte-tags-input-layout) {
		font-family: inherit;
	}
</style>
