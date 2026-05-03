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

	const SITE_URL = 'https://whatdown.lebaux.co';
	const TITLE = 'Markdown to WhatsApp Converter — Free Online Tool';
	const DESCRIPTION =
		'Convert Markdown to WhatsApp formatting in one paste. Turn ChatGPT and Claude output into bold, italic, and lists that render correctly in WhatsApp chats. Free, no signup, runs in your browser.';

	const softwareJsonLd = {
		'@context': 'https://schema.org',
		'@type': 'SoftwareApplication',
		name: 'Whatdown — Markdown to WhatsApp Converter',
		url: SITE_URL,
		applicationCategory: 'UtilitiesApplication',
		operatingSystem: 'Web',
		description: DESCRIPTION,
		offers: { '@type': 'Offer', price: '0', priceCurrency: 'USD' },
		author: { '@type': 'Person', name: 'lebaux', url: 'https://lebaux.cl' }
	};

	const howToJsonLd = {
		'@context': 'https://schema.org',
		'@type': 'HowTo',
		name: 'How to convert Markdown to WhatsApp formatting',
		description:
			'Convert standard Markdown (from ChatGPT, Claude, or any LLM) into WhatsApp-compatible formatting in three steps.',
		totalTime: 'PT10S',
		step: [
			{
				'@type': 'HowToStep',
				position: 1,
				name: 'Paste Markdown',
				text: 'Paste your Markdown text into the left input box. ChatGPT, Claude, and other LLM outputs work directly.'
			},
			{
				'@type': 'HowToStep',
				position: 2,
				name: 'See live preview',
				text: 'The right side renders a WhatsApp message bubble preview as you type, so you see exactly how it will look in a chat.'
			},
			{
				'@type': 'HowToStep',
				position: 3,
				name: 'Copy to WhatsApp',
				text: 'Click Copy and paste the result into any WhatsApp chat. Bold, italic, strikethrough, code, and lists are preserved.'
			}
		]
	};

	const faqJsonLd = {
		'@context': 'https://schema.org',
		'@type': 'FAQPage',
		mainEntity: [
			{
				'@type': 'Question',
				name: 'Does WhatsApp support Markdown?',
				acceptedAnswer: {
					'@type': 'Answer',
					text: 'WhatsApp supports a small subset of Markdown-like formatting, but not standard CommonMark. It uses single asterisks for bold (*bold*), single underscores for italic (_italic_), single tildes for strikethrough (~strike~), and triple backticks for code. Headings, links with custom anchor text, and nested lists are not supported.'
				}
			},
			{
				'@type': 'Question',
				name: 'How do I make text bold in WhatsApp?',
				acceptedAnswer: {
					'@type': 'Answer',
					text: 'Wrap the text in single asterisks: *bold text*. WhatsApp renders this as bold in chats, status, and channels. Note this differs from standard Markdown, which uses double asterisks (**bold**).'
				}
			},
			{
				'@type': 'Question',
				name: 'How do I make text italic in WhatsApp?',
				acceptedAnswer: {
					'@type': 'Answer',
					text: 'Wrap the text in single underscores: _italic text_. Standard Markdown uses single asterisks (*italic*), which WhatsApp instead reads as bold, so you cannot paste raw Markdown italics directly.'
				}
			},
			{
				'@type': 'Question',
				name: 'How do I copy ChatGPT or Claude output to WhatsApp?',
				acceptedAnswer: {
					'@type': 'Answer',
					text: 'LLM outputs use standard Markdown, so pasting directly into WhatsApp leaves visible asterisks and broken formatting. Paste the output into a Markdown to WhatsApp converter first — it rewrites bold, italic, lists, and code blocks into the syntax WhatsApp understands.'
				}
			},
			{
				'@type': 'Question',
				name: 'Does this tool send my text to a server?',
				acceptedAnswer: {
					'@type': 'Answer',
					text: 'No. Conversion runs entirely in your browser. Nothing is sent to any server, logged, or stored. The page is open source on GitHub if you want to verify.'
				}
			},
			{
				'@type': 'Question',
				name: 'Why does pasting Markdown directly into WhatsApp look broken?',
				acceptedAnswer: {
					'@type': 'Answer',
					text: 'WhatsApp uses different delimiters than standard Markdown. Double asterisks for bold and bracketed link syntax simply render as literal characters. The fix is to convert Markdown into WhatsApp’s single-character delimiter format before pasting.'
				}
			}
		]
	};
</script>

<svelte:head>
	<title>{TITLE}</title>
	<meta name="description" content={DESCRIPTION} />
	<link rel="canonical" href={SITE_URL} />
	<meta name="robots" content="index,follow,max-image-preview:large" />

	<meta property="og:type" content="website" />
	<meta property="og:site_name" content="Whatdown" />
	<meta property="og:title" content={TITLE} />
	<meta property="og:description" content={DESCRIPTION} />
	<meta property="og:url" content={SITE_URL} />
	<meta property="og:image" content="{SITE_URL}/whatdown-thumbnail.jpg" />
	<meta property="og:image:width" content="1400" />
	<meta property="og:image:height" content="1050" />
	<meta property="og:image:alt" content="Whatdown — Markdown to WhatsApp converter" />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content={TITLE} />
	<meta name="twitter:description" content={DESCRIPTION} />
	<meta name="twitter:image" content="{SITE_URL}/whatdown-thumbnail.jpg" />

	{@html `<script type="application/ld+json">${JSON.stringify(softwareJsonLd)}</script>`}
	{@html `<script type="application/ld+json">${JSON.stringify(howToJsonLd)}</script>`}
	{@html `<script type="application/ld+json">${JSON.stringify(faqJsonLd)}</script>`}
