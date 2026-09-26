# 02 Component and section specification

This is the authoritative implementation inventory.

**Sources reconciled:**
- Custom Sections Spec.dc.html (19 items)
- Component Library.dc.html
- SiteHeader.dc.html and SiteFooter.dc.html
- The approved page designs
- v2.6 sheet 10

Where a page design and a spec disagree, the **page design wins** and the difference is logged in [07](07_IMPLEMENTATION_OPEN_ITEMS.md).

**Shared conventions for every bespoke item:**
- **Files:** `sections/mm-*.liquid`, `snippets/mm-*.liquid`, `assets/mm-*.js|css`.
- **Settings:** every piece of customer-facing copy is a setting or block (SEC-01).
- **Anchors:** anchor targets use the global scroll-margin token (SEC-02, ≈96 px).
- **Accessibility baseline:**
  - Real `<button>` and `<a>` elements.
  - One anchor per card, never nested links (A11Y-01).
  - 44 px minimum targets.
  - Visible focus.
  - `aria-hidden` on decorative art.
  - Meaning never carried by colour or icon alone (A11Y-02).
  - Motion off under `prefers-reduced-motion`.
- **JavaScript:** vanilla modules, loaded deferred. Content must be visible without JavaScript.

---

## 1. Native Eurus sections to configure (not rebuild)

The mapping must be confirmed against the installed Eurus/Swirl version (**S9**). Anything that can't meet the design becomes bespoke and is added here.

| Use | Where | Required configuration |
|---|---|---|
| Announcement bar | Global | "Subscribe & get 10% off your first bundle →" linking to `/pages/get-10-off`; not dismissible; purple `#5A63BD`; 30 px tall on mobile with the whole bar as the link |
| Desktop header | Global ≥ desktop | Sticky; shrinks after 4 px of scroll. Logo 42 → 38 px. Nav: Shop Bundles (emphasised), How It Works, Why Mini Minds. Inline search pill (212 px); account icon; basket icon with count; "Find My Baby's Bundle" CTA that **appears after scrolling past the hero** (the design uses 420 px). If Eurus can't reveal the CTA on scroll, add `mm-header-cta-reveal.js` (S9) |
| Desktop footer | Global ≥ desktop | Background `#4B4699`. Brand column (logo on a white pill, tagline, socials). Columns: Shop (All + six stages), Learn, Help, Company. **Cookie settings is a button that opens the consent panel** (C-01). Payment icons are **Shopify-native payment icons**, not the design's text chips. Legal line |
| Rich text / image-with-text / image banner / multicolumn | Content pages, 404, Track, Offer | As listed in [01](01_PAGE_TEMPLATE_MAP.md) |
| Collapsible content (FAQ) | Home (multi-open), Why (single-open, first open), Track | S9 for single-open |
| Contact form (base) | Contact | Extended by `mm-contact-form` |
| Cart | `/cart` | Discount field **off**. Drawer behaviour per DES-12. Line items extended by `mm-cart-plan-label` |
| Search | `/search`, predictive search | Extended by `mm-search` |
| Policies | `/policies/*` | Shopify policy editor |

## 2. Global bespoke components

### 2.1 `sections/mm-mobile-header.liquid` (Custom Spec T1-7)

