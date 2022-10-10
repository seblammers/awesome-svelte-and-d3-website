<script>
	import { marked } from 'marked';
	export let url = '';
	export let name = '';
	export let slug = '';
	export let index = ''; // TODO use index to sort by server-side order
	export let alt = ''; // TODO add alt text for images
	export let description = '';
	export let demo = '';
	export let sourceCode = '';
	export let summary = 'Tap to show details';
</script>

<article class="card">
	<a sveltekit:prefetch href={`/projects/${slug}`}>
		<img src={url} alt={name} />

		<h4 class="title">
			{name}
		</h4>
	</a>
	<details class="accordion">
		<summary>
			<b>{summary}</b>
		</summary>

		<div class="details">
			<p>{@html marked(description.slice(0, 100))}</p>
			<a class="pill" href={demo}>Live Site</a>
			<a class="pill" href={sourceCode}>Source Code</a>
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

		& .hover .description {
			color: var(--text2-light);
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
