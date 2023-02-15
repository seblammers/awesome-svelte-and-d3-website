<script>
	import Pill from '$lib/components/Pill.svelte';
	import TagsSvg from '$lib/components/TagsSVG.svelte';
	import { marked } from 'marked';

	export let url = '';
	export let name = '';
	export let authors = '';
	export let slug = '';
	export let index = ''; // TODO use index to sort by server-side order
	export let alt = ''; // TODO add alt text for images
	export let description = '';
	export let demo = '';
	export let sourceCode = '';
	export let tags = '';
	export let taglist;

	tags.sort(); // sort alphabetically
	let show = true; // helper to toggle visibility

	// reactively toggle visibility
	// if all filter-tags match given project-tags
	$: {
		if (taglist) {
			show = taglist.every((r) => tags.includes(r));
		}
	}

	// TODO: add "&" before last author (and between 1 & 2 if only 2?)
	if (authors.length > 1) {
		authors = authors.join(', ');
	}
</script>

{#if show}
	<article class="card flow">
		<div class="title">
			<strong>{name}</strong>
		</div>
		<img src={url} alt={name} />
		<p class="authors">by {authors}</p>

		<p class="description">{@html marked(description)}</p>

		<div class="details grid flow">
			<a class="button" target="_blank" rel="noopener noreferrer" href={demo}>Live Site &nearr;</a>
			<a class="button" target="_blank" rel="noopener noreferrer" href={sourceCode}
				>Source Code &nearr;
			</a>
		</div>

		<div class="tags flow">
			{#each tags as tag}
				<Pill on:addTag {tag} {taglist} />
			{/each}
			<TagsSvg />
		</div>
	</article>
{/if}

<style lang="scss">
	img {
		box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
	}
	a:first-child {
		// make sure both buttons have the same margin
		margin-top: var(--space-xs);
	}
</style>
