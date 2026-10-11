# Base44 Update Prompt — Business Model Change

Paste this into Base44 as an update to the existing "Macia Ride" app. It describes changes to the business/payment model only — the rider/driver matching, map, and landmark functionality already built should stay as-is unless a line below says otherwise.

---

We are updating the business and revenue model for the Macia Ride app. Please apply the following changes:

## 1. Remove all ride-fare payment processing
The app must **no longer process, hold, or transfer money for the ride fare itself**. Remove any "Pay with M-Pesa" step tied to the ride/trip flow, and remove any driver payout screen or logic tied to ride earnings.

- At the end of a trip, replace the payment screen with a simple **"Trip complete"** confirmation showing the estimated fare as a reference amount, with text such as: *"Please pay your driver directly — [amount] MT is the typical fare for this route."*
- Payment between rider and driver happens entirely outside the app (cash or any mobile money method they agree on). The app does not need a payment method selector, PIN entry mock, or receipt generation for ride fares.
- Remove any "Earnings from rides" figures from the driver dashboard, since the app never processes ride payments. Do not remove the driver's trip history/count — only remove ride-fare earnings totals.

## 2. Add a driver subscription/billing system
Drivers must pay a **flat subscription fee** (daily or weekly, driver's choice or platform-set — make this configurable) to keep their account active on the platform. Build the following:

- **Subscription status** on the driver profile: Active / Grace Period / Suspended.
- **Payment screen**: driver selects daily or weekly plan, pays via a simulated M-Pesa collection flow (mock PIN-entry-style confirmation, then success state) — this is a **subscription fee payment**, separate and unrelated to any specific ride.
- **Reminder mechanic**: show a reminder/notification the day a payment is due, with a short grace period (e.g. a few hours) before the driver's account is automatically set to "Suspended" and they can no longer go online to receive ride requests.
- **Suspended state**: driver sees a clear screen explaining their account is suspended due to a missed subscription payment, with a button to pay and reactivate immediately.
- **Admin view**: add a list of drivers with their current subscription status and last payment date, so operations staff can see who is active, in grace period, or suspended.

## 3. Change ride pricing to a non-enforced reference estimate
Ride fares (from the existing zone-to-zone pricing table) should now be treated as a **reference/estimate only**, not a processed transaction amount.

- When a rider requests a ride, show the fare as: *"Typical fare: ~[amount] MT"* rather than a fixed, final price.
- Do not build any logic that "charges" this amount — it exists purely for the rider's information before they request a ride.
- Keep the existing admin-editable zone fare table as-is; it still drives what estimate is shown, it just doesn't get processed as a payment anymore.
- Add a simple **post-trip feedback option** for the rider: a toggle or short prompt such as *"Was the fare you paid close to the estimate?"* (Yes / No, no further detail required for this prototype) — this feeds into the driver's profile as a soft trust signal for operations to review, not an automated penalty system.

## 4. Keep unchanged
- Rider and driver onboarding, pin-drop pickup/destination, and landmark-based location selection.
- Matching, accept/decline flow, and trip status screens (requested → matched → arrived → started → ended).
- Masked "Call driver" / "Call rider" mock buttons.
- Ratings between rider and driver (this is separate from the fare-accuracy feedback in point 3).
- Vehicle type selection (Car / Scooter).
- Admin driver approval flow for new sign-ups.

## 5. Summary of the model for context
This app's revenue comes entirely from driver subscription fees, not from ride fares. Riders and drivers settle the actual ride payment directly between themselves, by whatever method they prefer. The app's job is discovery, matching, and coordination — not payment processing for rides. Please make sure no part of the rider-facing or driver-facing flow implies the app is handling, holding, or guaranteeing ride-fare money.
