<script>
	import { marked } from 'marked';
	export let url = '';
	export let name = '';
	export let slug = '';
	export let index = ''; // TODO use index to sort by server-side order
	export let description = '';
</script>

<article class="card">
	<img src={url} alt={name} />
	<a sveltekit:prefetch href={`/projects/${slug}`}>
		<div class="hover">
			<h3 class="title">
				{name}
			</h3>
			<div class="description">
				<p>{@html marked(description.slice(0, 100))}</p>
				<div class="pill">Show More</div>
			</div>
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
		max-width: 42rem;

		& > a {
			text-decoration: none;

			&:hover {
				color: inherit;
			}
		}

		&:hover {
			box-shadow: 0px 0px 20px var(--accent);

			.pill {
				background-color: var(--accent);
				color: var(--surface2-light);
			}
		}
		&:hover h3 {
			transform: scale(1.01) translateY(-10%);
		}
		h3:hover {
			--_p: 0%;
		}

		.pill {
			color: var(--ink);
			// transition-property: color;
			// transition-duration: 0.5s;
		}
		.more {
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: space-between;
		}
	}
</style>
