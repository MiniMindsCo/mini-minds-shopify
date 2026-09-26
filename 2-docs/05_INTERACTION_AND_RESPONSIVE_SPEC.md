# 05 Interaction and responsive specification

This document only contains behaviour shown or annotated in the approved root designs. Where the design is silent, the gap is logged as a DES item in [07](07_IMPLEMENTATION_OPEN_ITEMS.md) rather than invented. Component ownership is in [02](02_COMPONENT_AND_SECTION_SPEC.md).

---

## 1. Design tokens and breakpoints

**Colour** (map to Eurus colour schemes):

| Role | Values |
|---|---|
| Lilac (primary / CTA) | `#A183FF` |
| Indigo (links) | `#6A74CC` |
| Purple-700 (headings) | `#4B4699` |
| Ink (body) | `#1B1B1F` |
| Ink-600 (secondary) | `#55555E` |
| Beige (page backgrounds) | `#DBD1C5` / `#EFEBE4` |
| Aqua-white | `#F5FFFF` |
| Lilac wash | `#EDE7FF` / `#F6F3FF` |
| Hairline | `#E7E1D8` |
| Lavender border | `#AE9FCC` |
| Error text | `#B4545C` |
| Feedback: success | `#7FB98E` / `#D1EBC5` |
| Feedback: warning | `#D89A4E` / `#E7BF89` |
| Feedback: danger | `#D9787E` / `#F3D6D8` |

**Stage and pathway colours come from data.** Stage colours are `mm_stage.stage_colour` / `stage_wash_colour`; pathway colours are `mm_pathway.colour`. **The README "stage tints" line is stale** (DES-16).

**Type and layout:**

| Token | Value |
|---|---|
| Headings | Quicksand 700 |
| Body | Nunito 500 (600/700 for emphasis) |
| Icon font | Icons South St (glyph names used by `mm_help_category.icon_name`) |
| Case | Sentence case |
| Spacing | 4 px base: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96 |
| Container | 1200–1280 px |
| Reading width | 760 px |
| Radius | 8 / 12 / 16 (inputs) / 24 (cards) / 32 / pill (buttons, chips) |
| Shadows | `0 2px 8px rgba(75,70,153,.08)`, `0 8px 24px …/.10`, `0 16px 40px …/.14` |
| Easing | Soft `cubic-bezier(.34,1.2,.64,1)`; out `cubic-bezier(.22,.61,.36,1)` |
| Motion | 120–360 ms; fades and small translate/scale only |

**Breakpoints:**
- The design defines **desktop (1440 frame)** and **mobile (390 frame)**.
- QA widths are 375 / 390 / 430.
- **Tablet and intermediate desktop widths are not designed** (DES-15). Use Eurus's breakpoints and verify that fixed-width design elements don't overflow:
  - homepage H2 widths with `nowrap`
  - the How It Works rising timeline's fixed 1280 px geometry
- Mobile-specific imagery uses independent mobile image settings where the design has a separate crop (MOB-01).

## 2. Header and navigation

- **Desktop header:**
  - Sticky. After 4 px of scroll it gets a shadow, padding tightens from 11 px to 8 px, and the logo shrinks from 42 px to 38 px.
  - The current page's nav item is marked with weight 800.
  - The "Find My Baby's Bundle" CTA appears only after the user scrolls past the hero. The design uses 420 px for every page; if implemented, derive the threshold from the page's hero (DES-17).
