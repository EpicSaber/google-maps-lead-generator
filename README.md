[Google Maps Lead Generator](https://apify.com/netdesignr/google-maps-lead-generator?fpr=data)

Generate Google Maps business leads that are easier to use in outbound, enrichment, and website-audit pipelines.

This actor is built for two audiences:

- teams that want a practical Google Maps lead source for agencies, sales ops, growth, and local-market research
- AI agents and internal tools that need clear inputs, stable outputs, and lead records that are easy to parse downstream

It searches Google Maps by niche and location, extracts local business listings, applies quality filters, optionally pre-checks websites, and emits lead records designed to be usable in NetPulse-style diagnosis and enrichment workflows.

## What this actor is good at

- finding local businesses by service plus geography
- filtering for leads that actually have usable websites
- removing obvious duplicates and common chain businesses
- screening out weak leads before they hit a more expensive pipeline
- producing outputs that humans can export and AI agents can consume with minimal cleanup

## What data can I extract with Google Maps Lead Generator?

| Business identity | Local context | Website and qualification signals |
| --- | --- | --- |
| Business name, category, industry, phone, and website | Address, area, postcode, city, country, plus code, coordinates, and Google Maps URL | Rating, review count, opening status, opening hours, and temporary-closure hints |
| Google place hint data when visible in the Maps URL | Query and rank within the collected result set | Website pre-check result with redirect, SSL, response timing, and failure warnings |
| Stable `recordType` and `outputFormat` fields for downstream parsing | `scrapedAt` timestamp for pipeline traceability | Optional website enrichment with emails, phones, social links, and conservative decision-maker hints |

The actor also emits one `run-summary` record for the whole run with query counts, lead counts, and failure counts.

## What You Can Do With It

- build local-business lead lists for agencies and outbound teams
- source SMEs with websites for NetPulse-style website audits
- create prospect batches by niche, town, county, or metro area
- filter out chain businesses and obvious low-signal listings
- pre-check websites before sending them into a diagnosis or crawl pipeline
- feed the output into CRMs, spreadsheets, Airtable, or AI agent workflows

## Common Use Cases

### 1. Agency prospecting for local niches

Search for businesses like:

- `dentists in surrey`
- `accountants kingston upon thames`
- `removals companies london`

Use the actor to build a shortlist of local businesses with:

- a live website
- a minimum review threshold
- a reasonable Google rating
- enough structure for email or website-based follow-up

Typical output to use:

- default dataset `google-maps-lead` records
- `run-summary` record for batch reporting

### 2. NetPulse website-audit intake

Use `requireWebsite = true` and `websitePreCheck = true` when you want to:

- skip phone-only businesses
- avoid Facebook-only “websites”
- avoid parked or placeholder domains
- save scan budget for real business websites

Typical output to use:

- `website`
- `websitePreCheck`
- `verifiedWebsiteDomain`
- `query`, `category`, `reviewCount`, and `rating`

### 3. Local market mapping

Use the actor to answer questions like:

- who shows up for this service in this area?
- which businesses have strong review signals?
- how dense is the market in a specific postcode or town?

Typical output to use:

- `category`
- `address`, `area`, `city`, `postcode`
- `rating`
- `reviewCount`
- `googleMapsUrl`

### 4. AI-agent lead sourcing

This actor works well for AI agents that need:

- one input object with explicit filters
- one dataset with consistent record types
- lightweight fields that are easy to rank, filter, or summarize
- pre-qualified websites before launching a heavier audit or enrichment step

Recommended agent flow:

1. Run the actor with `searchQueries`, `minReviews`, `minRating`, `requireWebsite`, and `websitePreCheck`.
2. Read the default dataset and keep only `recordType = "google-maps-lead"`.
3. Rank by niche, review count, rating, and website status.
4. Send the surviving `website` URLs into the next pipeline step.

## Quick “Can I Use This For…” Guide

You can use this actor if you want to:

- find local service businesses from Google Maps
- build a website-first SME lead list
- screen leads before running a more expensive audit
- feed structured local-business records into an agent or CRM
- export a clean shortlist for spreadsheets or sales ops

This actor is probably not the right fit if you need:

- a full Google Places API replacement
- guaranteed place IDs for every result
- deep review-text extraction from Google reviews
- consumer-facing marketplace scraping outside local lead generation

## How to use Google Maps Lead Generator

### Step 1: Add your search queries

In **Search Queries**, add one query per line. Each query should combine:

- a business type
- a location

Good examples:

- `dentists in Surrey`
- `removals companies London`
- `accountants Kingston upon Thames`
- `family lawyers Manchester`
- `roofers leeds`

### Step 2: Decide how many results you want

Use **Max Results Per Query** to control run size.

- smaller values are faster and safer for quick prospecting runs
- larger values are better for market mapping or list building

### Step 2b: Tighten locality when Google drifts

Use **Strict Location** when you want results to stay anchored to a specific place such as `Chessington`.

Use **Location Radius (km)** when you want a hard geographic cutoff around that location.

Example:

- `strictLocation = "Chessington"`
- `locationRadiusKm = 5`

This is useful when Google Maps returns nearby towns that are relevant to the search term but outside your real catchment.

### Step 3: Set lead quality filters

Use these together to improve downstream lead quality:

- **Minimum Google Reviews**
- **Minimum Google Rating**
- **Require Website URL**
- **Skip Known Chains/Franchises**

These filters help reduce:

- inactive listings
- low-signal businesses
- large brands that are not your target ICP
- listings that have no website to audit or enrich

### Step 4: Enable website validation when needed

Use **Run Website Pre-Check** when you want to avoid spending time on bad websites.

The pre-check helps surface:

- HTTP status
- final redirect target
- SSL presence
- response timing
- Facebook-only redirects
- parked or placeholder domains

### Step 5: Run and export

Start the actor and read:

- `google-maps-lead` records for the actual lead rows
- the final `run-summary` record for run health and counts

You can export results as JSON, CSV, Excel, XML, HTML, or RSS from Apify, or fetch them via API.

## Input parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `searchQueries` | array | Yes | - | Google Maps search queries, each usually combining niche + location |
| `maxResultsPerQuery` | integer | No | `5` | Maximum businesses to collect per query |
| `countryFilter` | string | No | `GB` | Two-letter country filter used for Google Maps localization |
| `language` | string | No | `en` | Google Maps interface language |
| `strictLocation` | string | No | `null` | Optional locality name used to keep results tied to a specific place |
| `locationRadiusKm` | integer | No | `null` | Optional radius around `strictLocation` for tighter geographic filtering |
| `minReviews` | integer | No | `0` | Skip businesses below this review threshold. Use `0` when Google Maps hides review totals in limited view |
| `minRating` | number | No | `0` | Skip businesses below this rating threshold |
| `requireWebsite` | boolean | No | `true` | Skip listings with no website |
| `skipChains` | boolean | No | `true` | Filter out obvious chain or franchise businesses |
| `excludePlaceIds` | array | No | `[]` | Exclude already-known place IDs when available |
| `websitePreCheck` | boolean | No | `true` | Run lightweight website validation before emitting the lead |
| `skipFacebookOnly` | boolean | No | `true` | Skip websites that resolve to Facebook pages |
| `skipParkedDomains` | boolean | No | `true` | Skip parked or placeholder domains |
| `requestDelayMs` | integer | No | `1000` | Delay between page loads to reduce block risk |
| `maxRetries` | integer | No | `0` | Retries per failed query or navigation flow |
| `webhookUrl` | string | No | `null` | Optional completion webhook for downstream automation |
| `outputFormat` | string | No | `netpulse-ready` | `netpulse-ready`, `raw`, or `csv-friendly` |

## Example input

```
{
  "searchQueries": [
    "removals companies London",
    "dentists Surrey",
    "accountants Kingston upon Thames"
  ],
  "maxResultsPerQuery": 40,
  "countryFilter": "GB",
  "language": "en",
  "strictLocation": "Chessington",
  "locationRadiusKm": 5,
  "minReviews": 8,
  "minRating": 4.0,
  "requireWebsite": true,
  "skipChains": true,
  "websitePreCheck": true,
  "skipFacebookOnly": true,
  "skipParkedDomains": true,
  "requestDelayMs": 3000,
  "maxRetries": 2,
  "outputFormat": "netpulse-ready"
}
```

## Output records

### `google-maps-lead`

Each lead record can include:

- `outputFormat`
- `query`
- `rank`
- `businessName`
- `category`
- `industry`
- `address`
- `area`
- `postcode`
- `city`
- `countryCode`
- `latitude` / `longitude`
- `plusCode`
- `placeId`
- `googleMapsUrl`
- `website`
- `phone`
- `rating`
- `reviewCount`
- `openingStatus`
- `openingHours`
- `verifiedWebsiteDomain`
- `websitePreCheck`
- `enrichment`
- `warnings`

### `run-summary`

One final summary record with:

- `queriesRequested`
- `queriesCompleted`
- `totalLeads`
- `enrichedLeads`
- `failedQueries`
- `failedListings`
- `countryCode`
- `outputFormat`
- `finishedAt`

## Example output

```
{
  "recordType": "google-maps-lead",
  "outputFormat": "netpulse-ready",
  "query": "dentists Surrey",
  "rank": 3,
  "businessName": "Surrey Dental Studio",
  "category": "Dentist",
  "industry": "Dentist",
  "address": "21 High Street, Guildford GU1 3AA, United Kingdom",
  "area": "21 High Street, Guildford GU1 3AA",
  "postcode": "GU1 3AA",
  "city": "Guildford",
  "countryCode": "gb",
  "googleMapsUrl": "https://www.google.com/maps/place/...",
  "website": "https://surreydentalstudio.co.uk",
  "phone": "+44 1483 123456",
  "rating": 4.7,
  "reviewCount": 42,
  "openingStatus": "Open",
  "temporarilyClosed": false,
  "verifiedWebsiteDomain": "surreydentalstudio.co.uk",
  "websitePreCheck": {
    "status": "passed",
    "httpStatus": 200,
    "finalUrl": "https://surreydentalstudio.co.uk/",
    "sslEnabled": true,
    "responseTimeMs": 642,
    "redirectedToFacebook": false,
    "parkedDomain": false,
    "warnings": []
  },
  "enrichment": {
    "status": "not_requested",
    "emails": [],
    "phones": [],
    "decisionMakerHints": [],
    "warnings": []
  },
  "warnings": [],
  "scrapedAt": "2026-04-06T17:00:00.000Z"
}
```

## What makes it easy to integrate

- simple input structure for both humans and agents
- one main dataset with explicit `recordType`
- one run-level summary record for health checks
- optional website screening before downstream processing
- output fields that work well for CRM import, spreadsheets, or agent ranking

## API usage

### JavaScript

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });

