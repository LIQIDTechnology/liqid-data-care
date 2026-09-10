# Dataplex data quality

YAML source of truth for [Dataplex data quality scans](https://cloud.google.com/dataplex/docs/use-auto-data-quality). One spec file per table.

This folder is **data input only**. Apply is a later Terraform discussion. Do not put project numbers in the rule files.

## How to add a scan

1. Create `rules/<dataset>/<table>.yaml`. Dataset and table must match the BigQuery table.
2. Fill a [DataQualitySpec](https://cloud.google.com/dataplex/docs/reference/rest/v1/DataQualitySpec): `samplingPercent`, optional `rowFilter`, and `rules`.
3. Copy `rules/_example/example-table.yaml` if you want a starting set of completeness / uniqueness / validity / consistency rules.
4. Folders that start with `_` are templates only and are not applied.

Rule names: lowercase, hyphens, start with a letter.

## Folder layout

| Path | Role |
| --- | --- |
| `rules/<dataset>/<table>.yaml` | DataQualitySpec for that table |
| `rules/_example/` | Templates, not applied |

## Spec

- Rule YAML: [DataQualitySpec](https://cloud.google.com/dataplex/docs/reference/rest/v1/DataQualitySpec)
