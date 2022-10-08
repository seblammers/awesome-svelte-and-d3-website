<!-- This page handles any error encountered by the site. -->
<script>
	import { page } from '$app/stores';
</script>

<h1>{$page.status}</h1>
<p class="subhead">{$page.error.message}</p>

<p><strong>Sorry!</strong> Maybe try one of these links?</p>
<ul>
	<li><a href="/">Home</a></li>
</ul>
