<script>
	import Pill from '$lib/components/Pill.svelte';
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
				<Pill on:addTag {tag} />
			{/each}
		</div>
	</article>
{/if}

<style lang="scss">
	img {
		box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
	}
	.card {
		--radius-card: 0.1rem;
		font-family: var(--accentFont);
		border-radius: var(--radius-card);
		border: var(--radius-card) solid var(--accent);
		padding: var(--space-m);
		background-color: var(--surface0-light);

		--flow-space: var(--space-xs);

		& > .authors {
			font-size: var(--step-0);
		}

		&:hover {
			box-shadow: 0px 0px 20px var(--accent);
		}

		.tags {
			--flowspace: var(--space-xl);
			margin-top: var(--space-m);
			font-size: var(--step-0);
			color: var(--text2-light);

			.tag {
				margin-inline: var(--space-3xs);
				font-size: var(--step-0);
			}
		}
	}

	.title {
		min-height: var(--space-xl);
	}

	.description {
		min-height: var(--space-xl);
		font-size: var(--step-0);
		color: var(--text2-light);
	}
	.details {
		margin: var(--space-xs) 0;
		max-width: var(--max-width);
		font-family: var(--accentFont);
		font-size: var(--step-0);

		--flow-space: var(--space-xs);

		// allow less space for these buttons
		--min: 20ch;
	}

	a:first-child {
		// make sure both buttons have the same margin
		margin-top: var(--space-xs);
	}
</style>
