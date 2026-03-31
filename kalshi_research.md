# Kalshi Sports Markets — Research Notes

## Layout Structure
- **Left sidebar**: Sport category filters (All, Basketball, Baseball, Tennis, Soccer, Hockey, Golf, MMA, Cricket, Football, Esports, Motorsport, etc.) with counts
- **Sub-filters**: Games, Props, Futures, Tournament Props, Awards, Divisions, Draft, Set Winner, Total Maps, Wins, Events
- **Main feed**: Market cards (2-column or full-width)
- **Right panel**: Bet slip (Buy/Sell, Yes/No price, Amount input)

## Market Card Structure
Each card contains:
- Sport badge (colored pill: TENNIS, SOCCER, CRICKET, GOLF, ESPORTS)
- League/Tournament name (top right)
- Match title (bold, large)
- Status: LIVE badge + time, or scheduled date/time
- Team 1 row: team name + score (if live) + multiplier (e.g. 1.25x) + probability % button
- Team 2 row: same
- Tie row (for soccer): multiplier + probability %
- Volume traded (e.g. $2,645,544 vol)
- "N markets" link (e.g. "2 markets", "3 markets", "132 markets")

## Probability Display
- Shown as percentage on green pill buttons (e.g. 74%, 28%)
- Also shown as multiplier for futures (e.g. 15.6x for 6%)
- Yes/No binary market format

## Bet Slip (Right Panel)
- Buy / Sell toggle
- Yes price (e.g. Yes 74¢) / No price (e.g. No 27¢)
- Amount input in Dollars
- "Earn 3.25% Interest" note
- Sign up CTA

## Key UI Patterns for Gainr Adaptation
1. **Sport filter tabs** — horizontal scrollable pills at top
2. **Market type sub-filters** — Games | Props | Futures | etc.
3. **Live indicator** — red dot + "LIVE" text + current score/time
4. **Probability bars** — visual fill showing win probability
5. **Volume display** — trading volume per market
6. **Multi-market indicator** — "N markets" link to expand
7. **Bet slip sidebar** — sticky right panel

## Gainr Adaptation Notes
- Replace "Yes/No" binary with traditional 1X2 / Handicap / Totals
- Replace "cents" pricing with decimal odds (e.g. 2.10, 3.39, 3.82)
- Add analyst tips overlay on market cards (e.g. "3 analysts tipped Home Win")
- Add $GAINR stake tier priority for bet execution
- Volume = total USDC wagered via platform
- Markets sourced from: Football, Tennis, Basketball, Baseball, Cricket, Golf, MMA, Esports
