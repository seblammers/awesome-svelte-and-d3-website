<script>
	import { getContextClient, gql, queryStore } from '@urql/svelte';
	import ProjectCard from '$lib/components/Card.svelte';
	import Tags from 'svelte-tags-input';
	import Accordion from '$lib/components/Accordion.svelte';

	export let data;

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
		'webGL',
		'text',
		'video'
	];
</script>

<svelte:head>
	<title>Awesome Svelte & D3 | Learning</title>
</svelte:head>

<section class="flow">
	<h2>Learning resources</h2>

	<p>
		Check out these selected learning resources to start your journey into the world of <strong
			>interactive data visualization</strong
		> for the web with Svelte and D3.
	</p>
	<p>
		If you already have some experience, maybe you will find something to add to your tool belt?
		Ever created a map? A scrollytelling project? Dive into it below!
	</p>
</section>

<Accordion summary="Filter Tutorials {n_tags}">
	<h4>Use tags to filter out specific tutorials:</h4>
	<div class="instruction">
		<p>
			You can either start typing and use autocomplete or click on the tags within each Project-Card
			below.
		</p>
		<p>Use <em>text</em> and <em>video</em> tags to filter for written vs. video content.</p>
	</div>

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

{#if $tutorialsQueryStore.fetching}
	<p>Loading...</p>
{:else if $tutorialsQueryStore.error}
	<p>Oopsie! {$tutorialsQueryStore.error.message}</p>
{:else}
	<div class="u-grid">
		{#each $tutorialsQueryStore.data.tutorials as p}
			{#key taglist}
				<ProjectCard
					on:addTag={addTag}
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
			{/key}
		{/each}
	</div>
{/if}

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
		background-color: var(--awesome-red);
		padding: var(--space-2xs);
		border-radius: var(--radius);
		// border: 1px solid var(--text2-light);
		// color: var(--text2-light);
		// font-size: var(--step-0);
		line-height: 1;
	}

	.tag-wrapper :global(.svelte-tags-input-layout) {
		font-family: inherit;
	}
</style>
