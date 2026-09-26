# Mini Minds Co Shopify website

Private implementation repository for the Mini Minds Co store: six stage-matched developmental toy bundles covering 0–12 months.

## Build approach

- Shopify Online Store 2.0.
- **Eurus** theme, **Swirl** preset.
- Bespoke Mini Minds sections and snippets (`mm-*`) only where the specification says so.

## Where things live

| Source | Location |
|---|---|
| Execution tasks | **Notion Shopify Build Backlog** (the main execution layer) |
| Technical documentation (the implementation contract) | [`2-docs/`](2-docs/) |
| Canonical Shopify data model | [`3-data-model/Mini_Minds_Shopify_Data_Model_v2.6.xlsx`](3-data-model/) + `CHANGELOG_v2.6.md` |
| Production assets | [`4-assets/`](4-assets/) (see `ASSET_MANIFEST.md`) |
| Shopify theme source | [`5-theme/`](5-theme/) (empty until Step 8) |
| Visual reference | Claude Design project "Mini Minds Co Site Wireframe" (root pages only; `archive/` is historical) |

## Start here

1. Read [`2-docs/HANDOFF_CONTEXT.md`](2-docs/HANDOFF_CONTEXT.md).
2. Work from your assigned Notion ticket.
3. Consult the technical doc, data model or design that the ticket references.

## Source hierarchy

| Source | Role |
|---|---|
| **Notion ticket** | What to do now (execution task and acceptance criteria) |
| **`2-docs/`** | How it must work (implementation contract) |
| **v2.6 workbook** | Authoritative structured-data schema |
| **Claude Design** | Visual and UX source |
| **`4-assets/ASSET_MANIFEST.md`** | Production asset reference |

Unresolved questions live in [`2-docs/07_IMPLEMENTATION_OPEN_ITEMS.md`](2-docs/07_IMPLEMENTATION_OPEN_ITEMS.md).

## Important rules

- **Don't re-decide confirmed architecture.** See `HANDOFF_CONTEXT.md` §6.
- **Don't guess unresolved platform behaviour** (Seal, Shopify, RevenueHunt, Klaviyo, Judge.me). Use a technical spike ticket and record the answer.
- **Six bundles use one reusable Product template.**
- **The Product relationships are `custom.stage` and `custom.toys` only.** Next bundle, age label, pathways and toy count are derived.
- **The exact date of birth must never be persisted or transmitted.** Not in URLs, Shopify, Klaviyo, analytics or order data. It stays in the Finder's browser session only.

## Development workflow

Notion ticket status: **Ready for Development → In Progress → Ready for Testing → Testing → Complete**

### Branches and commits

- `main` is the protected integration branch. Never commit to it directly.
- Branch per ticket:
  - `feature/MMC-023-product-gallery`
  - `spike/MMC-0XX-seal-plan-behaviour`
  - `fix/MMC-0XX-short-description`
- Commit messages: `MMC-023: implement product gallery`

### Pull requests

1. Move the Notion ticket to **In Progress**.
2. Create a branch containing the ticket ID.
3. Implement and self-test (desktop, mobile 375/390/430, keyboard, reduced motion).
4. Push the branch and open a PR titled with the ticket ID, linking the Notion ticket.
5. In the PR, confirm the acceptance criteria and QA tests.
6. Move the ticket to **Ready for Testing**.
7. The owner/tester reviews against the Notion acceptance criteria, Claude Design and the responsive requirements.
8. Fix defects on the same branch and PR.
9. Merge after approval. The ticket becomes **Complete**.

## Shopify development safety

- Develop against a **development or duplicate theme**, never the live published theme.
- **Never publish a theme without explicit owner approval.**
- Don't edit theme code in the Shopify admin code editor once source-controlled development exists. `5-theme/` is the source of truth.
- Never commit Shopify, Seal, Klaviyo or other credentials. `.env*` is ignored.
- Theme implementation begins in **Step 8**. Don't initialise or pull a theme before then unless a ticket says so.
