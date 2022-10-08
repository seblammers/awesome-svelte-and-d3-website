<script>
	import { marked } from 'marked';
	export let url = '';
	export let name = '';
	export let slug = '';
	export let index = ''; // TODO use index to sort by server-side order
	export let description = '';
</script>

<article class="card">
	<a sveltekit:prefetch href={`/projects/${slug}`}>
		<img src={url} alt={name} />
		<div class="hover">
			<h4 class="title">
				{name}
			</h4>
			<div class="description">
				<p>{@html marked(description.slice(0, 100))}</p>
			</div>
			<div class="pill">Show More</div>
		</div>
	</a>
</article>

<style lang="scss">
	.card {
		font-family: var(--accentFont);
		border-radius: var(--radius);
		border: var(--radius) solid var(--accent);
		padding: var(--space-m);
		background-color: var(--surface2-light);
		max-width: 32rem;
		max-height: 32rem;
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

			.pill {
				background-color: var(--accent);
				color: var(--surface2-light);
			}
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
</style>
