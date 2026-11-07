# DSCR Analyzer

Investment property cash flow underwriting tool. Back-solves required rent for a target DSCR (1.00x by default), then lets you flex purchase price and rent with live sliders to see how coverage, cash flow, cap rate, and cash-on-cash respond.

## Features

- **⚡ Zillow Button** — one-click import from any Zillow listing (bookmarklet on desktop / iPhone Safari, or an iOS Share Sheet shortcut). Pulls price, sq ft, beds/baths, year built, tax history, Zestimate and Rent Zestimate straight from the listing page you're viewing.
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

Zillow has no public API and blocks cross-site fetches, so a static site can't load a pasted link by itself (pasting a link still fills in the address and ZIP from the URL). The **⚡ Zillow Button** gets around this by running on the Zillow page in *your* browser: it reads the listing JSON Zillow already sent to the page (`__NEXT_DATA__` / `gdpClientCache`) and opens SFRcalc with the data in the URL hash (`#zimport=…`). No server, no API key, and nothing is sent anywhere else. Open **⚡ Zillow Button** in the app for install steps.

If Zillow changes its page structure, the button may need an update; the source is `sfrZillowGrab()` in `index.html`.

## License

MIT — do whatever you want with it.
