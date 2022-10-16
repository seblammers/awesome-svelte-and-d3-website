<!-- This page handles any error encountered by the site. -->
<script>
	import { navItems } from '$lib/config';
	import { page } from '$app/stores';
</script>

<img src="/error.png" alt="a logo showing a broken heart, the logo of Svelte and the logo of D3." />
<h1>{$page.status}</h1>
<p class="subhead">{$page.error.message}</p>

<p><strong>Sorry!</strong> Maybe try one of these links?</p>
<ul>
	{#each navItems as page, i (i)}
		<li>
			<a href={page.route}>
				{page.title}
			</a>
		</li>
	{/each}
</ul>

<style>
	img {
		max-width: var(--space-3xl);
		margin-bottom: 0%;
	}

	h1 {
		margin: 0%;
	}
</style>
