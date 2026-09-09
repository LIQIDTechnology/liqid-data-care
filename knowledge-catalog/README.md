# Knowledge Catalog data input

YAML source of truth for [Google Knowledge Catalog](https://cloud.google.com/dataplex/docs/metadata-overview) (formerly Dataplex Universal Catalog) objects.

This repo is **data input only**. A later Terraform module will walk these files and create the matching GCP resources. Do not put project numbers or full resource names here.

## How to add a glossary term

1. Create `glossaries/<glossary-id>/terms/<term-id>.yaml`.
2. Set `kind: glossary-term`, a unique `id`, and `parent` to the glossary id or a category id.
3. Add `synonyms` or `related_terms` on the term if that is easier while drafting. Also add the same relationship under `links/` — that folder is the canonical source a generator should flatten into `google_dataplex_entry_link`.
4. IDs must be lowercase, use hyphens, and start with a letter.

## Folder to object to Terraform

| Folder | `kind` | Knowledge Catalog object | Future Terraform resource |
| --- | --- | --- | --- |
| `glossaries/*/glossary.yaml` | `glossary` | Business glossary | `google_dataplex_glossary` |
| `glossaries/*/categories/` | `glossary-category` | Category (max 3 levels) | `google_dataplex_glossary_category` |
| `glossaries/*/terms/` | `glossary-term` | Term (+ overview/contacts aspects) | `google_dataplex_glossary_term` |
| `aspect-types/` | `aspect-type` | Custom aspect schema | `google_dataplex_aspect_type` |
| `aspects/` | `entry-aspect` | Aspect values on an existing entry | applied via `entries.update-aspects` |
| `entry-types/` | `entry-type` | Template for custom entries | `google_dataplex_entry_type` |
| `entry-groups/` | `entry-group` | Container for custom entries | `google_dataplex_entry_group` |
| `entries/` | `entry` | Custom entry (GCP sources are auto-ingested) | `google_dataplex_entry` |
| `links/` | `entry-link` | synonym / related / definition | `google_dataplex_entry_link` |

`catalog.yaml` holds repo defaults (`location`, `project` placeholder). Terraform injects the real project later.

## Conventions

- Use **local IDs** everywhere (`parent: structure`, not `projects/.../glossaries/...`).
- Shared keys: `kind`, `id`, `display_name`, `description`, `labels`.
- Term/category extras (`overview`, `contacts`) become Knowledge Catalog aspects at apply time.
- Definition links (term → BigQuery table or column) live in `links/definitions.yaml`.
- Source-system (and other) aspect values on auto-ingested BigQuery entries live in `aspects/`. Do not recreate those tables under `entries/`.
- Data-quality scans are a separate Dataplex concern; they do not belong in this repo.