- **Basket badge:** shows the Shopify cart count and pops when the count changes. aria-label "Basket, N items" or "Basket, empty".
- **Mobile header:** see [02 §2.1](02_COMPONENT_AND_SECTION_SPEC.md#21-sectionsmm-mobile-headerliquid-custom-spec-t1-7). Search expands under the header; the drawer is a modal.
- **Desktop and mobile navigation differ on purpose:**
  - Desktop: 3 items.
  - Mobile drawer: Home, Shop, Why, How, About, Help Centre, Account.
  - About is not in the desktop header (DES-18 records this for confirmation).

## 3. Search

| Condition | Behaviour |
|---|---|
| Typing | Live suggestions (up to 4: bundles and key pages). The "Bundle/Page" label shows on desktop only. Clear (×) button |
| Fewer than 2 characters | Hint; digits are allowed |
| Results | `?q=` updated; `aria-live` status with the count |
| Ages ("newborn", "5", "5 months", ranges, "half a year", years) | Mapped to a stage → "Best match" bundle |
| Over 12 months / "toddler" | Over-age panel |
| No results | Finder route, Shop, and popular bundles. **Never a dead end** |

The desktop header search is a GET form to `/search`. On mobile, search expands inline. Help article results depend on **S3**.

## 4. Carousels and horizontal scrollers

| Where | Behaviour |
|---|---|
| Home stage grid (mobile) | Scroll-snap; 318 px cards with a peek; mouse drag; 6 dots (44 px targets); tapping a card that isn't current snaps to it rather than following its link; keeps position on resize; region label |
| Shop chip rail (mobile) | Sticky; edge fades; drag and wheel; the active chip follows the card in view; "Not sure?" appears once stuck |
| Product journey (mobile) | Horizontal 150 px cards |
| About timeline (mobile) | Horizontal scroller |
| Reviews (mobile) | Judge.me swipe rail + photo lightbox with previous/next and Escape (J4) |

All scrollers must be keyboard reachable, with no scroll traps.

## 5. Accordions

| Where | Mode | Default |
|---|---|---|
| Homepage FAQ | Several open at once | All closed; +/− indicator |
| Why Mini Minds "What guides every bundle" | One open at a time | First open |
| Product FAQ | 2 static + 4 accordions | Closed |
| Mobile footer | Several open at once | All closed |
| Bundle contents (mobile) | Accordion | Per design |
| Policy "On this page" (mobile) | Collapsible | Closed |

All use `aria-expanded`, animated height, and 44 px headers.

## 6. Product page

- **Gallery:** see [02 §4.3](02_COMPONENT_AND_SECTION_SPEC.md#43-product-page-all-read-productmetafieldscustomstage--customtoys).
  - Keyboard: arrows and thumbnail buttons.
  - Mobile: swipe with a 40 px threshold.
- **Plan selector:**
  - Radio cards with the badge inside the option.
  - Selecting a plan updates the price, the plan panel, the Add label and the quantity line.
  - Changing plan cancels any "✓ Added" flash (EDGE-007).
  - Plan and quantity are reflected in the URL via `replaceState`.
- **Quantity:**
  - 1–9, with plan-specific aria labels.
  - Quantity lines:
    - Prepay: "{q} prepaid plan(s) · {3q} bundles · £X today".
    - Subscribe: "{q} bundle(s) every 2 months · …". **The wording depends on SE9.**
- **Add to Basket:**
  - Stays on the page. The button shows "✓ Added to Basket" for 1.6 s.
  - An inline "Added to your basket · {title} · {plan} · {price}" box appears with "Go to Basket →".
  - **No drawer.**
- **Buy Now:** goes straight to checkout with the selected plan and quantity (S13).
- **"How does the subscription work?"** toggle.
- **Gift:** the checkbox reveals the fields, with a live 0/200 counter.
- **Sticky mobile bar:**
  - Shows "Choose how to buy" until the options have been scrolled into view, then the Add action.
  - The added confirmation expands inside the bar.
  - The quantity row shows for subscribe, prepay or quantity > 1. **DES-09:** add a quantity control for one-time purchases.
- **Recap band (desktop):** mirrors the selection. "Change purchase option" (DES-08).

## 7. Basket

- **Line "Change":**
  - Expands an inline radio group of eligible plans, each with price and consequence line; "Done" collapses it.
  - Switching to a plan that already has a line combines the two, with a notice.
- **Quantity:** "−" is disabled at 1 and "+" at 9, shown dimmed.
- **Remove:** removes the line immediately.
- **Summary:** sticky from 96 px from the top on desktop. Mobile has a sticky bottom bar (76 px) with total and Checkout, which releases above the footer and hides when the basket is empty.
- **Notices:** "We couldn't update your basket… Try again" on API failure. There's also a notice for a saved line whose plan is no longer eligible.

## 8. Finder states

The full specification is in [02 §4.4](02_COMPONENT_AND_SECTION_SPEC.md#44-find-my-bundle-sectionsmm-finderliquid).

- **Input:**
  - Three numeric boxes with auto-advance.
  - Errors appear on **blur of the whole group** or on submit, never while typing.
  - Focus moves to the first invalid field.
- **Messages:**

  | Condition | Message |
  |---|---|
  | Empty | "Enter your baby's date of birth to see their bundle." (the design's "or due date" is removed) |
  | Incomplete | "Add the day, month and full year." |
  | Impossible | "That date doesn't exist — please check the day and month." |
  | Future | "That date looks to be in the future." |

- **Consent:** ticking consent without an email blocks submission with a live error.
- **Screens:**
  - Result.
  - Timing choice: only when the stage was bumped.
  - Out-of-range: 12+ months. Its email field must be genuinely optional (DES-19).
- Screen changes use history without DOB in the URL.
- **Motion:** result image and mascot animations respect reduced motion.

## 9. Quiz states (RevenueHunt)

- **Screens:** intro → Q1–Q5 → email step → result.
- **Progress:** "Question N of 5", progress bar with the percentage hidden, `aria-live` announcement.
- **Answers:** tiles behave as toggles with a check mark. "Continue" with nothing selected shows "Choose an answer to continue."
- **Resume:** progress (not the email) survives a reload for the session.
- **Email step:**
  - Email.
  - Optional MM/YYYY. Validation: both fields or neither; month 01–12; not in the future.
  - Optional unticked consent.
  - "Skip for now", which skips validation.
- **Result:** "← Change my answers" goes back to Q5; "Start again".
- **Build:** achievable only within RevenueHunt's capabilities. Any gap is an R item.

## 10. Help interactions

- **Hub:**
  - An empty search submit shows an inline message and stays on the page.
  - Tiles open the filtered category view with breadcrumb; the browser Back button works (history).
- **Article:**
  - "Was this helpful?" Yes / No. **No** reveals the Contact route. Where the vote is recorded is C-03.
  - Related articles; "Still need help?" band.
- **Rich text:** renders emphasis correctly. The design's literal `<strong>` text is a prototype defect.

## 11. Forms (global)

- Validate on blur, not while typing.
- On submit, move focus to the first invalid field.
- Errors in `#B4545C` with `role="alert"` and `aria-invalid`.
- **Contact:** a 2000-character message with a counter from 1500; a 900 ms sending state; a success summary.
- **Email format error:** "That email address doesn't look right. Use the format name@example.com."

## 12. Consent interactions

- **Banner:** Reject and Accept at equal weight; Manage opens the panel.
- **Panel:** a modal with a focus trap and Escape; toggles use `role="switch"`.
- **Footer:** "Cookie settings" opens the panel on every page and never navigates.
- **Before consent:** nothing non-essential loads.
- **Other tabs:** stay in step when consent changes.
- Platform choice: C-01.

## 13. Motion and reduced motion

| Element | Normal | Reduced motion |
|---|---|---|
| Hero word rotator | Every 2.7 s, with a pause button | Static |
| `mm-reveal` | Rise + stagger | Content shown, no animation |
| Mascot spark | Plays once at ≥55 % visible; replays on hover | Resting pose |
| Collection mascot wave | Wave | Static |
| Card hovers | 2–12 px lift | No transform |
| Mobile drawer | 280 ms slide / 200 ms fade | Instant |

Nothing flashes more than three times a second.

## 14. Keyboard, focus and targets

- Visible focus on every interactive element: ring `0 0 0 4px rgba(161,131,255,.35)`. The prototype's accessibility helper also uses a 3 px `#2E2A6B` outline. Both meet the target, so pick one theme token (DES-20).
- Modals (drawer, lightbox, consent panel, review lightbox): background made inert, focus trapped, Escape closes, focus returns to the opener.
- Radio groups (plan selector, pricing toggle): arrow keys and roving tabindex.
- **Minimum touch target 44 × 44 px** everywhere: pause button, dots, steppers, accordion headers, drawer and search buttons, over-range links.
- Underline links inside paragraphs.
- A button nested in a link is a single tab stop.
- Accent buttons use a contrast-safe variant: the prototype forces low-contrast accents to `#7862D0` with white text.
