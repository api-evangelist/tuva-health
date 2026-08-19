---
name: Configure Tuva EMPI matching and import person records
description: Create a Splink matching configuration in Tuva EMPI, then stage and import a batch of person records from S3 under that configuration.
api: openapi/tuva-health-empi-openapi.yml
operations: [config_create, data_sources_retrieve, person_records_import_create, health_check_retrieve]
generated: '2026-08-15'
method: generated
source: openapi/tuva-health-empi-openapi.yml + https://tuva-health.github.io/tuva_empi/docs/configuration
---

# Configure matching and import person records

Tuva EMPI resolves person records from many source systems into unified persons using
Splink probabilistic matching. Before any records can be imported, a **config** must
exist: it carries the Splink settings and the two probability thresholds that decide
what is auto-matched and what is queued for human review.

## Before you start

- **Base URL.** Tuva EMPI is customer-deployed. There is no vendor host — the base is
  `https://<your-empi-host>/api/v1` (the local demo stack is `http://localhost:8000/api/v1`,
  fronted on `http://localhost:9000`).
- **Auth.** The API sits behind an OAuth2/OIDC proxy. Present the JWT from the customer's
  identity provider (Keycloak or AWS Cognito) on the header configured by
  `idp.<backend>.jwt_header`, default `X-Forwarded-Access-Token`. The OpenAPI itself
  declares no security scheme — do not conclude the API is open.
- **Sanity check.** `GET /api/v1/health-check` (`health_check_retrieve`) returns `200`
  with an empty object. Use it to confirm reachability and that your token is being
  forwarded before doing anything else.

## Steps

1. **Confirm the deployment is live** — `health_check_retrieve`
   `GET /api/v1/health-check`.

2. **See what is already loaded** — `data_sources_retrieve`
   `GET /api/v1/data-sources` returns `GetDataSourcesResponse` listing the data sources
   present in the EMPI. Use it to decide whether this import adds a new source or extends
   an existing one.

3. **Create the matching configuration** — `config_create`
   `POST /api/v1/config` with a `CreateConfigRequest`. All three fields are required:
   - `splink_settings` — the Splink model (`blocking_rules`, `comparisons` with
     `comparison_levels`).
   - `potential_match_threshold` — a double in `[0, 1]`; pairs at or above this land in
     the review queue.
   - `auto_match_threshold` — a double in `[0, 1]`; pairs at or above this are matched
     without review. Keep it above `potential_match_threshold`.

   The response (`CreateConfigResponse`) returns `config_id`. Hold onto it — the import
   is bound to a config, so a later config change does not retroactively re-match.

4. **Stage the records in S3, then import** — `person_records_import_create`
   `POST /api/v1/person-records/import` with `{config_id, s3_uri}`. Records travel
   through object storage, not through the request body. The response
   (`ImportPersonRecordsResponse`) returns a `job_id`.

5. **Let matching run.** Matching executes as a **separate Kubernetes job** (the
   `matching-service` container), so a `200` on the import means "accepted", not
   "matched". Poll `potential_matches_retrieve` (`GET /api/v1/potential-matches`) until
   results appear rather than expecting them on the import response.

## Rules an agent must follow

- **Do not retry blindly.** The API defines no idempotency key on any write operation
  (see `conventions/tuva-health-conventions.yml`). Re-POSTing an import after a timeout
  can load the same batch twice. Confirm state with `data_sources_retrieve` /
  `potential_matches_retrieve` before retrying.
- **Expect undocumented errors.** Every operation in the contract declares only `200`
  (see `errors/tuva-health-problem-types.yml`). Treat any non-2xx as an opaque failure,
  surface the raw body, and stop — do not infer a retry policy from the spec.
- **This is PHI.** Person records are protected health information. Never echo record
  contents into logs, prompts, or third-party tools.
