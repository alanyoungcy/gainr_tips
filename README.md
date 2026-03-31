# Gainr Protocol — Tipster EPIC Wireframes

High-fidelity interactive wireframe for the **Gainr Tipster / Tips-Selling EPIC**, built to match the [gainr.pro](https://gainr.pro) design system.

## Screens

| # | Screen | Description |
|---|--------|-------------|
| 1 | **Analyst Leaderboard** | Investor discovery — top performers carousel, sortable table with sparklines, $GAINR tier badges, sport/type filters |
| 2 | **Analyst Public Profile** | Full stats (Bets, Profit, Yield, CLV, equity curve), betting summary donut, locked bets paywall, sport breakdown table |
| 3 | **Subscribe / Buy Tips Modal** | Monthly vs Single Tip pricing, fee breakdown (67/33 split), wallet balance, Auto-Betting upsell |
| 4 | **Post a Tip (Analyst View)** | Match browser → odds selection → bet slip with Free/Premium visibility toggle, stake units, analysis field |
| 5 | **Analyst Dashboard** | Revenue cards, equity curve, recent tips table, PRO status, Post New Tip CTA |
| 6 | **Tips Feed** | Live tip stream with locked/free/PRO states, featured match distribution bar, sport/type filters |
| 7 | **Analyst Onboarding** | 5-step wizard: Profile → KYC → Free Tips → PRO Qualification → Earn Revenue; PRO requirements & revenue tier table |
| 8 | **Auto-Betting Setup** | Analyst selection, sportsbook connection, betting parameters, ZK Stealth execution toggle, activation summary |

## Design System

- **Font:** Figtree (Google Fonts)
- **Primary:** `#FF5A00` (Gainr Orange)
- **Background:** `#F6F8FC`
- **Nav:** Glassmorphism pill with backdrop-blur
- **Cards:** White, `border-radius: 16px`, subtle shadow
- **Badges:** PRO (orange), Free (green), Gold/Silver/Bronze ($GAINR tiers)

## Deploy to Vercel

1. Import this repo in [Vercel](https://vercel.com)
2. Framework: **Other** (static HTML)
3. Deploy — no build step required

## Reference

- [bet2invest.com](https://bet2invest.com) — functional reference for tipster marketplace
- [gainr.pro](https://gainr.pro) — design system reference
- Gainr Elaborated PRD — EPIC: Professional Analyst / Syndicate Tips Selling
