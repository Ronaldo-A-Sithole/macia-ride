# Base44 Update Prompt #2 — Confirmed Pricing, Earnings Tracker, In-App Payment Flow

Paste this into Base44 as a further update to the "Macia Ride" app, on top of the previous business-model update (removing ride-fare processing, adding driver subscriptions). This update adds three things: confirmed subscription prices, a driver/rider earnings-and-payments ledger, and a more realistic in-app subscription payment flow.

---

## 1. Confirmed subscription pricing (replace any placeholder values)

Use these exact figures in the subscription payment screen and admin settings:

| Vehicle type | Daily fee | Weekly fee | Weekly discount |
|---|---|---|---|
| Car | 99 MT | 504 MT | 27.3% |
| Bike / Scooter | 45 MT | 229 MT | 27.3% |

- Show both the daily and weekly option side by side when a driver goes to pay, with the weekly option labeled to highlight the savings (e.g. "Save 27%").
- Keep these values in an admin-editable settings table (not hardcoded), since pricing may be revisited later.

## 2. In-app subscription payment flow (replace the mock "link out to M-Pesa" idea)

Build the payment screen to simulate this realistic flow rather than a generic payment button:

1. Driver taps **"Pay Subscription"** and selects Daily or Weekly.
2. App shows a brief **"Sending payment request to your phone..."** loading state (simulating the backend calling the M-Pesa C2B API).
3. App then shows a **simulated USSD-style PIN prompt** (styled to resemble a native phone prompt, not an in-app form) asking the driver to "enter M-Pesa PIN to confirm payment of [amount] MT."
4. On confirming, show a success state and immediately update the driver's account status to **Active**, with the next payment due date shown clearly.
5. Include a small **"Didn't get the prompt? Pay manually"** fallback link, leading to a simple screen with instructions to dial the standard M-Pesa code and enter a reference number tied to their driver ID — for the prototype, this can just be a static instructional screen.

## 3. Driver & rider earnings/payments ledger (new feature)

### Driver side
- After each completed trip, show a quick post-trip prompt: **"How much did you receive, and how?"** — a numeric amount field, and a payment method selector: Cash / M-Pesa / e-Mola / mKesh / Other. Keep this to one screen, minimal taps.
- Build a **driver earnings dashboard** (separate from the subscription payment screen) showing:
  - Totals for: Today, Yesterday, This Week, Last Week, This Month, This Year
  - Trip count and average earnings per trip for the selected period
  - A simple breakdown by payment method (e.g. a small bar or list: Cash 60%, M-Pesa 30%, e-Mola 10%)
- Label this screen clearly as the driver's **own personal record**, e.g. a subtitle like "Your reported earnings — for your own tracking."

### Rider side (optional, lower-friction)
- After marking a trip complete, show an optional, easily-skippable prompt: **"How much did you pay, and how?"** — same amount + method fields as the driver's, but clearly optional (a visible "Skip" option, not just a small X).

### Admin view
- Add a simple view aggregating reported amounts and payment methods across all trips (for market insight, not for driver management) — e.g. total reported volume this week, most common payment method, average fare by zone.
- If a rider's reported amount and a driver's reported amount for the same trip differ significantly, flag it in a simple list for the operations team to review — do not surface this discrepancy to the rider or driver themselves in this prototype.

## 4. Do not change
- Everything from the previous update (no ride-fare processing by the app, reference-only fare estimate, subscription-gated account access) stays as built.
- Matching, map/landmark flows, masked calling, ratings, and driver approval remain unchanged.
