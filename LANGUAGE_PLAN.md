# WhatsApp language plan

A look at the largest WhatsApp language markets, used to prioritize i18n coverage for this app.

WhatsApp does not publish official user counts by language, so the figures below aggregate users across countries where each language is dominant. Numbers are approximate (mostly 2024–2026 data).

## Top 10 WhatsApp languages

| # | Language | Code | Estimated users | Main countries | Supported in app |
|---|---|---|---|---|---|
| 1 | Hindi (proxy for India) | `hi` | ~535–854M | India (~17% of all WhatsApp users worldwide) | ✅ |
| 2 | Spanish | `es` | ~200M+ | Mexico (65M), Spain (33M), Argentina, Colombia (29M), rest of LatAm | ✅ |
| 3 | Portuguese | `pt` | ~140–148M | Brazil (~140M, ~98% smartphone penetration), Portugal | ✅ |
| 4 | English | `en` | ~140M+ | USA (64M), UK (41M), Nigeria (~51M, primarily English), Australia, Canada | ✅ |
| 5 | Indonesian | `id` | ~112M | Indonesia (112M) | ✅ |
| 5 | Malay | `ms` | ~25M | Malaysia (~22M), Brunei, Singapore | ✅ |
| 6 | Arabic | `ar` | ~100M+ | Egypt (56M), Saudi Arabia, UAE, Morocco, Algeria, Iraq | ✅ |
| 7 | Russian | `ru` | ~72M | Russia (72M), CIS countries | ❌ |
| 8 | Turkish | `tr` | ~60M | Turkey (60M) | ❌ |
| 9 | German | `de` | ~55M | Germany (53M), Austria, Switzerland | ✅ |
| 10 | Italian | `it` | ~38M | Italy (37.5M) | ✅ |

Other locales currently in the app but outside the top 10 by user count:
- French (`fr`) — ✅ supported (significant footprint in France, Belgium, Switzerland, francophone Africa)
- Polish (`pl`) — ✅ supported

## Indian languages in detail

India is by far WhatsApp's largest market (~535–854M users depending on the source — roughly 17% of all global WhatsApp users). But "Indian" is not a language. India is multilingual, with 22 scheduled languages in its constitution and 100+ languages with significant speaker bases.

Per the 2011 Census of India (still the most-cited source for native-speaker counts):

| # | Language | Code | Native speakers | % of India | Region |
|---|---|---|---|---|---|
| 1 | Hindi | `hi` | ~528M | 43.6% | North India ("Hindi belt") | ✅ |
| 2 | Bengali | `bn` | ~97M | 8.0% | West Bengal, Tripura (also national language of Bangladesh, +160M more speakers there) | ❌ |
| 3 | Marathi | `mr` | ~83M | 6.9% | Maharashtra (Mumbai) | ❌ |
| 4 | Telugu | `te` | ~81M | 6.7% | Andhra Pradesh, Telangana | ❌ |
| 5 | Tamil | `ta` | ~69M | 5.7% | Tamil Nadu (also Sri Lanka, Singapore, Malaysia diaspora) | ❌ |
| 6 | Gujarati | `gu` | ~55M | 4.6% | Gujarat | ❌ |
| 7 | Urdu | `ur` | ~50M | 4.2% | spread across North India (also national language of Pakistan, ~230M total speakers including L2) | ❌ |
| 8 | Kannada | `kn` | ~43M | 3.6% | Karnataka (Bengaluru) | ❌ |
| 9 | Odia | `or` | ~37M | 3.1% | Odisha | ❌ |
| 10 | Malayalam | `ml` | ~35M | 2.9% | Kerala | ❌ |
| 11 | Punjabi | `pa` | ~33M | 2.7% | Punjab (also major diaspora in Canada/UK) | ❌ |

**Important caveat for the app:** shipping `hi` alone covers Hindi natively, and Hindi is widely understood as a second language across the Hindi belt — but it does **not** cover South India (Tamil, Telugu, Kannada, Malayalam) or the East (Bengali, Odia) where speakers often prefer English over Hindi as their second language. So for India specifically, the highest-leverage additions after Hindi are likely **English** (already supported) plus **Bengali, Tamil, Telugu, Marathi**.

## Current app support

Locales configured in `project.inlang/settings.json`:
- ✅ `en` (base) · `es` · `pt` · `fr` · `de` · `it` · `pl` · `hi` · `ar` · `id` · `ms`

Total: 11 locales.

RTL is wired through Paraglide's `getTextDirection` (called in `src/hooks.server.ts`) into `<html dir>` — Arabic is the first RTL locale in the app, future RTL adds (Urdu, Hebrew) will not need new infrastructure.

**Note on `id` vs `ms`:** Indonesian and Malay share roots and are mutually intelligible at a basic level, but they have diverged enough in vocabulary, spelling, and idiom that Indonesians and Malaysians generally prefer their own variant. They also have separate ISO 639-1 codes (`id`, `ms`), which means Google can serve them as distinct hreflang targets — collapsing them into one locale would leave SEO reach on the table in either Indonesia or Malaysia.

## Suggested priority order for new locales

Based on raw WhatsApp user reach, ordered by expected impact:

1. **Russian (`ru`)** — ~72M users in Russia + CIS reach.
2. **Turkish (`tr`)** — ~60M users, concentrated single market.
3. **Bengali (`bn`)** — ~97M in India + ~160M in Bangladesh. Highest-impact Indian-language add after Hindi.
4. **Tamil (`ta`)** — ~69M, covers South India where Hindi has weaker reach.
5. **Telugu (`te`)** — ~81M, similar reasoning to Tamil.
6. **Marathi (`mr`)** — ~83M, dominant in Mumbai region.
7. **Urdu (`ur`)** — ~230M total speakers (Pakistan + North India). RTL infrastructure now exists from Arabic, so this is low-friction to add.

Notes:
- Indian-language adds have multiplicative value because they don't just bring new users; they deepen reach in an already-massive market where the app likely already has traction in English/Hindi.

## Caveats

- The country-to-language mapping is a proxy, not telemetry. Multilingual countries (India, Nigeria, Indonesia, Switzerland) inflate or fragment counts.
- Bilingual users get double-counted across regions.
- WhatsApp's own UI supports 60+ languages; on-device message translation is currently rolling out in English, Spanish, Hindi, Portuguese, Russian, and Arabic — a useful signal of which language markets Meta itself prioritizes.
- All Indian-language speaker numbers come from the 2011 Census; real 2026 numbers are higher across the board but the relative rankings have not shifted meaningfully.

## Sources

- [WhatsApp Users by Country 2026 — World Population Review](https://worldpopulationreview.com/country-rankings/whatsapp-users-by-country)
- [Latest WhatsApp Statistics 2026 — DemandSage](https://www.demandsage.com/whatsapp-statistics/)
- [WhatsApp User Statistics 2026 — Backlinko](https://backlinko.com/whatsapp-users)
- [WhatsApp Statistics 2026 — Infobip](https://www.infobip.com/blog/whatsapp-statistics)
- [About the languages WhatsApp is available in — WhatsApp Help Center](https://faq.whatsapp.com/873422324183264)
- [Introducing Message Translations — WhatsApp Blog](https://blog.whatsapp.com/introducing-message-translations)
- [List of languages by number of native speakers in India — Wikipedia](https://en.wikipedia.org/wiki/List_of_languages_by_number_of_native_speakers_in_India)
- [Languages of India — Wikipedia](https://en.wikipedia.org/wiki/Languages_of_India)
