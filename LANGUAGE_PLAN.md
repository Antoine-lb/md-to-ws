# WhatsApp language plan

| #   | Language             | Code        | Status        | Estimated users                      | Main countries / demographic                                                                                 |
| --- | -------------------- | ----------- | ------------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| 1   | Hindi                | `hi`        | ✅ Done       | ~528M native, ~600M+ with L2         | India (Hindi belt — North India); ~17% of all WhatsApp users worldwide                                       |
| 2   | Spanish              | `es`        | ✅ Done       | ~200M+                               | Mexico (65M), Spain (33M), Argentina, Colombia (29M), rest of LatAm                                          |
| 3   | Portuguese           | `pt`        | ✅ Done       | ~140–148M                            | Brazil (~140M, ~98% smartphone penetration), Portugal                                                        |
| 4   | English              | `en` (base) | ✅ Done       | ~140M+                               | USA (64M), UK (41M), Nigeria (~51M), Australia, Canada                                                       |
| 5   | Indonesian           | `id`        | ✅ Done       | ~112M                                | Indonesia                                                                                                    |
| 6   | Arabic               | `ar`        | ✅ Done (RTL) | ~100M+                               | Egypt (56M), Saudi Arabia, UAE, Morocco, Algeria, Iraq                                                       |
| 7   | Bengali              | `bn`        | ✅ Done       | ~97M in India + ~160M in Bangladesh  | West Bengal, Tripura, Bangladesh                                                                             |
| 8   | Marathi              | `mr`        | ✅ Done       | ~83M                                 | Maharashtra (Mumbai)                                                                                         |
| 9   | Telugu               | `te`        | ✅ Done       | ~81M                                 | Andhra Pradesh, Telangana                                                                                    |
| 10  | Russian              | `ru`        | ✅ Done       | ~72M                                 | Russia, CIS countries                                                                                        |
| 11  | Tamil                | `ta`        | ✅ Done       | ~69M                                 | Tamil Nadu, Sri Lanka, Singapore, Malaysia diaspora                                                          |
| 12  | Turkish              | `tr`        | ✅ Done       | ~60M                                 | Turkey                                                                                                       |
| 13  | German               | `de`        | ✅ Done       | ~55M                                 | Germany (53M), Austria, Switzerland                                                                          |
| 14  | Gujarati             | `gu`        | ✅ Done       | ~55M                                 | Gujarat                                                                                                      |
| 15  | Urdu                 | `ur`        | ✅ Done (RTL) | ~50M in India + ~230M total incl. L2 | North India, Pakistan                                                                                        |
| 16  | Kannada              | `kn`        | ✅ Done       | ~43M                                 | Karnataka (Bengaluru)                                                                                        |
| 17  | Italian              | `it`        | ✅ Done       | ~38M                                 | Italy                                                                                                        |
| 18  | Odia                 | `or`        | ✅ Done       | ~37M                                 | Odisha                                                                                                       |
| 19  | Malayalam            | `ml`        | ✅ Done       | ~35M                                 | Kerala                                                                                                       |
| 20  | Punjabi              | `pa`        | ✅ Done       | ~33M                                 | Punjab; major diaspora in Canada/UK                                                                          |
| 21  | Malay                | `ms`        | ✅ Done       | ~25M                                 | Malaysia (~22M), Brunei, Singapore                                                                           |
| 22  | French               | `fr`        | ✅ Done       | significant                          | France, Belgium, Switzerland, francophone Africa                                                             |
| 23  | Polish               | `pl`        | ✅ Done       | ~38M                                 | Poland                                                                                                       |
| 24  | Japanese             | `ja`        | ✅ Done       | small WA share                       | Japan — LINE dominates domestic chat, but Japan is a major LLM-user market for international WhatsApp output |
| 25  | Chinese (Simplified) | `zh`        | ✅ Done       | large addressable                    | Hong Kong, Taiwan, Singapore, Malaysia, global diaspora (Mainland China blocks WhatsApp)                     |
| 26  | Vietnamese           | `vi`        | ✅ Done       | ~85M                                 | Vietnam — single-country reach but very high penetration                                                     |
| 27  | Persian / Farsi      | `fa`        | ✅ Done (RTL) | ~70M+                                | Iran, Afghanistan (also as Dari), Tajikistan                                                                 |
| 28  | Thai                 | `th`        | ✅ Done       | ~55M+                                | Thailand, Laos diaspora                                                                                      |
| 29  | Swahili              | `sw`        | ✅ Done       | ~80M (incl. L2)                      | Tanzania, Kenya, Uganda, DRC, Rwanda — East African lingua franca                                            |
| 30  | Hausa                | `ha`        | ✅ Done       | ~80M (incl. L2)                      | Northern Nigeria, Niger, Chad, Cameroon — major West African lingua franca                                   |
| 31  | Yoruba               | `yo`        | ✅ Done       | ~45M                                 | Southwestern Nigeria, Benin, Togo                                                                            |
| 32  | Igbo                 | `ig`        | ✅ Done       | ~30M                                 | Southeastern Nigeria                                                                                         |
| 33  | Amharic              | `am`        | ✅ Done       | ~32M                                 | Ethiopia (working language)                                                                                  |
| 34  | Filipino / Tagalog   | `tl`        | ✅ Done       | ~45M (~80M incl. L2)                 | Philippines                                                                                                  |
| 35  | Burmese              | `my`        | ✅ Done       | ~33M                                 | Myanmar                                                                                                      |
| 36  | Khmer                | `km`        | ✅ Done       | ~16M                                 | Cambodia                                                                                                     |
| 37  | Sinhala              | `si`        | ✅ Done       | ~17M                                 | Sri Lanka                                                                                                    |
| 38  | Nepali               | `ne`        | ✅ Done       | ~32M                                 | Nepal, parts of India (Sikkim, Darjeeling), Bhutan                                                           |
| 39  | Ukrainian            | `uk`        | ✅ Done       | ~33M                                 | Ukraine, diaspora across Europe                                                                              |
| 40  | Dutch                | `nl`        | ✅ Done       | ~24M                                 | Netherlands, Belgium (Flanders), Suriname                                                                    |
| 41  | Hebrew               | `he`        | ✅ Done (RTL) | ~9M                                  | Israel, global Jewish diaspora                                                                               |

