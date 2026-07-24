# Tuva Health (tuva-health)

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
