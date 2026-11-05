# Outlier Finder

A single-file web tool (`index.html`) that searches YouTube for **outlier videos and Shorts**: uploads that got far more views than their channel usually gets.

## Use it

1. Get a free API key: Google Cloud Console → APIs & Services → enable **YouTube Data API v3** → Credentials → Create API key.
   (Optional but recommended: restrict the key to the YouTube Data API v3 only.)
2. Open `index.html` in a browser (double-click it, or host it on GitHub Pages).
3. Paste your key, choose **Keyword**, **Hashtag** or **My niche** mode, set your filters and click **Find outliers**.

Live version: https://trentic.github.io/Outlier/

## Filters

| Filter | What it does |
|---|---|
| Format | Shorts + long-form, Shorts only, or long-form only |
| Min views | Hide videos below this view count |
| Min / max subscribers | Limit results to a channel-size range (channels that hide their sub count are excluded when either is set) |
| Min outlier score | Only show videos at or above this multiple of the channel's median (updates live) |
| Language | **English only** keeps videos whose language setting is English. For videos with no setting, it checks the title and description (non-Latin script, or common Spanish/Portuguese/French/German/etc. words, means it's dropped). It also tells YouTube's search to prefer English. |
| Uploaded | Any time, 24h, 7d, 30d, 90d, 1y, or a custom date range |
| Advanced | Search order, number of result pages (50 videos each), baseline size, region, language |

## Refining results and saved searches

- **Filters apply to your results instantly.** After a search, changing min views, subscriber range, format, language, date range, min score or the hashtag check updates the table straight away, at no quota cost. Only **Find outliers** runs a new search.
- There's also a **Filter results** box for finding videos by title or channel.
- If loosening a filter lets in videos whose channel hasn't been scored yet, a **Score N more channels** button appears. It costs about 2 units per channel.
- **Your last search is saved in your browser.** On refresh, your results, settings, mode and channel analysis all come back with no quota cost. **Clear** removes the saved results.

## The outlier formula

```
outlier score = video views ÷ median views of the channel's last N uploads
```

- The baseline leaves out the video being scored and any upload younger than 48 hours.
- Shorts are compared with the channel's Shorts, and long-form videos with its long-form videos. If a channel has fewer than 5 uploads of that format, all of its uploads are used.
- A video counts as a Short if it runs 3 minutes or less, or has `#shorts` in its title or description.
- The table also shows views/sub and views/day so you can sort by them.

## My niche mode

Enter your channel (`@handle`, channel URL, channel ID, or a link to one of your videos) and click **Analyze channel**. This costs about 3 quota units. The tool reads your last 50 uploads and shows:

- your subscriber count, Shorts vs long-form mix, median views and upload rate
- the **topics** you make content about: 1–3 word phrases from your titles and tags that appear in several videos, with videos that did well for you counting more
- the **hashtags** you use, leaving out generic ones like `#shorts` and `#viral`

The top 4 topics start selected. Click chips to turn them on or off, or add your own. Each selected topic is a separate search, about 100 units per page. **Match my channel** sets the format to your main format and the subscriber range to your sub count ÷10 to ×10, so you see outliers from channels near your size. Your own videos are left out of the results, and each result shows which topic found it.

## Hashtag mode

The YouTube API has no endpoint for hashtag pages, so the tool searches for `#tag`. With **"Only videos that actually use the hashtag"** turned on, it keeps only videos whose title, description or tags contain exactly that hashtag (`#fitness` won't match `#fitnessgear`).

## Deep scan

YouTube's search doesn't sort *every* matching video by views. It sorts only a limited set it picks itself, so a huge outlier can be missing from a normal search. **Deep scan** splits the date range into time windows (3, 5 or 8) and asks for the most-viewed videos in each one, plus one relevance search over the whole range. Each window has its own top list, so many more big videos come back. In hashtag mode it also searches the tag without the `#` (`starwars`), and you can add extra terms like `star wars`.

Cost: windows + 1 searches per search term, at 100 units each. A hashtag with 5 windows costs about 1,200 units, or about 1,800 with one extra term. The page shows the estimate before you search.

## Why isn't a video showing?

Paste any video link into the checker. It tests the video against your current mode, filters and last search, then tells you whether:
- a filter removed it, and which one (for example "3,000,000 subs; range is 0–1,000,000"), or
- it passes every filter but YouTube's search didn't return it, in which case try Deep scan.

Each check costs about 2–4 units.

## Quota

The free daily quota is 10,000 units and resets at midnight Pacific time.

- Each search page costs 100 units.
- Video and channel lookups cost about 1 unit per 50 videos or channels.
- Each channel baseline costs about 2 units. Baselines are cached in your browser for 12 hours.

A typical 2-page search costs about 210–300 units, so you get roughly 30–45 searches a day. The page shows a running estimate of the quota you've used.

## Privacy

Your API key is only stored if you tick "Remember in this browser", and then only in that browser's localStorage. Requests go directly from your browser to `googleapis.com`.
