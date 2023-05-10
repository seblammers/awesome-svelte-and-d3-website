<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';
	import Tags from 'svelte-tags-input';
	import Accordion from '$lib/components/Accordion.svelte';

	const projectsQueryStore = queryStore({
		client: getContextClient(),
		query: gql`
			{
				projects(orderBy: lastCommit_DESC) {
					name
					authors
					lastCommit
					slug
					description
					dropdownTags
					demo
					sourceCode
					image {
						url(transformation: { image: { resize: { width: 800, height: 800, fit: crop } } })
					}
				}
			}
		`
	});

	//$: console.log($projectsQueryStore.data.projects);
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
		'machine learning',
		'scrollytelling',
		'scatter',
		'line',
		'bar',
		'map',
		'bump',
		'sankey',
		'statistics',
		'interactive',
		'food',
		'histogram',
		'webGL'
	];
</script>

<svelte:head>
	<title>Awesome Svelte & D3 | Projects</title>
</svelte:head>

<section class="flow">
	<h2>Projects in the wild</h2>

	<p>
		Dive into <strong>real world projects</strong> that use Svelte and D3 under the hood.
	</p>
	<p>Found a pattern or a component that fascinates you? Dig into the code to learn more!</p>
</section>

<Accordion summary="Filter Projects {n_tags}">
	<h4>Use tags to filter out specific projects:</h4>
	<p class="instruction">
		You can either start typing and use autocomplete or click on the tags within each Project-Card
		below.
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
					tags={p.dropdownTags}
					{taglist}
				/>
			{/key}
		{/each}
	</div>
{/if}

<style lang="scss">
	.instruction {
		font-size: var(--step-0);
	}

	// the following had to be added in this manner to override the styles of the
	// svelte-tags-input component (how to solve this?)
	.tag-wrapper :global(.svelte-tags-input) {
		font-family: inherit !important;
		font-size: var(--step-0) !important;
	}

	.tag-wrapper :global(.svelte-tags-input-matchs) {
		font-family: inherit !important;
		font-size: var(--step-0) !important;
	}
	.tag-wrapper :global(.svelte-tags-input-tag) {
		font-family: inherit !important;
		font-size: var(--step-0) !important;
		background-color: var(--awesome-red) !important;
		padding: var(--space-2xs) !important;
		border-radius: var(--radius) !important;
		// border: 1px solid var(--text2-light);
		// color: var(--text2-light);
		// font-size: var(--step-0);
		line-height: 1 !important;
	}

	.tag-wrapper :global(.svelte-tags-input-layout) {
		font-family: inherit !important;
	}
</style>
