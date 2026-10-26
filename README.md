# Tuva Health (tuva-health)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Tuva Health, Inc. is a United States healthcare data company behind The Tuva Project, an open-source, warehouse-native data-transformation platform that harmonizes, validates, normalizes, and enriches raw healthcare data - medical and pharmacy claims, EHR clinical data, ADT feeds, and HL7 FHIR sources - into an analytics-ready Core Data Model and a library of Data Marts. It runs as a dbt package plus Python utilities and source connectors inside the customer's own cloud data warehouse (Snowflake, Databricks, Google BigQuery, Amazon Redshift, Microsoft Fabric).

Tuva is not an HTTP or FHIR API vendor. FHIR R4 is a supported input format that Tuva flattens and maps into its Input Layer, not a live FHIR server it operates. There is no public REST API, no FHIR CapabilityStatement, no SMART-on-FHIR configuration, and no OpenAPI to harvest. The developer surface is documentation plus open-source code.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/tuva-health/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/tuva-health/refs/heads/main/apis.yml)

## Tags

- Healthcare
- United States
- Health Data
- FHIR
- Interoperability
- Data Analytics
- Data Transformation
- Claims
- Open Source
- dbt

## Timestamps

- **Created:** 2026-07-24
- **Modified:** 2026-07-24

## Products

### The Tuva Project (dbt package)

The core open-source Tuva dbt package that transforms healthcare data from the Tuva Input Layer into the Tuva Core Data Model and Data Marts, with data-quality tests, normalization, claims preprocessing, and bundled terminology sets.

- **Docs:** [https://thetuvaproject.com/getting-started](https://thetuvaproject.com/getting-started)
- **dbt Package:** [https://hub.getdbt.com/tuva-health/the_tuva_project/latest/](https://hub.getdbt.com/tuva-health/the_tuva_project/latest/)
- **Source:** [https://github.com/tuva-health/tuva](https://github.com/tuva-health/tuva)

### FHIR Inferno

An open-source Python utility that flattens nested HL7 FHIR JSON into tabular CSV so FHIR-sourced data can be mapped into the Tuva Input Layer. A command-line tool that consumes FHIR - not a FHIR server.

- **Docs:** [https://thetuvaproject.com/guides/mapping/fhir](https://thetuvaproject.com/guides/mapping/fhir)
- **Source:** [https://github.com/tuva-health/fhir_inferno](https://github.com/tuva-health/fhir_inferno)

### Tuva Connectors

A family of open-source dbt connector projects (Medicare CCLF, BCDA, CMS synthetic, Aetna, BCBS, FHIR preprocessing, and a connector template) that map raw claims, EHR, ADT, and FHIR sources into the standardized Tuva Input Layer.

- **Docs:** [https://thetuvaproject.com/connectors/overview](https://thetuvaproject.com/connectors/overview)
- **Org:** [https://github.com/tuva-health](https://github.com/tuva-health)

## Links

- **Website:** [https://www.tuvahealth.com](https://www.tuvahealth.com)
- **Developer Portal / Docs:** [https://thetuvaproject.com](https://thetuvaproject.com)
- **GitHub Organization:** [https://github.com/tuva-health](https://github.com/tuva-health)
- **LinkedIn:** [https://www.linkedin.com/company/tuva-health](https://www.linkedin.com/company/tuva-health)
- **Pricing:** [https://www.tuvahealth.com/pricing](https://www.tuvahealth.com/pricing)
- **Contact / Support:** [https://www.tuvahealth.com/contact](https://www.tuvahealth.com/contact)

## Home Market

United States
