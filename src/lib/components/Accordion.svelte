<script>
	// what is displayed initially next to the triangle symbol
	export let summary = 'Toggle me';
</script>

<details class="accordion">
	<summary>
		<b>{summary}</b>
	</summary>

	<div class="details flow">
		<slot />
	</div>
</details>

<style>
	details {
		--flow-space: var(--space-s);
		border-radius: var(--radius);
		border: 0.1rem solid var(--accent);
		padding: var(--space-s);
		margin: var(--space-xs) 0;
		background-color: var(--surface0-light);
		font-family: var(--accentFont);
	}
	/* # The Rotating Marker # */
	/* h/t https://codepen.io/redesigned/pen/wvoEvqG */
	summary {
		display: block;
		position: relative;
		cursor: pointer;
		padding: 1rem 2rem;
		padding-left: 2.9rem;
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
