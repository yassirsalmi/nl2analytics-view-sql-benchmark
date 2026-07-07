# DDL Files

This folder contains the schema artifacts for the 12 tables. All tables are stored in schema called `AV_AI`. 

The `ddl.jsonl` file is the primary source of table definitions, and the files in `avs_ddl_csv` store exported DDL for analytic views, attribute dimensions, and hierarchies built on top of those tables.

## Folder Contents

| Artifact | File | Entries | Purpose |
| --- | --- | ---: | --- |
| Table DDL | `ddl.jsonl` | 12 | One JSON object per line containing the schema name, table name, and full `CREATE TABLE` statement. |
| Analytic view DDL | `avs_ddl_csv/av_ddl.csv` | 12 | Two-column CSV export of analytic view names and their `CREATE OR REPLACE ANALYTIC VIEW` statements. |
| Attribute dimension DDL | `avs_ddl_csv/dimensions_ddl.csv` | 98 | Two-column CSV export of attribute dimension names and their DDL. |
| Hierarchy DDL | `avs_ddl_csv/hierarchies_ddl.csv` | 121 | Two-column CSV export of hierarchy names and their DDL. |

## Format

- `ddl.jsonl` uses JSON Lines format, with one DDL payload per line.
- Each JSON object in `ddl.jsonl` currently contains the keys `schema`, `table`, and `ddl`.
- The files in `avs_ddl_csv` are two-column CSV exports: one name column and one `DDL` column.
- `Columns` below are counted from the `CREATE TABLE` column definitions in `ddl.jsonl` and include generated or virtual columns where they are part of the schema definition.

```json
{
  "schema": "AV_AI",
  "table": "BROOKLYN_SALES",
  "ddl": "CREATE TABLE \"AV_AI\".\"BROOKLYN_SALES\" (...)"
}
```

## Tables Description

The descriptions below are inferred from the schema definitions in `ddl.jsonl`.

| Table | Schema | Columns | Description |
| --- | --- | ---: | --- |
| `BROOKLYN_SALES` | `AV_AI` | 114 | Brooklyn property sales data with parcel, tax, zoning, building, sale price/date, and PLUTO-style geographic attributes. |
| `CALF_EMPLOYMENT_SURVEY` | `AV_AI` | 10 | California employment survey observations by area, reporting month, industry, seasonal-adjustment flag, and current employment. |
| `CALF_VEHICLE_FUEL_TYPES` | `AV_AI` | 9 | California vehicle counts by ZIP code, model year, fuel type, make, duty class, and reporting month/year. |
| `ITALY_WEEKLY_FUEL_PRICES` | `AV_AI` | 13 | Weekly Italy fuel price observations by survey date, product, product type/color, price component, and euro price measure. |
| `MASS_WELL_DRILLING` | `AV_AI` | 19 | Massachusetts well drilling and completion records with location, well category/type, work performed, and depth or water-level measurements. |
| `NYC_FOR_HIRE_VEHICLES` | `AV_AI` | 27 | NYC for-hire vehicle license roster with vehicle IDs, base details, accessibility, certification and expiration dates, and status fields. |
| `NYC_PAYROLL` | `AV_AI` | 20 | NYC payroll records by fiscal year, agency, employee identifier, start date, job title, work borough, salary, hours, overtime, and other pay. |
| `NYPD_SHOOTING_INCIDENTS_2024` | `AV_AI` | 23 | 2024 NYPD shooting incidents with occurrence date/time, borough and precinct, location type, perpetrator and victim demographics, coordinates, and victim counts. |
| `NYS_SPENDING_T` | `AV_AI` | 10 | New York State spending facts by function, agency, fund structure, financial plan category, year, dollar amount, and CPI. |
| `ONTIME_PERFORMANCE` | `AV_AI` | 36 | Airline on-time performance records by flight date, carrier, origin/destination market and airport, status fields, elapsed time, miles, and delay causes. |
| `SMALL_AREA_HEALTH_INSURANCE_ESTIMATES` | `AV_AI` | 14 | County-level health insurance estimates by year, geography, age band, gender, income band, population, insured count, and uninsured count. |
| `USDA_COUNTY_POPULATION` | `AV_AI` | 15 | USDA county population estimates segmented by year, state/county FIPS, race, sex, age group, and population total. |
