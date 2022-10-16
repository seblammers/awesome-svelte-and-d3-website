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
	export let summary = 'Tap to show details';
	// TODO: open more info about each project in a modal via https://svelte.dev/repl/629f732d77fb48f79826b58b6ec4137f?version=3.37.0 ?

	// TODO: add "&" before last author (and between 1 & 2 if only 2?)
	if (authors.length > 1) {
		authors = authors.join(', ');
	}
</script>

<article class="card">
	<!-- <a sveltekit:prefetch href={`/projects/${slug}`}> -->
	<strong class="title">
		{name}
	</strong>
	<img src={url} alt={name} />
	<p class="authors">by {authors}</p>

	<!-- </a> -->
	<details class="accordion">
		<summary>
			<b>{summary}</b>
		</summary>

		<div class="details">
			<p>{@html marked(description)}</p>
			<a class="pill" target="_blank" rel="noopener noreferrer" href={demo}>Live Site &nearr;</a>
			<a class="pill" target="_blank" rel="noopener noreferrer" href={sourceCode}
				>Source Code &nearr;</a
			>
		</div>
	</details>
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
		&:hover h4 {
			transform: scale(1.01) translateY(-10%);
		}

		.pill {
			color: var(--ink);
			// transition-property: color;
			// transition-duration: 0.5s;
		}
	}

	details {
		margin: var(--space-xs) 0;
		max-width: var(--max-width);
		background-color: var(--surface4-light);
		font-family: var(--accentFont);
		font-size: var(--step-0);

		& > div > a {
			margin-top: var(--space-xs);
		}
	}

	/* # The Rotating Marker # */
	/* hattip https://codepen.io/redesigned/pen/wvoEvqG */
	summary {
		display: block;
		position: relative;
		cursor: pointer;
		padding: 1rem 2rem;
		padding-left: 2.9rem;
		color: lightgrey;

		&:hover {
			color: inherit;
		}
	}

	/* # The Rotating Marker # */
	details summary::-webkit-details-marker {
		display: none;
	}

	summary::before {
		content: '▶';
		position: absolute;
		top: 1rem;
		left: 0.8rem;
		transform: rotate(0);
		transform-origin: center;
		transition: 0.25s transform ease;
	}

	details[open] > summary:before {
		transform: rotate(90deg);
		transition: 0.25s transform ease;
	}
</style>
