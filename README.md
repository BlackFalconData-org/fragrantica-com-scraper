# Fragrantica Perfume Scraper - Notes, Accords & Ratings

Extract structured data from [fragrantica.com](https://fragrantica.com) — perfume data from Fragrantica: name, brand, year, rating and review counts, main accords, note pyramid and perfumers, plus community votes on longevity and sillage. Search by name or brand, or paste perfume page links.

**[Fragrantica Perfume Scraper - Notes, Accords & Ratings on Apify →](https://apify.com/blackfalcondata/fragrantica-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from fragrantica.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from fragrantica.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Sauvage",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Perfume, brand or note to search for (e.g. "Sauvage", "oud", "Chanel No 5"). Leave empty to browse everything — but then set a Brand or Start URLs so the run has something to work on. |
| `brand` | string | — | Optional. Restrict results to one brand/designer, exactly as spelled on the site (e.g. "Dior", "Tom Ford", "Maison Francis Kurkdjian"). |
| `maxResults` | integer | `100` | Maximum total perfumes (0 = unlimited). |
| `startUrls` | array | `[]` | Paste perfume page URLs from the site. Each URL becomes its own task; results are merged and deduped across all URLs. When provided, startUrls REPLACE the search fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Open each perfume page for the full record: note pyramid, main accords, community vote data, description, similar perfumes, pros & cons. Turn off for a fast search-only list. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from your search inputs (search terms, brand, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip perfumes whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit perfumes whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit perfumes that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED perfumes. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of perfumes included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every product returns the same 89-field schema. Missing values are `null` — never omitted.

- `recordType`
- `perfumeId`
- `url`
- `language`
- `name`
- `brand`
- `brandUrl`
- `fullTitle`
- `gender`
- `genderLabel`
- `year`
- `collection`
- `perfumeSlug`
- `fragranceFamily`
- `ratingValue`
- `ratingValueExact`
- `ratingCount`
- `ratingDistribution`
- `reviewsCount`
- `reviewsByLanguage`
- `reviewsEn`
- `reviewsDe`
- `reviewsEs`
- `reviewsFr`
- `reviewsCs`
- `reviewsIt`
- `reviewsRu`
- `reviewsPl`
- `reviewsPt`
- `reviewsEl`
- `reviewsZh`
- `reviewsJa`
- `reviewsNl`
- `reviewsSr`
- `reviewsRo`
- `reviewsAr`
- `reviewsUk`
- `reviewsMn`
- `reviewsTr`
- `reviewsSv`
- `reviewsHe`
- `reviewsKo`
- `reviewsHu`
- `popularityScore`
- `recentPopularityScore`
- `popularityCompoundScore`
- `lastReviewAt`
- `mainAccords`
- `notesTop`
- `notesMiddle`
- `notesBase`
- `notesUnsorted`
- `hasPyramid`
- `allNotes`
- `perfumers`
- `perfumerDetails`
- `longevity`
- `sillage`
- `priceValue`
- `genderVotes`
- `seasonVotes`
- `ownership`
- `descriptionHtml`
- `descriptionText`
- `descriptionMarkdown`
- `imageUrl`
- `imageUrlLarge`
- `alternativeNames`
- `languageUrls`
- `visitorTrend`
- `similarPerfumes`
- `pros`
- `cons`
- `awards`
- `sealedPayloadKeys`
- `searchQuery`
- `position`
- `source`
- `scrapedAt`
- `contentHash`
- `portalUrl`
- `reviewsScraped`
- `voteDataAvailable`
- `shopOffers`
- `sourceMode`
- `listingId`
- `contentQuality`
- `detailFetched`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "recordType": "perfume",
  "perfumeId": 31861,
  "url": "https://www.fragrantica.com/perfume/Dior/Sauvage-31861.html",
  "language": "en",
  "name": "Sauvage",
  "brand": "Dior",
  "brandUrl": "https://www.fragrantica.com/designers/Dior.html",
  "fullTitle": "Sauvage Dior for men",
  "gender": "male",
  "genderLabel": "for men",
  "year": 2015,
  "collection": "Sauvage Collection"
}
```

*Truncated — full records contain 89 fields. See Output fields for the complete schema.*

**[Try Fragrantica Perfume Scraper - Notes, Accords & Ratings now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/fragrantica-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.0045 |

See the [actor on Apify](https://apify.com/blackfalcondata/fragrantica-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape fragrantica.com?**
Use this actor on Apify to extract structured data from fragrantica.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get fragrantica.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape fragrantica.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check fragrantica.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Fragrantica Perfume Scraper - Notes, Accords & Ratings](https://apify.com/blackfalcondata/fragrantica-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
