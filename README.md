# Wayback Machine Scraper — Python, Node.js & cURL examples

Get the **Internet Archive Wayback Machine** history of any website: **every snapshot** with date and status, **all archived URLs of a domain** (find deleted pages to redirect), the **capture closest to a date**, and the **title and text of old page versions**. **$0.50 per 1,000 snapshots**.

Hosted on Apify: **[Wayback Machine Scraper](https://apify.com/tidydata/wayback-machine-scraper)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/wayback-machine-scraper").call(run_input={
    "urls": [
        "example.com"
    ],
    "mode": "urls",
    "matchType": "domain",
    "mimeType": "text/html",
    "maxResultsPerUrl": 1000
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['url', 'date', 'statusCode', 'archiveUrl']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/wayback-machine-scraper').call({"urls": ["https://example.com/pricing"], "collapse": "year", "fetchContent": "text"});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~wayback-machine-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"urls": ["https://example.com/pricing"], "collapse": "year", "fetchContent": "text"}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
