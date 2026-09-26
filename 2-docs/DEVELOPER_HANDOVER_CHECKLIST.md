# Developer handover checklist

**Owner action** marks steps only the Mini Minds Co owner can do.

## 1. Access the developer needs

| Access | Level | Who grants | Status |
|---|---|---|---|
| GitHub repository `mini-minds-shopify` (private) | Write (not admin) | **Owner action:** invite the developer's GitHub username (Settings → Collaborators) | ☐ |
| Notion "Shopify Build Backlog" | Edit on tickets | **Owner action:** share the workspace or page | ☐ |
| Shopify store | Collaborator account (themes, products, metaobjects, apps as scoped) | **Owner action:** Settings → Users → Collaborators, using the developer's collaborator request code | ☐ |
| Development theme | A duplicate of the live theme, **never the published theme** | Developer creates it after access; the owner confirms | ☐ |
| Claude Design "Mini Minds Co Site Wireframe" | View | **Owner action:** share the project | ☐ |
| Seal Subscriptions | Staff or sandbox access | **Owner action** | ☐ |
| Klaviyo | Scoped user (forms, lists, properties) | **Owner action** | ☐ |
| Judge.me | Staff access | **Owner action** | ☐ |
| RevenueHunt | Collaborator | **Owner action** | ☐ |
| Consent platform / GA4 / Meta | When selected (C-01, C-04) | **Owner action** | ☐ later |

## 2. Read first

1. Root `README.md`.
2. `2-docs/HANDOFF_CONTEXT.md`.
3. The assigned Notion ticket.

Then consult only what the ticket references: the `2-docs/0x_*.md` docs, the v2.6 workbook, Claude Design pages, and `4-assets/ASSET_MANIFEST.md`.

## 3. Before coding (developer confirms)

- ☐ Can clone the repo, create a branch and push.
- ☐ Has a Shopify development theme (not published).
- ☐ Has access to the apps the ticket needs.
- ☐ Can update the Notion ticket status.
- ☐ Can open Claude Design.
- ☐ Understands the ticket's acceptance criteria.
- ☐ Any technical spike the ticket depends on (e.g. Seal SE1–SE10, Shopify S1–S15 in `07`) is resolved, or the ticket is itself the spike.
- ☐ Will not publish a theme, edit live theme code, or commit credentials.

## 4. Owner prerequisites still open

- ☐ **Notion full backlog completion** (Step 6, being prepared in Claude Cowork).
- ☐ GitHub branch protection on `main`, if it wasn't configured automatically (see the Step 7 report).
- ☐ The invitations and access in §1.
- ☐ Platform, legal and content items in `2-docs/07_IMPLEMENTATION_OPEN_ITEMS.md`. They don't block starting, but they block the specific tickets that depend on them.
