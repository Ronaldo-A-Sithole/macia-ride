# Macia Ride — standalone source prototype

Recreated from the two supplied Base44 update prompts. This is newly written code, not extracted Base44 source. The original onboarding, matching implementation, maps, landmarks and zone fare table were not provided; those parts have working illustrative replacements.

## Run

Requires Python 3 to serve the app; Node 20+ is only needed for the tests. No npm installation or external packages required.

1. Extract the ZIP.
2. Open a terminal in `macia-ride`.
3. Run `python3 -m http.server 8000` (Windows: `py -m http.server 8000`).
4. Open `http://localhost:8000`.

Use a web server rather than opening index.html directly because the source uses JavaScript modules.

## Try a complete trip

1. Rider: choose two different landmarks, Car or Scooter, and optional illustrative map pins. Request a ride.
2. Driver: the seeded Car driver is approved and has one demo day of access. Go online, accept, mark arrived, start, then end the trip.
3. Driver: record the amount received and method, and rate the rider.
4. Rider: open History. Optionally record the amount paid or explicitly Skip, give fare-accuracy feedback, and rate the driver.
5. Driver: open Earnings to explore six periods and payment method totals.
6. Admin: inspect subscription status and soft feedback; change fees or zone reference fares; inspect reported market volume and discrepancies.
7. Driver > Profile: select the pending scooter driver, or register a new driver. Admin must approve them, then Driver must simulate a subscription before going online.

## Subscription model

Default editable prices: Car 99 MT/day and 504 MT/week; Scooter 45 MT/day and 229 MT/week. The display computes rounded savings from current prices (both default to 27%). A configurable four-hour grace period starts at the due time. Grace-period drivers may receive requests; suspended drivers may not. Existing trips may finish after subscription expiry. Expiry is checked on render, when matching and every minute while the page is open. Production needs server-side enforcement and notifications while the app is closed.

Renewal adds one or seven days to the later of the current expiry or payment time, so paying early does not discard unused access. Subscription receipts are simulated metadata only. Actual ride fares are never charged, held, transferred or guaranteed by this code. Driver/rider ledger entries are self-reported personal records, not receipts from the platform.

## Files and code explanation

- `index.html`: accessible page shell and demo role selector.
- `styles.css`: responsive interface, map illustration, status badges and simulated phone prompt.
- `domain.js`: pricing defaults, subscription eligibility/expiry, reference fare lookup, report validation, trip transitions, date-period aggregation and discrepancy rules. These pure functions can be reused in a future frontend or backend.
- `app.js`: screen rendering and event handlers. Data is kept in localStorage under `macia-ride-v1`. Names are HTML-escaped before display. Roles share a single browser state for demonstration.
- `tests.mjs`: behavioral tests for subscription boundaries, exact fees, matching gates, reporting, discrepancy flags, date periods and trip transitions.
- `prompts/`: copies of the supplied requirements.

Run tests: `node --test tests.mjs`.

## Ledger definitions

Trip count includes completed trips in the selected period. Average reported earnings uses only trips with a driver report; unreported trips are not interpreted as zero earnings. Payment-method percentages are shares of reported value. The week starts Monday; dates use the browser timezone (set the device to Mozambique time for local reporting). Admin volume uses driver reports once per trip, never adds rider reports to the same total. Route averages and most-common method by count are all-time and are labeled accordingly. Discrepancies use `abs(driver - rider) > max(driver, rider) * threshold / 100`; the default threshold is 20%. Missing reports are not flagged.

## Prototype limits and production work

This is a local prototype, not a production transport or payment service. It has no authentication, server database, multi-device synchronization, real geolocation/geographic map, live dispatch, calls, notifications, verified merchant payment instructions or M-Pesa integration. Roles are a testing control, not authorization; all demo data and admin views are accessible to the browser user. Original fares/landmarks were unavailable and the included values are illustrative. The map saves relative illustration positions, not latitude/longitude. Matching requires a demo driver to accept manually.

The simulated phone prompt deliberately does not accept or store real PINs. The manual fallback is instructional only and cannot reactivate an account. Production must obtain a verified merchant account and current provider instructions. Real subscription payment requests need a backend, secret management, authenticated callbacks, idempotency, verified amounts and driver references. Activation must follow provider verification, not a frontend success button.

A production backend must enforce roles, driver approval, subscription access, valid ride transitions, one active assignment per driver, record ownership and private admin-only discrepancy data. Add validation on the server, concurrency-safe matching, geospatial routing, background billing jobs, audit records, privacy controls, appropriate onboarding fields and real masked communication. No real rider/driver personal data should be entered into this demo.

## Reset

Use the browser's site-data controls to clear data for localhost, or remove only `macia-ride-v1` from localStorage in browser developer tools. This resets the demo records.

## Validation

Seven automated domain tests cover the critical rules; JavaScript syntax checks also run. Full browser UI testing has not been performed. The source is independent of Base44 and is not automatically importable as a Base44 project.
