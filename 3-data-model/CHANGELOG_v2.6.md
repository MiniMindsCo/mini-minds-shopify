# Mini Minds Co Shopify data model: v2.5 → v2.6

**Workbook:** `Mini_Minds_Shopify_Data_Model_v2.6.xlsx`
**Source:** `source-v2.5/Mini_Minds_Shopify_Data_Model_2-0d97c037.xlsx`. The older `Mini_Minds_Shopify_Data_Model_2.xlsx` is kept beside it. Both are unchanged copies of the Claude Design uploads.
**Status:** approved schema, including the final Step 4 patch (§1a). Nothing has been created in Shopify. No theme work has started and no Notion tickets exist yet.

## 1a. Final Step 4 patch (M32–M37)

| Ref | Change |
|---|---|
| M32 | The `mm_stage.stage_theme_label` limit rises from 24 to **32 characters**. The approved design labels stay as written, e.g. "Movement becomes intention" and "Balance, intent & precision". |
| M33 | `mm_help_article` gains **`secondary_cta_label`** (single-line text, optional, up to 60 characters) and **`secondary_cta_link`** (url, optional). The primary `cta_label` and `cta_link` are kept. New rule **HELP-03**: a label always needs a link. |
| M34 | `mm_help_block` gains **`list_style`** (single-line text, with the choices `bulleted` and `numbered`), required only when `block_type = list`. There is still one list block type. New rule **HELP-04**. |
| M35 | The blocks for all 22 articles are seeded from the design on the new sheet **06b Help Article Blocks**. **`sub-pause`** ("Can I pause or skip a delivery?") is kept and revised. It now also says a cancelled plan is never resumed or moved to a new stage automatically; a new plan starts at the stage the customer chooses. New rule **SUB-04**: no pause or skip. |
| M36 | **`gift-delivery`** revised. The "A verified recipient can update delivery details…" line is removed. The recipient email and gift message are entered **on the bundle page** (the design said "at checkout"). The recipient email is used to send the privacy notice (the design said "for delivery updates"). The gift insert is stated. The recipient is told to contact us about faults, damage or safety concerns. |
| M37 | The Help inventory is confirmed at **22 articles**. The earlier count of 21 was an analysis error. |

## 1. What changed

**Architecture (unchanged in principle, now enforced)**

- There are **6 metaobject types**: `mm_stage`, `mm_pathway`, `mm_toy`, `mm_help_category`, `mm_help_article` and `mm_help_block`.
- **Product** is a commerce object with exactly two metafields, `custom.stage` and `custom.toys`.
- These product fields are **not created**:
  - `custom.pathways`
  - `custom.next_stage_product`
  - `custom.age_range`
  - a toy-count field
  - purchase-eligibility flags
- **Which purchase routes a bundle offers is set by Seal selling-plan membership only:**
  - Subscribe: bundles 1–5
  - Prepay 3: bundles 1–4

**`mm_stage`**

- **Added:**
  - `stage_theme_label`, `pdp_subhead`, `stage_intro`, `noticing_signals` (exactly 3). These are seeded from the root Bundle Product Page.
  - `stage_wash_colour`
  - `guide_cover_image` and `guide_cover_alt`
  - `guide_spread_image` and `guide_spread_alt`. These are optional; the site-wide spread is the fallback.
- **Renamed:** `stage_image` → `lifestyle_image`, and `stage_image_alt` → `lifestyle_image_alt`.
- **Not created:** `pathways` (it would be identical on every stage), `faq_whats_inside` (the answer is global), and `faq_between_stages` (two template variants cover it).
- **Changed:** `featured_pathways` is now **curated by hand** from the approved narrative. The v2.5 values calculated from toy counts are superseded.

**`mm_pathway`**

- `colour`, `definition` and `example` are reseeded from the root design.
- `summary` is added for the short line on the product page.

**`mm_toy`**

- Field limits tightened:

  | Field | v2.5 | v2.6 |
  |---|---|---|
  | `name` | max 100 | max 34 |
  | `what_it_invites` | max 280 | max 90, admin label "Benefit line" |
  | `pathways` | max 5 | max 2 |

