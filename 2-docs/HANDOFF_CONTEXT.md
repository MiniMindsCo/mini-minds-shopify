# HANDOFF CONTEXT: Mini Minds Co Shopify website

Read this first in any new technical session. Details are in the numbered docs in `2-docs/`.

## 1. Project structure

| Path | Contents |
|---|---|
| `1-design-reference/` | Workspace note only. The design itself lives in Claude Design |
| `2-docs/` | Implementation specification (00–07 + this file) |
| `3-data-model/Mini_Minds_Shopify_Data_Model_v2.6.xlsx` | **Canonical structured data** (schema, seed data, rules, Klaviyo and line-item contracts) |
| `3-data-model/CHANGELOG_v2.6.md` | What was decided in v2.6 |
| `3-data-model/source-v2.5/` | Historical workbooks. Read-only; never edit |
| `4-assets/` | Empty. Assets stay in Claude Design until upload (see 06) |
| `5-theme/` | Empty. The Eurus theme build will go here |

**Claude Design project:** "Mini Minds Co Site Wireframe" (`16363810-1b37-42dd-ac5d-48f53a3788ad`).
- The root `*.dc.html` files are the approved UX, at 1440 desktop and 390 mobile.
- `archive/` is historical only.
- `uploads/` holds older source material and must not be built from.

## 2. Authority order

1. Owner decisions (v2.6 CHANGELOG and these docs).
2. Root Claude Design pages.
3. The v2.6 workbook.
4. Root handoff and spec pages.
5. Anything older.

Contradictions are logged in `07`, never silently resolved.

## 3. Architecture (approved)

- **Theme:** Eurus (OS 2.0), Swirl preset. Native sections are configured first; bespoke `mm-*` sections and snippets are listed in `02`.
- **One product template** serves six bundles:
  - `new-beginnings`
  - `awakening-senses`
  - `little-explorer`
  - `mover-and-shaker`
  - `curious-climber`
  - `independent-explorer`
- **Product metafields:** only `custom.stage` (→ `mm_stage`) and `custom.toys` (→ ordered `mm_toy`). Next bundle, age label, pathways and toy count are **derived**, never stored.
- **Metaobjects (6):**
  - `mm_stage` (6)
  - `mm_pathway` (5)
  - `mm_toy` (43; B2 Tummy Time Mirror and B4 Crinkle Animal Mirror are separate)
  - `mm_help_category`
  - `mm_help_article` (22, renderable)
  - `mm_help_block`
- **Purchase eligibility:** Seal selling-plan membership only (Subscribe: products 1–5; Prepay 3: products 1–4).

## 4. Commercial rules (confirmed)

**Prices and routes:**
- **One-time:** £72.99, no compare-at price.
- **Subscribe every 2 months:** £68.99 per bundle.
- **Prepay 3:** £195.99, worded "Save £22.98" or "Save over 10%".
- **WELCOME10:** the first subscription bundle is **£65.69** (10% off £72.99, which must be stated), then £68.99. Subscription first order only.

**Subscription behaviour:**
- Moves one stage per paid bundle and ends after Independent Explorer.
- **No pause or skip. No resume or re-base.**
- Prepay 3 is an upfront purchase of three deliveries with no self-service cancel.
- Stage 6 is one-time only, with a completion card.

**Delivery and gifts:**
- Free UK delivery on every parcel; 3–5 business days.
- Gift: optional recipient email and message (≤200 characters) captured on the product page before checkout, printed on an insert. No verification claim.

**Finder and quiz:**
- Finder: exact age at arrival (today + 5 days); bump when fewer than 14 days of the stage remain at arrival (19 days from today); 12+ months shows the out-of-range screen.
- Quiz: month-level `YYYY-MM`, no bump.
- **The exact DOB stays in the Finder's browser session only.** Never in URLs, storage, Shopify, Klaviyo, analytics or orders (DATA-01).

## 5. Integrations

| System | Role |
|---|---|
| Shopify | Commerce, checkout, accounts, search, tracking |
| Seal | Subscriptions and portal |
| Judge.me | Six review pools; hidden while empty |
| RevenueHunt | Quiz |
| Klaviyo | Consent-based email and WELCOME10 |
| Consent platform | **Not yet chosen** |

One sender per event (see `00 §6`).

## 6. Do not re-decide without new evidence

- Prices and wording.
- WELCOME10 base.
- Eligibility matrix.
- No pause or skip.
- DOB handling.
- Product handles.
- Stage colours: `#AFA0CC`, `#D1EBC4`, `#E7C089`, `#A1D0ED`, `#EDBA6B`, `#AE96D0`.
- Curated featured pathways.
- Two product metafields only.
- Help on metaobjects (unless S3 proves impossible).
- 22 Help articles.
- `Basket Page` is the basket (no drawer auto-open).
- "Help Centre" naming.
- Free delivery.
- Judge.me, Seal, Klaviyo, RevenueHunt as the chosen apps.

## 7. Outstanding blockers (full list in `07`)

**Platform:**
- S3: Help in search.
- S6: account customisation.
- S7 / SE7: the £65.69 mechanism.
- S9: Eurus native capability.
- S11: checkout disclosure.
- SE1–SE4: Seal eligibility, counting, scheduling, prepay.
- SE9: quantity semantics.
- SE10: theme-rendered plan selector.
- R2 / R3: quiz data.
- C-01: consent platform.

**Legal:** L1–L8. Nothing in the policy pack is published before L7/L8 close.

**Content:** wash colours, featured pathways, stage copy approval, alt texts (VAL-*); About hero and founder portrait (AST-01/02).

## 8. Implementation sequence

1. **Platform verification:** Seal sandbox (SE1–SE10); Shopify checks S1–S13. Record the answers in `07`.
2. **Store set-up:** create the six metaobject definitions and two product metafields from v2.6. Create the 6 products with the handles above. Seed stages, pathways, toys and Help. Upload assets per `06`.
3. **Theme foundation:** install Eurus/Swirl; map tokens and colour schemes; configure the global header and footer; add the utilities (`mm-a11y`, `mm-reveal`, `mm-accordion`, anchor offset); add consent hooks.
4. **Shared sections:** timeline, pathways, Play/Pause/Progress, Parent Guide, quiz CTA, bundle card and grid, subscription panel.
5. **Marketing pages:** Home, Shop, How It Works, Why Mini Minds, About.
6. **Product template:** gallery, buy box, plan selector, gift, sticky bar, education, contents, journey, FAQ, reviews.
7. **Conversion tools:** Finder, Quiz embed, Get 10% Off, basket plan labels and plan change.
8. **Support pages:** Help hub and article, Contact, Track My Order, Search, Policies, 404.
9. **Integrations:** Seal portal, Judge.me, Klaviyo forms and flows, analytics after consent.
10. **QA:** Test Plan journeys; widths 375, 390, 430 and desktop; keyboard and screen reader; reduced motion; live parcel test (S15); WCAG audit.

**Not started (as of this document):** theme code, Shopify store configuration, Notion tickets.
