# 01 Page and template map

This document covers the customer-facing routes only. Specification pages (Custom Sections Spec, Component Library, handoffs, Test Plan) are **not routes**.

- Section names in `mm-*` form are bespoke and specified in [02](02_COMPONENT_AND_SECTION_SPEC.md).
- "Native" means a Eurus/Swirl section configured in the theme editor, subject to **S9** (confirm Eurus capability).
- Interaction detail is in [05](05_INTERACTION_AND_RESPONSIVE_SPEC.md).
- Blocker IDs refer to [07](07_IMPLEMENTATION_OPEN_ITEMS.md).

Every page has the same frame:
- Announcement bar (native).
- Header: native on desktop, `mm-mobile-header` on mobile.
- Footer: native on desktop, `mm-mobile-footer` on mobile.
- The consent banner (C-01).

These aren't repeated in the tables below.

---

## 1. Homepage

**Route:** `/` · **Template:** `index.json` · **Design:** `Mini Minds Co Website Homepage V2.dc.html`

| # | Section | Type | Dynamic inputs | Notes |
|---|---|---|---|---|
| 01 | Hero with pathway word rotator, price line, CTA, quiz link, stage-pill strip | `mm-home-hero` + `mm-pathway-rotator` | Section settings. The price line should read product/selling-plan prices (not hard-coded). | Stage-pill strip is desktop only |
| 02 | Trust strip (4 items) | Native multicolumn / icon row | Settings | 2×2 on mobile. "· EN 71" is appended only when evidence is confirmed (L8) |
| 03 | The challenge ("Sound familiar?"), statistic plus 3 questions | `mm-challenge` | Settings | The statistic needs its Harvard source shown (CNT-05) |
| 04 | Reassurance bridge | Native rich text | Settings | |
| 05 | The Mini Minds Way (3 cards) with the mascot idea animation | `mm-way-cards` + `mm-mascot-spark` | Settings | The Parent Guide card links to `/pages/why-mini-minds#parent-guide` |
| 06 | How it works (3 steps) | `mm-three-steps` | Settings | Vertical stepper without images on mobile |
| 07 | Shop by developmental stage, with Buy once/Subscribe toggle and 6 cards | `mm-bundle-grid` + `mm-bundle-card` | Collection products → `custom.stage`; selling plans | Mobile: scroll-snap carousel with dots |
| 08 | Subscription panel ("Stay stage-matched as they grow") | `mm-subscription-panel` | Settings; subscription price from selling plan | The prototype swaps content from `?stage=` (DES-05). **Do not implement.** |
| 09 | Parent reviews | Judge.me app block | Judge.me | **Hidden while there are no reviews** (J5) |
| 10 | Founder story preview | Native image-with-text | Settings | Uses a stand-in image until IMG-401 arrives (AST-02) |
| 11 | FAQ (5 questions, each with a Help Centre deep link) | Native collapsible | Settings | Several items can be open at once; all start closed |
| 12 | Final CTA | Native image-with-text | Settings | On mobile the image sits above the text |
| 13 | Play Snapshot Quiz banner | `mm-quiz-cta` | Settings | Present in the approved design, though the Custom Sections Spec omits the homepage (DES-04) |

- **Apps:** Judge.me.
- **Blockers:** J5, S9, CNT-05, AST-02. The pricing toggle depends on SE1.

## 2. Shop Bundles

**Route:** `/collections/bundles` · **Template:** `collection.bundles.json` · **Design:** `Shop Bundles.dc.html`

| # | Section | Type | Dynamic inputs | Notes |
|---|---|---|---|---|
| 1 | Hero with range image and mascot card | `mm-collection-hero` | Settings (IMG-303) | The mascot card sits below the image on mobile |
| 2 | "Shop by stage" chips | `mm-stage-chip-rail` | `mm_stage` loop | Desktop: links to each product page. Mobile: sticky rail with anchors to the cards and an active chip that follows scrolling |
| 3 | "Buy once, or grow with them" band | `mm-subscription-panel` (band variant) | Settings | Not in the Custom Sections Spec (DES-06) |
| 4 | "One year, six stages" grid with Buy once/Subscribe toggle | `mm-bundle-grid` | Collection products | Stage 6 shows "Final stage — sold on its own." in Subscribe mode |
| 5 | Why Mini Minds: When → What → How | `mm-when-what-how` | `mm_stage`, `mm_pathway`, settings | Cards stack with ↓ on mobile |
| 6 | "Not sure where to start?" | `mm-quiz-cta` (finder link) | Settings | |

