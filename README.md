# 🏆 MTG Booster Prize Calculator

> 🌐 **Live Web Application**: [https://zekvyrin.github.io/mtg-booster-prize-calculator/](https://zekvyrin.github.io/mtg-booster-prize-calculator/)

A modern, standalone, client-side web application for Magic: The Gathering Tournament Organizers (TOs) and Local Game Stores (LGS). It takes raw tournament standings exported or copied from **Wizards EventLink** and calculates mathematically fair, proportional booster pack payouts across Swiss point brackets.

Zero dependencies, no servers, no telemetry, and 100% private — runs entirely in your browser.

---

## 🌟 Key Features

* **Direct EventLink Copy-Paste**:
  * Copy the standings table directly from your browser in EventLink and paste it straight into the input box.
  * Robust parsing for tab-separated and space-separated lines.
  * Preserves official EventLink Swiss tiebreakers (`Rank`, `Match Record`, `OMW%`).
  * Full safety for duplicate player names, team names, or bye placeholders.

* **4 Standard Prize Curves + Custom Dynamic Mode**:
  1. 🤝 **Casual** *(0.10 step, 25% marginal baseline)*: Flat spread tailored for Prereleases, Open House, and Commander nights.
  2. ⚖️ **Balanced** *(0.30 step, 50% marginal baseline)*: Recommended sweet spot for weekly Friday Night Magic (FNM).
  3. 🏆 **Top-Heavy** *(0.50 step, 75% marginal baseline)*: Competitive spread for Store Championships and Game Days.
  4. 🔥 **Win-a-Box** *(0.70 step, 100% pure marginal)*: High-stakes curve for RCQs and Qualifiers where top finishes receive the lion's share.
  5. 🎯 **Custom**: Full control with an interactive Marginal Baseline Slider ($0\%$ to $100\%$) and custom multiplier step.

* **Side-by-Side Version Comparison Matrix**:
  * Compare pack allocations and leftover unassigned boosters across all 5 models on a single screen before committing.
  * Click any scenario column header or button to immediately drill down into individual player allocations.
  * **One-Click Summary Copy**: Copy a clean, tab-separated breakdown of the payout structure to paste directly into Discord, Facebook event pages, or spreadsheets.

* **Two-Headed Giant (2HG) Mode**:
  * One-click toggle switches the engine to team-aware mode.
  * Guarantees even pack allocations (multiples of 2 packs) so teammates never have to split an odd booster.
  * In participation mode, awards 2 packs per team (1 per player).

* **Detailed Player Payout Lists**:
  * View every player's exact rank, record, match points, prize boosters, participation boosters, and total payout.
  * Gold champion badge (`#1`) and pill tags for match records (e.g. `4/0/0`, `3/1/0`).
  * **One-Click Player List Export**: Exports a formatted table ready for tournament announcements or record-keeping.

---

## 📐 How the Apportionment Works

Tournament prize pools often cannot be divided into fractional boosters. Rounding down naively causes massive booster leftovers, while rounding up exceeds the store's inventory. 

This calculator implements a modified **Largest Remainder Method (Hamilton-Hare method)** coupled with **Strict Monotonicity** and **Marginal Baseline adjustments**:

### 1. Qualifying Threshold
By default, only players finishing with strictly more than half of the maximum possible match points qualify for extra prize packs:
$$\text{Min Points} = \left\lfloor \frac{\text{Rounds} \times 3}{2} \right\rfloor + 1$$
*(e.g., In a 4-round Swiss tournament, $\lfloor 12/2 \rfloor + 1 = 7$ points, representing records of 2-1-1 or better).*

### 2. Marginal Point Baseline Adjustment
When assigning prize shares to points brackets, using raw match points can overly reward low-tier qualifiers at the expense of undefeated players. A marginal baseline $\beta \in [0, 1]$ deducts a proportion of the baseline threshold:
$$\text{Effective Points}(p) = p - \beta \cdot (\text{Threshold} - 1)$$
* At $\beta = 0\%$ (Raw Points): A 7-point entrant receives 7 base shares, while a 12-point entrant receives 12 base shares.
* At $\beta = 50\%$ (Moderate Baseline): A 7-point entrant receives $7 - 0.5 \times 6 = 4$ shares, while a 12-point entrant receives $12 - 3 = 9$ shares.
* At $\beta = 100\%$ (Pure Marginal): A 7-point entrant receives $7 - 6 = 1$ share, while a 12-point entrant receives $12 - 6 = 6$ shares (strong incentive for top finishes).

### 3. Step Multiplier
A step multiplier $m$ scales the weight given to higher brackets:
$$\text{Multiplier}(p) = 1.0 + m \cdot (p - \text{Threshold})$$
$$\text{Votes}(p) = \text{Effective Points}(p) \cdot \text{Multiplier}(p)$$

### 4. Apportionment & Strict Monotonicity
1. **Integer Floor Assignment**: Each bracket receives $\lfloor \text{Booster Entitlement} \rfloor$.
2. **Strict Hierarchy Check**: If strict monotonicity is enabled, any bracket with fewer points is guaranteed strictly fewer packs than the bracket above it ($P_{k} > P_{k+1}$).
3. **Remainder Distribution**: Remaining boosters are assigned one-by-one to brackets with the largest fractional remainders, provided the strict hierarchy invariant is maintained.
4. **2HG Pairing**: In 2HG mode, boosters are assigned in blocks of 2 to ensure teammates receive pairs.

---

## 🚀 Getting Started

Simply open `index.html` in any modern web browser:

```bash
# Clone the repository
git clone https://github.com/Zekvyrin/mtg-booster-prize-calculator.git

# Open in your preferred browser
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or host it for free on **GitHub Pages**, **Vercel**, **Netlify**, or **Cloudflare Pages** by deploying this repository.

---

## 👥 Credits & Acknowledgments

* **Developed & Maintained by**: [Zekvyrin](https://github.com/Zekvyrin)
* **Based on original work and algorithms by**: [karlosss](https://github.com/karlosss) from [mtg-prize-distribution](https://github.com/karlosss/mtg-prize-distribution)

---

## 📄 License

This project is open-source under the GPL-3.0 license.

