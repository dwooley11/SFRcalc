# DSCR Analyzer

Investment property cash flow underwriting tool. Back-solves required rent for a target DSCR (1.00x by default), then lets you flex purchase price and rent with live sliders to see how coverage, cash flow, cap rate, and cash-on-cash respond.

## Features

- **Manual address entry** with optional Zillow paste-parser (pulls price, sqft, zip from copied listing text)
- **Zillow Tax History parsing** — paste the property's tax table and the app computes the actual effective tax rate from the most recent year's bill ÷ assessed value, then applies it to your purchase price (catches the Texas post-sale reassessment bump)
- **Auto-calculated insurance** based on a configurable rate per sq ft + minimum premium
- **ZIP-code-driven property tax rates** as a fallback when Zillow tax data isn't pasted, with editable lookup table (pre-loaded with Central Texas / McLennan County rates)
- **Back-solve rent** for any target DSCR
- **Live sliders** for price and rent — DSCR, cash flow, cap rate, and CoC update in real time
- **Fully adjustable financing** — down payment, rate, term
- **Operating expense controls** — vacancy, management, maintenance, CapEx reserve
- **No backend** — pure HTML/JS, deploys as static site
- **Local storage** — settings persist in your browser

## Deploy to GitHub Pages

1. Create a new GitHub repo
2. Push these files (or just `index.html`)
3. Settings → Pages → Source: `main` branch, `/` root
4. Done. Your app lives at `https://<username>.github.io/<repo-name>/`

## Local Use

Just open `index.html` in any browser. No build step, no dependencies.

## Configuration

Click **Settings** (top right) to set:
- Insurance premium per sq ft (default $0.65, typical Central Texas DP-3 range)
- Minimum insurance premium (default $850)
- Property tax rates by ZIP (pre-loaded with Waco-area ZIPs)
- Default tax rate fallback (default 2.30%)

All settings save to localStorage.

## Math

```
Loan Amount    = Price × (1 − Down %)
Monthly P&I    = standard amortization formula
Annual Tax     = Price × ZIP-specific rate
Annual Ins     = max(SqFt × $/sqft, minimum)
EGI            = Gross Rent × (1 − Vacancy)
Variable Opex  = Gross Rent × (Mgmt + Maint + CapEx) %
NOI            = EGI − Variable Opex − Tax − Ins
DSCR           = NOI / Annual Debt Service
Cash Flow      = NOI − Annual Debt Service
Cap Rate       = NOI / Price
Cash-on-Cash   = Cash Flow / Down Payment

Required Rent  = (TargetDSCR × ADS + Tax + Ins) / (12 × (1 − (Vacancy + Mgmt + Maint + CapEx)/100))
```

## Notes on Zillow

Zillow killed their public API in 2021. This app does **not** scrape Zillow — instead, you paste listing text from a Zillow page and the parser extracts what it can. If you want true API-driven pulls, look at RapidAPI Zillow scrapers (~$10–50/mo) or RentCast's free tier (50 calls/mo).

## License

MIT — do whatever you want with it.
