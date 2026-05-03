<script lang="ts">
	import { mdToWhatsApp, whatsappToHtml } from '$lib/whatsapp';

	const SAMPLE = `# Trip recap

Hey! Quick rundown of the **weekend in Lisbon** — three highlights:

- Pastéis de nata at *Manteigaria* (worth the queue)
- Sunset at Miradouro da Senhora do Monte
- ~~Tried~~ Survived the tram 28 ride

Booking link: [Airbnb](https://airbnb.com/rooms/123) — or just https://maps.app.goo.gl/example

Useful snippet I shared with the group:

\`\`\`
flight: TP223
landing: 19:45
hotel: check-in after 16:00
\`\`\`

Ping me on \`whatsapp\` if anything changes.`;

	let input = $state(SAMPLE);
	let output = $derived(mdToWhatsApp(input));
	let outputHtml = $derived(whatsappToHtml(output));
	let copied = $state(false);
	let showRaw = $state(false);
	let inputEl = $state<HTMLTextAreaElement | null>(null);

	// Auto-grow the input textarea to fit its content (above a baseline min-height
	// driven by CSS, so empty state still feels roomy).
	$effect(() => {
		// re-run when input changes
		void input;
		const el = inputEl;
		if (!el) return;
		el.style.height = 'auto';
		el.style.height = el.scrollHeight + 'px';
	});

	async function copy() {
		await navigator.clipboard.writeText(output);
		copied = true;
		setTimeout(() => (copied = false), 1200);
	}
</script>

<svelte:head><title>ws-md · Markdown → WhatsApp</title></svelte:head>

<main class="mx-auto flex max-w-6xl flex-col gap-4 p-6">
	<header class="flex items-center gap-3">
		<img src="/whatdown.svg" alt="" class="h-12 w-auto shrink-0" />
		<div>
			<h1 class="text-xl font-semibold leading-tight">Markdown → WhatsApp</h1>
			<p class="text-sm leading-tight text-gray-500">
				Paste LLM markdown on the left, copy WhatsApp-formatted text on the right.
			</p>
		</div>
	</header>

	<div class="grid grid-cols-1 items-stretch gap-4 md:grid-cols-2">
		<label class="flex flex-col gap-1">
			<span class="text-sm font-medium text-gray-700">Input (Markdown)</span>
			<textarea
				bind:this={inputEl}
				bind:value={input}
				placeholder="Paste markdown here…"
				class="min-h-[300px] flex-1 resize-none overflow-hidden rounded border border-gray-300 p-3 font-mono text-sm focus:border-gray-500 focus:outline-none"
			></textarea>
		</label>

		<div class="flex flex-col gap-1">
			<div class="flex items-center justify-between">
				<span class="text-sm font-medium text-gray-700">Output (WhatsApp)</span>
				<div class="flex items-center gap-2">
					<button
						type="button"
						onclick={() => (showRaw = !showRaw)}
						aria-pressed={showRaw}
						class="rounded border border-gray-300 px-3 py-1 text-xs font-medium text-gray-700 hover:bg-gray-100"
					>
						{showRaw ? 'Show rendered' : 'Show raw'}
					</button>
					<button
						type="button"
						onclick={copy}
						disabled={!output}
						class="rounded bg-gray-900 px-3 py-1 text-xs font-medium text-white disabled:opacity-40"
					>
						{copied ? 'Copied!' : 'Copy'}
					</button>
				</div>
			</div>

			<!-- WhatsApp outgoing message bubble (rendered preview) -->
			<div class="relative flex min-h-[300px] flex-1 justify-end pr-3">
				<div
					class="relative flex max-w-full flex-1 rounded-lg rounded-br-none bg-[#d9fdd3]"
				>
					<!-- bubble tail (extends 1px back into the bubble so there's no seam) -->
					<svg
						aria-hidden="true"
						viewBox="0 0 9 13"
						class="absolute -right-2 bottom-0 h-3 w-[9px] text-[#d9fdd3]"
					>
						<path d="M1 13 L9 13 L1 0 Z" fill="currentColor" />
					</svg>
					{#if output}
						{#if showRaw}
							<pre
								class="min-w-0 flex-1 p-3 font-mono text-xs whitespace-pre-wrap text-gray-900">{output}</pre>
						{:else}
							<div
								class="wa-bubble min-w-0 flex-1 p-3 text-sm leading-snug text-gray-900"
							>
								{@html outputHtml}
							</div>
						{/if}
					{:else}
						<div class="flex-1 p-3 text-sm text-gray-500/70">WhatsApp preview…</div>
					{/if}
				</div>
			</div>
		</div>
	</div>

	<!-- Why doesn't WhatsApp use standard Markdown? — long-form prose -->
	<section class="mt-16 flex flex-col gap-4 text-gray-800">
		<header class="flex flex-col gap-1">
			<p class="text-xs font-medium tracking-wide text-gray-500 uppercase">
				Speculation — not an official answer
			</p>
			<h2 class="text-2xl font-semibold text-gray-900">
				Why doesn't WhatsApp just use standard Markdown?
			</h2>
		</header>

		<div class="flex flex-col gap-4 text-base leading-relaxed">
			<p>
				WhatsApp has never publicly explained the choice, and developers who
				have looked for one keep coming up empty. What follows is informed
				guesswork, not fact.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">
				Mobile keyboards favour single delimiters
			</h3>
			<p>
				CommonMark uses <code class="wa-code">**bold**</code> and
				<code class="wa-code">*italic*</code>. On a phone, asterisks live
				behind a symbols toggle — four taps per bold word is a lot. WhatsApp
				picked <code class="wa-code">*bold*</code>,
				<code class="wa-code">_italic_</code>,
				<code class="wa-code">~strike~</code>: one character per side, no
				overlap between marks.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">
				Markdown was built for documents, not chat bubbles
			</h3>
			<p>
				Headings, paragraphs, link references, nested lists — almost none of
				it maps cleanly to a short message. Adopting the full spec would mean
				importing a parser and a pile of edge cases to use roughly four
				features.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">
				Chat had its own conventions first
			</h3>
			<p>
				IRC and early IM clients used <code class="wa-code">*bold*</code> and
				<code class="wa-code">_italic_</code> as typographic shorthand long
				before Markdown existed. Slack, Telegram, Discord and Signal all
				diverge from CommonMark in different ways — chat apps as a category
				never standardised on it.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">
				Lock-in came fast
			</h3>
			<p>
				Once shipped to a billion users in 2016, switching to
				<code class="wa-code">**bold**</code> would have silently broken every
				older message anyone scrolled back to.
			</p>

			<p class="mt-4 border-t border-gray-300/70 pt-4 text-gray-600">
				Best honest summary: a mix of thumb-typing ergonomics, inheritance
				from IRC/IM rather than from Markdown, and not wanting a
				document-oriented spec for a four-feature use case. Anyone claiming a
				definitive reason is guessing — including us.
			</p>
		</div>
	</section>
</main>
