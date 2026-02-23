# Twitter (X) Followers Scraper + Optional Email Enrichment for n8n (Apify Agent)

Build a production-ready **Twitter/X lead generation workflow** in n8n using two Apify actors:

- **Followers/Following collection** (usernames at scale)
- **Optional email enrichment** (only when you need it)

This template is designed for agencies, founders, growth teams, and sales ops who need a practical **Twitter scraper workflow**, not a toy demo.

## Why this template

Most teams need the same pipeline:

1. Put an X profile link
2. Choose **followers / following / both**
3. Choose amount (**exact number** or max)
4. Decide if email enrichment is needed
5. Get structured output for outreach and CRM

This template ships that flow as a conversational webhook agent inside n8n.

## What you get

- Conversational input flow (asks for missing fields)
- Username extraction from `x.com` / `twitter.com` links
- Hardcoded actor wiring for stability
- Optional email step (`includeEmails=true/false`)
- JSON response + CSV-ready payload
- Sticky notes inside workflow for quick onboarding

## Apify actors used

### 1) Twitter (X) Follower Scraper
- Actor ID: `bIYXeMcKISYGnHhBG`
- Link: [Open actor](https://console.apify.com/actors/bIYXeMcKISYGnHhBG/source)
- Typical use: scrape followers/following as usernames or IDs

### 2) Twitter (X.com) Email Scraper
- Actor ID: `mSaHt2tt3Z7Fcwf0o`
- Link: [Open actor](https://console.apify.com/actors/mSaHt2tt3Z7Fcwf0o/source)
- Typical use: enrich username lists with emails when available

## Input schema

```json
{
  "targetLink": "https://x.com/elonmusk",
  "collectType": "followers",
  "limitMode": "number",
  "limit": 1000,
  "includeEmails": true,
  "outputTarget": "json"
}
```

### Field reference

- `targetLink`: X/Twitter profile URL or `@username`
- `collectType`: `followers` | `following` | `both`
- `limitMode`: `number` | `all`
- `limit`: required when `limitMode="number"`
- `includeEmails`: `true` runs email actor, `false` skips email stage
- `outputTarget`: currently best used as `json`

## Example request (cURL)

```bash
curl -X POST 'https://YOUR_N8N_DOMAIN/webhook/apify-agent-collector' \
  -H 'Content-Type: application/json' \
  -d '{
    "targetLink":"https://x.com/elonmusk",
    "collectType":"followers",
    "limitMode":"number",
    "limit":1000,
    "includeEmails":true,
    "outputTarget":"json"
  }'
```

## Example response (summary)

```json
{
  "ok": true,
  "mode": "run_complete",
  "targetUsername": "elonmusk",
  "collectType": "followers",
  "totalCollected": 1000,
  "emailsFound": 2,
  "googleSheetsPushed": false
}
```

## Setup in n8n

1. Import `workflow/apify-agent-collector-conversational.json`
2. Create/attach **Apify credentials** (`Apify account`) in n8n
3. Test with webhook test URL
4. Publish + activate for production webhook

## How the decision to collect emails works

This is explicit and controllable:

- `includeEmails=true` -> workflow runs **Email Actor**
- `includeEmails=false` -> workflow skips email branch

So you can run faster/cheaper follower-only jobs and enable enrichment only when needed.

## Where results go

Current template returns results directly in webhook response:

- summary metrics
- normalized rows
- CSV-ready string

You can extend with:

- Google Sheets append
- Telegram summary notifications
- CRM sync (HubSpot, Pipedrive, Airtable, custom API)

## SEO topics this template targets

- twitter follower scraper n8n
- x followers scraper automation
- twitter lead generation workflow
- apify n8n integration
- twitter email enrichment pipeline
- scrape twitter following at scale
- no-code x.com growth data workflow

## Repository structure

```text
.
├── README.md
└── workflow
    └── apify-agent-collector-conversational.json
```

## Notes

- Actor pricing/limits are defined by Apify actor pages.
- Email discovery is availability-dependent (not every profile has discoverable email).
- For production use, add retries, logging, and destination storage.

## License

The Unlicense (Public Domain). See `LICENSE`.
