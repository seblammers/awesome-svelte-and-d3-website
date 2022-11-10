<footer>
	<p>Made with &#10084; in Germany and Brazil</p>
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
		align-items: baseline;
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
</style>
