---
name: Review and adjudicate Tuva EMPI potential matches
description: Work the Tuva EMPI review queue — list potential matches, open one to read its pairwise probabilities, and adjudicate it into a person-record match with optimistic-concurrency safety.
api: openapi/tuva-health-empi-openapi.yml
operations: [potential_matches_retrieve, potential_matches_retrieve_2, matches_create, persons_retrieve_2]
generated: '2026-08-15'
method: generated
source: openapi/tuva-health-empi-openapi.yml
---

# Review and adjudicate potential matches

Pairs that score above `potential_match_threshold` but below `auto_match_threshold` are
queued for human review. This flow works that queue. It **changes who is considered the
same patient**, so it is the highest-consequence surface in the API.

## Steps

1. **List the queue** — `potential_matches_retrieve`
   `GET /api/v1/potential-matches` returns `GetPotentialMatchesResponse.potential_matches[]`
   of `PotentialMatchSummary`: `id`, `first_name`, `last_name`, `data_sources[]`,
   `max_match_probability`. There are **no pagination or filter parameters** — the whole
   queue comes back. Sort client-side; `max_match_probability` is the useful ranking key.

2. **Open one** — `potential_matches_retrieve_2`
   `GET /api/v1/potential-matches/{id}` returns `GetPotentialMatchResponse` with the
   `PotentialMatchDetail`: the constituent `PersonRecord`s, the `PredictionResult`
   pairwise probabilities, and a `version`.

3. **Check the persons involved** — `persons_retrieve_2`
   `GET /api/v1/persons/{id}` shows a `PersonDetail` — the unified person and the records
   already attached to it. Do this before merging: you are deciding whether two existing
   identities collapse.

4. **Adjudicate** — `matches_create`
   `POST /api/v1/matches` with a `CreateMatchRequest`. Required fields:
   - `potential_match_id` — the id from step 2.
   - `potential_match_version` — the `version` you read in step 2. **This is optimistic
     concurrency.** Send the version you actually saw; if another reviewer has acted since,
     the write should be rejected rather than silently overwriting their decision. Never
     re-read a fresh version purely to force the write through.
   - `person_updates[]` — the `PersonUpdate` set describing how records are assigned to
     persons (merge, split, leave).
   - `comments[]` — optional `PersonRecordComment`s recording the reviewer's reasoning.

## Rules an agent must follow

- **Never auto-adjudicate.** Route step 4 to a human. A false merge combines two
  patients' clinical and claims histories; a false split fragments one patient's record.
  Both are patient-safety events, not data-quality nits.
- **The version field is the only safety net.** There is no idempotency key
  (`conventions/tuva-health-conventions.yml`), so `potential_match_version` is what stops
  a duplicate or stale decision from landing.
- **Errors are undocumented.** The contract declares only `200`
  (`errors/tuva-health-problem-types.yml`). On any non-2xx, stop, re-read the potential
  match, and hand the raw response to a human — do not retry the write.
- **PHI.** Names, data sources and record contents here are protected health information.
