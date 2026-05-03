<script lang="ts">
	import type { Pathname } from '$app/types';
	import { resolve } from '$app/paths';
	import { page } from '$app/state';
	import { locales, localizeHref } from '$lib/paraglide/runtime';
	import './layout.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" type="image/png" href="/whatdown.png" />

	<link rel="icon" type="image/svg+xml" href="/whatdown.svg" />
</svelte:head>

{@render children()}

<div style="display:none">
	{#each locales as locale (locale)}
		<a href={resolve(localizeHref(page.url.pathname, { locale }) as Pathname)}>{locale}</a>
	{/each}
</div>
