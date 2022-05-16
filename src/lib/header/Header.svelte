<script lang="ts">
	import { page } from '$app/stores';
	import logo from './header_logo.svg';
</script>

<header class="flex w-full bg-white text-stone-800">
	<!-- <div class="absolute top-2 left-0">
		<div class="space-y-2 rounded">
			<span class="block h-1 w-4 animate-pulse bg-black" />
			<span class="block h-1 w-4 animate-pulse bg-black" />
			<span class="block h-1 w-4 animate-pulse bg-black" />
		</div>
	</div> -->
	<div class="h-10 w-full sm:h-auto sm:transition-all">
		<img class="m-auto h-16 bg-contain p-4" src={logo} alt="Canada Goose" />
		<nav class="scale-0 sm:scale-100 sm:transition-all">
			<ul
				class="flex-column relative flex h-10 items-center justify-center bg-contain font-semibold"
			>
				<li class:active={$page.url.pathname === '/'}>
					<div class="p-12">
						<a sveltekit:prefetch href="/" class="hover:underline hover:underline-offset-4">LIVE</a>
					</div>
				</li>
				<li class:active={$page.url.pathname === '/about'}>
					<div class="p-12">
						<a sveltekit:prefetch href="/about" class="hover:underline hover:underline-offset-4"
							>ANALYSIS</a
						>
					</div>
				</li>
				<li class:active={$page.url.pathname === '/about'}>
					<div class="p-12">
						<a sveltekit:prefetch href="/about" class="hover:underline hover:underline-offset-4"
							>ABOUT</a
						>
					</div>
				</li>
				<li class:active={$page.url.pathname === '/about'}>
					<div class="p-12">
						<a sveltekit:prefetch href="/about" class="hover:underline hover:underline-offset-4"
							>TODO</a
						>
					</div>
				</li>
			</ul>
		</nav>
	</div>
	<!-- 	
	ul {
		position: relative;
		padding: 0;
		margin: 0;
		height: 3em;
		display: flex;
		justify-content: center;
		align-items: center;
		list-style: none;
		background: var(--background);
		background-size: contain;
	}

	li {
		position: relative;
		height: 100%;
	} -->

	<!-- <div class="fixed top-0 left-0 flex w-full justify-center bg-white">
		<img class="p-2" src={logo} alt="SvelteKit" />
	</div> -->
</header>
<!-- <div class="corner">
		<a href="https://kit.svelte.dev">
			<img src={logo} alt="SvelteKit" />
		</a>
	</div>

	<nav>
		<svg viewBox="0 0 2 3" aria-hidden="true">
			<path d="M0,0 L1,2 C1.5,3 1.5,3 2,3 L2,0 Z" />
		</svg>
		<ul>
			<li class:active={$page.url.pathname === '/'}><a sveltekit:prefetch href="/">Home</a></li>
			<li class:active={$page.url.pathname === '/about'}>
				<a sveltekit:prefetch href="/about">About</a>
			</li>
			<li class:active={$page.url.pathname === '/todos'}>
				<a sveltekit:prefetch href="/todos">Todos</a>
			</li>
		</ul>
		<svg viewBox="0 0 2 3" aria-hidden="true">
			<path d="M0,0 L0,3 C0.5,3 0.5,3 1,2 L2,0 Z" />
		</svg>
	</nav>

	<div class="corner">
		Logo here
	</div>
</header>

<style>
	header {
		display: flex;
		justify-content: space-between;
	}

	.corner {
		width: 3em;
		height: 3em;
	}

	.corner a {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}

	.corner img {
		width: 2em;
		height: 2em;
		object-fit: contain;
	}

	nav {
		display: flex;
		justify-content: center;
		--background: rgba(255, 255, 255, 0.7);
	}

	svg {
		width: 2em;
		height: 3em;
		display: block;
	}

	path {
		fill: var(--background);
	}

	ul {
		position: relative;
		padding: 0;
		margin: 0;
		height: 3em;
		display: flex;
		justify-content: center;
		align-items: center;
		list-style: none;
		background: var(--background);
		background-size: contain;
	}

	li {
		position: relative;
		height: 100%;
	}

	li.active::before {
		--size: 6px;
		content: '';
		width: 0;
		height: 0;
		position: absolute;
		top: 0;
		left: calc(50% - var(--size));
		border: var(--size) solid transparent;
		border-top: var(--size) solid var(--accent-color);
	}

	nav a {
		display: flex;
		height: 100%;
		align-items: center;
		padding: 0 1em;
		color: var(--heading-color);
		font-weight: 700;
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		text-decoration: none;
		transition: color 0.2s linear;
	}

	a:hover {
		color: var(--accent-color);
	}
</style> -->
