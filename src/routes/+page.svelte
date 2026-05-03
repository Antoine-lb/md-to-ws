<script lang="ts">
	import { mdToWhatsApp, whatsappToHtml } from '$lib/whatsapp';
	import { m } from '$lib/paraglide/messages.js';
	import type { Locale } from '$lib/paraglide/runtime';
	import { getLocale, locales, localizeUrl } from '$lib/paraglide/runtime';

	let input = $state<string>(m.sample());
	let isSample = $state(true);
	let output = $derived(mdToWhatsApp(input));
	let outputHtml = $derived(whatsappToHtml(output));
	let copied = $state(false);
	let pasted = $state(false);
	let showRaw = $state(false);
	let inputEl = $state<HTMLTextAreaElement | null>(null);

	$effect(() => {
		void input;
		const el = inputEl;
		if (!el) return;
		el.style.height = 'auto';
		el.style.height = el.scrollHeight + 'px';
	});

	function clearSampleOnFirstEdit() {
		if (isSample) {
			input = '';
			isSample = false;
		}
	}

	async function copy() {
		await navigator.clipboard.writeText(output);
		copied = true;
		setTimeout(() => (copied = false), 1200);
	}

	async function paste() {
		try {
			const text = await navigator.clipboard.readText();
			input = text;
			isSample = false;
			pasted = true;
			setTimeout(() => (pasted = false), 1200);
		} catch {
			// Clipboard read may be denied; fail silently.
		}
	}

	const SITE_URL = 'https://whatdown.lebaux.co';

	function pathFor(locale: Locale): string {
		return new URL(localizeUrl(SITE_URL + '/', { locale })).pathname;
	}

	const currentLocale = $derived(getLocale());

	// Normalize: '/' for English (root), '/<locale>' (no trailing slash) for others.
	// SvelteKit redirects trailing-slash variants by default, so we link to the
	// canonical no-slash form to avoid hreflang/canonical pointing at a 308.
	function normalizedPath(locale: Locale): string {
		const p = pathFor(locale);
		if (p === '/') return '/';
		return p.replace(/\/$/, '');
	}

	const canonical = $derived(
		SITE_URL + (normalizedPath(currentLocale) === '/' ? '' : normalizedPath(currentLocale))
	);

	const softwareJsonLd = $derived({
		'@context': 'https://schema.org',
		'@type': 'SoftwareApplication',
		name: 'Whatdown — ' + m.h1(),
		url: canonical,
		applicationCategory: 'UtilitiesApplication',
		operatingSystem: 'Web',
		description: m.meta_description(),
		offers: { '@type': 'Offer', price: '0', priceCurrency: 'USD' },
		author: { '@type': 'Person', name: 'lebaux', url: 'https://lebaux.cl' },
		inLanguage: currentLocale
	});

	const howToJsonLd = $derived({
		'@context': 'https://schema.org',
		'@type': 'HowTo',
		name: m.howto_h2(),
		description: m.intro_p1(),
		totalTime: 'PT10S',
		inLanguage: currentLocale,
		step: [
			{
				'@type': 'HowToStep',
				position: 1,
				name: m.howto_step1_title(),
				text: m.howto_step1_body()
			},
			{
				'@type': 'HowToStep',
				position: 2,
				name: m.howto_step2_title(),
				text: m.howto_step2_body_before() + m.howto_step2_body_em() + m.howto_step2_body_after()
			},
			{
				'@type': 'HowToStep',
				position: 3,
				name: m.howto_step3_title(),
				text: m.howto_step3_body_before() + m.howto_step3_body_em() + m.howto_step3_body_after()
			}
		]
	});

	const faqJsonLd = $derived({
		'@context': 'https://schema.org',
		'@type': 'FAQPage',
		inLanguage: currentLocale,
		mainEntity: [
			{
				'@type': 'Question',
				name: m.faq_q1(),
				acceptedAnswer: { '@type': 'Answer', text: m.faq_a1() }
			},
			{
				'@type': 'Question',
				name: m.faq_q2(),
				acceptedAnswer: {
					'@type': 'Answer',
					text: m.faq_a2_before_code() + m.faq_a2_code() + m.faq_a2_after_code()
				}
			},
			{
				'@type': 'Question',
				name: m.faq_q3(),
				acceptedAnswer: {
					'@type': 'Answer',
					text:
						m.faq_a3_before_code1() +
						m.faq_a3_code1() +
						m.faq_a3_middle() +
						m.faq_a3_code2() +
						m.faq_a3_after_code2()
				}
			},
			{
				'@type': 'Question',
				name: m.faq_q4(),
				acceptedAnswer: { '@type': 'Answer', text: m.faq_a4() }
			},
			{
				'@type': 'Question',
				name: m.faq_q5(),
				acceptedAnswer: {
					'@type': 'Answer',
					text: m.faq_a5_before_link() + 'GitHub' + m.faq_a5_after_link()
				}
			},
			{
				'@type': 'Question',
				name: m.faq_q6(),
				acceptedAnswer: { '@type': 'Answer', text: m.faq_a6() }
			},
			{
				'@type': 'Question',
				name: m.faq_q7(),
				acceptedAnswer: { '@type': 'Answer', text: m.faq_a7() }
			}
		]
	});

	function hreflangFor(locale: Locale): string {
		return SITE_URL + (normalizedPath(locale) === '/' ? '/' : normalizedPath(locale));
	}
