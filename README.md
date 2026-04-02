# Gainr Protocol — Tipster EPIC Wireframes

> **For developers.** Interactive HTML wireframe for the Gainr Tipster EPIC. 10 screens covering the full analyst and investor journeys. Matches the [gainr.pro](https://gainr.pro) design system exactly.

## Quick Start

```bash
git clone https://github.com/alanyoungcy/gainr_tips.git
cd gainr_tips
python3 -m http.server 8080
# Open http://localhost:8080
```

Deploy to Vercel: import `alanyoungcy/gainr_tips`, framework = **Other**, no build step.

---

## Screen Index

| # | Screen | Role | Key Features |
|---|--------|------|-------------|
| s1 | Analyst Leaderboard | Investor | Top performers carousel, sortable table, sparklines, $GAINR tier badges, sport/type/min-bets filters |
| s2 | Analyst Public Profile | Investor | Stats grid, donut betting summary, equity curve (5 tabs), locked bets paywall, Subscribe/Buy CTAs |
| s3 | Subscribe to Strategy Modal | Investor | Strategy subscription pricing, 67/33 fee split, USDC wallet, Auto-Betting upsell |
| s4 | Post a Signal | Analyst | Strategy selector, match browser, odds selection, target price, Kelly ratio, in-play trigger, Free/Premium toggle |
| s5 | Analyst Dashboard | Analyst | Revenue cards, equity curve, recent signals table, PRO status, Post New Signal CTA |
| s6 | Signal Feed | Investor | Live signal stream, locked/free/PRO-only states, sport/type filters |
| s7 | Analyst Onboarding | Analyst | 5-step wizard: Identity, Strategy, KYC, Qualification, PRO Earn |
| s8 | Auto-Betting Setup | Investor | Analyst selection, sportsbook connection, capital/stake params, ZK Stealth toggle |
| s9 | Markets | Investor | Sport categories, live market cards, no self-betting panel, analyst tip overlays |
| s10 | Portfolio | Investor | Strategy pots, pot P&L sparklines, cumulative equity curve, withdrawal flow, bet history |

---

## Access Control Rules

Two hard rules enforced across all screens:

1. **Individual picks are analyst-only.** Investors see aggregate stats and equity curves only. Actual selections (team, odds, stake) are always behind a subscribe paywall.
2. **No self-directed betting.** The platform is tips/signals only. Investors cannot place their own bets. The Markets screen shows odds for context only — no bet slip, no clickable odds.

---

## User Journey Flows

### Journey 1 — Analyst Onboarding and PRO Qualification

The analyst journey begins at the onboarding wizard (s7) and progresses through five sequential steps before the analyst can post premium signals and earn revenue.

```mermaid
flowchart TD
    A([Analyst visits Gainr]) --> B[s7: Analyst Onboarding Wizard]

    subgraph WIZARD ["5-Step Onboarding Wizard"]
        B --> S1[Step 1: Identity and Profile\nNom de plume · Real name · Background\nPrimary sport · Market type · Photo]
        S1 --> S2[Step 2: Strategy Setup\nStrategy name · Description\nTheoretical bank size · Risk profile]
        S2 --> S3[Step 3: KYC Verification\nzkMe privacy-preserving ID check\nPassport or national ID · Selfie]
        S3 --> S4[Step 4: Qualification Period\nPost FREE signals · Min 50 bets\nTracked against Pinnacle closing odds]
        S4 --> S5[Step 5: Go PRO and Earn\nROI 4pct+ unlocks PRO badge\nPricing auto-set by ROI tier]
    end

    S5 --> PRO{PRO Qualification\nROI 4pct or higher?}
    PRO -->|Yes| EARN[s5: Analyst Dashboard\nRevenue = 67pct of subscriber fees\nplus 7.5pct performance carry]
    PRO -->|No| CONT[Continue posting free signals\nRe-evaluated monthly]
    CONT --> S4

    EARN --> POST[s4: Post a Signal]
    POST --> PUBLISH{Visibility setting}
    PUBLISH -->|Premium| LOCK[Locked - subscribers only]
    PUBLISH -->|Free| OPEN[Visible to all followers]
```

**Key design decisions:**
- The nom de plume protects the analyst's IP while building a verifiable public track record.
- KYC uses zkMe's privacy-preserving protocol — identity is verified without storing raw documents.
- PRO status is evaluated monthly. If ROI drops below 4%, the analyst reverts to free-only posting.
- Pricing tiers are platform-set based on ROI, not analyst-set, to prevent manipulation.

---

### Journey 2 — Investor Discovery and Subscription

The investor journey starts at the leaderboard and flows through profile evaluation, subscription, and execution setup.

```mermaid
flowchart TD
    I([Investor visits Gainr]) --> LB[s1: Analyst Leaderboard\nBrowse top performers\nFilter by sport · tier · min bets]

    LB --> PROF[s2: Analyst Public Profile\nView stats: Bets · Profit · Yield · CLV\nSee equity curve and betting summary\nLocked bets paywall visible]

    PROF --> DECISION{Decision}
    DECISION -->|Follow Free| FOLLOW[Follow analyst\nFree signals only\nNo payment required]
    DECISION -->|Subscribe| SUB[s3: Subscribe Modal\nSubscribe to Strategy\nPay in USDC from wallet]
    DECISION -->|Browse more| LB

    SUB --> AUTOBET{Enable Auto-Betting?}
    AUTOBET -->|Yes| AB[s8: Auto-Betting Setup\nConnect sportsbook\nSet capital · stake · odds limits\nEnable ZK Stealth Execution]
    AUTOBET -->|No| MANUAL[Manual mode\nReceive signal alerts\nView picks in Tips Feed]

    AB --> EXEC[Bets auto-placed via ZK protocol\nFront-running prevented]
    MANUAL --> FEED[s6: Signal Feed\nView analyst signals\nSubscribed picks unlocked]

    EXEC --> PORT[s10: Portfolio\nTrack strategy pots\nView per-pot P&L and sparklines\nWithdraw from pot to balance to bank]
    FEED --> PORT
    FOLLOW --> PORT
```

**Key design decisions:**
- Investors can follow for free (see free signals only) or subscribe (unlock premium picks).
- The profile page shows all aggregate stats publicly — only the individual picks are gated.
- Auto-Betting uses ZK Stealth Execution to prevent front-running by other market participants.

---

### Journey 3 — Signal Lifecycle (Analyst to Investor to Settlement)

This sequence diagram shows the full lifecycle of a single signal across all system participants.

```mermaid
sequenceDiagram
    participant A as Analyst
    participant P as Platform
    participant SC as Smart Contract
    participant I as Investor
    participant AB as Auto-Betting Engine
    participant BK as Sportsbook

    A->>P: Post signal via s4\nSelection, Market, Target Price\nPoints, Kelly, Strategy, Visibility
    P->>SC: Record signal on-chain\ntimestamp, odds, analyst ID
    SC-->>P: Signal hash confirmed

    alt Premium Signal
        P->>I: Push notification to subscribers only
        I->>P: View locked pick in s6\nRequires active subscription
    else Free Signal
        P->>I: Push notification to all followers\nFull pick visible immediately
    end

    alt Auto-Betting enabled via s8
        P->>AB: Relay signal to auto-bet engine
        AB->>BK: Place bet via ZK Stealth Execution
        BK-->>AB: Bet confirmation and bet ID
        AB-->>P: Execution confirmed
    else Manual mode
        I->>P: Investor acknowledges signal\nNo bet placed by platform
    end

    Note over P,SC: Event settles

    SC->>SC: Verify result vs Pinnacle closing odds\nCalculate CLV and yield contribution
    SC->>P: Settlement: WON, LOST, or REFUNDED
    P->>I: Update s10 Portfolio\nPot P&L updated, history entry added
    P->>A: Revenue distributed\n67pct of subscription fees\nplus 7.5pct performance carry
    A->>P: View s5 Dashboard\nUpdated stats and revenue cards
```

---

### Journey 4 — Portfolio and Pot Management (Investor)

The portfolio is structured around strategy pots — each pot is tied to a specific analyst and strategy, allowing granular allocation and withdrawal.

```mermaid
flowchart TD
    DEPOSIT([Investor deposits USDC]) --> BAL[Account Balance\nUnallocated USDC shown in nav]

    BAL --> ALLOC[Allocate to Strategy Pot\nChoose analyst and strategy\nSet allocation amount in USDC]

    ALLOC --> POT1[Pot: AiTips / Value Football\n6840 USDC · ROI +14.0pct]
    ALLOC --> POT2[Pot: Luminaar / Asian Handicap\n4210 USDC · ROI +10.8pct]
    ALLOC --> POT3[Pot: Sportbettingfund / General\n1400 USDC · ROI +2.5pct]

    POT1 --> BETS[Bets auto-placed from pot\nPot value fluctuates with results]
    POT2 --> BETS
    POT3 --> BETS

    BETS --> SETTLE[Settlement per bet\nWON: pot grows\nLOST: pot shrinks\nREFUNDED: returned to pot]

    SETTLE --> VIEW[View in s10 Portfolio\nPer-pot sparkline and P&L\nCumulative equity curve across all pots]

    VIEW --> WITHDRAW{Withdraw?}
    WITHDRAW -->|From pot| WPOT[Withdraw from pot to Balance\nPartial or full withdrawal allowed]
    WITHDRAW -->|From balance| WBANK[Withdraw balance to Bank\nExternal wallet or bank account]
    WPOT --> BAL
    WBANK --> DONE([Funds returned to investor])
```

**Key design decisions:**
- Each strategy pot is independent — an investor can withdraw from one pot without affecting others.
- Pot value is always denominated in USDC, not units, for clarity.
- The cumulative equity curve aggregates across all pots to show total portfolio performance.

---

### Journey 5 — Revenue and Pricing Model

Analyst pricing is platform-controlled and auto-set based on verified ROI tier. This prevents race-to-the-bottom pricing and aligns incentives.

```mermaid
flowchart LR
    subgraph TIERS ["PRO Pricing Tiers (auto-set by platform based on verified ROI)"]
        T1["Tier 1\nROI 4 to 8pct\n$34.95 per month"]
        T2["Tier 2\nROI 8 to 15pct\n$44.95 per month"]
        T3["Tier 3\nROI 15pct+\n$59.95 per month"]
    end

    SUB([Subscriber pays monthly fee]) --> SPLIT{Revenue Split}
    SPLIT -->|67pct| ANALYST[Analyst Revenue\nPaid monthly\nMin 3 active subscribers to qualify]
    SPLIT -->|33pct| PLATFORM[Platform Fee\nOperations, smart contract\nVerification infrastructure]

    ANALYST --> CARRY[Plus 7.5pct Performance Carry\non net profits generated for subscribers\nAligns analyst with investor outcomes]

    CARRY --> TOTAL[Total Analyst Earnings\nBase 67pct plus 7.5pct carry]

    TOTAL --> DASH[Visible in s5 Analyst Dashboard\nRevenue cards and monthly breakdown]
```

**Key design decisions:**
- The 7.5% performance carry ensures analysts are incentivised to generate actual profits, not just volume.
- If an analyst's ROI drops below 4% in any rolling 30-day window, they revert to Tier 1 pricing automatically.
- Subscribers receive a pro-rated refund if their analyst's ROI drops below the minimum threshold mid-month.

---

## Design System

All CSS custom properties extracted from [gainr.pro](https://gainr.pro):

| Token | Value | Usage |
|-------|-------|-------|
| `--brand` | `#FF5A00` | Primary orange — CTAs, active states, highlights |
| `--brand-light` | `rgba(255,90,0,0.12)` | Orange tint — card backgrounds, badges |
| `--green` | `#1F7A39` | Positive P&L, win states, PRO badge, free badge |
| `--red` | `#D93025` | Negative P&L, loss states, live badge |
| `--dark` | `#1B0B0C` | Near-black — nav background |
| `--text` | `#1A1A1A` | Primary body text |
| `--mid-gray` | `#7A7F8C` | Secondary text, labels |
| `--border` | `#E2E4EA` | Card borders, dividers |
| `--font` | `Figtree` | All text — loaded from Google Fonts |
| `--radius-sm` | `8px` | Input fields, small cards |
| `--radius-md` | `12px` | Cards, modals |
| `--radius-lg` | `20px` | Large cards, containers |
| `--radius-pill` | `999px` | Buttons, badges, nav pills |

---

## Architecture Notes

**Single-file design.** `index.html` contains all HTML, CSS, and JS. No build step, no dependencies except Chart.js (CDN) and Figtree font (Google Fonts). This is intentional for the wireframe phase — it makes the file easy to share, review, and deploy.

**Screen switching.** `showScreen(id, btn)` hides all `.screen` divs and shows the target by toggling the `.active` class. The screen label at the top updates automatically. Charts are initialised lazily via `initCharts()` called on each screen switch with a 100ms debounce.

**Chart.js.** Sparklines (leaderboard table), equity curves (profile, dashboard, portfolio), donut charts (betting summary, portfolio breakdown), and pot sparklines (portfolio pots) all use Chart.js 4.4. Each canvas is guarded with `canvas._chartInit = true` to prevent double-initialisation on repeated screen switches.

**Analyst Onboarding Wizard.** `gotoStep(n)` controls the 5-step wizard by toggling `wpanel1`–`wpanel5` visibility and applying `active` or `completed` classes to `wstep1`–`wstep5` for the progress indicator.

**Markets interactivity.** `filterSport(sport)` filters market cards by sport category. All odds buttons are display-only — clicking them does nothing — to enforce the no-self-betting access control rule.

**Portfolio pots.** `initPotCharts()` renders sparklines for each strategy pot using Chart.js line charts with fill. Called automatically when the Portfolio screen (s10) is activated.
