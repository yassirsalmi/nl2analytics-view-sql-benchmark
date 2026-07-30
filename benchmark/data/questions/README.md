# NL2SQL Task Files

This folder contains the NL2SQL task files for 12 `AV_AI` analytic views documented in the companion DDL and DML resources:

- [`../../../README.md`](../../../README.md) for the dataset overview and source context.
- [`../ddl/DDL_README.md`](../ddl/DDL_README.md) for table, analytic view, attribute dimension, and hierarchy DDL.
- [`../dml/DML_README.md`](../dml/DML_README.md) for the generated insert payloads used to populate the base tables.

Each primary task file named `tasks_<ANALYTIC_VIEW_NAME>.jsonl` maps one-to-one to an analytic view defined in `../ddl/avs_ddl_csv/av_ddl.csv`. Every line is a single NL2SQL task record containing a natural-language prompt that describes the task and a generated SQL query for that task, and the separate `tasks_unit_test.jsonl` file is a cross-analytic-view sample used only for testing.

## Format

- Naming pattern: `tasks_<ANALYTIC_VIEW_NAME>.jsonl`
- Each line is a standalone JSON object.
- Each primary `tasks_<ANALYTIC_VIEW_NAME>.jsonl` file contains records for exactly one `AV_NAME`.
- The current task-set version is `0.1`.

Each JSON object currently contains these keys:

- `QUERY_SET_VERSION`
- `QUERY_ID`
- `META_QUERY_ID`
- `PROMPT_ID`
- `AV_NAME`
- `PROMPT`
- `QUERY_TEXT`

```json
{
  "QUERY_SET_VERSION": "0.1",
  "QUERY_ID": "97950",
  "META_QUERY_ID": "97683",
  "PROMPT_ID": "98858",
  "AV_NAME": "OTP_ONTIME_PERFORMANCE_AV",
  "PROMPT": "Select total carrier delay hours, avg delay minutues, avg actual elapsed time minutes at levels year, arrival delay group when Year equal to 'YR-2019'",
  "QUERY_TEXT": "SELECT ..."
}
```

## Current Files

The current selected task set contains 1,400 records across 12 files.

`tasks_unit_test.jsonl` is a 100-question unit-test task file created for faster benchmark checks. It is sampled from the same selected tasks as the complete per-analytic-view dataset, so its percent accuracy results are expected to be similar to runs over the full task set.

| Analytic view | Task file | Rows |
| --- | --- | ---: |
| `BKS_BROOKLYN_SALES_AV` | `tasks_BKS_BROOKLYN_SALES_AV.jsonl` | 79 |
| `CAES_CALF_EMPLOYMENT_SURVEY_AV` | `tasks_CAES_CALF_EMPLOYMENT_SURVEY_AV.jsonl` | 76 |
| `CTF_CALF_VEHICLE_FUEL_TYPES_AV` | `tasks_CTF_CALF_VEHICLE_FUEL_TYPES_AV.jsonl` | 46 |
| `IT_ITALY_WEEKLY_FUEL_PRICES_AV` | `tasks_IT_ITALY_WEEKLY_FUEL_PRICES_AV.jsonl` | 165 |
| `MA_MASS_WELL_DRILLING_AV` | `tasks_MA_MASS_WELL_DRILLING_AV.jsonl` | 149 |
| `NYC_PAY_NYC_PAYROLL_AV` | `tasks_NYC_PAY_NYC_PAYROLL_AV.jsonl` | 91 |
| `NYC_VFH_NYC_FOR_HIRE_VEHICLES_AV` | `tasks_NYC_VFH_NYC_FOR_HIRE_VEHICLES_AV.jsonl` | 103 |
| `NYPD_SHOOT_SHOOTING_INCIDENTS_2024_AV` | `tasks_NYPD_SHOOT_SHOOTING_INCIDENTS_2024_AV.jsonl` | 123 |
| `NYS_SPENDING_AV` | `tasks_NYS_SPENDING_AV.jsonl` | 158 |
| `OTP_ONTIME_PERFORMANCE_AV` | `tasks_OTP_ONTIME_PERFORMANCE_AV.jsonl` | 146 |
| `SAHIE_SMALL_AREA_HEALTH_INSURANCE_ESTIMATES_AV` | `tasks_SAHIE_SMALL_AREA_HEALTH_INSURANCE_ESTIMATES_AV.jsonl` | 173 |
| `UDSA_COUNTY_POPULATION_AV` | `tasks_UDSA_COUNTY_POPULATION_AV.jsonl` | 91 |

## Field Notes

- `PROMPT_ID` identifies the selected natural-language prompt variant.
- `QUERY_ID` identifies the SQL query text.
- `META_QUERY_ID` identifies the underlying logical query intent or template.
- `QUERY_TEXT` is the Oracle SQL for the task.