</script>

<svelte:head>
	<title>{m.meta_title()}</title>
	<meta name="description" content={m.meta_description()} />
	<link rel="canonical" href={canonical} />
	<meta name="robots" content="index,follow,max-image-preview:large" />

	{#each locales as locale (locale)}
		<link rel="alternate" hreflang={locale} href={hreflangFor(locale)} />
	{/each}
	<link rel="alternate" hreflang="x-default" href="{SITE_URL}/" />

	<meta property="og:type" content="website" />
	<meta property="og:site_name" content="Whatdown" />
	<meta property="og:title" content={m.meta_title()} />
	<meta property="og:description" content={m.meta_description()} />
	<meta property="og:url" content={canonical} />
	<meta property="og:locale" content={currentLocale} />
	<meta property="og:image" content="{SITE_URL}/whatdown-thumbnail.jpg" />
	<meta property="og:image:width" content="1400" />
	<meta property="og:image:height" content="1050" />
	<meta property="og:image:alt" content={m.og_image_alt()} />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content={m.meta_title()} />
	<meta name="twitter:description" content={m.meta_description()} />
	<meta name="twitter:image" content="{SITE_URL}/whatdown-thumbnail.jpg" />

	{@html `<script type="application/ld+json">${JSON.stringify(softwareJsonLd)}</script>`}
	{@html `<script type="application/ld+json">${JSON.stringify(howToJsonLd)}</script>`}
	{@html `<script type="application/ld+json">${JSON.stringify(faqJsonLd)}</script>`}
</svelte:head>

<main class="mx-auto flex max-w-6xl flex-col gap-4 p-6">
	<header class="flex items-center gap-3">
		<img src="/whatdown.svg" alt="Whatdown" class="h-12 w-auto shrink-0" />
		<div>
			<h1 class="text-xl leading-tight font-semibold">{m.h1()}</h1>
			<p class="text-sm leading-tight text-gray-500">{m.tagline()}</p>
		</div>
	</header>

	<div class="grid grid-cols-1 items-stretch gap-4 md:grid-cols-2">
		<div class="flex flex-col gap-1">
			<div class="flex items-center justify-between">
				<span class="text-sm font-medium text-gray-700">{m.input_label()}</span>
				<button
					type="button"
					onclick={paste}
					class="rounded border border-gray-300 px-3 py-1 text-xs font-medium text-gray-700 hover:bg-gray-100"
				>
					{pasted ? m.pasted() : m.paste()}
				</button>
			</div>
			<textarea
				bind:this={inputEl}
				bind:value={input}
				onfocus={clearSampleOnFirstEdit}
				placeholder={m.input_placeholder()}
				class="min-h-[300px] flex-1 resize-none overflow-hidden rounded border border-gray-300 p-3 font-mono text-xs focus:border-gray-500 focus:outline-none"
			></textarea>
		</div>

		<div class="flex flex-col gap-1">
			<div class="flex items-center justify-between pr-3">
				<span class="text-sm font-medium text-gray-700">{m.output_label()}</span>
				<div class="flex items-center gap-2">
					<button
						type="button"
						onclick={() => (showRaw = !showRaw)}
						aria-pressed={showRaw}
						class="rounded border border-gray-300 px-3 py-1 text-xs font-medium text-gray-700 hover:bg-gray-100"
					>
						{showRaw ? m.show_rendered() : m.show_raw()}
					</button>
					<button
						type="button"
						onclick={copy}
						disabled={!output}
						class="rounded bg-gray-900 px-3 py-1 text-xs font-medium text-white disabled:opacity-40"
					>
						{copied ? m.copied() : m.copy()}
					</button>
				</div>
			</div>

			<div class="relative flex min-h-[300px] flex-1 justify-end pr-3">
				<div class="relative flex max-w-full flex-1 rounded-lg rounded-br-none bg-[#d9fdd3]">
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
							<div class="wa-bubble min-w-0 flex-1 p-3 text-sm leading-snug text-gray-900">
								{@html outputHtml}
							</div>
						{/if}
					{:else}
						<div class="flex-1 p-3 text-sm text-gray-500/70">{m.preview_placeholder()}</div>
					{/if}
				</div>
			</div>
		</div>
	</div>

	<section class="mt-12 flex flex-col gap-3 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">{m.intro_h2()}</h2>
		<p class="text-base leading-relaxed">{m.intro_p1()}</p>
		<p class="text-base leading-relaxed">
			{m.intro_p2_before_link()}<a
				href="https://github.com/Antoine-lb/md-to-ws"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">{m.intro_p2_link()}</a
			>{m.intro_p2_after_link()}
		</p>
	</section>

	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">{m.howto_h2()}</h2>
		<ol class="flex list-decimal flex-col gap-3 pl-6 text-base leading-relaxed">
			<li>
				<strong>{m.howto_step1_title()}</strong>
				{m.howto_step1_body()}
			</li>
			<li>
				<strong>{m.howto_step2_title()}</strong>
				{m.howto_step2_body_before()}<em>{m.howto_step2_body_em()}</em>{m.howto_step2_body_after()}
			</li>
			<li>
				<strong>{m.howto_step3_title()}</strong>
				{m.howto_step3_body_before()}<em>{m.howto_step3_body_em()}</em>{m.howto_step3_body_after()}
			</li>
		</ol>
	</section>

	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">{m.ref_h2()}</h2>
		<p class="text-base leading-relaxed">{m.ref_intro()}</p>
		<div class="overflow-x-auto">
			<table class="w-full border-collapse text-sm">
				<thead>
					<tr class="border-b border-gray-300 text-left">
						<th class="py-2 pr-4 font-semibold text-gray-900">{m.ref_col_format()}</th>
						<th class="py-2 pr-4 font-semibold text-gray-900">{m.ref_col_whatsapp()}</th>
						<th class="py-2 pr-4 font-semibold text-gray-900">{m.ref_col_markdown()}</th>
					</tr>
				</thead>
				<tbody class="divide-y divide-gray-200">
					<tr>
						<td class="py-2 pr-4">{m.ref_bold()}</td>
						<td class="py-2 pr-4"><code class="wa-code">*bold*</code></td>
						<td class="py-2 pr-4"><code class="wa-code">**bold**</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_italic()}</td>
						<td class="py-2 pr-4"><code class="wa-code">_italic_</code></td>
						<td class="py-2 pr-4"><code class="wa-code">*italic*</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_strike()}</td>
						<td class="py-2 pr-4"><code class="wa-code">~strike~</code></td>
						<td class="py-2 pr-4"><code class="wa-code">~~strike~~</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_inline_code()}</td>
						<td class="py-2 pr-4"><code class="wa-code">`code`</code></td>
						<td class="py-2 pr-4"><code class="wa-code">`code`</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_code_block()}</td>
						<td class="py-2 pr-4"><code class="wa-code">```block```</code></td>
						<td class="py-2 pr-4"><code class="wa-code">```block```</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_bullet_list()}</td>
						<td class="py-2 pr-4">
							<code class="wa-code">- item</code>{m.ref_bullet_or()}<code class="wa-code"
								>* item</code
							>
						</td>
						<td class="py-2 pr-4"><code class="wa-code">- item</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_headings()}</td>
						<td class="py-2 pr-4 text-gray-500">{m.ref_headings_unsupported()}</td>
						<td class="py-2 pr-4"><code class="wa-code"># Heading</code></td>
					</tr>
					<tr>
						<td class="py-2 pr-4">{m.ref_anchor_links()}</td>
						<td class="py-2 pr-4 text-gray-500">{m.ref_anchor_links_unsupported()}</td>
						<td class="py-2 pr-4"><code class="wa-code">[text](url)</code></td>
					</tr>
				</tbody>
			</table>
		</div>
	</section>

	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<h2 class="text-2xl font-semibold text-gray-900">{m.faq_h2()}</h2>

		<div class="flex flex-col gap-5 text-base leading-relaxed">
			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q1()}</h3>
				<p>{m.faq_a1()}</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q2()}</h3>
				<p>
					{m.faq_a2_before_code()}<code class="wa-code">{m.faq_a2_code()}</code
					>{m.faq_a2_after_code()}
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q3()}</h3>
				<p>
					{m.faq_a3_before_code1()}<code class="wa-code">{m.faq_a3_code1()}</code
					>{m.faq_a3_middle()}<code class="wa-code">{m.faq_a3_code2()}</code
					>{m.faq_a3_after_code2()}
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q4()}</h3>
				<p>{m.faq_a4()}</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q5()}</h3>
				<p>
					{m.faq_a5_before_link()}<a
						href="https://github.com/Antoine-lb/md-to-ws"
						target="_blank"
						rel="noopener noreferrer"
						class="text-[#027eb5] underline">{m.faq_a5_link()}</a
					>{m.faq_a5_after_link()}
				</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q6()}</h3>
				<p>{m.faq_a6()}</p>
			</div>

			<div>
				<h3 class="text-lg font-semibold text-gray-900">{m.faq_q7()}</h3>
				<p>{m.faq_a7()}</p>
			</div>
		</div>
	</section>

	<section class="mt-12 flex flex-col gap-4 text-gray-800">
		<header class="flex flex-col gap-1">
			<p class="text-xs font-medium tracking-wide text-gray-500 uppercase">{m.why_overline()}</p>
			<h2 class="text-2xl font-semibold text-gray-900">{m.why_h2()}</h2>
		</header>

		<div class="flex flex-col gap-4 text-base leading-relaxed">
			<p>{m.why_intro()}</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">{m.why_h3_keyboards()}</h3>
			<p>
				{m.why_p_keyboards_1()}<code class="wa-code">**bold**</code>{m.why_p_keyboards_2()}<code
					class="wa-code">*italic*</code
				>{m.why_p_keyboards_3()}<code class="wa-code">*bold*</code>{m.why_p_keyboards_4()}<code
					class="wa-code">_italic_</code
				>{m.why_p_keyboards_5()}<code class="wa-code">~strike~</code>{m.why_p_keyboards_6()}
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">{m.why_h3_documents()}</h3>
			<p>{m.why_p_documents()}</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">{m.why_h3_chat()}</h3>
			<p>
				{m.why_p_chat_1()}<code class="wa-code">*bold*</code>{m.why_p_chat_2()}<code class="wa-code"
					>_italic_</code
				>{m.why_p_chat_3()}
			</p>

			<h3 class="mt-2 text-lg font-semibold text-gray-900">{m.why_h3_lockin()}</h3>
			<p>
				{m.why_p_lockin_1()}<code class="wa-code">**bold**</code>{m.why_p_lockin_2()}
			</p>
		</div>
	</section>

	<footer class="mt-16 flex flex-col gap-3 border-t border-gray-300/70 pt-6 text-sm text-gray-600">
		<p>
			{m.footer_p1_before_lebaux()}<a
				href="https://lebaux.cl"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">{m.footer_p1_lebaux()}</a
			>{m.footer_p1_middle()}<a
				href="https://github.com/Antoine-lb/md-to-ws"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">{m.footer_p1_github()}</a
			>{m.footer_p1_after()}
		</p>
		<p>
			{m.footer_p2_before_link()}<a
				href="https://github.com/Antoine-lb/md-to-ws/issues"
				target="_blank"
				rel="noopener noreferrer"
				class="text-[#027eb5] underline">{m.footer_p2_link()}</a
			>{m.footer_p2_after()}
		</p>
		<nav aria-label="Language" class="flex flex-wrap items-center gap-2 pt-2">
			{#each locales as locale (locale)}
				{@const active = locale === currentLocale}
				<a
					href={normalizedPath(locale)}
					hreflang={locale}
					lang={locale}
					aria-current={active ? 'page' : undefined}
					data-sveltekit-reload
					class="rounded-full border px-3 py-1 text-xs font-medium transition-colors {active
						? 'border-gray-900 bg-gray-900 text-white'
						: 'border-gray-300 text-gray-700 hover:border-gray-500 hover:bg-gray-100'}"
				>
					{#if locale === 'en'}English{/if}
					{#if locale === 'es'}Español{/if}
					{#if locale === 'pt'}Português{/if}
					{#if locale === 'fr'}Français{/if}
					{#if locale === 'de'}Deutsch{/if}
					{#if locale === 'nl'}Nederlands{/if}
					{#if locale === 'it'}Italiano{/if}
					{#if locale === 'pl'}Polski{/if}
					{#if locale === 'hi'}हिन्दी{/if}
					{#if locale === 'ar'}العربية{/if}
					{#if locale === 'id'}Bahasa Indonesia{/if}
					{#if locale === 'ms'}Bahasa Melayu{/if}
					{#if locale === 'ru'}Русский{/if}
					{#if locale === 'tr'}Türkçe{/if}
					{#if locale === 'bn'}বাংলা{/if}
					{#if locale === 'mr'}मराठी{/if}
					{#if locale === 'ta'}தமிழ்{/if}
					{#if locale === 'te'}తెలుగు{/if}
					{#if locale === 'gu'}ગુજરાતી{/if}
					{#if locale === 'ur'}اردو{/if}
					{#if locale === 'kn'}ಕನ್ನಡ{/if}
					{#if locale === 'pa'}ਪੰਜਾਬੀ{/if}
					{#if locale === 'or'}ଓଡ଼ିଆ{/if}
					{#if locale === 'ml'}മലയാളം{/if}
					{#if locale === 'ja'}日本語{/if}
					{#if locale === 'zh'}中文{/if}
					{#if locale === 'sw'}Kiswahili{/if}
					{#if locale === 'vi'}Tiếng Việt{/if}
					{#if locale === 'ha'}Hausa{/if}
					{#if locale === 'th'}ไทย{/if}
					{#if locale === 'fa'}فارسی{/if}
					{#if locale === 'yo'}Yorùbá{/if}
					{#if locale === 'ig'}Igbo{/if}
					{#if locale === 'am'}አማርኛ{/if}
					{#if locale === 'tl'}Filipino{/if}
					{#if locale === 'my'}မြန်မာ{/if}
					{#if locale === 'km'}ភាសាខ្មែរ{/if}
					{#if locale === 'uk'}Українська{/if}
					{#if locale === 'si'}සිංහල{/if}
					{#if locale === 'ne'}नेपाली{/if}
					{#if locale === 'he'}עברית{/if}
				</a>
			{/each}
		</nav>
	</footer>
</main>
