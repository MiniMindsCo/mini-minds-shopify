# 07 Implementation open items

This is **the single definitive register** of what remains unresolved. The other docs reference these IDs rather than restating them.

- **Owner:** who must answer.
- **Proceed?**
  - **Yes:** build around it with a safe default.
  - **Partial:** build the structure; the final behaviour waits.
  - **No:** the dependent work waits.
- **Status:** every item below is **OPEN** unless stated.

Decided and therefore **removed** (see `CHANGELOG_v2.6.md` §2):
- Prices, WELCOME10 base price, Prepay wording.
- DOB persistence.
- Finder vs quiz precision rule; the 5 + 14-day bump rule.
- Stage colours.
- Featured-pathway method (curated).
- Product handles.
- Gift capture.
- Stage-6 completion approach.
- The Help model, 22-article inventory, second CTA and list style.
- Theme-label limit.
- The B2/B4 mirror toys.
- Long-pause rule (no pause or skip).
- Reviews platform, subscription app, email platform, order-tracking approach.

---

## 1. Shopify (S)

| ID | Question | Owner | Why it matters | Blocks | Proceed? |
|---|---|---|---|---|---|
| S1 | Can the product gallery be built from metaobject images (Guide cover, toys, lifestyle), with only the bundle master as product media? Do cart, checkout, Google and Judge.me need more than the featured image? | Developer | Avoids duplicating toy images as product media | `mm-product-gallery` | Partial |
| S2 | Can `custom.stage → next_stage → stage_product` be traversed inside app blocks and customer-account pages? | Developer | Decides whether an override field is ever needed (v2.6 forbids adding one without evidence) | `mm-next-stage` in the account | Yes (storefront) |
| S3 | Are `mm_help_article` web pages included in storefront search and predictive search? | Developer | Search must cover Help articles; the Help model stays metaobjects unless this fails | `mm-search`, Help findability | Partial |
| S4 | Can the metaobject URL handle give `/pages/help/{handle}`? | Developer | Stable Help URLs and deep links | Help links, FAQ deep links | Partial |
| S5 | Do line-item properties survive Buy Now and express wallets (Shop Pay, Apple Pay, Google Pay) from the product page? | Developer | Gift data capture before checkout | `mm-gift-fields`, Buy Now | Partial |
| S6 | How far can the new Shopify customer accounts be customised (order list, order detail, addresses, name)? | Developer | The Account design assumes custom layouts | Account styling | Yes (use native) |
| S7 | Can checkout produce exactly **£65.69** on a £68.99 subscription line (a percentage gives £62.09; a fixed £3.30 is the likely method)? | Developer + owner | The confirmed offer price | WELCOME10 set-up, offer copy | No (discount set-up) |
| S8 | Customer Privacy API mapping for the chosen consent platform | Developer | Legally required consent gating | Consent, analytics, Klaviyo onsite, Judge.me, Seal cookies | Partial |
| S9 | Which sections in [02 §1](02_COMPONENT_AND_SECTION_SPEC.md#1-native-eurus-sections-to-configure-not-rebuild) Eurus/Swirl can meet natively: header CTA reveal on scroll, single-open accordion, image overlap, trust strip, mobile header/footer | Developer | Decides whether each item is native or bespoke | Build estimate and scope | Partial |
| S10 | Can `/policies/privacy-policy` and `/policies/terms-of-service` carry anchor navigation? What goes in the native Shipping and Refund policies (checkout links)? | Developer + legal | Policy layout and checkout consistency | Policy build | Partial |
| S11 | How checkout can show the subscription commitment (remaining bundles, total) and the Prepay schedule | Developer + Seal | Required disclosure | Checkout readiness | No (go-live) |
| S12 | Changing a cart line's selling plan via the cart API, and the merge behaviour when the target line already exists | Developer | Basket "Change" control | `mm-cart-plan-label` | Partial |
| S13 | Can Buy Now (cart permalink or direct checkout) carry `selling_plan` and properties? | Developer | Buy Now with a plan | Buy Now | Partial |
| S14 | Enforcing the maximum of 9 per line beyond the UI | Developer | Edge-case integrity | Cart QA | Yes |
| S15 | Live tracked-parcel test (carrier, links, guest access, split shipments, recurring orders) | Owner + developer | Native tracking decision validation; Track123 fallback | Track My Order sign-off | Yes |

## 2. Seal (SE)

| ID | Question | Why it matters | Blocks | Proceed? |
|---|---|---|---|---|
| SE1 | Does selling-plan group membership (Subscribe: products 1–5; Prepay: products 1–4) enforce eligibility on the product page, cart and Buy Now? | Eligibility has no other source (SUB-03) | Plan selector, pricing toggle, basket | Partial |
| SE2 | Does checkout count as payment 1? Does a swap advance on a failed payment? Is Prepay capped at 3 cycles? Do order line items change product at each renewal? | Payment counts 6/5/4/3/2 and Prepay integrity | Seal configuration, disclosure copy | No (config) |
| SE3 | Scheduling: same calendar-date anchor with month-end adjustment; deferred first dispatch ("Start next stage"); minimum parcel gap | The finder timing option and `_mm_first_delivery` depend on it | Finder timing option, DES-01 | Partial |
| SE4 | Prepay 3: £0 renewals hidden, no reminder, no delivery charge; **hide self-service cancel for Prepay only** | Confirmed Prepay behaviour | Prepay go-live, portal | No (Prepay go-live) |
| SE5 | Does the portal capture cancellation reasons, and where are they stored? | Design's 8-reason dropdown | Portal | Yes |
| SE6 | Mixed baskets (one-time + Subscribe + Prepay) in one checkout, including Klarna and express wallets | The design assumes mixed baskets | Basket and checkout | Partial |
| SE7 | Limit WELCOME10 to the Subscribe plan's first order and exclude Prepay 3 | Offer integrity | WELCOME10 set-up | No (discount set-up) |
| SE8 | Line-item and contract properties available to the theme | Plan labels and next-stage notes in the basket | `mm-cart-plan-label` | Partial |
| SE9 | **Quantity: does quantity 2 create two contracts, or one contract with quantity 2 on renewals and swaps?** | Quantity copy, max-9 semantics, dunning | Quantity UI copy, basket notes | Partial (keep qty UI, hold the copy) |
| SE10 | Is a theme-rendered selling-plan selector (`product.selling_plan_groups`) fully supported by Seal, or must the Seal widget be used? | The design's custom radio cards and panels | `mm-plan-selector` | Partial |

## 3. RevenueHunt (R)

| ID | Question | Why it matters | Blocks | Proceed? |
|---|---|---|---|---|
| R1 | Property names and types sent to Klaviyo and Shopify | Klaviyo contract (v2.6 sheet 07) | Quiz → Klaviyo mapping | Partial |
| R2 | Can it store the birth month as `YYYY-MM` only? | Privacy §6 | Email step | No (quiz launch) |
| R3 | Deleting or anonymising answers after 12 months | Retention | Quiz launch | No (quiz launch) |
| R4 | Result recommendation links to a product by handle, with no birth data in the URL | DES-03 | Result → product page | Partial |
| R5 | Published route or embed method | Quiz CTA link pickers | `page.quiz`, `mm-quiz-cta` | Yes (link picker) |

## 4. Klaviyo (K)

| ID | Question | Why it matters | Blocks | Proceed? |
|---|---|---|---|---|
| K1 | Property key conventions and reserved names for the proposed `mm_*` keys | Stable data contract | All capture points | Partial |
| K2 | Recording consent source and wording version for each form | Register requirement | Forms | Partial |
| K3 | Two-way suppression sync with Shopify | Unsubscribe integrity | Go-live | Yes |
| K4 | Same code on screen and in the email (or email-only delivery, the launch mode) | Offer consistency | Get 10% Off | Yes (email-only) |
| K5 | Automatic expiry of quiz properties and `mm_next_stage_from` | Retention | Go-live | Yes |
| K6 | Whether Klaviyo embedded forms can show the designed states (already subscribed, network error, required consent tick, finder/quiz variants) | Get 10% Off and finder forms | Form build | Partial |

## 5. Judge.me (J)

| ID | Question | Why it matters | Blocks | Proceed? |
|---|---|---|---|---|
| J1 | Six separate per-product pools | Confirmed review structure | Reviews | Yes |
| J2 | Renewal review requests for the swapped next-stage product, with duplicates suppressed | Correct product reviews | Review flows | Yes |
| J3 | Tester and verified-badge labelling | Credibility and compliance | Reviews go-live | Yes |
| J4 | Swiping photos across reviews on mobile | Design requirement | Review widget | Yes |
| J5 | Hiding the homepage and product modules while a pool is empty | "Never show empty or invented reviews" | Home 09, product 8 | Partial |

## 6. Legal (L)

Nothing in the policy pack may be published until L7 and L8 close.

| ID | Item | Why it matters | Blocks | Proceed? |
|---|---|---|---|---|
| L1 | WELCOME10 reference-price wording ("10% off the £72.99 one-time price — £65.69, then £68.99") | Pricing practices | Offer copy | Partial |
| L2 | Month- or week-level values derived from DOB (`mm_next_stage_from`, `_mm_first_delivery`); Privacy §6/§13 wording; stating that the finder DOB is not stored | Privacy | Finder reminder, timing hand-off | Partial |
| L3 | Quiz month-level edge: calendar month 12 treated as Independent Explorer | Recommendation accuracy | Quiz mapping | Yes |
| L4 | Prepay 3 in Terms §7 as an upfront purchase of three deliveries; statutory cancellation and refund; savings claim | Consumer law | Prepay go-live | No (Prepay go-live) |
| L5 | Gifts: remove "verified recipient" from Terms §12; recipient privacy notice; gift insert | Consumer and privacy law | Gift go-live, Help `gift-delivery` wording | Partial |
| L6 | Mandatory consent tick on the Get 10% Off form | Consent validity | Offer form | Partial |
| L7 | Policy version control: the uploaded .docx and Policy Pages both say "v1.1" but differ. One signed-off text and one wording version are needed | Sign-off and consent records | All policy pages | No (publish) |
| L8 | Existing sign-off list: staged contract and cooling-off trigger, child-media licence, ADR body, return address, UKCA/CE claim and EN 71 evidence ("Safety-tested" trust item) | Launch compliance | Policies, trust strip, reviews media | No (publish) |

## 7. Content approvals (VAL / CNT)

| ID | Item | Owner | Blocks | Proceed? |
|---|---|---|---|---|
| VAL-01 | Stage wash colours: stages 1, 2 and 4 use a different hue family from their stage colour | Design | `mm-stage-education`, finder result | Yes (fall back to the stage colour) |
| VAL-02 | Featured pathways: curate 3 per stage, in order | Content | Product-page Pathways panel | Yes (the panel stays hidden until set) |
| VAL-03 | Web approval of stage copy (theme label, subhead, intro, signals, descriptions) | Content | Product-page content QA | Yes |
| VAL-04 | B2 Tummy Time Mirror alt text | Content | Accessibility QA | Yes |
| VAL-05 | Alt text for the 6 lifestyle images and 6 Guide covers | Content | Accessibility QA | Yes |
| VAL-06 | Stage-6 completion card copy and CTA; journey-complete message | Content | Product page stage 6; C-02 | Yes |
| VAL-07 | Help wording approval, including the revised `sub-pause` and `gift-delivery` articles (L5) | Content + legal | Help publish | Yes |
| VAL-08 | Help CTA link routes | Developer | Help article CTAs | Yes (after routes exist) |
| VAL-09 | Re-applying rich-text emphasis in Help blocks | Content | Help publish | Yes |
| CNT-01 | SEO titles and meta descriptions; structured data (Product/Offer, BreadcrumbList, Organization, FAQ) is **not specified** | Copy + developer | SEO readiness | Yes |
| CNT-02 | Product-page FAQ answers: fix "14 days to return" to the 14 + 14 wording; align delivery-time wording | Copy | Product-page FAQ | Yes |
| CNT-03 | Remove "due date" from Why Mini Minds CTA, Search, Help, Contact and Component Library copy | Copy | Those sections | Yes |
| CNT-04 | Contact "[Privacy wording — to be supplied.]" | Copy + legal | Contact publish | Yes |
| CNT-05 | Homepage statistic: show the Harvard Center on the Developing Child citation | Copy | Home 03 | Yes |
| CNT-06 | Search synonym list (ages, skills, pathway names) | Owner | `mm-search` age and skill intent | Yes |
| CNT-07 | Toy-grid layout rule for 6–8 toys alongside the Guide card (no orphan card) | Design | `mm-bundle-contents` | Yes |
| CNT-08 | Out-of-stock behaviour (one-time purchases and subscription swaps) | Owner + Seal | Product page, Seal | Partial |
| CNT-09 | Gift-insert printing process and owner | Operations | Gift go-live | Yes |
| CNT-10 | Remove the tracking parameter from the Instagram URL | Copy | Footer | Yes |

## 8. Assets (AST)

See [06 §4](06_ASSET_IMPLEMENTATION_MAP.md#4-missing-pending-or-problem-assets): AST-01 to AST-09 (About hero IMG-306/306M, founder portrait IMG-401/401M, oversized PNGs, naming, mobile crops, orphans). All can proceed with placeholders except go-live QA.

## 9. Consent and analytics (C)

| ID | Item | Owner | Blocks | Proceed? |
|---|---|---|---|---|
| C-01 | Choose the consent platform; produce a cookie inventory from a production scan | Owner + developer | Analytics, advertising, Klaviyo onsite tracking, Judge.me/Seal cookies; footer "Cookie settings" | Partial (build the gating hooks) |
| C-02 | Which system sends the journey-complete message on Seal EXPIRED (Shopify or Klaviyo, transactional) | Owner + developer | Stage-6 completion | Yes |
| C-03 | Where the Help "Was this helpful?" vote is recorded, and the event name | Support + analytics | Help article | Yes (render the UI) |
| C-04 | GA4 / Meta event names and parameters. Proposed: no birth month in parameters | Developer + owner | Analytics | Yes |

## 10. Design clean-up and design gaps (DES)

The page map and component spec already follow the design-side decisions marked "doc decision". Items marked "confirm" need the owner.

| ID | Issue | Resolution in these docs | Status |
|---|---|---|---|
| DES-01 | The product-page dated delivery schedule is computed from DOB in the prototype | Derive from coarse finder timing, or show relative timing; never DOB | Confirm presentation |
| DES-02 | Finder confirmation promises a code without consent | Show only when consent is ticked | Doc decision (defect fix) |
| DES-03 | Quiz → product link carries `dob=YYYY-MM` | Product handle only | Confirm (proposed) |
| DES-04 | Homepage quiz banner exists in the design; the Custom Spec says How It Works and Shop only | Follow the design | Doc decision |
| DES-05 | Homepage subscription panel swaps content from `?stage=` | Not implemented | Doc decision |
| DES-06 | Shop "Buy once, or grow with them" band had no spec | Specified as `mm-subscription-panel` band | Doc decision |
| DES-07 | Root README still lists `Basket.dc.html` as the drawer spec | `Basket Page.dc.html` is authoritative | Clean-up |
| DES-08 | Product recap "Change purchase option" only toggles once ↔ sub | Must reach every eligible plan | Confirm |
| DES-09 | Mobile product page has no quantity control for one-time at quantity 1 | Add a quantity control | Confirm |
| DES-10 | How It Works timeline links on mobile but not desktop | Follow the design | Confirm |
| DES-11 | About closing CTA makes Shop primary (the finder is primary elsewhere) | Follow the design | Confirm |
| DES-12 | Whether Eurus's cart drawer is disabled entirely | Icon → `/cart`; no auto-open | Confirm |
| DES-13 | Contact form has no network/server error state | Use native Shopify form errors | Confirm |
| DES-14 | Track My Order "Open my order email link" has no target on a generic page | Needs copy or target change | Confirm |
| DES-15 | Tablet and intermediate widths not designed | Use Eurus breakpoints; QA overflow | Confirm |
| DES-16 | README "stage tints" line is stale (the pages use the v2.6 stage colours) | v2.6 values | Clean-up |
| DES-17 | Header CTA reveal threshold hard-coded at 420 px | Derive from the hero | Confirm |
| DES-18 | About and Help Centre absent from the desktop header | Follow the design | Confirm |
| DES-19 | Finder out-of-range email labelled optional but required on submit | Make it genuinely optional | Confirm |
| DES-20 | Two focus-ring styles (lilac glow vs 3 px `#2E2A6B`) | Choose one token | Confirm |