- **Row 13 corrected.** The v2.5 source had renamed B2's toy "Crinkle Animal Mirror" and copied B4's alt text onto it. It is back to **Tummy Time Mirror** (`tummy-time-mirror`, `b2-tummy-time-mirror.jpg`) with alt text **pending**.
- **B4 Crinkle Animal Mirror** remains a separate record.

**Help Centre**

- On `mm_help_category`, the image field `icon` is replaced by `icon_name`, a choice list of design-system icon glyph names. A `tint` field is added.
- The seed now matches the approved Help design:
  - 8 categories. "Account & website" is only created once it has an article.
  - **22 articles**, 10 of them Subscriptions. The reconciliation note said 21; parsing the design found 22.
- The stale v2.5 seed is removed: `pause-or-cancel` (which described pausing), `gift-bundles`, and the £2.50 delivery wording. The design's `sub-pause` article is a different, legitimate article: it explains that pause and skip are not offered.

**Commerce (new sheet 05)**

- Handles are `new-beginnings`, `awakening-senses`, `little-explorer`, `mover-and-shaker`, `curious-climber` and `independent-explorer`.
- Price is **£72.99** with **no compare-at price**. The v2.5 £68.99 price and £74.99 compare-at price are removed.

**New sheets**

- 06b: Help article blocks
- 07: customer and Klaviyo properties
- 08: line-item properties
- 09: rules and data flow
- 10: the 19 build components (replacing the 13 in v2.5)
- 11: open items
- 12: change log (M1–M31)

**Rules**

- DOB-01 and DOB-03 are replaced by **FND-01…05** (finder) and **QZ-01…03** (quiz). DOB-02 stays as FND-04.
- New rules: **STG-06**, **SUB-03**, **SUB-04**, **DATA-01**, **GIFT-01**, **PTH-04**, **HELP-03**, **HELP-04**.
- Existing rules changed: STG-05, PTH-02, TOY-04.

## 2. Confirmed decisions

| Topic | Decision |
|---|---|
| WELCOME10 | First subscription bundle **£65.69**, which is 10% off the **£72.99 one-time price**. Later bundles are £68.99. Applies to the Subscribe plan's first order only; not one-time, not Prepay 3. Valid 30 days, single use, not combinable. The wording must state the reference price. |
| Prepay 3 | **£195.99**, an upfront purchase of three deliveries. Wording is "Save £22.98" or "Save over 10%", never an exact "10% off". There is no ordinary self-service cancellation. |
| Date of birth | The finder's exact date of birth is used **only in the browser session**. It is never persisted or passed on: not in Shopify, Klaviyo, line-item properties, **URLs**, analytics parameters or order data (DATA-01). Downstream, only derived values move on: stage or product handle, whether the stage was moved up, the timing choice, and a coarse month- or week-level delivery value if one is technically needed. The hand-off mechanism, such as sessionStorage, is a developer detail. |
| Finder vs quiz | Both use the same six stage boundaries. **Finder:** exact age at expected arrival, where arrival = today + `LEAD_TIME_DAYS` (5). It moves up a stage when fewer than `BUMP_WINDOW` (14) days of the current stage remain at arrival, so the effective threshold is 19 days from today. **Quiz:** month-level only, with no move-up rule. The difference is deliberate: the quiz has less precise input. At 12 months or older, both show the out-of-range screen. |
| Stage colours | #AFA0CC, #D1EBC4, #E7C089, #A1D0ED, #EDBA6B, #AE96D0, as used consistently across the root design pages. The README tint line is stale. Photography backgrounds are treated separately. |
| Featured pathways | Curated by hand: three per stage, in order. |
| Toys | 43 records. B2 Tummy Time Mirror and B4 Crinkle Animal Mirror are separate toys. |
| Gifts | Recipient email and a gift message (max 200) are captured on the product page before checkout as line-item properties, then printed on a gift insert. No claim of recipient verification. |
| Stage 6 | The completion card lives in section settings. No 12–18-month product or waitlist in the data model. |

## 3. Values still pending (not final)

