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
	<header class="flex items-center gap-3">
		<img src="/whatdown.svg" alt="" class="h-12 w-auto shrink-0" />
		<div>
			<h1 class="text-xl font-semibold leading-tight">Markdown → WhatsApp</h1>
			<p class="text-sm leading-tight text-gray-500">
				Paste LLM markdown on the left, copy WhatsApp-formatted text on the right.
			</p>
		</div>
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

			<!-- WhatsApp outgoing message bubble -->
			<div class="relative flex min-h-0 flex-1 justify-end pt-1 pr-3">
				<div
					class="relative flex min-h-0 max-w-full flex-1 rounded-lg rounded-br-none bg-[#d9fdd3]"
				>
					<!-- bubble tail (extends 1px back into the bubble so there's no seam) -->
					<svg
						aria-hidden="true"
						viewBox="0 0 9 13"
						class="absolute -right-2 bottom-0 h-3 w-[9px] text-[#d9fdd3]"
					>
						<path d="M1 13 L9 13 L1 0 Z" fill="currentColor" />
					</svg>
					<textarea
						readonly
						value={output}
						placeholder="WhatsApp output…"
						class="min-h-0 flex-1 resize-none rounded-lg rounded-br-none border-none bg-transparent p-3 pb-5 font-mono text-sm text-gray-900 placeholder-gray-500/70 outline-none focus:border-none focus:ring-0 focus:outline-none"
					></textarea>
				</div>
			</div>
		</div>
	</div>
</main>
