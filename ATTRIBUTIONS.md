# Benchmark Data Sources and Attribution Notice

This benchmark includes Oracle-created benchmark prompts, Oracle Analytic View definitions, SQL examples, schemas, transformations, and/or metadata derived from publicly available datasets.

Oracle has modified certain source datasets for benchmark purposes, including schema normalization, Oracle SQL / Analytic View adaptation, prompt creation, benchmark packaging, and, where applicable, masking or obfuscation of direct identifiers.

Unless otherwise noted, Oracle does not claim ownership of the underlying third-party or public government datasets. Third-party and public datasets remain subject to their original source terms.

No source agency, municipality, government entity, or data provider endorses Oracle, this benchmark, Oracle products, any LLM provider, or any analysis, model, benchmark result, publication, or conclusion based on this benchmark.

## Datasets / Sources

### USDA_COUNTY_POPULATION

- **Source:** U.S. Department of Agriculture / U.S. federal public data source.
- **3PLTA:** LD 243505 / BA 390002
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### CALF_EMPLOYMENT_SURVEY

- **Source:** California public data source.
- **3PLTA:** LD 243494 / BA 389991
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### SMALL_AREA_HEALTH_INSURANCE_ESTIMATES

- **Source:** U.S. Census / U.S. federal public data source.
- **3PLTA:** LD 243504 / BA 390001
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### ONTIME_PERFORMANCE

- **Source:** Public transportation / aviation performance data source.
- **3PLTA:** LD 243503 / BA 390000
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### NYS_SPENDING_T

- **Source:** New York State public data source.
- **3PLTA:** LD 243502 / BA 389998
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### NYPD_SHOOTING_INCIDENTS_2024

- **Source:** NYC Open Data / City of New York.
- **3PLTA:** LD 243501 / BA 389997
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.
- **No Endorsement:** The City of New York and its agencies do not endorse Oracle, this benchmark, or any related benchmark results.

### NYC_PAYROLL

- **Source:** NYC Open Data / City of New York.
- **3PLTA:** LD 243500 / BA 389996
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use; `NAME_ID` and `PAYROLL_NUMBER` were obfuscated/masked before release.
- **No Endorsement:** The City of New York and its agencies do not endorse Oracle, this benchmark, or any related benchmark results.

### NYC_FOR_HIRE_VEHICLES

- **Source:** NYC Open Data / City of New York.
- **3PLTA:** LD 243499 / BA 389995
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use; `VEHICLE_LICENSE_NUMBER`, `NAME`, `DMV_LICENSE_PLATE_NUMBER`, and `VEHICLE_VIN_NUMBER` were obfuscated/masked before release.
- **No Endorsement:** The City of New York and its agencies do not endorse Oracle, this benchmark, or any related benchmark results.

### MASS_WELL_DRILLING

- **Source:** Massachusetts public data source.
- **3PLTA:** LD 243498 / BA 389994
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### ITALY_WEEKLY_FUEL_PRICES

- **Source:** Italian public/open government data source.
- **3PLTA:** LD 243496 / BA 389993
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### CALF_VEHICLE_FUEL_TYPES

- **Source:** California public data source.
- **3PLTA:** LD 243495 / BA 389992
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use.

### BROOKLYN_SALES

- **Source:** NYC Department of Finance / City of New York.
- **3PLTA:** LD 243492 / BA 389989
- **Modifications:** Adapted for Oracle Analytic View SQL benchmark use; `OWNERNAME`, `ADDRESS`, `ADDRESS_2`, `XCOORD`, and `YCOORD` were obfuscated/masked before release.
- **No Endorsement:** The City of New York and its agencies do not endorse Oracle, this benchmark, or any related benchmark results.