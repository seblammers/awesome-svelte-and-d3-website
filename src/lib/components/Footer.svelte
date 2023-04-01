<script>
	let width;
	$: mobile = width < 900;
</script>

<svelte:window bind:innerWidth={width} />

<footer class:mobile>
	<p>
		&copy; {new Date().getFullYear()} Design & Code
		<a href="https://www.sebastianlammers.com/" target="_blank" rel="noopener noreferrer"
			>Sebastian Lammers</a
		>
	</p>
	<img src="/favicon.svg" alt="Svelte and D3 logos placed over a heart" />
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
	footer {
		font-family: var(--accentFont);
		font-weight: 300;
		background: var(--surface1-dark);
		color: var(--text1-dark);
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
		align-items: center;
		padding: var(--space-m) var(--space-m);
		width: 100%;

		& > * {
			flex: 0 1 auto;
		}
		p {
			margin: 0;
			font-size: var(--step-0);
		}
	}

	.scroll-to-top {
		cursor: pointer;
	}

	img {
		max-width: var(--space-l);
		padding: 0;
	}
	.mobile {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-around;
		& > * {
			margin: var(--space-xs);
		}
	}
</style>