- **Purpose:** the mobile header.
- **Pages:** all, below the desktop breakpoint.
- **Data:** section settings, cart count, menu (`linklists`).
- **Settings:** announcement text and link; logo; drawer menu; CTA label and link (default finder); sticky on/off.
- **Layout and states:**
  - One 56 px row: logo, Search, Basket (with count), Menu. Buttons are 44×44.
  - Tapping Search expands a search row with autofocus and a close button; Escape closes it.
  - Menu opens a right drawer: 300 px, max 84 %, dimmed backdrop, 280 ms slide, page scroll locked.
  - **Drawer contents:** logo and close button; a full-width "Find My Baby's Bundle" button; links to Home, Shop Bundles, Why Mini Minds, How It Works, About Us, **Help Centre** (the design's label "FAQs" is overridden by the naming decision); a divider; Account.
- **Accessibility:** `aria-expanded` and `aria-controls` on the toggles; focus trapped in the drawer; Escape and the backdrop close it; focus returns to the opener; the hamburger becomes an X.
- **JavaScript:** yes (`mm-mobile-header.js`, using the `mm-a11y` modal helper).

### 2.2 `sections/mm-mobile-footer.liquid` (T1-8)

- **Purpose:** the mobile footer.
- **Settings:** logo, tagline, social links, four menus (Shop / Learn / Help / Company), payment icons (native), copyright, legal line.
- **States:** four accordions (44 px buttons, rotating caret), **collapsed by default**; several can be open at once.
- **Cookie settings:** a `<button>` that opens the consent panel.
- **JavaScript:** accordion (`mm-accordion.js`, shared).

### 2.3 `snippets/mm-bundle-card.liquid`

- **Purpose:** the bundle card used by `mm-bundle-grid` and Search results.
- **Inputs:** `product`, and `mode` (`once` | `sub`).
- **Content:**
  - Image area at a 1200/896 aspect ratio on the stage tint.
  - Age pill; "Stage n of 6"; title; short description; price line; "View Bundle".
- **Price line by mode:**
  - `once`: "£72.99 · one bundle · free UK delivery".
  - `sub`: "£68.99 · per bundle · every 2 months · free UK delivery". The link carries the plan preselection.
  - If the product has no subscribe plan: stays one-time and shows "Final stage — sold on its own."
- **Prices** come from the product and selling-plan data, never literals.
- **Hover:** lift animation (off under reduced motion). The card is one anchor.

### 2.4 Consent banner and panel (`snippets/mm-consent.liquid` + app)

**Depends on the consent platform choice (C-01).** The requirements come from the `mm-consent.js` prototype:
- **Banner:** a fixed card at the bottom, titled "Your cookie choices", with a Cookie Policy link. Buttons: "Reject non-essential" and "Accept all" at **equal visual weight**, plus "Manage preferences".
- **Panel:** Essential is always active; functional, analytics and advertising are toggles, all **off by default**. Buttons: Reject, Accept, Save.
- **Behaviour:** re-prompt after 180 days or when the version changes. Scripts register per category and load only after consent. Withdrawing consent deletes that category's cookies. Map choices to Shopify's Customer Privacy API (S8).
- **Categories:**
  - Functional: `jdgm*`, `seal*`
  - Analytics: `_ga*`, `_shopify_s/y`
  - Advertising: `_fbp`, `_gcl*`, `__kla_id`, `_ttp`
- **Access:** reachable from the footer "Cookie settings" control on every page.

### 2.5 Utilities

| File | Purpose | Notes |
|---|---|---|
| `assets/mm-reveal.js` + `mm-reveal.css` (T1-6) | Scroll reveal. Presets: standard, calm, quick. Rises 4–8 px; 90–175 ms stagger | IntersectionObserver. The hidden state is only applied after the script arms it, so content stays visible without JavaScript. Off under reduced motion |
| `assets/mm-a11y.js` | Modal manager for `[data-mm-modal]`: inerts the background, traps focus, closes on Escape, returns focus. Also makes a button wrapped in a link a single tab stop | Port of the prototype helper. The prototype's global CSS overrides move into theme CSS tokens instead |
| `assets/mm-accordion.js` | Shared accordion (single-open or multi-open; animated height; `aria-expanded`) | Used by the mobile footer, product FAQ, bundle contents on mobile, and policy anchors on mobile |
| Anchor offset (CSS) | `scroll-margin-top` token | SEC-02. CSS only |

## 3. Shared (multi-page) bespoke sections

### 3.1 `sections/mm-stage-timeline.liquid` (T1-1)

- **Pages:** How It Works (flat and rising), About (rail), product page (rail, via `mm-product-journey`).
- **Data:** `shop.metaobjects.mm_stage.values`, sorted by `stage_order`. The current stage comes from `product.metafields.custom.stage` when available.
- **Settings:** heading; intro; layout (`flat` | `rising` | `rail`); highlight current stage; show descriptions; link tiles to products; anchor ID; padding.
- **States:** current stage shows a filled node plus a **text label** ("You are here"); earlier stages are muted ("Earlier stage"); later ones read "Coming up". 2–3 px hover lift.
- **Mobile:** `flat` becomes a vertical line with 6 nodes, and items link to products. **`rising` doesn't render on mobile**; a list of link rows replaces it. `rail` becomes a horizontal scroller (product page cards 150 px).
- **Accessibility:** `<ol>`; the current state is announced as text.
- **JavaScript:** none (a horizontal scroller may use `mm-carousel.js` for drag and keyboard).

### 3.2 `sections/mm-five-pathways.liquid` (T1-2)

- **Pages:** How It Works (expanded), About (compact), product page (product density).
- **Data:** `mm_pathway` sorted by `pathway_order`: `name`, `icon_asset`, `colour`, `definition`, `example`, `summary`. On the product page, `stage.featured_pathways` (3, in order) are badged "Especially active" and the rest "Also supported".
- **Settings:** heading; intro; density (`compact` | `expanded` | `product`); show examples; columns; anchor; optional CTA (e.g. "Take the Quiz").
- **Text by density:** product density shows `summary`; the others show `definition` + `example`.
- **Mobile:** 2-column grid with the fifth card full width. Icons ≥34 px.
- **Rules:** PTH-01, PTH-03 (hide the "Most active" panel if fewer than three are set), PTH-04.

### 3.3 `sections/mm-play-pause-progress.liquid` (T1-3)

- **Pages:** How It Works (panels), About (rows), product page.
- **Settings:** eyebrow, heading, line. For each of the three **fixed** panels: label, title, copy, tint, icon, photo. Also image heights, variant (`panels` | `rows`), connectors, anchor.
- **Images:** 128 px desktop / 146 px mobile, `object-fit: cover`.
- **Mobile:** stacked, with ↓ connectors (`aria-hidden`).
- The panels can't be removed or reordered.

### 3.4 `sections/mm-parent-guide.liquid` (T1-4)

- **Pages:** How It Works, Why Mini Minds. On the product page, the Guide card inside `mm-bundle-contents` uses `stage.guide_cover_image`.
- **Settings:** eyebrow; heading; body; spread image; cover image; lightbox on/off; chip label; three callout blocks; four card blocks; CTA; reverse layout; anchor; visual width %.
- **Data:** on the product page, prefers `stage.guide_spread_image` and falls back to the section's image (IMG-201).
- **Lightbox:** trigger is a real button; closes on ×, Escape or backdrop click; native pinch zoom; the same alt text inline and expanded; the Guide's text is also provided as screen-reader-only text.
- **Mobile:** visual first, ≥240 px tall, never cropped.
- **JavaScript:** lightbox (`mm-a11y` modal).

### 3.5 `sections/mm-quiz-cta.liquid` (T1-5)

- **Pages:** Home (section 13), How It Works, Shop Bundles ("Not sure where to start?").
- **Settings:** eyebrow, heading, copy, CTA label, **link picker** (the quiz route is not hard-coded, R5), microcopy, colour scheme, alignment, spacing (compact | standard), illustration (mascot).
- **No quiz logic.**

### 3.6 `sections/mm-three-steps.liquid` (T1-9)

- **Page:** Home 06.
- **Blocks:** step (number, title, copy, illustration: IMG-501, bundle master, IMG-302).
- **Settings:** connector; primary CTA (Find My Bundle); secondary CTA (Shop All); anchor.
- **Mobile:** vertical stepper without images; CTAs stacked.

### 3.7 `sections/mm-bundle-grid.liquid` (not in the Custom Spec; required by the designs)

- **Pages:** Home 07, Shop Bundles 4, About 8 (with the toggle off).
- **Data:** a collection (the six bundles, sorted by `custom.stage.stage_order`); `mm-bundle-card`.
- **Settings:** heading; intro; show the pricing toggle; show the gradient timeline header; "Not sure where to start?" link; anchor (`bundles-grid`).
- **Toggle:** "Buy once / Subscribe". A `role="radiogroup"` with roving tabindex and arrow keys. It switches every card's price mode. The label is "Pricing shown for all six bundles".
- **Mobile:**
  - Scroll-snap carousel with 318 px cards and a peek of the next card.
  - Mouse drag; six dots with 44 px targets.
  - Tapping a card that isn't current scrolls it into place instead of following its link.
  - Region label "Stage bundles, scrolls sideways".
- **JavaScript:** `mm-carousel.js` + toggle.
- **Dependencies:** SE1 (the membership that drives `sub` mode).

### 3.8 `sections/mm-subscription-panel.liquid` (not in the Custom Spec)

- **Pages:** Home 08 ("Stay stage-matched as they grow"); Shop Bundles 3 ("Buy once, or grow with them", band variant).
- **Settings:** heading; copy; benefit list; CTA 1 ("Start a subscription" → finder); CTA 2 ("How subscriptions work" → `/pages/how-it-works#subscriptions`); variant (panel | band).
- **Prices:** the subscription and one-time prices are read from the first bundle's selling plan and variant.
- **Do not implement** the prototype's `?stage=` content swap (DES-05).

## 4. Page-specific bespoke sections

### 4.1 Homepage

| File | Purpose / settings | States and behaviour | JavaScript |
|---|---|---|---|
| `mm-home-hero` | Eyebrow; H1; supporting line; price line (from product data); primary CTA; quiz text link; image with blob clip; stage-pill strip (desktop only) | Contains the `mm-pathway-rotator` snippet | Via the rotator |
| `snippets/mm-pathway-rotator` (T3-4) | Lead-in ("Learning to"); **word blocks** (word, pathway icon, text colour); interval ≈2.7 s | 400 ms fade/slide. **44 px pause/play button** (`aria-pressed`). Static under reduced motion. The visual rotator is `aria-hidden` and a screen-reader sentence lists every word. No layout shift | Yes |
| `mm-challenge` (T2) | Eyebrow; heading; rich-text statement (**with source citation**, CNT-05); three question blocks (illustration + text); reveal preset | Staggered reveal; icon hover lift; illustrations have empty alt text; units capped at ~300 px on mobile | `mm-reveal` |
| `mm-way-cards` (T2) | Heading; supporting and closing copy; up to 4 card blocks (image, heading, body, link, label, tint, elevate); CTA; mascot animation on/off | **Each card is one anchor.** Hover lifts 4 px (featured card 12 px) and scales the image slightly. Mobile: horizontal rows with 88 px thumbnails | `mm-mascot-spark` |
| `snippets/mm-mascot-spark` | Mascot "idea" animation (bulb, glow, sparkles, wink), 1.45 s, **plays once** when ≥55 % visible; replays on hover | Static resting pose under reduced motion. Assets `mascot-base.png`, `mascot-pupil-right.png` | Yes (port of `mascot-idea-spark.jsx` to vanilla JS) |

### 4.2 Shop Bundles

| File | Purpose / settings | Behaviour |
|---|---|---|
| `mm-collection-hero` (T2) | Heading; line; chips; buttons; image (IMG-303); mascot pose and card copy; finder link; scheme | Mascot card below the image on mobile. Mascot wave animation (reduced-motion safe) |
| `mm-stage-chip-rail` | Six stage chips + "Not sure? Find their best fit →" | **Desktop:** chips link to product pages. **Mobile:** sticky horizontal rail with edge fades and drag/wheel scrolling; chips anchor-scroll to cards; the active chip follows the card in view; once stuck, a "Not sure?" link appears. JavaScript: yes |
| `mm-when-what-how` (T2) | Exactly three cards (illustration, label, heading, copy, accent, motif type) | WHEN capsules come from `mm_stage`; WHAT icons from `mm_pathway`; HOW uses the PPP icons. CSS arrows; motif opacity 62 % → 95 % on hover. Mobile stacks with ↓ |

### 4.3 Product page (all read `product.metafields.custom.stage` / `custom.toys`)

| File | Purpose | Key behaviour | JavaScript |
|---|---|---|---|
| `mm-product-gallery` | Gallery: bundle master → Guide cover → toys → lifestyle | **Desktop:** 92 px vertical thumbnail rail (5 visible, ↑/↓ buttons dim at the ends); 560 px main image with ‹ › arrows hidden at the ends; "n / N" counter. **Mobile:** 330 px main image, swipe (40 px threshold), 58 px thumbnail strip, caption "Swipe the image or tap a thumbnail · n/N". Images `contain` on the stage tint | Yes. S1 |
| `mm-product-buy-box` | Age pill, "Stage n of 6", H1 `{stage_name} Bundle`, `pdp_subhead`, price + badge, "Close to the next stage?" link (hidden on stage 6), what you get (`{toys.size}` toys; Guide; PPP line), plan selector, plan panels, qty 1–9, Add, Buy Now, gift, trust grid with policy links | See the snippets below and [03](03_COMMERCE_AND_SUBSCRIPTION_RULES.md) | Yes |
| `snippets/mm-plan-selector` | Radio cards: One-time £72.99; Subscribe £68.99 "per bundle" (Popular, Save 5%); Prepay 3 £195.99 "upfront" (Save over 10% / Save £22.98). Only the plans the product belongs to are rendered | One-time is preselected. A requested plan that isn't eligible falls back to One-time with a visible notice. Plan and qty are written to the URL (`replaceState`) | Yes. **SE10** (theme-rendered selector vs Seal widget) |
| `snippets/mm-next-stage` (T3-1) | Subscribe panel: Today → Next (`next_stage.stage_product`); "How it works" steps; totals line. Prepay panel: three stage tiles plus the price breakdown | Hidden for One-time. Shows three stages for Prepay. On stage 6 (no `next_stage`) there's no Next. **Dated schedules only from derived Finder timing, never DOB (DES-01)** | Via the buy box |
| `snippets/mm-gift-fields` | "Send as a gift" checkbox reveals recipient email (optional, validated) and gift message (≤200, live counter) as line-item properties | "Buying as a gift and unsure of the stage?" links to the finder. No verification wording | Yes. S5 |
| `mm-sticky-buy-bar` (mobile) | Price × qty and plan summary + main button | Until the plan options have been seen, the button reads "Choose how to buy" and scrolls to them; after that it becomes Add. The added confirmation expands inside the bar with "Go to Basket →". **DES-09:** give one-time buyers a quantity control too | Yes |
| `mm-stage-education` | Eyebrow `{age_range_label} · {stage_theme_label}`; H2; `stage_intro`; three `noticing_signals` cards; `lifestyle_image` on `stage_wash_colour` | Text left and image right on desktop; stacked on mobile | No |
| `mm-bundle-contents` (T2) | Guide feature card (`guide_cover_image`, 4 bullets) + toy grid from `custom.toys` (image, name, `what_it_invites`, ≤2 pathway labels) | 4 per row on desktop; handles 3–12 toys with no orphan rule decided (CNT-07); accordion on mobile; the toy count is derived (TOY-03). **Remove the prototype's "pull from product metafields" annotation** | Mobile accordion |
| `mm-product-journey` | "The journey continues": intro and line (plan-aware); `mm-stage-timeline` rail; next-stage card ("View That Bundle"); previous/next nav cards | **Stage 6:** a completion card from section settings (heading, description, optional CTA; copy pending VAL-06) in place of the next card. No 12–18-month promise | No |
| `mm-product-faq` | Two fixed Q&As + four accordions, each with a Help Centre link | The "between stages" answer uses variant A (has a next stage) or B (stage 6), both as settings | `mm-accordion` |
| `mm-product-recap` (desktop only) | Recap image, price + badge, plan summary, Add, "Change purchase option" | DES-08: the control must reach every eligible plan | Yes |

### 4.4 Find My Bundle: `sections/mm-finder.liquid`

- **Data:** Liquid renders a JSON island of the six stages (`stage_order`, `age_start`, `age_end`, `stage_name`, `age_range_label`, product handle and URL, featured image, `stage_colour`). No stage boundaries are hard-coded in JavaScript.
- **Settings:** all copy; `LEAD_TIME_DAYS` (5) and `BUMP_WINDOW` (14) as **separate number settings**; the Klaviyo form or list configuration.
- **Screens:**
  - **Form:** DD/MM/YYYY boxes with auto-advance and `bday-*` autocomplete; optional email; optional unticked consent; "Find My Baby's Bundle"; "No email needed to see your recommendation."
  - **Result:** "We recommend {stage}", image, pills, arrival line, Subscribe/Buy once CTAs (the Subscribe CTA only if the product is eligible), "Or start with {alt}", "Use a different date", journey strip, "Not ready yet?" reminder.
  - **Timing choice:** shown only when the stage was bumped: "Start at N months" (recommended) or "Send it now".
  - **Out-of-range:** 12+ months: "Our bundles cover 0–12 months"; optional "Keep me updated" with its own consent; links to Independent Explorer and Shop.
- **Validation:** on blur and on submit (empty, incomplete, impossible, future). Errors use `role="alert"` and `#B4545C`. Focus moves to the first invalid field.
- **Privacy:** FND-01…05 and DATA-01. **The exact DOB never leaves the browser session.** The hand-off to the product page carries derived state only ([04 §7](04_DATA_AND_INTEGRATION_CONTRACTS.md#7-finder-and-quiz-data-flow)).
- **Consent:** the confirmation copy that promises a code appears **only if consent was ticked** (DES-02).
- **JavaScript:** yes (`mm-finder.js`). Uses pushState/popstate for screens **without DOB in the URL**.

### 4.5 Help Centre

| File | Purpose | Key behaviour |
|---|---|---|
| `mm-help-hub` | Search form (→ `/search`; an empty submit shows an inline message), topic tiles (`mm_help_category`: `icon_name` glyph, `tint`, name, blurb), 6 most-read (`is_most_read`, `most_read_order`), and a category view as a client-side filter with breadcrumb | 4-up desktop, 2-up mobile. Only categories with ≥1 active article render (HELP-01) |
| `mm-help-article` (metaobject template) | Breadcrumb; category chip; H1 `title`; `summary`; `last_updated`; `content_blocks` rendered by type (paragraph, heading, list with `list_style`, note with label, progression as stage chips from `mm_stage`); `cta_label/link` (primary) + `secondary_cta_label/link` (outline); "Was this helpful?" (No reveals the Contact route); up to 3 `related_articles`; "Still need help?" band | `rich_text_field` rendered with `metafield_tag`. HELP-03/04. The helpful-vote destination is C-03 |

### 4.6 Other pages

| File | Purpose | Notes |
|---|---|---|
| `mm-contact-form` | Shopify `{% form 'contact' %}` with fields `contact[name]`, `[email]`, `[topic]`, `[order_number]`, `[message]`, `[subscription_reason]` | Topic-dependent order-number help text; the Subscription topic reveals the reason field and a self-serve panel. Validates on blur; focus moves to the first error; `aria-invalid`; 2000-character counter from 1500; success summary. JavaScript: yes |
| `mm-search` (enhancements to the native search template and predictive search) | Discovery state, age-intent parsing, over-age panel, no-results panel with finder route, grouped results (Bundles via `mm-bundle-card`; Pages & guides) | `aria-live` result count; `?q=` in the URL. **S3** decides whether Help articles appear |
| `snippets/mm-policy-anchors` (T3-3) | Builds the "On this page" navigation from the headings in the policy body | Sticky 250 px sidebar on desktop; collapsible on mobile. Separate IDs aren't needed (one DOM). S10 for `/policies/*` |
| `snippets/mm-cart-plan-label` (T3-2) | Per-line route chip ("One-time purchase" / "Subscription — every 2 months" / "Prepay 3 bundles — paid today"), plan note (next stage and date or "Final stage"), gift note; **"Change" control** listing eligible plans with consequence lines | Reads `line_item.selling_plan_allocation` + Seal properties (SE8). Changing the plan uses the cart API with `selling_plan` (S12). Removing a line is removal, not cancellation (CART-02) |
| `snippets/mm-cart-summary-notes` | Subscription and prepay summary notes; reassurance (codes at checkout; free delivery; 14 + 14 returns) | Text driven by the plans in the cart |

## 5. App blocks and embeds

| App | Block | Where | Requirements |
|---|---|---|---|
| Seal | Selling plans (data); customer portal; widget only if SE10 requires it | Product, cart, account | Portal labels: "Journey complete" / "Cancelled". No self-service cancel for Prepay 3 (SE4). Cancellation reasons (SE5) |
| Judge.me | Product review widget; homepage carousel | Product 8; Home 09 | Six separate product pools; **hide when empty** (J5); photo gallery with cross-review swipe on mobile (J4); no invented ratings |
| RevenueHunt | Quiz embed | `page.quiz` | Design block mapping and guardrails; results link by product handle (R4) |
| Klaviyo | Embedded forms | Get 10% Off; Finder email and reminder; out-of-range "Keep me updated" | Consent fields per [04 §5](04_DATA_AND_INTEGRATION_CONTRACTS.md#5-klaviyo); states (K6) |

## 6. Reconciliation with older component sources

| Source item | Decision |
|---|---|
| Custom Spec 19 items (T1-1…9, T2 ×6, T3 ×4) | All kept above. T2 "help hub / article" is split into two files |
| Custom Spec: quiz CTA only on How It Works and Shop | **Overridden:** the homepage design includes it (DES-04) |
| Custom Spec: next stage via `custom.next_stage_product` | **Overridden by v2.6:** traversal through `custom.stage → next_stage → stage_product` |
| v2.5 layered hero; solution cards | Replaced by `mm-collection-hero` and `mm-way-cards` |
| Eurus Handoff: "Buy once, or grow with them" band, homepage stage grid + toggle, `#subscription` panel (unspecified) | Now specified: `mm-bundle-grid` and `mm-subscription-panel` |
| Eurus Handoff: "Today → Next" as an app block | Theme snippet `mm-next-stage` reading Seal and Shopify data |
| Component Library: "Launch offer" chip, email sign-up block ("Join the Mini Minds journey"), "Due date works too" step copy | **Obsolete, not built** |
| Component Library: pathway badges using design-system icon glyphs | **Not used.** Pages use the `pathway-*.png` art (`mm_pathway.icon_asset`) |
| Component Library: buttons (primary/secondary/outline/ghost × sm/md/lg), inputs, accordion, qty stepper, trust row, review placeholder | Implement as theme styles and snippets matching the tokens |
| SiteHeader: basket `onCartClick` drawer mode | Not used (the basket icon goes to `/cart`) |
| SiteFooter: payment text chips | Replaced by Shopify native payment icons |
| `Basket.dc.html` drawer | Obsolete |
| v2.6 sheet 10 (19 components) | A build-scope reference. **This document supersedes it as the implementation inventory.** The data model is unaffected |
