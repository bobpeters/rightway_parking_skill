# Rightway Parking Assistant — Prompt & Workflow Examples

This guide provides sample user prompts, expected agent workflows, MCP tool interaction sequences, and standard response formats for testing and evaluating the `rightway-parking` skill.

---

## Example 1: Airport Parking Search with Shuttle Filter

### User Prompt
> "I'm flying out of Tampa (TPA) from Friday, November 14 at 7:00 AM to Monday, November 17 at 8:00 PM. What are the cheapest hotel lots that have a free airport shuttle?"

### Tool Execution Flow
1. **`find_locations`**
   - Arguments: `{"query": "TPA"}`
   - Result: `location_slug: "tampa-international-airport-tpa"`
2. **`search_parking`**
   - Arguments: 
     - `location_slug`: `"tampa-international-airport-tpa"`
     - `checkin`: `"2026-11-14T07:00"`
     - `checkout`: `"2026-11-17T20:00"`
     - `shuttle_required`: `true`

### Expected Agent Behavior
- Filters out properties that do not provide dedicated airport shuttles (e.g., rideshare-only garages).
- Lists 3 to 4 competitive hotel options sorted by total stay price.
- Clearly states shuttle operational hours and frequency for each property.
- Shows total stay duration (4 calendar days) and stay-specific estimated cost.

---

## Example 2: Cruise Terminal Parking Search

### User Prompt
> "We're taking a 7-day cruise leaving out of Port Canaveral. We need parking from Dec 6 at 11 AM to Dec 13 at 10 AM. What are our options?"

### Tool Execution Flow
1. **`find_locations`**
   - Arguments: `{"query": "Port Canaveral"}`
   - Result: `location_slug: "port-canaveral-cruise-parking"`
2. **`search_parking`**
   - Arguments:
     - `location_slug`: `"port-canaveral-cruise-parking"`
     - `checkin`: `"2026-12-06T11:00"`
     - `checkout`: `"2026-12-13T10:00"`

### Expected Agent Behavior
- Clarifies whether the lots service the specific cruise terminal/line if applicable.
- Highlights port transfer details (e.g., shuttle to ship terminal vs. walking distance).
- Compares total 8-day parking costs across secured off-site cruise lots.

---

## Example 3: Property Selection, Detailed Quote & Promo Code

### User Prompt
> "Let's go with the SpringHill Suites TPA. Can you apply promo code SAVE5 and show me the price breakdown?"

### Tool Execution Flow
1. **`get_quote`**
   - Arguments:
     - `property_slug`: `"springhill-suites-tampa-airport"`
     - `checkin`: `"2026-11-14T07:00"`
     - `checkout`: `"2026-11-17T20:00"`
     - `promo_code`: `"SAVE5"`
     - `parkguard`: `false`

### Expected Agent Output Format
Presents a transparent itemized table:
- Base parking total
- Applied promo discount amount
- Subtotal
- Applicable lot taxes/fees
- ParkGuard status: Not included ($0.00)
- Online payment due vs. on-site payment due

---

## Example 4: Direct Checkout Handoff

### User Prompt
> "Looks great, give me the checkout link."

### Tool Execution Flow
1. **`create_checkout_link`**
   - Arguments:
     - `property_slug`: `"springhill-suites-tampa-airport"`
     - `checkin`: `"2026-11-14T07:00"`
     - `checkout`: `"2026-11-17T20:00"`
     - `vehicles`: `1`
     - `promo_code`: `"SAVE5"`
     - `client`: `"gemini"`

### Expected Agent Behavior
- Returns the pre-populated deep link:
  `https://rightwayparking.com/reservation/book/springhill-suites-tampa-airport?checkin=2026-11-14T07:00&checkout=2026-11-17T20:00`
- Reminds traveler that generating the URL does not hold inventory until payment and contact information are completed securely on RightwayParking.com.
- Never solicits payment card numbers or personal identity data in the chat interface.

---

## Example 5: Handling Constraints & Missing Times

### User Prompt
> "Find parking near LAX next Tuesday to Friday."

### Expected Agent Behavior
- Recognizes missing check-in/checkout hours.
- Prompts briefly for approximate vehicle drop-off and pickup times rather than assuming flight takeoff/landing times.
- Resolves `LAX` to `los-angeles-international-airport-lax` in preparation for the date confirmation.