const run = await client.actor('netdesignr/google-maps-lead-generator').call({
  searchQueries: [
    'removals companies London',
    'dentists Surrey'
  ],
  maxResultsPerQuery: 25,
  countryFilter: 'GB',
  minReviews: 8,
  requireWebsite: true,
  websitePreCheck: true,
  outputFormat: 'netpulse-ready'
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### Python

```
from apify_client import ApifyClient
import os

client = ApifyClient(os.environ["APIFY_TOKEN"])

run = client.actor("netdesignr/google-maps-lead-generator").call(run_input={
    "searchQueries": ["accountants Kingston upon Thames"],
    "maxResultsPerQuery": 20,
    "countryFilter": "GB",
    "requireWebsite": True,
    "websitePreCheck": True,
    "outputFormat": "netpulse-ready",
})

items = client.dataset(run["defaultDatasetId"]).list_items().items
print(items)
```

### cURL

```
curl -X POST "https://api.apify.com/v2/acts/netdesignr~google-maps-lead-generator/runs?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchQueries": ["dentists Surrey", "removals companies London"],
    "maxResultsPerQuery": 30,
    "countryFilter": "GB",
    "requireWebsite": true,
    "websitePreCheck": true,
    "outputFormat": "netpulse-ready"
  }'
```

## Limitations

- Google Maps layout changes can break selectors over time.
- Some searches can trigger consent walls, throttling, or anti-bot interstitials.
- Place IDs are best-effort because Google Maps does not expose them consistently in every page state.
- Chain detection is heuristic, not perfect.
- Website pre-checks are intentionally lightweight and are not a substitute for a full crawl or audit.
- Decision-maker hints, when enabled, are inferred from public website text and should not be treated as verified identity data.

## Good defaults for AI agents

If an AI agent needs a safe starting configuration, use:

- `maxResultsPerQuery: 20`
- `countryFilter: "GB"`
- `minReviews: 0`
- `minRating: 0`
- `requireWebsite: true`
- `skipChains: true`
- `websitePreCheck: true`
- `skipFacebookOnly: true`
- `skipParkedDomains: true`
- `requestDelayMs: 3000`
- `maxRetries: 2`
- `outputFormat: "netpulse-ready"`

This gives a reasonable balance between speed, lead quality, and downstream usefulness.

## Local validation

```
pnpm --filter @apify-actors/shared build
pnpm --filter google-maps-lead-generator build
pnpm --filter google-maps-lead-generator test
pnpm --filter google-maps-lead-generator run type-check
```