<script lang="ts">
	import './layout.css';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" type="image/png" href="/whatdown.png" />
	<link rel="icon" type="image/svg+xml" href="/whatdown.svg" />
</svelte:head>
{@render children()}
