# DSCR Analyzer

Investment property cash flow underwriting tool. Everything updates live as you type. Rent starts at the Zillow Rent Zestimate (or the rent needed to hit your target DSCR), down payment starts at 25%, and a single Hit Target DSCR panel back-solves rent, cash down, or max price.

## Features

- **⚡ Zillow Button** — one-click import from any Zillow listing (bookmarklet on desktop / iPhone Safari, or an iOS Share Sheet shortcut). Pulls price, sq ft, beds/baths, year built, tax history, Zestimate and Rent Zestimate straight from the listing page you're viewing.
- **Manual address entry** with optional Zillow paste-parser (pulls price, sqft, zip from copied listing text)
- **Zillow Tax History parsing** — paste the property's tax table and the app computes the actual effective tax rate from the most recent year's bill ÷ assessed value, then applies it to your purchase price (catches the Texas post-sale reassessment bump)
- **Auto-calculated insurance** based on a configurable rate per sq ft + minimum premium
- **ZIP-code-driven property tax rates** as a fallback when Zillow tax data isn't pasted, with editable lookup table (pre-loaded with Central Texas / McLennan County rates)
- **Hit Target DSCR panel** — three answers for your target (saved between visits): rent needed, cash down needed, and max price, each holding the other two levers where they are, with one-tap Apply
- **Cash Down + Down %** — starts at 25%; type an exact cash amount to lock it as price moves, or a % to scale with price
- **Save Report as PDF** — one-page report with listing photo, purchase & financing math, assumptions, target-DSCR solves, and a monthly/annual income & cash-flow projection
- **Live sliders** for price and rent — DSCR, cash flow, cap rate, and CoC update in real time
- **Fully adjustable financing** — cash down / down %, rate, term
- **Operating expense controls** — vacancy, management, maintenance, CapEx reserve
- **Saved deals + Portfolio tab** — *Add to portfolio* or *Save to watching* from any analysis. The Portfolio tab totals every In-portfolio deal against your cash to deploy (down + closing costs), shows cost, loan, monthly expense, rent, cash flow and DSCR per property, and charts a 10-year wealth build. *Open in analyzer* reloads a deal with all its numbers; saving again updates it. Deals live in this browser; copy/paste a backup to move them to another device.
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

RentNOI        = Gross Rent × (1 − (Vacancy + Mgmt + Maint + CapEx)/100) − Ins
k              = annual P&I per $1 of loan
Down Needed    = Price − (RentNOI − Price × TaxRate) / (TargetDSCR × k)
Max Price      = RentNOI / (TaxRate + TargetDSCR × k × (1 − Down %))        (Down % held)
               = (RentNOI + TargetDSCR × k × CashDown) / (TaxRate + TargetDSCR × k)   (Cash Down held)
```

## Notes on Zillow

Zillow has no public API and blocks cross-site fetches, so a static site can't load a pasted link by itself so the app no longer asks for a link. The **⚡ Zillow Button** gets around this by running on the Zillow page in *your* browser: it reads the listing JSON Zillow already sent to the page (`__NEXT_DATA__` / `gdpClientCache`) and opens SFRcalc with the data in the URL hash (`#zimport=…`). No server, no API key, and nothing is sent anywhere else. Setup steps for computer, iPhone/iPad (bookmark or Share Sheet shortcut) and Android are on the app's home screen.

If Zillow changes its page structure, the button may need an update; the source is `sfrZillowGrab()` in `index.html`.

## License

MIT — do whatever you want with it.
