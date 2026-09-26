# 06 Asset implementation map

- **Asset source:** the Claude Design project `assets/` folder. IDs and crops come from `Image Asset Handoff.dc.html`. Toy file names come from v2.6 sheet 04.
- **Masters only:** Shopify generates the resized versions. Assets are **not** copied into `4-assets/` at this stage.

**Destination key:**

| Destination | Meaning |
|---|---|
| **Product media** | Uploaded to the product |
| **Files → metaobject** | Shopify Files, referenced by a metaobject `file_reference` field |
| **Files → section setting** | Shopify Files, chosen in an `image_picker` setting |
| **Theme asset** | `assets/` in the theme, used by code |
| **Native** | Shopify or Eurus built-in; don't upload |

---

## 1. Product and data-driven assets

| Asset (design file) | ID | Size / ratio / fit | Destination | Used by | Status |
|---|---|---|---|---|---|
| `b1…b6-bundle-master.jpg` | IMG-001…006 | 2048², 1:1, contain | **Product media** (featured image) | Product gallery, bundle cards (Home, Shop, About), search, cart, checkout, account, finder and quiz results, prev/next | Supplied |
| 43 toy shots `b{n}-{toy}.jpg` (see v2.6 sheet 04 "Image file") | IMG-1xx | 2048², 1:1, contain | **Files → `mm_toy.image`** | `mm-bundle-contents`, product gallery | Supplied. **B2 Tummy Time Mirror alt text pending (VAL-04)**; other 42 alts in v2.6 |
| `b1…b6-parent-guide.jpg` | IMG-204 | 2048², 1:1, contain | **Files → `mm_stage.guide_cover_image`** | Product gallery "Guide" thumbnail, Guide card | Supplied; alts pending (VAL-05) |
| `b1…b6-stage-lifestyle.jpg` | IMG-3xx | 2000×1500, 4:3, cover; 460 px desktop / 210 px mobile | **Files → `mm_stage.lifestyle_image`** | `mm-stage-education`, gallery "In use" | Supplied; alts pending (VAL-05) |
| Per-stage Guide spread | — | 2048×1102 | **Files → `mm_stage.guide_spread_image`** (optional) | Product-page Parent Guide | **Not supplied.** Falls back to IMG-201 |
| `pathway-{mind,talking,moving,hands,friendliness}.png` | — | Icon | **Files → `mm_pathway.icon_asset`** | Pathways sections, toy tags, rotator, quiz | Supplied |
| Help category icons | — | Icon-font glyph | **Theme asset** (Icons South St font), selected by `mm_help_category.icon_name` | Help hub tiles | Supplied (font) |

## 2. Section-setting imagery by template

