# 03 Commerce and subscription rules

This is the implementation contract for buying.

**Legend for each section:**
- **Rule:** a confirmed Mini Minds decision.
- **Shopify / Seal / Theme:** who is responsible for what.
- **Verify:** an unresolved platform question from [07](07_IMPLEMENTATION_OPEN_ITEMS.md). **Do not implement an assumed answer.**

Data sources: v2.6 sheets 05, 08 and 09.

---

## 1. Products

- **Rule:**
  - Six products, one per stage.
  - Handles: `new-beginnings`, `awakening-senses`, `little-explorer`, `mover-and-shaker`, `curious-climber`, `independent-explorer`.
  - One default variant each at **£72.99**, with **no compare-at price**. Never show £74.99 or a "Launch offer" badge.
  - Title as designed: "{stage name} Bundle".
- **Shopify:** products, price, inventory, the featured image (the bundle master).
- **Theme:** everything stage-specific is read through `custom.stage` and `custom.toys` ([04](04_DATA_AND_INTEGRATION_CONTRACTS.md)).
- **Verify:** out-of-stock behaviour for one-time purchases and for a subscription's next swap (CNT-08, SE3).

## 2. Purchase routes

| Route | Price | Badge / wording | Ends |
|---|---|---|---|
| One-time purchase | £72.99 | "Free UK delivery" | Single order |
| Subscribe every 2 months | £68.99 per bundle | "Popular", "Save 5%" (5.48 % actual; understating is acceptable) | Automatically after Independent Explorer; no seventh payment |
| Prepay 3 bundles | **£195.99** paid once (3 × £72.99 = £218.97, saving £22.98) | **"Save £22.98" or "Save over 10%"**, never an exact "10% off" | After the third delivery; does not roll into a subscription |

- **Theme:**
  - One-time is preselected.
  - Buy Now carries the selected plan and quantity.
  - Add-to-basket button labels: "Add to Basket" / "Add subscription to Basket" / "Add prepaid 3-bundle plan to Basket".
- **Verify:**
  - SE10: whether the theme renders the selling-plan radios from `product.selling_plan_groups` or Seal's widget must be used.
  - S13: whether a Buy Now link or permalink can carry `selling_plan`.

## 3. Stage eligibility

**Rule:**

| Starting stage | One-time | Subscribe | Prepay 3 (covers) |
|---|---|---|---|
| 1 New Beginnings | ✔ | ✔ | ✔ (1, 2, 3) |
| 2 Awakening Senses | ✔ | ✔ | ✔ (2, 3, 4) |
| 3 Little Explorer | ✔ | ✔ | ✔ (3, 4, 5) |
| 4 Mover & Shaker | ✔ | ✔ | ✔ (4, 5, 6) |
| 5 Curious Climber | ✔ | ✔ | ✘ |
| 6 Independent Explorer | ✔ | ✘ | ✘ |

- **Seal:** membership of the selling-plan groups is the **only** source of eligibility (SUB-03):
  - Subscribe group: products 1–5.
  - Prepay 3 group: products 1–4.
- **Theme:**
  - Render only the plans attached to the product.
  - If a plan requested via URL or hand-off isn't eligible, fall back to One-time with a visible notice: "{Plan} isn't offered for the {title} Bundle, so One-time purchase is selected."
  - The basket drops lines with an ineligible plan and shows a notice.
  - No eligibility metafields.
- **Verify:** SE1: does membership alone enforce this on the product page, cart and Buy Now?

## 4. Basket line identity, merging and quantity

- **Rule:**
  - Lines are distinct by **variant + selling plan + gift properties**. Identical combinations merge.
  - Quantity is 1–9 per line. "−" stops at 1; **Remove** is the only way to delete.
  - At the maximum: "Already 9 in your basket — the most per bundle".
  - A plan line's "Remove" is basket removal, never "cancel" (CART-02).
- **Shopify:** merges identical cart lines (same variant, plan and properties).
- **Theme:**
  - Enforce the maximum of 9 in the UI.
  - The count label reads "{n} items · {m} bundles", where a Prepay 3 line counts as 3 bundles.
  - The basket "Change" control switches a line's plan. If the target already exists as a line, the lines combine (capped at 9) and a notice explains it.
  - One basket count is shared by the header, cart page and product page. It comes from the Shopify cart, **not** the prototype's localStorage.
