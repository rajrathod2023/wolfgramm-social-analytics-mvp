# Wolfgramm Social Analytics MVP

An n8n workflow prototype that provides standardised Instagram and Facebook analytics through a public JSON API.

## Project purpose

This MVP demonstrates how social-media analytics data could be collected, processed and prepared for a future reporting portal.

The current version uses mock Metricool-style data so the workflow structure can be developed and tested before connecting a live analytics API.

## Live endpoint

```text
https://raj-social-analytics.app.n8n.cloud/webhook/metricool-analytics
```

### Instagram

```text
https://raj-social-analytics.app.n8n.cloud/webhook/metricool-analytics?platform=instagram
```

### Facebook

```text
https://raj-social-analytics.app.n8n.cloud/webhook/metricool-analytics?platform=facebook
```

## Workflow

The n8n workflow contains four stages:

1. **Webhook** — receives a GET request and reads the `platform` query parameter.
2. **Load Dummy Metricool Data** — supplies mock Instagram and Facebook analytics.
3. **Process & Standardise Analytics** — selects the requested platform and calculates reporting rates.
4. **Respond to Webhook** — returns structured JSON to the requester.

## Supported platforms

- Instagram
- Facebook

Platform names are processed in lowercase. If no platform is supplied, the workflow defaults to Instagram.

## Calculated metrics

The workflow calculates:

- Engagement rate = engagement ÷ reach × 100
- Follower growth rate = follower growth ÷ followers × 100
- Click-through rate = link clicks ÷ impressions × 100

Rates are rounded to two decimal places.

## Example successful response

```json
{
  "success": true,
  "source": "mock-metricool",
  "platform": "Instagram",
  "reportingPeriod": {
    "start": "2026-08-01",
    "end": "2026-08-23"
  },
  "views": 18600,
  "reach": 11400,
  "impressions": 21500,
  "engagement": 1260,
  "engagementRate": 11.05,
  "followers": 3420,
  "followerGrowth": 126,
  "followerGrowthRate": 3.68,
  "profileVisits": 640,
  "linkClicks": 184,
  "clickThroughRate": 0.86,
  "postsPublished": 12
}
```

## Invalid-platform handling

An unsupported platform produces a clear JSON response:

```json
{
  "success": false,
  "source": "mock-metricool",
  "error": "Unsupported platform",
  "requestedPlatform": "tiktok",
  "supportedPlatforms": [
    "instagram",
    "facebook"
  ]
}
```

## Testing completed

| Test | Expected result | Status |
|---|---|---|
| `platform=instagram` | Structured Instagram analytics | Passed |
| `platform=facebook` | Structured Facebook analytics | Passed |
| `platform=tiktok` | Clear unsupported-platform response | Passed |

## Importing the workflow

1. Download `WH Social Analytics MVP.json` from this repository.
2. Open an n8n workspace.
3. Create or open a workflow.
4. Select **Import from File**.
5. Choose the downloaded JSON file.
6. Review the webhook URL before publishing.

## Current limitations

- The dataset is mock data and is not connected to the live Metricool API.
- The reporting dates and metrics are fixed demonstration values.
- The endpoint does not currently use authentication.
- Invalid-platform responses currently return a clear error body using the workflow’s standard webhook response.
- Human review remains necessary before using analytics in company reporting or published claims.

## Future improvements

- Connect the workflow to the Metricool API.
- Store credentials securely in n8n.
- Add authentication and access controls.
- Add dynamic date-range parameters.
- Return dynamic HTTP status codes.
- Store historical analytics for comparison.
- Connect the standardised output to a reporting portal.
- Add automated tests and error notifications.

## Technology

- n8n Cloud
- JavaScript
- Webhooks
- JSON
- GitHub

## Author

Raj Rathod  
AI & Cloud Engineer Intern, Wolfgramm Holdings Ltd