## Files to update when adding a locale

Empirical checklist (matches what recent locale-add commits actually touched):

| File                                                         | Change                                                                                                                                            | Notes                                                                                                                                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [project.inlang/settings.json](project.inlang/settings.json) | Add the code to the `locales` array                                                                                                               | Single source of truth for Paraglide. Triggers regeneration of `src/lib/paraglide/*` on next build.                                                             |
| [messages/{locale}.json](messages/)                          | Create the file                                                                                                                                   | Mirror the key set of [messages/en.json](messages/en.json) (~109 keys). Path comes from `pathPattern` in inlang settings.                                       |
| [static/sitemap.xml](static/sitemap.xml)                     | Add `<url>` block for `/{locale}` and add the new `<xhtml:link rel="alternate" hreflang="{locale}" ...>` line to **every existing** `<url>` block | Hand-maintained, not generated. This is the SEO-critical step — without it, Google will not know the locale exists or that it's an alternate of the other URLs. |
| [src/routes/+page.svelte](src/routes/+page.svelte)           | Add a `{#if locale === '{code}'}{native name}{/if}` line in the language picker (around line 488)                                                 | Native script (e.g. `日本語`, `ਪੰਜਾਬੀ`), not English name.                                                                                                      |

Auto-handled, no manual edit needed:

- `src/lib/paraglide/*` — gitignored, regenerated by Paraglide from `settings.json` + message files on `npm run build` (or `npx paraglide-js compile`).
- `<html dir>` — Paraglide's `getTextDirection` (called in [src/hooks.server.ts](src/hooks.server.ts)) reads RTL from CLDR automatically. Arabic, Urdu, Persian, and Hebrew will all flip to RTL with no code change.

Post-add verification:

- `npm run build` succeeds (Paraglide compiles cleanly).
- Visit `/{locale}` locally and confirm: page renders, language picker shows the new entry, RTL locales flip direction.
- View page source on `/{locale}` and confirm `<link rel="alternate" hreflang="{locale}">` appears (or check the sitemap directly at `/sitemap.xml`).
- Update this file: move the row from ⏳ To do to ✅ Done.

## Variant notes

- **`id` vs `ms`:** Indonesian and Malay share roots and are mutually intelligible at a basic level, but they have diverged enough in vocabulary, spelling, and idiom that Indonesians and Malaysians generally prefer their own variant. Separate ISO 639-1 codes (`id`, `ms`) also let Google serve them as distinct hreflang targets.
- **`zh` (Simplified vs Traditional):** Chinese splits into Simplified (`zh-Hans`, Mainland China and Singapore) and Traditional (`zh-Hant`, Hong Kong and Taiwan). Current `zh` ships Simplified, which most Chinese readers can decode even if they prefer Traditional. If usage data later shows meaningful HK/TW traffic, splitting into `zh-Hans` and `zh-Hant` is straightforward.
- **`fa` regional variants:** Iranian Persian (`fa-IR`), Dari (`fa-AF`/`prs`), and Tajik (`tg`, written in Cyrillic) diverge in vocabulary and script. Shipping `fa` first covers Iran and most of Afghanistan; Tajik would be a separate add.

## Caveats

- WhatsApp does not publish official user counts by language; figures aggregate users across countries where each language is dominant (mostly 2024–2026 data).
- The country-to-language mapping is a proxy, not telemetry. Multilingual countries (India, Nigeria, Indonesia, Switzerland) inflate or fragment counts.
- Bilingual users get double-counted across regions.
- WhatsApp's own UI supports 60+ languages; on-device message translation is currently rolling out in English, Spanish, Hindi, Portuguese, Russian, and Arabic — a useful signal of which language markets Meta itself prioritizes.
- All Indian-language speaker numbers come from the 2011 Census of India; real 2026 numbers are higher across the board but the relative rankings have not shifted meaningfully.

## Sources

- [WhatsApp Users by Country 2026 — World Population Review](https://worldpopulationreview.com/country-rankings/whatsapp-users-by-country)
- [Latest WhatsApp Statistics 2026 — DemandSage](https://www.demandsage.com/whatsapp-statistics/)
- [WhatsApp User Statistics 2026 — Backlinko](https://backlinko.com/whatsapp-users)
- [WhatsApp Statistics 2026 — Infobip](https://www.infobip.com/blog/whatsapp-statistics)
- [About the languages WhatsApp is available in — WhatsApp Help Center](https://faq.whatsapp.com/873422324183264)
- [Introducing Message Translations — WhatsApp Blog](https://blog.whatsapp.com/introducing-message-translations)
- [List of languages by number of native speakers in India — Wikipedia](https://en.wikipedia.org/wiki/List_of_languages_by_number_of_native_speakers_in_India)
- [Languages of India — Wikipedia](https://en.wikipedia.org/wiki/Languages_of_India)
