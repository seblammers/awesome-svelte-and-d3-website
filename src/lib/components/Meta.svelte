<script>
	import { Head } from 'svead';
	import image from '$lib/assets/images/og-image.png';

	let title = 'Awesome Svelte & D3';
	let description =
		'Awesome Svelte & D3 is a website to learn about great data visualization projects and underlying techniques.';
	let url = 'https://awesome-svelte-and-d3.netlify.app';
	let authorName = 'Sebastian Lammers et al.';
	let website = url;
</script>

<Head {title} {description} {image} {url} {authorName} {website} />
