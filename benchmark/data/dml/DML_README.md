# DML Files

This folder contains the generated DML payloads for the same 12 `AV_AI` schema tables, as well as, a smaller subset variant under [test_subset](test_subset/README.md) which can be used for faster evaluation.

The table descriptions below are inferred from the schema definitions in `../ddl/ddl.jsonl`. Each `.jsonl` file maps one-to-one to a source CSV export.

## Format

- Naming pattern: `dml_inserts_AV_AI.<TABLE_NAME>.jsonl`
- Each file currently stores a single JSON object with three top-level keys: `schema`, `table`, and `inserts`.
- Even though the extension is `.jsonl`, each file currently has one JSON object on one line (not multiple line-delimited records).
- The `inserts` value is an array of SQL `INSERT INTO ... VALUES ...;` statements for the target table.
- `Columns` below come from the DDL in `../ddl/ddl.jsonl`.
- `Insert statements` below are counted from the number of elements in each file's `inserts` array.

```json
{
  "schema": "AV_AI",
  "table": "CALF_EMPLOYMENT_SURVEY",
  "inserts": [
    "INSERT INTO AV_AI.CALF_EMPLOYMENT_SURVEY (...) VALUES (...);"
  ]
}
```

## How the JSONL Inserts Are Used

- Create the `AV_AI` tables first using the DDL definitions in `../ddl/ddl.jsonl`.
- For each table, read the matching `dml_inserts_AV_AI.<TABLE_NAME>.jsonl` file from `dml_jsonl/`.
- Parse the single JSON object, then execute each SQL statement in the `inserts` array against that table.
- The loaded rows become the benchmark base data consumed by analytic views and the downstream NL2SQL task files in `../questions/`.
- `JSONL inserts` is the effective load size from these files. For some large sources, it is intentionally lower than `CSV rows` (for example, capped at `100,000`).

## Split JSONL Files

Two DML JSONL payloads are stored as multiple Git LFS objects because the original single-line JSON files exceed GitHub LFS's per-object size limit:

- `dml_jsonl/dml_inserts_AV_AI.ONTIME_PERFORMANCE.jsonl.part01`
- `dml_jsonl/dml_inserts_AV_AI.ONTIME_PERFORMANCE.jsonl.part02`
- `dml_jsonl/dml_inserts_AV_AI.ONTIME_PERFORMANCE.jsonl.part03`
- `dml_jsonl/dml_inserts_AV_AI.USDA_COUNTY_POPULATION.jsonl.part01`
- `dml_jsonl/dml_inserts_AV_AI.USDA_COUNTY_POPULATION.jsonl.part02`

Before loading those two tables, concatenate the parts in numeric order to recreate the original JSONL file name:

```sh
cat dml_jsonl/dml_inserts_AV_AI.ONTIME_PERFORMANCE.jsonl.part* > dml_jsonl/dml_inserts_AV_AI.ONTIME_PERFORMANCE.jsonl
cat dml_jsonl/dml_inserts_AV_AI.USDA_COUNTY_POPULATION.jsonl.part* > dml_jsonl/dml_inserts_AV_AI.USDA_COUNTY_POPULATION.jsonl
```

After reconstruction, use the recreated `.jsonl` files the same way as the other DML files: parse the single JSON object and execute each SQL statement in its `inserts` array.

## Data Description

The descriptions below are inferred from the schema definitions in `../ddl/ddl.jsonl`.
This table describes the full published dataset for the 12 tables.

