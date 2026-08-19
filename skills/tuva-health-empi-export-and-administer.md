---
name: Export Tuva EMPI persons and administer users
description: Read the resolved person index out of Tuva EMPI to S3 for downstream warehouse use, and manage the reviewer roles that control who can adjudicate matches.
api: openapi/tuva-health-empi-openapi.yml
operations: [persons_retrieve, persons_retrieve_2, person_records_export_create, users_retrieve, users_create]
generated: '2026-08-15'
method: generated
source: openapi/tuva-health-empi-openapi.yml + https://tuva-health.github.io/tuva_empi/docs/configuration
---

# Export persons and administer users

Two administrative flows against the same deployment: getting resolved identities back
out into the warehouse, and controlling who is allowed to change them.

## Export the resolved index

1. **List unified persons** — `persons_retrieve`
   `GET /api/v1/persons` returns `GetPersonsResponse.persons[]` of `PersonSummary`
   (`id`, `first_name`, `last_name`, `data_sources[]`). No query parameters exist, so this
   is the full collection — do not expect a cursor or a page size.

2. **Inspect one** — `persons_retrieve_2`
   `GET /api/v1/persons/{id}` returns `GetPersonResponse` / `PersonDetail` with the
   underlying `PersonRecord`s.

3. **Bulk export** — `person_records_export_create`
   `POST /api/v1/person-records/export` with `{s3_uri}` (required). Like import, bulk
   movement is staged through object storage rather than the response body. Point it at a
   bucket the EMPI's execution role can write to.

4. **Land it downstream.** The exported person index is what carries the unified identity
   into the warehouse, where `person_id` is the hub key of the Tuva Core Data Model
   (see `data-model/tuva-health-data-model.yml`).

## Administer users

5. **List users** — `users_retrieve`
   `GET /api/v1/users` returns `GetUsersResponse.users[]` of `UserSummary` with their
   current role.

6. **Change a role** — `users_create`
   `POST /api/v1/users/{id}` with an `UpdateUserRoleRequest` `{user_id, role}` where
   `role` is `admin`, `member`, or `null` to revoke. The response is
   `UpdateUserRoleResponse` / `UpdatedUser`.

   Note the shape mismatch: the operationId says *create* and the method is POST, but this
   is an **update to an existing user's role**. Users originate in the customer's identity
   provider (Keycloak or AWS Cognito); this endpoint does not create accounts. The very
   first admin is granted out-of-band at bootstrap from `initial_setup.admin_email`.

## Rules an agent must follow

- **Role changes are access control.** Granting `admin` grants the ability to adjudicate
  identity. Require explicit human authorization; never grant a role to unblock your own
  task.
- **Export destinations are exfiltration paths.** `s3_uri` sends PHI somewhere. Only use a
  bucket a human has named for this purpose in the request.
- **No idempotency, no documented errors.** See `conventions/` and `errors/` — treat any
  non-2xx as opaque and stop rather than retrying a write.