</svelte:head>

<main class="mx-auto flex max-w-6xl flex-col gap-4 p-6">
	<header class="flex items-center gap-3">
		<img src="/whatdown.svg" alt="Whatdown logo" class="h-12 w-auto shrink-0" />
		<div>
			<h1 class="text-xl font-semibold leading-tight">Markdown to WhatsApp Converter</h1>
			<p class="text-sm leading-tight text-gray-500">
				Paste Markdown from ChatGPT, Claude, or any LLM. Copy WhatsApp-formatted text
				that renders correctly in chats — bold, italic, lists, and code, no broken
				asterisks.
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
				class="min-h-[300px] flex-1 resize-none overflow-hidden rounded border border-gray-300 p-3 font-mono text-xs focus:border-gray-500 focus:outline-none"
			></textarea>
		</label>

		<div class="flex flex-col gap-1">
			<div class="flex items-center justify-between pr-3">
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

	<!-- Lead intro — keyword-rich, plain language -->
	<section class="mt-12 flex flex-col gap-3 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">
			Convert Markdown to WhatsApp formatting in one paste
		</h2>
		<p class="text-base leading-relaxed">
			WhatsApp does not support standard Markdown. Pasting output from
			ChatGPT, Claude, or any other LLM directly into a chat leaves visible
			asterisks, broken bold, and unclickable links. This converter rewrites
			Markdown into the formatting WhatsApp actually understands — single
			asterisks for bold, single underscores for italic, tildes for
			strikethrough, and code blocks that survive the trip.
		</p>
		<p class="text-base leading-relaxed">
			It runs entirely in your browser. Nothing is uploaded, logged, or
			stored. Open source on
			<a
				href="https://github.com/Antoine-lb/md-to-ws"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">GitHub</a
			>.
		</p>
	</section>

	<!-- How to use — 3 steps -->
	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">
			How to convert Markdown to WhatsApp
		</h2>
		<ol class="flex list-decimal flex-col gap-3 pl-6 text-base leading-relaxed">
			<li>
				<strong>Paste your Markdown.</strong> Drop ChatGPT, Claude, or any
				LLM output into the input box on the left. The sample text shows the
				formats supported.
			</li>
			<li>
				<strong>Check the live preview.</strong> The right side renders a
				WhatsApp message bubble exactly as it will appear in a chat. Toggle
				<em>Show raw</em> to inspect the converted source.
			</li>
			<li>
				<strong>Copy and paste into WhatsApp.</strong> Click <em>Copy</em>,
				switch to WhatsApp, and paste. Bold, italic, strikethrough, inline
				code, code blocks, and bullet lists are preserved.
			</li>
		</ol>
	</section>

	<!-- WhatsApp formatting reference — table snippet bait -->
	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">
			WhatsApp formatting syntax reference
		</h2>
		<p class="text-base leading-relaxed">
			WhatsApp supports a small set of inline formatting marks. Standard
			Markdown uses different delimiters for most of these, which is why
			pasting raw Markdown produces broken output.
		</p>
		<div class="overflow-x-auto">
			<table class="w-full border-collapse text-sm">
				<thead>
					<tr class="border-b border-gray-300 text-left">
						<th class="py-2 pr-4 font-semibold text-gray-900">Format</th>
						<th class="py-2 pr-4 font-semibold text-gray-900">WhatsApp syntax</th>
						<th class="py-2 pr-4 font-semibold text-gray-900">Standard Markdown</th>
					</tr>
				</thead>
				<tbody class="divide-y divide-gray-200">
					<tr>
						<td class="py-2 pr-4">Bold</td>
						<td class="py-2 pr-4"><code class="wa-code">*bold*</code></td>
						<td class="py-2 pr-4"><code class="wa-code">**bold**</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Italic</td>
						<td class="py-2 pr-4"><code class="wa-code">_italic_</code></td>
						<td class="py-2 pr-4"><code class="wa-code">*italic*</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Strikethrough</td>
						<td class="py-2 pr-4"><code class="wa-code">~strike~</code></td>
						<td class="py-2 pr-4"><code class="wa-code">~~strike~~</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Inline code</td>
						<td class="py-2 pr-4"><code class="wa-code">`code`</code></td>
						<td class="py-2 pr-4"><code class="wa-code">`code`</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Code block</td>
						<td class="py-2 pr-4"><code class="wa-code">```block```</code></td>
						<td class="py-2 pr-4"><code class="wa-code">```block```</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Bullet list</td>
						<td class="py-2 pr-4">
							<code class="wa-code">- item</code> or
							<code class="wa-code">* item</code>
						</td>
						<td class="py-2 pr-4"><code class="wa-code">- item</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Headings</td>
						<td class="py-2 pr-4 text-gray-500">Not supported</td>
						<td class="py-2 pr-4"><code class="wa-code"># Heading</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">Anchor links</td>
						<td class="py-2 pr-4 text-gray-500">Not supported (URL only)</td>
						<td class="py-2 pr-4"><code class="wa-code">[text](url)</code></td>
					</tr>
				</tbody>
			</table>
		</div>
	</section>

	<!-- FAQ -->
	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">Frequently asked questions</h2>

		<div class="flex flex-col gap-5 text-base leading-relaxed">
			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					Does WhatsApp support Markdown?
				</h3>
				<p>
					WhatsApp supports a small subset of Markdown-like formatting, but
					not standard CommonMark. It uses single asterisks for bold, single
					underscores for italic, single tildes for strikethrough, and triple
					backticks for code. Headings, link anchor text, and nested lists
					are not supported.
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					How do I make text bold in WhatsApp?
				</h3>
				<p>
					Wrap the text in single asterisks:
					<code class="wa-code">*bold text*</code>. Standard Markdown uses
					double asterisks, which WhatsApp shows as literal characters.
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					How do I make text italic in WhatsApp?
				</h3>
				<p>
					Wrap the text in single underscores:
					<code class="wa-code">_italic text_</code>. Markdown italics use
					single asterisks (<code class="wa-code">*italic*</code>), which
					WhatsApp instead reads as bold — the cause of most paste mishaps.
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					How do I copy ChatGPT or Claude output to WhatsApp?
				</h3>
				<p>
					LLM output is standard Markdown, so pasting it directly leaves
					visible asterisks and broken formatting. Run it through this
					converter first — it rewrites bold, italic, lists, and code blocks
					into WhatsApp's syntax in one click.
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					Does this tool send my text to a server?
				</h3>
				<p>
					No. Conversion runs entirely in your browser. Nothing leaves the
					page. The source is on
					<a
						href="https://github.com/Antoine-lb/md-to-ws"
						target="_blank"
						rel="noopener noreferrer"
						class="text-[#027eb5] underline">GitHub</a
					>
					if you want to verify.
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					Why does pasting Markdown directly into WhatsApp look broken?
				</h3>
				<p>
					WhatsApp uses different delimiters than Markdown. Double asterisks
					and bracketed link syntax render as literal characters. Converting
					to WhatsApp's single-character delimiter format before pasting
					fixes the output.
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">
					Does the formatting work in WhatsApp Web, mobile, and desktop?
				</h3>
				<p>
					Yes. WhatsApp's formatting rules are consistent across iOS,
					Android, WhatsApp Web, and the WhatsApp desktop apps. A message
					formatted on one client renders the same on the others.
				</p>
			</div>
		</div>
	</section>

	<!-- Why doesn't WhatsApp use standard Markdown? — long-form prose, tightened -->
	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<header class="flex flex-col gap-1">
			<p class="text-xs font-medium tracking-wide text-gray-500 uppercase">
				Background
			</p>
			<h2 class="text-2xl font-semibold text-gray-900">
				Why doesn't WhatsApp use standard Markdown?
			</h2>
		</header>

		<div class="flex flex-col gap-4 text-base leading-relaxed">
			<p>
				WhatsApp has never publicly explained the choice. The likely reasons
				are a mix of mobile typing ergonomics, chat conventions that predate
				Markdown, and shipping constraints once the format was locked in.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">
				Mobile keyboards favour single delimiters
			</h3>
			<p>
				CommonMark uses <code class="wa-code">**bold**</code> and
				<code class="wa-code">*italic*</code>. On a phone, asterisks live
				behind a symbols toggle — four taps per bold word adds up. WhatsApp
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
				it maps cleanly to a short message. Adopting the full spec would
				mean importing a parser and a pile of edge cases to use four
				features.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">
				Chat had its own conventions first
			</h3>
			<p>
				IRC and early IM clients used <code class="wa-code">*bold*</code> and
				<code class="wa-code">_italic_</code> as typographic shorthand long
				before Markdown existed. Slack, Telegram, Discord and Signal each
				diverge from CommonMark in different ways. Chat apps never
				standardised on it as a category.
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">Lock-in came fast</h3>
			<p>
				Once formatting shipped to over a billion users in 2016, switching
				to <code class="wa-code">**bold**</code> would silently break every
				older message anyone scrolled back to.
			</p>
		</div>
	</section>

	<footer class="mt-16 flex flex-col gap-2 border-t border-gray-300/70 pt-6 text-sm text-gray-600">
		<p>
			This project is open source and made by
			<a
				href="https://lebaux.cl"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">lebaux.cl</a
			>. Source on
			<a
				href="https://github.com/Antoine-lb/md-to-ws"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">GitHub</a
			>.
		</p>
		<p>
			Contributions are more than welcome. Found a bug? The best way to flag
			it is by filing an
			<a
				href="https://github.com/Antoine-lb/md-to-ws/issues"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">issue on GitHub</a
			>.
		</p>
	</footer>
</main>
