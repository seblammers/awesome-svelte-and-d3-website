<script>
	let width;
	import Logo from '$lib/assets/svg/Logo.svelte';
	$: mobile = width < 900;
</script>

<svelte:window bind:innerWidth={width} />

<footer class:mobile>
	<p>
		&copy; 2023-{new Date().getFullYear()} Design & Development
	</p>
	<p class="no-top">
		<a href="https://www.sebastianlammers.com/" target="_blank" rel="noopener noreferrer"
			>Sebastian Lammers</a
		>
	</p>
	<Logo
		fillHeart="var(--text1-dark)"
		strokeHeart="var(--surface3-light)"
		fill="var(--text1-dark)"
		stroke="var(--surface3-light)"
	/>
	<p
		class="scroll-to-top"
		on:click={() => {
			window.scrollTo({
				top: 0,
				left: 0,
				behavior: 'smooth'
			});
		}}
	>
		Scroll to top &uarr;
	</p>
</footer>

<style lang="scss">
	.no-top {
		margin-top: 0;
	}
	footer {
		font-family: var(--accentFont);
		font-weight: 300;
		background: var(--surface1-dark);
		color: var(--text1-dark);
		display: flex;
		flex-direction: column;
		flex-wrap: wrap;
		justify-content: space-around;
		align-items: center;
		padding: var(--space-m) var(--space-m);
		width: 100%;

		& > * {
			flex: 0 1 auto;
		}
		p {
			margin: var(--space-s);
			font-size: var(--step-0);
		}
	}

	.scroll-to-top {
		cursor: pointer;
	}
	.mobile {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-around;
		& > * {
			margin: var(--space-xs);
		}
		& > .no-top {
			margin-top: 0;
		}
	}
</style>
