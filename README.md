# Wayfair Scraper

Extract structured data from [Wayfair.com search results into structured product rows](https://Wayfair.com) — Wayfair.com search results into structured product rows: price, was-price and discount, brand, rating and review count, stock status, shipping and delivery estimate, size and color option, specs and category path. Search by keyword or paste a Wayfair search URL.

**[Wayfair Scraper on Apify →](https://apify.com/blackfalcondata/wayfair-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by 🌍 country, ↕️ sort by, description format, and more.

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
Integrate with your ETL pipeline to collect structured products from Wayfair.com search results into structured product rows on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from Wayfair.com search results into structured product rows.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "desk",
  "country": "US",
  "maxResults": 100,
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
| `query` | string | — | Search terms, exactly as you would type them into the Wayfair search box (e.g. "desk", "outdoor sectional"). Required unless you paste start URLs. |
| `location` | string | — | Wayfair search takes only a keyword: products, prices and stock are the same nationwide, so leave this empty. Delivery estimates follow the shopper ZIP that Wayfair picks for the session. |
| `country` | enum | `"US"` | Which market to search. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `includeDetails` | boolean | `false` | Also open each product page and add the product description, feature bullets, materials (top, base, frame and other material specs), the full specification table, the product dimensions and the "Get it by" delivery message. One extra page per product, so runs take longer. |
| `startUrls` | array | `[]` | Paste Wayfair search or category page URLs (filters and sort included). Each URL becomes its own search and is paged to the end or to Max results; results are deduplicated by product SKU. When provided, start URLs replace the query field. |
| `productUrls` | array | `[]` | Wayfair product page URLs (…/pdp/…-w009559366.html) or bare Wayfair SKUs (W009559366). Each becomes one product record with the same fields as a search result. Combines with a search or start URLs. |
| `sortBy` | enum | `"recommended"` | Order of search and category results, as on Wayfair. Ignored for product URLs/SKUs. |
| `minPrice` | number | — | Only deliver products at or above this price. Products without a price are left out when set. |
| `maxPrice` | number | — | Only deliver products at or below this price. Products without a price are left out when set. |
| `minRating` | number | — | Only deliver products rated at least this many stars (0–5). Unrated products are left out when set. |
| `brand` | string | — | Only deliver products of these brands (comma-separated, case-insensitive), e.g. "Wade Logan, Inbox Zero". |
| `inStockOnly` | boolean | `false` | Only deliver products Wayfair shows as in stock. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
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

Every product returns the same 42-field schema. Missing values are `null` — never omitted.

- `listingId`
- `sku`
- `variantId`
- `title`
- `brand`
- `manufacturerId`
- `url`
- `portalUrl`
- `price`
- `originalPrice`
- `discountPercent`
- `currency`
- `rating`
- `reviewCount`
- `imageUrl`
- `imageUrls`
- `selectedOption`
- `selectedOptions`
- `dimensions`
- `optionCounts`
- `specifications`
- `badge`
- `availability`
- `stockStatus`
- `shipping`
- `deliveryEstimate`
- `isSponsored`
- `category`
- `categoryId`
- `classId`
- `categoryPath`
- `position`
- `page`
- `totalResults`
- `sourceUrl`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "78c2e10d28d9052f45885736a35578f26a922a2d4734d1814bf2e5f7b40a3906",
  "sku": "W111003401",
  "variantId": "W111003401_1458745312_1458745324",
  "title": "Ovidia Standing & Height-Adjustable Desks",
  "brand": "Inbox Zero",
  "manufacturerId": 55652,
  "url": "https://www.wayfair.com/furniture/pdp/inbox-zero-ovidia-standing-height-adjustable-desks-w111003401.html?piid=1458745312%2C1458745324",
  "portalUrl": "https://www.wayfair.com/furniture/pdp/inbox-zero-ovidia-standing-height-adjustable-desks-w111003401.html?piid=1458745312%2C1458745324",
  "price": 89.99,
  "originalPrice": 350,
  "discountPercent": 74,
  "currency": "USD"
}
```

*Truncated — full records contain 42 fields. See Output fields for the complete schema.*

**[Try Wayfair Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/wayfair-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.00135 |

See the [actor on Apify](https://apify.com/blackfalcondata/wayfair-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape Wayfair.com search results into structured product rows?**
Use this actor on Apify to extract structured data from Wayfair.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get Wayfair.com search results into structured product rows data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape Wayfair.com search results into structured product rows?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check Wayfair.com search results into structured product rows's terms of service for your specific use case.

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
2. Open [Wayfair Scraper](https://apify.com/blackfalcondata/wayfair-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
