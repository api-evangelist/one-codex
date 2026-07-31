---
name: Run a versioned job/workflow on a sample and collect analysis output
description: >-
  Discover an available One Codex job (workflow), run it against a sample, then fetch
  the resulting analysis output files.
api: openapi/one-codex-openapi-original.json
operations:
  - get_jobs_instances
  - get_jobs_self
  - post_jobs_run
  - get_analyses_self
  - get_analyses_file_details
  - get_analyses_results
---

# Run a job on a sample

Base URL: `https://app.onecodex.com/api/v1/`. Auth: `X-API-Key: $ONE_CODEX_API_KEY`.

## Steps

1. **List jobs.** Call `get_jobs_instances` to enumerate available versioned jobs
   (workflows); inspect one with `get_jobs_self`. Every job is strictly versioned so a
   run is reproducible.
2. **Run the job.** Call `post_jobs_run` with the target sample id (and any job
   parameters) to launch an analysis.
3. **Track the analysis.** Poll `get_analyses_self` on the returned analysis id until
   its status is complete.
4. **Inspect outputs.** Call `get_analyses_file_details` to list produced output
   files, then `get_analyses_results` to read the structured result payload.

## Rules

- Keep under 10 requests/second; retry `429` with exponential backoff.
- A run can be cancelled (`post_analyses_cancel`) or re-run (`post_analyses_rerun`).
- Result-bearing analysis subtypes (classifications, panels, alignments, MLSTs,
  functional profiles, reports, controls) each have their own `..._results` endpoint.
