<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let tag = '';
	export let taglist;
	let active = false; // helper to toggle style

	// reactively toggle active-styling
	// if a given project-tag is included in the taglist
	$: {
		active = taglist.some((r) => tag.includes(r));
	}

	function addTag() {
		dispatch('addTag', {
			text: tag
		});
	}
</script>

<button class:active class="pill tag" on:click={addTag} {tag}>
	{#if active}
		<span>{tag}&#215;</span>
	{:else}
		<span>{tag}</span>
	{/if}
</button>

<style lang="scss">
	.tag {
		position: relative;
		background-color: var(--surface2-light);
		border-color: var(--surface2-light);
		color: var(--surface0-light);
		font-weight: 400;

		&:hover {
			background-color: var(--surface1-dark);
		}
	}
	.active {
		background-color: var(--awesome-red);
		border-color: var(--awesome-red);
		color: var(--text1-dark);

		&:hover {
			background-color: var(--surface2-dark);
			border-color: var(--surface2-dark);
		}
	}
	button {
		margin: var(--space-3xs);
		z-index: 1;
	}
</style>
