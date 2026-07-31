---
name: Upload a sample and get its taxonomic classification
description: >-
  Upload a FASTA/FASTQ sequencing sample to One Codex, wait for the metagenomic
  classification to finish, and read the taxonomic results.
api: openapi/one-codex-openapi-original.json
operations:
  - post_samples_instances_init_upload
  - post_samples_instances_confirm_upload
  - get_samples_self
  - get_classifications_instances
  - get_classifications_self
  - get_classifications_results
---

# Upload a sample and classify it

Base URL: `https://app.onecodex.com/api/v1/`. Authenticate every request with the
`X-API-Key: $ONE_CODEX_API_KEY` header (HTTPS only).

## Steps

1. **Start the upload.** Call `post_samples_instances_init_upload` to begin a sample
   upload and receive the signed upload target for your FASTA/FASTQ file. Large files
   use the multipart path (`post_samples_instances_init_multipart_upload`).
2. **Confirm the upload.** After transferring the bytes, call
   `post_samples_instances_confirm_upload` to finalize the Sample.
3. **Poll the sample.** Use `get_samples_self` (by sample id) until processing has
   produced an associated primary classification.
4. **Find the classification.** List with `get_classifications_instances` (offset
   pagination via `page`/`per_page`; read `X-Total-Count` and the `Link` header) or
   fetch directly with `get_classifications_self` using the classification id from the
   sample's `primary_classification`.
5. **Read results.** Call `get_classifications_results` for the taxonomic abundance
   table. Analyses are tied to a strictly versioned `job` for reproducibility.

## Rules

- Rate limit is 10 requests/second across all routes; back off exponentially on `429`.
- Errors return `{"message": ..., "status": <http code>}` — check `status`, not just
  the body. `401` = bad/missing key, `403` = insufficient permission, `404` = not found.
- Analyses run asynchronously; do not assume results exist immediately after upload.
