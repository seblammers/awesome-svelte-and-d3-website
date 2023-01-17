<script>
	import { navItems } from '$lib/config';
	import { slide, fade, fly } from 'svelte/transition';
	import { beforeNavigate } from '$app/navigation';
	import { navigating, page } from '$app/stores';
	import { Hamburger } from 'svelte-hamburgers';
	import { mounted } from 'svelte-mount';

	let width;
	let scrollY;

	let open = false;

	$: mobile = width < 900;
	$: scroll = scrollY > 0;

	$: if (!mobile || $navigating) open = false;

	$: isActive = (url) => $page.url.pathname === url;

	beforeNavigate(() => {
		open = false;
	});
</script>

<svelte:window bind:innerWidth={width} bind:scrollY />

<div class="wrapper" class:open class:scroll>
	<nav in:fade>
		<div class="nav-fh">
			<a href="/">
				<div class="logo">
					<img src="/logo.png" alt="Svelte and D3 logos placed over a heart" />
				</div>
			</a>
			<div class="hamburger">
				<Hamburger --color="var(--text1-light)" type="vortex" bind:open />
			</div>
		</div>

		{#if $mounted && (open || !mobile)}
			<div class="nav-sh" transition:slide class:open={!$navigating && (open || !mobile)}>
				<div class="links">
					{#each navItems as page, i (i)}
						<a
							href={page.route}
							class:active={isActive(`${page.route}`)}
							in:fly={{
								y: -20,
								duration: 750,
								delay: (i + 1) * 100
							}}
						>
							{page.title}
						</a>
					{/each}
				</div>
			</div>
		{/if}
	</nav>
</div>

<style lang="scss">
	.wrapper {
		font-family: var(--accentFont);
		position: fixed;
		z-index: 300;
		top: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 100%;

		border-bottom: 4px solid rgba(0, 0, 0, 0);
		transition: border-color 0.2s ease-in-out;

		background-color: var(--surface0-light);

		&.scroll {
			box-shadow: 0 4px 10px -2px rgba(0, 0, 0, 0.2), 0 4px 20px 0 rgba(0, 0, 0, 0.19);
			border-color: var(--accent);
		}
		@media (min-width: 900px) {
			&.open {
				box-shadow: 0 4px 10px -2px rgba(0, 0, 0, 0.2), 0 4px 20px 0 rgba(0, 0, 0, 0.19);
				border-color: var(--accent);
			}
		}
	}
	@media (min-width: 900px) {
		.hamburger {
			display: none;
		}
	}
	nav {
		width: 100%;
		max-width: 1600px;
		margin: 0 auto;
		padding: var(--space-s);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		.nav-sh {
			display: contents;
		}
		.nav-fh {
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			align-items: center;
			width: 100%;
		}
		@media (min-width: 900px) {
			flex-direction: row;
			justify-content: start;
			.nav-fh {
				width: 100%;
				display: flex;
				align-items: start;
				justify-content: space-between;
			}
			.nav-sh {
				display: flex;
				flex-direction: column;
				gap: var(--space-m);
				height: 100%;
				.links {
					margin: 0px auto;
					flex-direction: row;
					gap: var(--space-s);
					a {
						font-size: var(--step-0);
						border-width: 2px;
						// border: none;
					}
				}
			}
		}
	}
	.links {
		display: flex;
		align-items: center;
		flex-direction: column;
		gap: var(--space-s);
		font-size: var(--step-2);
		a {
			// text-transform: capitalize;
			font-weight: 600;

			padding: var(--space-xs) var(--space-3xs);
			border-bottom: 4px solid rgba(0, 0, 0, 0);
			transition: border-color 0.2s ease-in-out, color 0.2s ease-in-out;
			color: var(--text1-light);
			text-decoration: none;
			&.active {
				border-color: var(--selected-item);
			}
			&:hover,
			&:focus {
				color: var(--text2-light);
				background-color: inherit;
				border-color: var(--text2-light);
			}
		}
	}
</style>
