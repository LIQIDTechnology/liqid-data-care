# Dataplex data scans

YAML source of truth for [Dataplex data scans](https://cloud.google.com/dataplex/docs/data-scans). One spec file per target.

This folder is **data input only**. A later Terraform module will walk these files and create the matching GCP resources. Do not put project numbers or full resource names here.

## How to add a scan

1. Create a spec file in the folder for that scan type (see the table below). Dataset, table, or bucket id must match the GCP resource.
2. Fill the matching Dataplex spec. Copy the `_example` template in that folder if you want a starting spec.
3. Folders that start with `_` are templates only and are not applied.

Rule names and ids: lowercase, hyphens, start with a letter.

## Folder to object to Terraform

| Folder | `kind` | Dataplex object | Future Terraform resource |
| --- | --- | --- | --- |
| `rules/<dataset>/<table>.yaml` | `data-quality-scan` | Data quality scan | `google_dataplex_datascan` (`data_quality_spec`) |
| `profile/<dataset>/<table>.yaml` | `data-profile-scan` | Data profile scan | `google_dataplex_datascan` (`data_profile_spec`) |
| `discovery/<bucket-id>.yaml` | `data-discovery-scan` | Data discovery scan | `google_dataplex_datascan` (`data_discovery_spec`) |

`../knowledge-catalog/catalog.yaml` holds repo defaults (`location`, `project` placeholder). Terraform injects the real project, the `data.resource` path, `data_scan_id`, and `execution_spec` (on-demand vs schedule).

## Folder layout

| Path | Role |
| --- | --- |
| `rules/<dataset>/<table>.yaml` | DataQualitySpec for that BigQuery table |
| `profile/<dataset>/<table>.yaml` | DataProfileSpec for that BigQuery table |
| `discovery/<bucket-id>.yaml` | DataDiscoverySpec for that Cloud Storage bucket |
| `*/_example/` | Templates, not applied |

## Spec

- Quality YAML: [DataQualitySpec](https://cloud.google.com/dataplex/docs/reference/rest/v1/DataQualitySpec)
- Profile YAML: [DataProfileSpec](https://cloud.google.com/dataplex/docs/reference/rest/v1/DataProfileSpec)
- Discovery YAML: [DataDiscoverySpec](https://cloud.google.com/dataplex/docs/reference/rest/v1/DataDiscoverySpec)
- Terraform resource: [`google_dataplex_datascan`](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/dataplex_datascan)

gcloud accepts camelCase. Terraform in this folder accepts camelCase or snake_case.
