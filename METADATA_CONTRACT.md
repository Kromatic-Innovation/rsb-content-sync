# Real Startup Book — Content Metadata Contract

> Defines the YAML frontmatter schema for all content files in this repo.
> Consumers: `realstartupbook-online` (Astro website), Orca knowledge ingestion.

## Purpose

Every content file should carry structured metadata in YAML frontmatter so that
downstream consumers (the website, search engines, social platforms, Orca's RAG
index) can use it without parsing markdown body content.

## Schema

### Required

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `title` | string | Page title. Used in `<title>`, JSON-LD, OG. | `Contextual Inquiry` |
| `description` | string | 1-2 sentence summary. Used in `<meta description>`, OG, Orca. | `A combination of semi-structured interviews and observations done in the location where the problem occurs.` |

### Recommended

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `tags` | string[] | Topic tags for filtering and Orca indexing. | `[B2C, B2B, Qualitative]` |
| `category` | string | Parent chapter slug. Matches directory name. | `generative-market-research` |
| `helps_answer` | string[] | Business/product questions this method helps answer. **Not FAQs** — these are the research or experiment questions the method addresses. | `[What are the customer's pain points?, Are there makeshift solutions?]` |
| `schema_type` | `HowTo` \| `WebPage` | JSON-LD type for structured data. Method pages with a "How to" section should use `HowTo`. | `HowTo` |
| `updated` | date (YYYY-MM-DD) | Last meaningful content edit. Drives sitemap `<lastmod>`. | `2026-03-30` |

### Optional

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `slug` | string | Canonical URL slug override. Defaults to filename without extension. | `contextual-inquiry` |
| `time_required` | string | How long the method takes. Feeds HowTo schema `totalTime`. | `1-2 hours per session` |
| `difficulty` | `beginner` \| `intermediate` \| `advanced` | Skill level for filtering and recommendations. | `intermediate` |
| `og_image` | string | Social share image path (relative to repo root). | `.gitbook/assets/illustration - contextual inquiry.png` |
| `og_title` | string | Override title for social cards. Falls back to `title`. | `Contextual Inquiry: A Complete Guide` |
| `og_description` | string | Override description for social cards. Falls back to `description`. | |
| `indexable` | boolean | Set `false` to add `noindex`. Default: `true`. | `false` |
| `aliases` | string[] | Alternative names for cross-referencing and search. | `[Field study, Site visit]` |
| `related` | string[] | Slugs of related methods. | `[customer-discovery-interviews, experience-sampling]` |
| `faq` | array of `{q, a}` | FAQ pairs for JSON-LD FAQPage schema and on-page FAQ section. These are questions *about the page content itself*, distinct from `helps_answer`. | See example below. |

### FAQ example

```yaml
faq:
  - q: What is contextual inquiry?
    a: A UX research method combining interviews and observation in the user's natural environment.
  - q: When should I use contextual inquiry vs. interviews?
    a: Use contextual inquiry when you need to observe actual behavior, not just reported behavior.
```

## Notes

- `helps_answer` vs `faq`: `helps_answer` lists the business/product questions the method helps a practitioner answer (e.g., "What are the customer's pain points?"). `faq` lists questions a reader might ask about the method itself (e.g., "What is contextual inquiry?"). They serve different purposes and should not be conflated.
- Navigation order is defined by `SUMMARY.md`, not frontmatter. No `weight` or `sort_order` field is needed.
- `author` and `locale` are omitted — set at site level (Tristan Kromer, English).
- `og_title` and `og_description` fall back to `title` and `description` if not set.
- `indexable` defaults to `true` if omitted. Only set explicitly to exclude a page.
- `faq` entries are optional and can be added incrementally. The website should generate JSON-LD FAQPage schema when present.