**Collection:** a manual collection containing the six bundle products, sorted by stage order. The grid sorts by `custom.stage.stage_order` regardless.

**Blockers:** S9, SE1.

## 3. Bundle product (one template, six URLs)

**Route:** `/products/{handle}` · **Template:** `product.bundle.json` · **Design:** `Bundle Product Page.dc.html`

| # | Section | Type | Dynamic inputs | Notes |
|---|---|---|---|---|
| 1 | Breadcrumb "Home / Shop Bundles / {title}" | Native or snippet | Product | |
| 2a | Gallery: bundle master → Guide cover → toys → lifestyle | `mm-product-gallery` | Featured image; `stage.guide_cover_image`; `toys[].image`; `stage.lifestyle_image` | S1 |
| 2b | Buy box: age pill, "Stage n of 6", H1, subhead, price, what you get, plan selector, plan panels, qty, Add, Buy Now, gift, trust | `mm-product-buy-box` + `mm-plan-selector`, `mm-next-stage`, `mm-gift-fields` | Product, `stage.*`, selling plans, `toys.size` | See [03](03_COMMERCE_AND_SUBSCRIPTION_RULES.md). SE1, SE10 |
| 3 | "Designed for your baby's stage": eyebrow, intro, 3 noticing cards, lifestyle image | `mm-stage-education` | `stage_theme_label`, `stage_intro`, `noticing_signals`, `lifestyle_image`, `stage_wash_colour` | Wash colours pending (VAL-01) |
| 4 | "Inside the {title} bundle": Parent Guide card plus toy grid | `mm-bundle-contents` | `custom.toys` (ordered), `guide_cover_image` | 6–8 toys; accordion on mobile |
| 5 | Pathways of Growth, "Most active in this bundle" | `mm-five-pathways` (product density) | `stage.featured_pathways` (3); others shown as "Also supported" | Values pending (VAL-02) |
| 6 | Play. Pause. Progress. | `mm-play-pause-progress` | Settings | |
| 7 | The journey continues: 6-stage timeline, next-stage card, previous/next navigation | `mm-product-journey` (uses `mm-stage-timeline` rail + `mm-next-stage`) | Stage graph | Stage 6 shows the completion card (VAL-06) |
| 8 | Parent reviews | Judge.me product widget | Judge.me pool for this product | Hidden while empty |
| 9 | "Before you buy" FAQ: 2 fixed items plus 4 accordions | `mm-product-faq` | Settings; the between-stages answer has two variants, chosen by whether `next_stage` exists | |
| 10 | Recap band (desktop only) | `mm-product-recap` | Product, selected plan | DES-08: the "Change purchase option" control can't reach Prepay |
| — | Sticky purchase bar (mobile only) | `mm-sticky-buy-bar` | Selected plan and qty | DES-09: no quantity control for one-time qty 1 |

- **Apps:** Seal (selling plans), Judge.me.
- **Blockers:**
  - Seal: SE1, SE2, SE3, SE9, SE10.
  - Shopify: S1, S5, S7.
  - Content: VAL-01, VAL-02, VAL-03, VAL-06.
  - Design: DES-01 (delivery schedule without DOB).

## 4. Find My Bundle

**Route:** `/pages/find-my-bundle` · **Template:** `page.finder.json` · **Design:** `Find My Bundle.dc.html`

| # | Section | Type | Dynamic inputs |
|---|---|---|---|
| 1 | Finder: form → result / timing choice / out-of-range screens; journey strip; "Not ready yet?" reminder | `mm-finder` | `mm_stage` age bounds, names, products (JSON rendered by Liquid); Klaviyo form endpoint |
| 2 | Reassurance row (3 items) | Native multicolumn | Settings |

