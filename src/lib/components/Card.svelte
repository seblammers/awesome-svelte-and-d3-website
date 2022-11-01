<script>
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
	// TODO: open more info about each project in a modal via https://svelte.dev/repl/629f732d77fb48f79826b58b6ec4137f?version=3.37.0 ?

	// TODO: add "&" before last author (and between 1 & 2 if only 2?)
	if (authors.length > 1) {
		authors = authors.join(', ');
	}
</script>

<article class="card flow">
	<div class="title">
		<strong>{name}</strong>
	</div>
	<img src={url} alt={name} />
	<p class="authors">by {authors}</p>

	<p class="description">{@html marked(description)}</p>

	<div class="details flow">
		<a class="pill" target="_blank" rel="noopener noreferrer" href={demo}>Live Site &nearr;</a>
		<a class="pill" target="_blank" rel="noopener noreferrer" href={sourceCode}
			>Source Code &nearr;
		</a>
	</div>
</article>

<style lang="scss">
	.card {
		font-family: var(--accentFont);
		border-radius: var(--radius);
		border: var(--radius) solid var(--accent);
		padding: var(--space-m);
		background-color: var(--surface2-light);
		// max-width: 32rem;
		// max-height: 32rem;
		overflow: hidden;
		--flow-space: var(--space-xs);

		& > .authors {
			font-size: var(--step-0);
		}
		& > a {
			text-decoration: none;

			&:hover {
				color: inherit;
			}
		}

		&:hover {
			box-shadow: 0px 0px 20px var(--accent);
			cursor: pointer;
			max-height: fit-content;
		}

		.pill {
			color: var(--ink);
			padding: var(--space-m) var(--space-2xl);
			margin-inline: var(--space-m);
			width: min(60ch, 90%);
			text-align: center;
		}
	}

	.title {
		min-height: var(--space-xl);
	}
	.description {
		min-height: var(--space-xl);
		font-size: var(--step-0);
	}
	.details {
		display: grid;
		place-content: center;
		margin: var(--space-xs) 0;
		max-width: var(--max-width);
		background-color: var(--surface4-light);
		font-family: var(--accentFont);
		font-size: var(--step-0);

		--flow-space: var(--space-xs);
	}
</style>
