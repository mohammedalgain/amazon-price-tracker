# Amazon.sa Price Tracker

A small scraper that pulls a product's price, rating, and brand from **Amazon.sa** and logs each check to a CSV, so the price can be tracked over time.
Used for educational purposes 

## Tech stack

- **BeautifulSoup** + **Requests** — page scraping
- **Pandas** — reading back and inspecting the collected data
- **CSV** — lightweight storage for price history

## How it works

1. `scrape_product(url)` fetches a product page and pulls out title, price, brand, and rating.
2. `log_to_csv(row)` appends that reading to `AmazonWebScraperDataset.csv`, writing the header only on the first run.
3. `check_price()` ties the two together, and can be run once or on a loop (`CHECK_INTERVAL_SECONDS`, default 24 hours) to build up a price history for a product.

## Setup

```bash
pip install -r requirements.txt
```

Open `amazon_price_tracker.ipynb` and run the cells — you'll be prompted to paste in the Amazon.sa product URL you want to track.

## Sample data

`AmazonWebScraperDataset.csv` includes a few sample rows showing the output format, collected from an [Unmatched: Cobble & Fog](https://www.amazon.sa/-/en/Unmatched-Cobble-Dracula-Sherlock-Invisible/dp/B095MQ3BLN) board game listing.

## Known limitations

- **Bot detection:** Amazon.sa occasionally serves a CAPTCHA / bot-detection page instead of the real listing. A custom `User-Agent` header helps but isn't a complete workaround, so `scrape_product` returns `None` (and logs nothing) rather than crashing when this happens.
- **Email alerts:** `smtplib` is imported as scaffolding for a "notify me when the price drops" feature, but that logic isn't wired in yet.
- **No retry/backoff:** a failed request currently just skips that check rather than retrying.

## Roadmap

- [ ] Wire up email alerts on price drops
- [ ] Retry with backoff on failed requests
- [ ] Support tracking multiple products in one run