- **Rules:** FND-01…05 and DATA-01. See [04 §7](04_DATA_AND_INTEGRATION_CONTRACTS.md#7-finder-and-quiz-data-flow).
- **Remove from the prototype:**
  - The due-date path.
  - The inline error above 18 months (use the out-of-range screen instead).
  - The `?dob=` hand-off.
  - The confirmation banner that promises a code without consent (DES-02).
- **Apps:** Klaviyo (optional email, reminder, future-range interest).
- **Blockers:** DES-01, K1, K2, L2, L6.

## 5. Play Snapshot Quiz

**Route:** `/pages/quiz` (R5) · **Template:** `page.quiz.json` · **Design:** `Play Snapshot Quiz.dc.html`

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | RevenueHunt quiz embed | App block / embed | Intro → Q1–Q5 → email step (email, optional MM/YYYY, optional unticked consent, "Skip for now") → result |

- **Rules:** QZ-01…03.
- **RevenueHunt configuration** follows the design's handoff panel: block mapping, scoring weights, language guardrails. Recommend exactly one bundle, placed last.
- **Result links** need the product handle only. Dropping the prototype's `dob=YYYY-MM` from the URL is **proposed** (DES-03).
- **Blockers:** R1–R5, K1, K2, L3.

## 6. How It Works

**Route:** `/pages/how-it-works` · **Template:** `page.how-it-works.json` · **Design:** `How It Works.dc.html`

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | Hero with presenting mascot (mascot desktop only) | Native image-with-text | The mascot overlap may need a bespoke setting (S9) |
| 2 | Connected 0–12 timeline | `mm-stage-timeline` (flat) | Mobile items link to the product pages; desktop items don't (DES-10) |
| 3 | How the stages work | Native multicolumn | Anchor `#subscriptions` |
| 4 | How toys are selected | Native image-with-text + list | IMG-102 |
| 5 | Five Pathways + quiz CTA | `mm-five-pathways` (expanded) | Anchor `#five-pathways`. Mobile 2-column grid with the fifth card full width |
| 6 | Play, Pause, Progress | `mm-play-pause-progress` (panels) | Anchor `#play-pause-progress` |
| 7 | How each bundle builds on the last | `mm-stage-timeline` (rising) | **Not rendered on mobile**; a list of link rows instead |
| 8 | The Parent Guide brings it together | `mm-parent-guide` (lightbox) | Links to `/pages/why-mini-minds#parent-guide` |
| 9 | Final CTA | Native image-with-text | |

**Blockers:** S9, AST (Guide spread).

## 7. Why Mini Minds

**Route:** `/pages/why-mini-minds` · **Template:** `page.why.json` · **Design:** `Why Mini Minds.dc.html`

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | Hero (text only) | Native rich text | |
| 2 | Why the first year matters | Native image-with-text | IMG-305 |
| 3 | What makes Mini Minds different (3 pillars) | Native multicolumn | |
| 4 | The Parent Guide + "Inside the Parent Guide" (4 cards) | `mm-parent-guide` | Anchor `#parent-guide`. The design's CSS mock book is a placeholder; production uses the Guide spread image |
| 5 | Two frameworks (Pathways, PPP) | Native multicolumn (2) | Links to `#five-pathways` and `#play-pause-progress` |
| 6 | What guides every bundle (6 rows) | Native collapsible | **One row open at a time, first row open**. Differs from the homepage FAQ, which is intentional per the design. Confirm Eurus supports it (S9) |
| 7 | CTA band | Native image-with-text | The design copy mentions "due date" and needs a copy fix (CNT-03) |

## 8. About

**Route:** `/pages/about` · **Template:** `page.about.json` · **Design:** `About.dc.html`

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | Hero "Our story" with large image | Native image banner | **IMG-306 / 306M pending** (AST-01) |
| 2 | Why Mini Minds began (4 rows) + mascot callout | Native multicolumn / rich text | |
| 3 | The Mini Minds idea chips + six-stage timeline | `mm-stage-timeline` (rail) | Horizontal scroller on mobile |
| 4 | More than toys (3 pillars) | Native multicolumn | Reuses IMG-101 / 201 / 301 |
| 5 | Five pathways | `mm-five-pathways` (compact) | |
| 6 | Play. Pause. Progress. | `mm-play-pause-progress` (rows) | |
| 7 | What we believe (4) | Native multicolumn | |
| 8 | Six stages. One incredible first year | `mm-bundle-grid` (no toggle) | Bundle masters |
| 9 | Closing CTA | Native rich text + buttons | Shop is primary here; elsewhere the finder is primary (DES-11) |

About isn't in the desktop header. It is linked from the mobile drawer and the footer.

## 9. Basket

**Route:** `/cart` · **Template:** `cart.json` · **Design:** `Basket Page.dc.html` (authoritative). `Basket.dc.html` (drawer) is **obsolete**. `Basket and Checkout Entry.dc.html` is reference only.

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | Heading + count ("{n} items · {m} bundles"; a Prepay line counts as 3 bundles) | Native cart + snippet | |
| 2 | Line cards with route chip, "Change" plan control, notes, qty (1–9), Remove, savings | Native cart items + `mm-cart-plan-label` | Plan change: S12 |
| 3 | Sticky summary with subscription/prepay notes and reassurance | Native cart summary + `mm-cart-summary-notes` | **No discount field in the basket.** Codes are entered at checkout only |
| 4 | Empty state | Native cart empty + settings | Mascot and two CTAs |
| — | Mobile sticky bar (total, Checkout) | `mm-sticky-buy-bar` (cart variant) or native | Hidden when the basket is empty |

**Drawer:** the header basket icon opens the page and the drawer never auto-opens. Whether to disable Eurus's drawer entirely is **DES-12**.

**Blockers:** SE6, SE8, SE9, S12.

## 10. Checkout (implications only)

Checkout is Shopify-hosted, with branding limited to Checkout settings (logo, colours, typography, imagery).

- **Where things are captured:** gift fields are captured **before** checkout (S5). WELCOME10 is entered in Shopify's discount field.
- **Delivery:** the only rate is "Standard UK delivery — FREE".
- **Seal's responsibilities:** the subscription and prepay disclosure (remaining bundles and total commitment) is Seal's or Shopify's job. Whether it can be shown is SE2 and S11.
- **Express wallets and Klarna** with subscriptions: SE6.
- `Checkout Flow.dc.html` is a **reference mock**. Its WELCOME10 maths (£6.90) is wrong; the rule is in [03 §5](03_COMMERCE_AND_SUBSCRIPTION_RULES.md#5-welcome10).

## 11. Account

**Route:** Shopify customer accounts + Seal customer portal · **Design:** `Account.dc.html`

- **Shopify-owned:** passwordless code sign-in, orders, order detail, addresses and name. The design's custom layouts apply **only as far as Shopify allows** (S6).
- **Seal-owned:** My Subscription (active, cancelled, none, journey complete). Seal portal styling follows the design where Seal allows (SE4, SE5).
- **Prepay 3:** shows shipped and scheduled bundles with no self-service cancellation (SE4).
- **Labels:** EXPIRED shows as "Journey complete"; CANCELLED shows as "Cancelled".

**Blockers:** S6, SE4, SE5, SE8.

## 12. Help Centre hub and article

**Hub route:** `/pages/help` · **Template:** `page.help.json` → `mm-help-hub`
**Article route:** `/pages/help/{handle}` (S4) · **Template:** `metaobject/mm_help_article.json` → `mm-help-article`
**Design:** `Help Article.dc.html` (index, category and article views)

| View | Content | Data |
|---|---|---|
| Hub | Search form (submits to `/search`), topic tiles, 6 most-read | `mm_help_category` (7 at launch; "Account & website" is created only once it has an article), `mm_help_article.is_most_read` |
| Category | Filtered article list with breadcrumb | Client-side filter on the hub (no category web page in v2.6) |
| Article | Breadcrumb, chip, H1, summary, last updated, blocks, 0–2 CTAs, "Was this helpful?", 3 related, "Still need help?" band | `mm_help_article` + `mm_help_block` |

**Blockers:**
- S3: Help in search.
- S4: URL.
- VAL-07: wording.
- VAL-08: CTA routes.
- VAL-09: emphasis.
- C-03: helpful-vote destination.

## 13. Contact

**Route:** `/pages/contact` · **Template:** `page.contact.json` · **Design:** `Contact Us.dc.html`

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | Route cards ("You might find your answer faster", 4) | Native multicolumn | |
| 2 | Contact form | `mm-contact-form` (Shopify `contact` form) | Fields: Name, Email, Topic (9), Order number (optional), Message (2000). A Subscription topic reveals an optional "What's it about?" field and a self-serve panel. Validation on blur; focus moves to the first error |
| 3 | Success state | Part of the form section | Uses `mascot-celebrate` |

**Content fixes:**
- Replace "[Privacy wording — to be supplied.]" (CNT-04).
- Remove the "skip" preset wording (SUB-04).
- There is no server/network error state in the design. Use the native Shopify form error (DES-13).

## 14. Track My Order

**Route:** `/pages/track-my-order` · **Template:** `page.track.json` · **Design:** `Track My Order.dc.html`

- **Sections, all native:**
  - Intro.
  - Two route cards: "Open my order email link" and "Sign in and view orders" (→ account).
  - "What you will see there" with a status rail (illustrative only).
  - Fallback panel (Help Centre, Contact).
  - FAQ ×3.
- **No lookup form**, per the decision.
- **DES-14:** the first card has no meaningful target on a generic page.
- The desktop-only launch notes in the design are internal and **not to be published**.

## 15. Search

**Route:** `/search` · **Template:** `search.json` · **Design:** `Search.dc.html`

| State | Content |
|---|---|
| Empty / discovery | "What can we help you find?", 5 quick links, finder promo |
| Too short (<2 characters; digits allowed) | Hint |
| Results | "Bundles" (product cards from product data, **no hard-coded prices**) + "Pages & guides" |
| Age intent ("newborn", "5 months", ranges, years) | Best-match bundle |
| Over-age (>12 months, "toddler") | Info panel → Independent Explorer, Shop |
| No results | Finder route, Shop, 3 popular bundles. **Never a dead end** |

- **Build:** native search and predictive search, plus `mm-search` enhancements (age-intent parsing, over-age and no-results panels).
- **Blockers:**
  - S3: Help metaobjects in search.
  - CNT-06: search synonyms.

## 16. Get 10% Off

**Route:** `/pages/get-10-off` · **Template:** `page.offer.json` · **Design:** `Get 10 Percent Off.dc.html`

| # | Section | Type | Notes |
|---|---|---|---|
| 1 | Hero + sign-up card | Native image-with-text + Klaviyo embedded form | Heading "Get 10% off your first subscription bundle". Copy must state the £72.99 reference price (L1). States: form, loading, success (code emailed, the launch mode), already subscribed, network error (K6) |
| 2 | "Using the code at checkout" (Subscribe accepted; One-time and Prepay rejected) | Native multicolumn | Static explainer |

- The desktop-only "What we record" panel is **internal documentation** and is **not published**.
- **Blockers:** K1, K2, K4, K6, L1, L6.

## 17. Policy pages

**Design:** `Policy Pages.dc.html` (six pages, v1.1 copy, **not approved for publication**, L7/L8)

| Page | Route | Resource |
|---|---|---|
| Privacy | `/policies/privacy-policy` | Shopify policy |
| Terms | `/policies/terms-of-service` | Shopify policy |
| Delivery | `/pages/delivery` | Page `page.policy` |
| Returns (with model cancellation form) | `/pages/returns` | Page `page.policy` |
| Cookies (with "Cookie settings" control) | `/pages/cookie-policy` | Page `page.policy` |
| Accessibility | `/pages/accessibility` | Page `page.policy` |

- **Page structure:** the "On this page" navigation (sticky 250 px sidebar on desktop, collapsible on mobile) comes from `mm-policy-anchors`. Mobile tables become key/value cards. There is a shared company block.
- **Shopify policy routes:** the native `/policies/*` routes have a fixed layout. Anchor navigation on Privacy and Terms depends on S10.
- **Checkout policy links:** the native Shipping and Refund policies must be filled with, or point to, the approved text so the links shown at checkout match (S10).

## 18. 404

**Template:** `404.json` · **Design:** `404 Page.dc.html`

Native rich text and image:
- "404 — Oops, this page has wandered off."
- Shop Bundles and Find My Bundle buttons.
- "Or try" links.
- `mascot-waving`: to the right on desktop, above the text on mobile.

No dependencies.
