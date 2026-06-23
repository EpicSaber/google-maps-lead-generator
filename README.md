[Google Maps Lead Generator](https://apify.com/strange-advanced-marketing/google-maps-lead-generator?fpr=data)

Extract business leads from Google Maps with automatic enrichment. Get names, phones, websites, addresses, ratings, reviews - then automatically visit each website to find emails, social media profiles, and score each lead.

## What it does

1. **Searches Google Maps** for any business type and location
2. **Extracts business data** - name, phone, website, address, rating, review count, category
3. **Enriches each lead** by visiting their website:

- Finds email addresses
- Finds Instagram, Facebook, TikTok, LinkedIn, Twitter profiles
4. **Scores each lead** as Hot, Warm, or Cold based on their digital presence:

- **Hot** = No website or no digital presence (easiest to sell to)
- **Warm** = Has website but missing email or social media
- **Cold** = Has everything (website, email, social media)

## Use cases

- **Lead generation** for marketing agencies
- **Prospecting** for B2B sales teams
- **Market research** for any industry
- **Competitor analysis** by location
- **Finding businesses that need marketing help** (Hot leads)

## AI-friendly

This actor has a clean API that any AI model can call. Ask your AI assistant to "find me 20 plumbers in Dallas" and it can use this actor to deliver results.

## Input

| Field | Type | Description |
| --- | --- | --- |
| searchQuery | string | What to search on Google Maps (e.g., "plumbers in Dallas TX") |
| maxResults | integer | Maximum results to extract (1-100, default: 20) |
| enrichLeads | boolean | Visit websites for email/social enrichment (default: true) |

## Output

Each result includes:

- `name` - Business name
- `phone` - Phone number
- `website` - Website URL
- `address` - Full address
- `rating` - Google Maps rating (1-5)
- `review_count` - Number of reviews
- `category` - Business category
- `emails` - Email addresses found on website
- `social_instagram` - Instagram handle
- `social_facebook` - Facebook page
- `social_tiktok` - TikTok handle
- `social_linkedin` - LinkedIn profile
- `social_twitter` - Twitter/X handle
- `has_website` - true/false
- `has_email` - true/false
- `has_social_media` - true/false
- `lead_score` - hot/warm/cold
- `maps_url` - Direct Google Maps link

## Built by

**Strange Advanced Marketing (S.A.M.)**
AI-powered business tools for trade contractors and small businesses.