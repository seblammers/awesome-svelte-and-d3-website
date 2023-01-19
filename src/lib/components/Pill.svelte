<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let tag = '';

	function addTag() {
		dispatch('addTag', {
			text: tag
		});
	}
</script>

<button class="pill tag" on:click={addTag} {tag}>
	{tag}
</button>

<style>
	button {
		background-color: inherit;
		margin: var(--space-3xs);
	}
</style>
