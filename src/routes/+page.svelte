<script lang="ts">
	import { mdToWhatsApp } from '$lib/whatsapp';

	let input = $state('');
	let output = $derived(mdToWhatsApp(input));
	let copied = $state(false);

	async function copy() {
		await navigator.clipboard.writeText(output);
		copied = true;
		setTimeout(() => (copied = false), 1200);
	}
</script>

<svelte:head><title>ws-md · Markdown → WhatsApp</title></svelte:head>

<main class="mx-auto flex h-screen max-w-6xl flex-col gap-4 p-6">
	<header>
		<h1 class="text-xl font-semibold">Markdown → WhatsApp</h1>
		<p class="text-sm text-gray-500">
			Paste LLM markdown on the left, copy WhatsApp-formatted text on the right.
		</p>
	</header>

	<div class="grid min-h-0 flex-1 grid-cols-1 gap-4 md:grid-cols-2">
		<label class="flex min-h-0 flex-col gap-1">
			<span class="text-sm font-medium text-gray-700">Input (Markdown)</span>
			<textarea
				bind:value={input}
				placeholder="Paste markdown here…"
				class="min-h-0 flex-1 resize-none rounded border border-gray-300 p-3 font-mono text-sm focus:border-gray-500 focus:outline-none"
			></textarea>
		</label>

		<div class="flex min-h-0 flex-col gap-1">
			<div class="flex items-center justify-between">
				<span class="text-sm font-medium text-gray-700">Output (WhatsApp)</span>
				<button
					type="button"
					onclick={copy}
					disabled={!output}
					class="rounded bg-gray-900 px-3 py-1 text-xs font-medium text-white disabled:opacity-40"
				>
					{copied ? 'Copied!' : 'Copy'}
				</button>
			</div>
			<textarea
				readonly
				value={output}
				placeholder="WhatsApp output…"
				class="min-h-0 flex-1 resize-none rounded border border-gray-300 bg-gray-50 p-3 font-mono text-sm"
			></textarea>
		</div>
	</div>
</main>