| Table | CSV file | DML JSONL file | Schema | Columns | JSONL inserts | Description |
| --- | --- | --- | --- | --- | --- | --- |
| `BROOKLYN_SALES` | `AV_AI.BROOKLYN_SALES.csv` | `dml_inserts_AV_AI.BROOKLYN_SALES.jsonl` | `AV_AI` | 114 | 68,700 | Brooklyn property sales data with parcel, tax, zoning, building, sale price/date, and PLUTO-style geographic attributes. |
| `CALF_EMPLOYMENT_SURVEY` | `AV_AI.CALF_EMPLOYMENT_SURVEY.csv` | `dml_inserts_AV_AI.CALF_EMPLOYMENT_SURVEY.jsonl` | `AV_AI` | 10 | 1,674,006 | California employment survey observations by area, reporting month, industry, seasonal-adjustment flag, and current employment. |
| `CALF_VEHICLE_FUEL_TYPES` | `AV_AI.CALF_VEHICLE_FUEL_TYPES.csv` | `dml_inserts_AV_AI.CALF_VEHICLE_FUEL_TYPES.jsonl` | `AV_AI` | 9 | 3,080,971 | California vehicle counts by ZIP code, model year, fuel type, make, duty class, and reporting month/year. |
| `ITALY_WEEKLY_FUEL_PRICES` | `AV_AI.ITALY_WEEKLY_FUEL_PRICES.csv` | `dml_inserts_AV_AI.ITALY_WEEKLY_FUEL_PRICES.jsonl` | `AV_AI` | 13 | 23,744 | Weekly Italy fuel price observations by survey date, product, product type/color, price component, and euro price measure. |
| `MASS_WELL_DRILLING` | `AV_AI.MASS_WELL_DRILLING.csv` | `dml_inserts_AV_AI.MASS_WELL_DRILLING.jsonl` | `AV_AI` | 19 | 177,416 | Massachusetts well drilling and completion records with location, well category/type, work performed, and depth or water-level measurements. |
| `NYC_FOR_HIRE_VEHICLES` | `AV_AI.NYC_FOR_HIRE_VEHICLES.csv` | `dml_inserts_AV_AI.NYC_FOR_HIRE_VEHICLES.jsonl` | `AV_AI` | 27 | 70,850 | NYC for-hire vehicle license roster with vehicle IDs, base details, accessibility, certification and expiration dates, and status fields. |
| `NYC_PAYROLL` | `AV_AI.NYC_PAYROLL.csv` | `dml_inserts_AV_AI.NYC_PAYROLL.jsonl` | `AV_AI` | 20 | 72,500 | NYC payroll records by fiscal year, agency, employee identifier, start date, job title, work borough, salary, hours, overtime, and other pay. |
| `NYPD_SHOOTING_INCIDENTS_2024` | `AV_AI.NYPD_SHOOTING_INCIDENTS_2024.csv` | `dml_inserts_AV_AI.NYPD_SHOOTING_INCIDENTS_2024.jsonl` | `AV_AI` | 23 | 905 | 2024 NYPD shooting incidents with occurrence date/time, borough and precinct, location type, perpetrator and victim demographics, coordinates, and victim counts. |
| `NYS_SPENDING_T` | `AV_AI.NYS_SPENDING_T.csv` | `dml_inserts_AV_AI.NYS_SPENDING_T.jsonl` | `AV_AI` | 10 | 116,126 | New York State spending facts by function, agency, fund structure, financial plan category, year, dollar amount, and CPI. |
| `ONTIME_PERFORMANCE` | `AV_AI.ONTIME_PERFORMANCE.csv` | `dml_inserts_AV_AI.ONTIME_PERFORMANCE.jsonl` | `AV_AI` | 36 | 11,536,122 | Airline on-time performance records by flight date, carrier, origin/destination market and airport, status fields, elapsed time, miles, and delay causes. |
| `SMALL_AREA_HEALTH_INSURANCE_ESTIMATES` | `AV_AI.SMALL_AREA_HEALTH_INSURANCE_ESTIMATES.csv` | `dml_inserts_AV_AI.SMALL_AREA_HEALTH_INSURANCE_ESTIMATES.jsonl` | `AV_AI` | 14 | 1,223,156 | County-level health insurance estimates by year, geography, age band, gender, income band, population, insured count, and uninsured count. |
| `USDA_COUNTY_POPULATION` | `AV_AI.USDA_COUNTY_POPULATION.csv` | `dml_inserts_AV_AI.USDA_COUNTY_POPULATION.jsonl` | `AV_AI` | 15 | 17,071,178 | USDA county population estimates segmented by year, state/county FIPS, race, sex, age group, and population total. |