| ID | Pending value |
|---|---|
| VAL-01 | The **wash colour** values for all 6 stages need design verification. Stages 1, 2 and 4 use a different hue family from their stage colour. |
| VAL-02 | **Featured pathways**: 3 per stage, in order. Candidates from the narrative are on sheet 02. |
| VAL-03 | Web approval of the stage copy (D-09). |
| VAL-04 | **B2 Tummy Time Mirror alt text.** |
| VAL-05 | Alt text for the 6 lifestyle images and 6 Guide covers. |
| VAL-06 | Stage 6 completion copy and call to action; the journey-complete message. |
| VAL-07 | Approval of the Help wording. All 22 articles are seeded. The revised `sub-pause` and `gift-delivery` wording still needs content approval, and `gift-delivery` also needs legal approval (L5). |
| VAL-08 | Help link routes: the pages each call-to-action points to (e.g. Track My Order, Manage My Subscription, Contact Us). These are marked `PENDING - route for '…'` until the final page and account URLs exist. |
| VAL-09 | Bold and italic formatting was removed from the design text in the seed and needs re-applying in the rich-text field. Affected blocks are flagged on sheet 06b. |

The v2.6 patch settles the earlier items like this:
- The theme-label limit (formerly VAL-08) is resolved by M32.
- The second call to action (SCH-01) is resolved by M33.
- Numbered vs bulleted lists (SCH-02) are resolved by M34.

## 4. Unresolved platform and legal questions (not guessed)

**Shopify**
- S1: Can the product gallery be built from metaobject images?
- S2: Can templates follow the reference chain (stage → next stage → product) in app blocks and customer accounts?
- S3: **Are Help metaobject pages found by storefront search?**
- S4: Can Help URLs use `/pages/help/{handle}`?
- S5: Do gift line-item properties survive Buy Now and express wallets?
- S6: How far can customer accounts be customised?
- S7: Can checkout produce exactly £65.69 on a £68.99 subscription line?
- S8: How consent maps to Shopify's Customer Privacy API.

**Seal**
- SE1: Does selling-plan membership actually enforce which purchase routes a bundle shows?
- SE2: How payments are counted, including the prepay cap.
- SE3: Scheduling: deferred first dispatch, date anchoring, the minimum gap between parcels.
- SE4: Prepay 3 renewals at £0, and whether cancel can be hidden.
- SE5: Capturing cancellation reasons.
- SE6: Mixed baskets, and Klarna.
- SE7: Limiting WELCOME10 to the right plan and first order.
- SE8: Seal's data contract.
- SE9: **Quantity: does quantity 2 mean two contracts, or one contract with quantity 2?**

**RevenueHunt**
- R1: Data mapping.
- R2: Storing YYYY-MM only.
- R3: 12-month deletion.
- R4: Result links by product handle.
- R5: Published route or embed.

**Klaviyo**
- K1: Property key names.
- K2: Recording consent source and wording version.
- K3: Two-way suppression sync.
- K4: Code parity.
- K5: Property expiry.

**Judge.me**
- J1: Review pools.
- J2: Requests after a renewal swaps the product.
- J3: Labels.
- J4: Swiping mobile photos across reviews.

**Legal**
- L1: WELCOME10 reference-price wording.
- L2: Month- or week-level dates derived from the date of birth; Privacy §6 and §13 wording and retention.
- L3: The quiz treating calendar month 12 as Independent Explorer.
- L4: Prepay 3 in Terms §7, statutory cancellation, and the savings claim.
- L5: Removing "verified recipient" from Terms §12; the recipient notice.
- L6: A mandatory consent tick on the Get 10% Off form.
- L7: Policy version control. The .docx policies and Policy Pages both say v1.1 but differ.
- L8: The existing sign-off list: staged contract and cooling-off, child-media licence, ADR, return address, UKCA/CE and EN 71.

## 5. Recorded for later clean-up (not changed here)

These are in the Claude Design project and README:
- The README stage-tint line.
- The product page's `active` pathways, which should become the curated values.
- The finder's inline error above 18 months, its due-date code, and its `?dob=` URL hand-off. The hand-off breaks DATA-01.
- The prototype's product keys (`beginnings`, and so on).
- The scope of the basket drawer.