| Template / section | Asset (design file) | ID | Size / ratio | Destination | Status |
|---|---|---|---|---|---|
| Home hero | `hp-hero.jpg` | IMG-300 | 2400×1800, 4:3, cover; subject centre-left | Files → section setting (desktop + mobile settings) | Supplied. The design notes ask for a dedicated 1:1 mobile crop (AST-06) |
| Home challenge | `challenge-milestones.png`, `challenge-toys.png`, `challenge-next-stage.png` | — | Illustration | Files → blocks | Supplied (decorative, empty alt) |
| Home Way cards | `img-101-stage-toy-flatlay.jpg`, `img-201-parent-guide-spread.jpg`, `img-301-everyday-play-at-home.jpg` | IMG-101 / 201 / 301 | 2000×1500 / 2048×1102 / 2000×1500 | Files → blocks | Supplied |
| Home three steps | `img-501-dob-step.jpg`, `b3-bundle-master.jpg`, `img-302-parent-reading-guide.jpg` | IMG-501 / 003 / 302 | 1200×900 / 2048² / 2000×1500 | Files → blocks | Supplied. IMG-501 is registered as SVG but supplied as JPG (AST-05) |
| Home founder story | `founder-story.jpg` (still-life stand-in) | IMG-401 | 1600×2000, 4:5; mobile IMG-401M 1400² | Files → section setting | **Portrait pending (AST-02)** |
| Home final CTA | `img-601-final-cta.jpg` | IMG-310 | 1600², 1:1 | Files → section setting | Supplied. File carries a retired ID; rename (AST-04) |
| Home quiz banner, 404, empty basket | `mascot-waving.png` | IMG-801 (404) | PNG | Files → section setting | Supplied |
| Home Way cards animation | `mascot-base.png`, `mascot-pupil-right.png` | — | PNG layers | **Theme asset** (used by `mm-mascot-spark` JS) | Supplied |
| Shop hero | `shop-hero-bundle-range.png` | IMG-303 | 2048², 1:1, cover; 204 px tall on mobile | Files → section setting | Supplied as a **7.2 MB PNG**; optimise or convert (AST-03) |
| Shop hero mascot, finder form, quiz intro | `mascot-waving.png`, `mascot-puzzle.png` | — | PNG | Files → section setting | Supplied |
| Shop When / What / How | `why-when-baby-calendar.png`, `why-what-toys.png`, `why-how-guide-book.png` | — | Illustration | Files → blocks | Supplied (the "why-" prefix is historical) |
| PPP icons (product page, PPP cards) | `ppp-play.png`, `ppp-pause.png`, `ppp-progress.png` | — | Icon | Files → section blocks | Supplied |
| How It Works hero | `hiw-hero.jpg` + `mascot-presenting.png` | IMG-304 | 2400×1800, 4:3 | Files → section setting | Supplied |
| How It Works toy selection | `hiw-selection.jpg` | IMG-102 | 1600×1200, 4:3 | Files → section setting | Supplied. The register's status text contradicts itself (AST-07) |
| PPP photo panels | `hiw-play.jpg`, `hiw-pause.jpg`, `hiw-progress.jpg` | IMG-701–703 | 1600×1200, 4:3; 128 px desktop / 146 px mobile | Files → `mm-play-pause-progress` blocks | Supplied |
| Parent Guide lightbox | `parent-guide-spread-v2.png` (lightbox) and `img-201-…jpg` (inline) | IMG-201 | 2048×1102 (2.9 MB PNG) | Files → `mm-parent-guide` | Supplied; keep the two in step (AST-03 size) |
| Why first year | `why-first-year-matters.jpg` | IMG-305 | 2400×1792; 340 px desktop / 224 px mobile | Files → section setting | Supplied |
| Why CTA band, About closing CTA | `mascot-reading.png` | — | PNG | Files → section setting | Supplied |
| About hero | — | **IMG-306** / **306M** | 2400×1200, 2:1 / 1200×1500, 4:5 | Files → section setting | **Pending shoot + alt text (AST-01)** |
| About callout | `mascot-pointing-up.png` | — | PNG | Files → section setting | Supplied |
| About / How It Works pillars | IMG-101 / 201 / 301 (reused) | — | — | Files → blocks | Supplied |
| Finder result, contact success | `mascot-celebrate.png` | — | PNG | Files → section setting | Supplied |
| Track fallback | `mascot-reading.png` | — | PNG | Files → section setting | Supplied |

## 3. Global and brand assets

| Asset | Destination | Notes |
|---|---|---|
| `mini-minds-logo.png` | Native (theme logo setting) | The footer shows it on a white pill |
| `icon-search.svg`, `icon-account.svg`, `icon-bag.svg` | Theme asset (or Eurus icon set if equivalent) | Header icons |
| `social-instagram.svg`, `social-facebook.svg`, `social-tiktok.svg` | Native Eurus social icons if equivalent; otherwise theme asset | Remove the share-tracking parameter from the Instagram URL (CNT-10) |
| `payment-*.svg` | **Native Shopify payment icons** | Replaces the design's SVG and text-chip mix |
| Quicksand, Nunito | Native theme fonts if available in the Shopify font library; otherwise theme asset | Check at build |
| Icons South St icon font | Theme asset | Used by UI and Help `icon_name` |

## 4. Missing, pending or problem assets

| ID | Item | Blocks |
|---|---|---|
| AST-01 | About hero IMG-306 + mobile IMG-306M + alt text | About hero (placeholder until supplied) |
| AST-02 | Founder portrait IMG-401 + IMG-401M. The register gives two different placements: homepage only, or homepage and About | Home founder section (stand-in available) |
| AST-03 | Oversized PNGs: `shop-hero-bundle-range.png` 7.2 MB, `parent-guide-spread-v2.png` 2.9 MB. No file-size limit is defined | Performance QA |
| AST-04 | `img-601-final-cta.jpg` uses a retired ID (it is IMG-310) | Naming only |
| AST-05 | IMG-501 registered as SVG, supplied as JPG | None (JPG works) |
| AST-06 | Homepage hero dedicated mobile crop; IMG-302 "separate 4:5" mobile crop is listed but not registered | Mobile art direction |
| AST-07 | IMG-102 status contradiction in the register | None (file supplied) |
| AST-08 | Orphaned files not used by the approved designs: `img-502-stage-result.jpg`, `mm-packaging-box.png`, `problem-*.png` (two are identical) | Don't upload |
| AST-09 | No asset follows the `MMC_[ID]_…` naming convention | Decide whether to rename at upload |
| VAL-04 / 05 | Alt text: B2 toy, 6 lifestyle images, 6 Guide covers, IMG-306 | Accessibility QA |