- **Verify:**
  - **SE9: what quantity 2 means for Seal** (two contracts, or one contract of quantity 2). This affects the copy ("{q} bundles every 2 months…") and the maximum. **Not confirmed.**
  - S12: plan change and merge via the cart API.
  - Whether a maximum of 9 can also be enforced server-side (S14).

## 5. WELCOME10

- **Rule:**
  - The first **subscription** bundle costs **£65.69** (10% off the **£72.99 one-time price**); later bundles cost £68.99.
  - Not valid on One-time or Prepay 3. Valid 30 days, single use per customer, not combinable, product price only.
  - Customer-facing wording must state the reference price.
  - Entered at checkout only; the basket has no discount field.
  - Issued by email through Klaviyo (launch mode).
- **Shopify:** the discount itself.
- **Klaviyo:** issuing the code and the welcome email ([04 §5](04_DATA_AND_INTEGRATION_CONTRACTS.md#5-klaviyo)).
- **Theme:**
  - Never applies the discount itself.
  - Rejection messages come from Shopify checkout.
  - The Get 10% Off page explains the three outcomes.
- **Verify:**
  - **S7:** a percentage off a £68.99 plan line gives £62.09, so exactly £65.69 probably needs a fixed £3.30-off mechanism.
  - **SE7:** limiting the code to the Subscribe plan's first order and excluding Prepay 3.
  - **K4:** the code on screen matching the code in the email.
  - **L1:** reference-price wording.

## 6. Gifting

- **Rule:**
  - "Send as a gift" on the product page reveals an **optional recipient email** and an **optional gift message (≤200 characters)**. Both are saved as line-item properties **before checkout** (GIFT-01).
  - The recipient's address is entered as the shipping address at checkout.
  - The message is **printed on a gift insert**.
  - No wrap, box or gift receipt.
  - The purchaser controls payment, cancellation and refunds.
  - The recipient email is used **only** to send the privacy notice. If there's no email, the notice goes in the parcel. Recipients are never added to marketing.
  - **No recipient-verification claim.**
- **Line properties** (v2.6 sheet 08): `Gift` = "Yes", `Gift recipient email`, `Gift message`. These are part of the line identity.
- **Operations:** the gift-insert printing process (CNT-09).
- **Verify:**
  - S5: properties with Buy Now and express wallets.
  - L5: Terms §12 wording.

## 7. Delivery

- **Rule:**
  - Free standard UK delivery on every parcel: **one £0.00 rate**, "Standard UK delivery — FREE".
  - Dispatch within 2 business days. Arrival is estimated at 3–5 business days after order confirmation.
  - UK mainland and Northern Ireland only; the Channel Islands and Isle of Man are excluded.
- **Shopify:**
  - A shipping profile with one free rate.
  - Delivery zones.
  - The free rate applies to subscription renewals too.
- **Verify:**
  - SE4: no delivery charge on Prepay 3's £0 renewals.
  - The live parcel test (S15).

## 8. Stage progression (Subscribe)

- **Rule:**
  - One paid bundle every 2 months, each renewal the **next** stage (never a repeat).
  - A later starting stage pays only for the stages that remain:

    | Starting stage | Payments |
    |---|---|
    | 1 | 6 |
    | 2 | 5 |
    | 3 | 4 |
    | 4 | 3 |
    | 5 | 2 |

  - Price held at £68.99.
  - Reminder 7 days before each charge.
  - A failed payment gets up to 2 retries within 7 days, with no fee and no dispatch until paid. **The stage never advances on an unpaid bundle.**
  - **No pause or skip.** A cancelled plan is never resumed or re-based; a returning customer starts a new plan at the stage they choose (SUB-04).
- **Seal:**
  - Selling plans.
  - Product-swap sequences per starting stage.
  - Maximum payment counts.
  - Charge scheduling, reminders, dunning.
  - Next-bundle, charge, failure and cancellation notices.
- **Theme:**
  - Displays Today → Next (`next_stage.stage_product`) and totals ("{n} bundles · £68.99 each · £X in total · then the journey ends").
  - **Never computes fulfilment** (SUB-01).
- **Verify:**
  - SE2: does checkout count as payment 1? Does a swap advance on a failed payment?
  - SE3: same-date anchor with month-end adjustment; deferred first dispatch; minimum gap between parcels.
  - That the order line items actually change product at each renewal.

## 9. Stage 6 and journey completion

- **Rule:**
  - Independent Explorer is one-time only as a starting purchase.
  - As the final subscription stage, it is the last paid bundle, followed by automatic completion.
  - The product page shows a **completion card** (section settings) instead of a next stage.
  - No 12–18-month product or waitlist promise.
- **Seal:** the EXPIRED status. The portal shows "Journey complete", not "Cancelled".
- **Journey-complete message:** Seal does not send one. It is sent as a transactional message triggered by EXPIRED, from Shopify or Klaviyo (C-02). Copy is pending (VAL-06).

## 10. Prepay 3

- **Rule:**
  - An **upfront purchase of three deliveries** (stage s, s+1, s+2; s ≤ 4), charged £195.99 once, with free delivery on all three.
  - Implemented as a Seal plan limited to three billing cycles: payment 1 is £195.99; payments 2 and 3 are £0 with product swaps.
  - The £0 renewals must be invisible to the customer, send no reminder and carry no delivery charge.
  - **No ordinary self-service cancellation control.** Statutory cancellation and refund are handled by support, subject to legal sign-off (L4).
- **Account:** shows which bundles have shipped and which are scheduled, with nothing further to pay.
- **Verify:**
  - SE2: 3-cycle cap.
  - SE4: hidden £0 renewals, reminder suppression, and hiding cancel for Prepay 3 only.
  - SE7: WELCOME10 exclusion.

## 11. Cancellation rules

| Route | Rule |
|---|---|
| Subscribe | Cancel any time in the portal or via support. Cancelling stops future unpaid bundles. A bundle already charged is still supplied unless the order can be stopped or a statutory right applies. Bundles already received are kept. Statutory rights on each bundle: 14 days to cancel after delivery, then 14 days to return. |
| Prepay 3 | No self-service cancel. Statutory rights via support (L4). |
| One-time | Statutory 14 + 14. Refund of product price plus basic outbound delivery (free). No restocking fee. |

- **Seal:** the cancel flow and cancellation-reason capture (SE5).
- **Theme:** Help and policy copy only.

## 12. Account and subscription portal

- **Shopify customer accounts:**
  - Passwordless six-digit code sign-in; orders; order detail; addresses; name.
  - The email address can't be edited ("Contact us").
  - There is no separate register flow.
  - The design's custom layouts apply as far as S6 allows.
- **Seal portal (My Subscription):**
  - Current → next stage; price; policy summary (7-day reminder; advance only after paid; retry rules); journey strip; delivery address; payment method; Cancel.
  - Prepay 3: schedule only.
  - The subscription address and payment method can be updated where Seal supports it.
- **Verify:**
  - S6: customisation limits.
  - SE4, SE5: prepay cancel and cancellation reasons.
  - SE8: data contract.
  - S2: showing Today → Next inside the account.

## 13. Checkout implications

- Shopify one-page checkout with branding only.
- **Payment methods:** Visa, Mastercard, Amex, Apple Pay, Google Pay, Shop Pay, PayPal, Klarna. Availability with subscriptions and mixed baskets: **SE6**.
- **Disclosure:** checkout must show the subscription commitment (remaining bundles and total) and the charge schedule. How it can be shown is **S11 / SE2**.
- **Guest checkout** is the default; account creation is offered after the order is placed.
- **Marketing consent at checkout:** Shopify's optional, unticked checkbox.
- **Mixed baskets:** one-time, subscribe and prepay lines in the same basket are **intended** by the design. Supported behaviour is **SE6 (unverified)**.

## 14. Responsibility summary

| Concern | Shopify | Seal | Theme | Klaviyo |
|---|---|---|---|---|
| Price, variant, inventory | ✔ | | reads | |
| Selling plans, eligibility | | ✔ | reads | |
| Stage progression, charges, dunning, reminders | | ✔ | displays | |
| Cart lines, merging, checkout, discounts | ✔ | | UI, max 9 | |
| Gift properties | stores | | captures | |
| WELCOME10 issue | | | | ✔ |
| WELCOME10 redemption | ✔ (S7) | scope (SE7) | explains | |
| Journey-complete message | ✔ or Klaviyo (C-02) | status only | | ✔ or Shopify |
| Portal | | ✔ | styling where allowed | |
