<script>
	import Icon from '$lib/components/Icon/index.svelte';
	import { categories } from '$lib/stores/recipes';
	import { page } from '$app/stores';
	import Seo from '$lib/components/Seo.svelte';

	const childrenNodes = $categories.find((c) => c.path === $page.url.pathname).children || [];

	export let title;
</script>

<Seo {title} />

<main>
	<div class="TOC">
		<strong>Table of Contents</strong>
		<div class="TOCList">
			{#each $categories as node}
				<div class="TOCLink" class:active={$page.url.pathname.includes(node.path)}>
					<Icon name={node.icon} />
					<a href={node.path}>{node.title}</a>
				</div>
			{/each}
		</div>
	</div>
	<article>
		<h1>{title}</h1>
		<slot />

		<ul>
			{#each childrenNodes as node}
				<li>
					<a href={node.path}>{node.title}</a>
				</li>
			{/each}
		</ul>
	</article>
</main>

<style>
	article {
		display: grid;
		grid-template-columns: minmax(0, 1fr);
		align-content: flex-start;
	}

	strong {
		font-size: var(--font-500);
	}

	.TOCLink {
		display: flex;
		gap: var(--s-2);
		padding: 1rem 0;
		border-bottom: 1px solid var(--gray);
		font-size: 1.1em;
	}
	.TOCLink a {
		border-bottom: none;
		position: relative;
		top: 3px;
	}
	.TOCLink.active a {
		font-weight: bold;
	}
	.TOCList {
		padding-block: var(--s-4);
		padding-inline: var(--s-4) 0;
	}
	h1 {
		margin-top: 0;
	}
	@media (min-width: 1024px) {
		main {
			display: flex;
		}
	}
	main {
		margin: 0 auto;
		max-width: var(--width-content);
		padding: 2rem 1rem;
	}
	.TOC {
		margin-right: 2rem;
		flex: 1;
		font-family: Overpass;
		line-height: 150%;
	}
	.TOC :global(a) {
		color: #2e2e35;
		font-weight: normal;
	}
	article {
		flex: 3;
		overflow-x: hidden;
	}
	@media (min-width: 1024px) {
		article {
			margin-left: 2rem;
		}
	}
</style>
