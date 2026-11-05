<!-- PUBLISH AFTER: google-maps-businesses is public on the Apify Store -->
# Google Maps Business Scraper: Python example

**The problem:** You need a list of local businesses for a trade in a city (name, phone, website, address, rating) without copying them from Google Maps by hand.

**The tool:** [Google Maps Business Scraper](https://apify.com/aurelix/google-maps-scraper), an Apify Actor by Aurelix. This repository is a small, runnable Python example for it.

**The price:** $1.50 per 1,000 places on Apify's free plan and $1.05 on paid plans, plus $0.001 per run; you pay only for results. Apify's free plan includes $5 of credit a month.

**Try it** (needs an Apify API token from Console > Settings > API & Integrations):

```bash
pip install -r requirements.txt
export APIFY_TOKEN=your_token_here
python main.py
```

[![Google Maps Business Scraper: 75-second demo video](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/google-maps-scraper-demo-thumb-v2.png)](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/google-maps-scraper-demo-v2.mp4)

**[▶ Watch the 75-second demo](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/google-maps-scraper-demo-v2.mp4)**

## What it does

Google Maps Business Scraper turns Google Maps searches into one row per business: name, all categories, full and split address, phone in E.164, website, average rating and review count, opening hours for all 7 days, coordinates, time zone, place ID and CID. Give search terms and locations; every term runs in every location, up to 300 places each, and a place found twice is returned once. Review text and reviewer names are never collected.

## Why this over alternatives

Compared with the leading alternatives on the Apify Store (full table on the Store page):

- $1.50 per 1,000 places, less than half the $4-5 the two leading Google Maps scrapers charge.
- Website emails and socials at $1.50 per 1,000, charged only when found (the leaders charge $2.00-2.50).
- Many locations in one run and an "only new places since last run" mode, which neither leader has. Not offered: review text and rating/category filters.

## When to use it

- Agencies and B2B sellers building local lead lists by trade and city
- Franchise and retail teams mapping competitors and locations in a market
- Local SEO consultants comparing ratings, review counts and categories in an area
- Data teams adding coordinates, hours and place IDs to store or prospect lists

## Input example

This is the `plumbers-toronto` input used in `main.py`:

```json
{
  "searchTerms": [
    "plumbers"
  ],
  "locations": [
    "Toronto ON"
  ],
  "maxResultsPerSearch": 100
}
```

Every input field is documented on the [Store page](https://apify.com/aurelix/google-maps-scraper).

## How to run

1. Get an Apify API token from Apify Console (Settings, API & Integrations).
2. Install and run:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export APIFY_TOKEN=your_token_here
python main.py
```

The script starts a run of `aurelix/google-maps-scraper`, prints a few fields from the first results and writes every result to `results.csv`. Lists and nested objects are stored in the CSV as JSON text. Each run is billed to your Apify account (see Pricing below).

## Sample output

One record as returned by a real run (long text is shortened):

```json
{
  "place_id": "ChIJ243-C381K4gRHMKWCGU86iM",
  "cid": "2587947340511232540",
  "name": "Black+Blue Toronto",
  "category": "Restaurant",
  "categories": [
    "Restaurant",
    "Brunch restaurant",
    "Event venue",
    "Fine dining restaurant",
    "Lounge bar",
    "... [4 more]"
  ],
  "address": "130 King St W, Toronto, ON M5X 2A2, Canada",
  "street": "130 King St W",
  "neighborhood": "Old Toronto",
  "city": "Toronto",
  "region": "Ontario",
  "postal_code": "M5X 2A2",
  "country_code": "CA",
  "phone": "+16473688283",
  "phone_display": "+1 647-368-8283",
  "website": "https://blackandbluesteakhouse.ca/toronto-home/",
  "rating": 4.8,
  "review_count": 6748,
  "hours": [
    {
      "day": "Monday",
      "hours": "11:30 AM–12 AM"
    },
    {
      "day": "Tuesday",
      "hours": "11:30 AM–12 AM"
    },
    {
      "day": "Wednesday",
      "hours": "11:30 AM–12 AM"
    },
    {
      "day": "Thursday",
      "hours": "11:30 AM–12 AM"
    },
    {
      "day": "Friday",
      "hours": "11:30 AM–12 AM"
    },
    "... [2 more]"
  ],
  "latitude": 43.648099599999995,
  "longitude": -79.3831247,
  "timezone": "America/Toronto",
  "google_maps_url": "https://www.google.com/maps/place/?q=place_id:ChIJ243-C381K4gRHMKWCGU86iM",
  "search_term": "restaurant",
  "search_location": "Toronto, ON",
  "source_url": "https://www.google.com/maps/place/?q=place_id:ChIJ243-C381K4gRHMKWCGU86iM",
  "scraped_at": "2026-11-04T14:24:30Z"
}
```

## Export to CSV

`main.py` writes `results.csv` with the `csv` module. You can also download any run's results directly:

```
https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true
```

Send your token in an `Authorization: Bearer <token>` header (or `?token=`) if the dataset is not readable without one. Other formats: `format=json`, `format=xlsx`.

## Google Sheets

- Simplest: use Apify's Google Sheets integration on the Actor or a saved task, so each run's results land in a sheet with no token in the sheet itself.
- Quick pull of one run: `=IMPORTDATA("https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true")`. This only works without a token if your account lets anyone with the dataset ID read it. Do not paste an API token into a sheet formula: anyone who can view or copy the sheet can read it and use your account.

## n8n

- Apify node: choose "Run an Actor", enter `aurelix/google-maps-scraper`, paste the input above, then read the run's dataset items.
- HTTP Request node: `POST https://api.apify.com/v2/acts/aurelix~google-maps-scraper/run-sync-get-dataset-items` with header `Authorization: Bearer <token>` (stored as an n8n credential) and the input as the JSON body. The response is the list of results. This endpoint waits for the run to finish, so for large runs start the run asynchronously and fetch the dataset when it ends.

## Limits

- Up to 300 places per search term and location (fewer when Google has fewer); emails are not on Google Maps, so use the website with the Contact Details Scraper.
- Fields are read from Google Maps' search results; Google changes this format from time to time, and the daily health check flags any break.
- Rating, review count, website and hours are null or empty when the business has none on Google Maps.
- Places are listed in Google's order for the search, which depends on relevance and location.
- category is the category Google lists first for your search, usually the one matching your search term (e.g. Piano tuning service for a music store found by 'piano tuner'); the business's other categories, including its main one, are in categories.
- Places whose address Google hides have no address and no coordinates.
- A place whose name contains a name on the suppression list is skipped (titles such as Dr., case and accents ignored).

## Pricing

At the time of writing:

Pay per event: you pay only for what the run returns, plus a small start fee. Bronze, Silver, Gold and higher Apify plans pay less per result.

| Event | Free plan | Paid plans (Bronze, Silver, Gold) |
|---|---|---|
| `place-result` | $1.50 per 1,000 places | $1.05 per 1,000 places |
| `contact-result` | $1.50 per 1,000 places | $1.05 per 1,000 places |
| `apify-actor-start` | $0.001 per run | $0.001 per run |

Example: 1,000 places cost $1.50 on the free plan and $1.05 on a paid plan, plus the start fee. Apify's free plan includes $5 of credit a month, enough for about 3,300 places.

Places already returned in the run, places returned by an earlier run (with 'Only new places since last run'), suppressed places, geographic results (cities, areas) and failed searches are not charged. Website contacts are charged as a contact-result only when at least one email, extra phone or social link was found on the place's website.

Check the [Store page](https://apify.com/aurelix/google-maps-scraper) for current prices.

## Links

- Actor: https://apify.com/aurelix/google-maps-scraper
- Questions, bugs and feature requests: the Issues tab on the Store page.

## Licence

The example code in this repository is released under the MIT licence (see `LICENSE`). The Actor itself runs on Apify and is billed through your Apify account.
