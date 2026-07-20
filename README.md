ORACLE AND ITS AFFILIATES DO NOT PROVIDE ANY WARRANTY WHATSOEVER, EXPRESS OR IMPLIED, FOR ANY SOFTWARE, MATERIAL OR CONTENT OF ANY KIND CONTAINED OR PRODUCED WITHIN THIS REPOSITORY, AND IN PARTICULAR SPECIFICALLY DISCLAIM ANY AND ALL IMPLIED WARRANTIES OF TITLE, NON-INFRINGEMENT, MERCHANTABILITY, AND FITNESS FOR A PARTICULAR PURPOSE. FURTHERMORE, ORACLE AND ITS AFFILIATES DO NOT REPRESENT THAT ANY CUSTOMARY SECURITY REVIEW HAS BEEN PERFORMED WITH RESPECT TO ANY SOFTWARE, MATERIAL OR CONTENT CONTAINED OR PRODUCED WITHIN THIS REPOSITORY. IN ADDITION, AND WITHOUT LIMITING THE FOREGOING, THIRD PARTIES MAY HAVE POSTED SOFTWARE, MATERIAL OR CONTENT TO THIS REPOSITORY WITHOUT ANY REVIEW. USE AT YOUR OWN RISK.

# NL 2 Analytic Views SQL Benchmark

This repository contains resources for benchmarking natural language to [Oracle Analytic View (AV)](https://docs.oracle.com/en/database/oracle/oracle-database/21/dwhsg/overview-analytic-views.html) SQL generation. It provides Oracle Analytic View definitions over tables from multiple public-data domains, along with natural language questions and gold Analytic View SQL answers.

The benchmark is intended to test how well LLMs and agentic NL2SQL systems can discover analytic-view metadata, choose the right dimensions, hierarchies, levels, and measures, and produce valid Oracle SQL using `FROM ANALYTIC VIEW (USING ...)` patterns.

The data artifacts live under `benchmark/` and include the following.

## Agent Specs

The `benchmark/agent/` directory includes two [Open Agent Spec](https://oracle.github.io/agent-spec/26.1.2/index.html) exports used by the benchmark. The Analytic View agent uses schema-discovery and AV-specific metadata tools, executes candidate SQL for validation, and returns final AV SQL. The vanilla agent provides a baseline using only general schema-discovery and SQL-execution tools, without the Analytic View skill or AV-specific tools.

- Analytic View agent: [benchmark/agent/analytic_view_agent_spec.json](benchmark/agent/analytic_view_agent_spec.json)
- Vanilla baseline agent: [benchmark/agent/vanilla_agent_spec.json](benchmark/agent/vanilla_agent_spec.json)
- Import and deployment notes: [benchmark/agent/README.md](benchmark/agent/README.md)
- Benchmark results: [benchmark/agent/RESULTS.md](benchmark/agent/RESULTS.md)

The results highlight that an AV SQL skill with specialized Analytic View tools substantially
outperforms a vanilla agent baseline across model families.

## Data Tables

Source links and licensing or reuse notes below were checked against the current source pages or portal terms on 2026-04-28.

| Table | Host | Download link | CSV file | License or reuse terms | License link | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| `BROOKLYN_SALES` | [www.nyc.gov](https://www.nyc.gov) | [Download](https://www.nyc.gov/assets/finance/downloads/pdf/rolling_sales/annualized-sales/2017/2017_brooklyn.xls) | [BROOKLYN_SALES.csv](benchmark/data/dml/csv/AV_AI.BROOKLYN_SALES.csv) | `NYC Open Data Terms of Use (Public Domain/Open Data)` | [NYC Open Data Public Policies](https://cityofnewyork.github.io/opendatatsm/publicpolicies.html) | The data is provided "as is". Although it is free of restrictive rights, the user assumes full responsibility for verifying the accuracy of prices and cadastral information before any commercial or legal use. |
| `CALF_EMPLOYMENT_SURVEY` | [data.ca.gov](https://data.ca.gov) | [Download](https://lab.data.ca.gov/dataset/current-employment-statistics-ces-2) | [CALF_EMPLOYMENT_SURVEY.csv](benchmark/data/dml/csv/AV_AI.CALF_EMPLOYMENT_SURVEY.csv) | `Creative Commons Attribution` | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) | Listed directly on the CA Open Data dataset page. |
| `CALF_VEHICLE_FUEL_TYPES` | [data.ca.gov](https://data.ca.gov) | [Download](https://data.ca.gov/dataset/15179472-adeb-4df6-920a-20640d02b08c/resource/d599c3d3-87af-4e8c-8694-9c01f49e3d93/download/vehicle-fuel-type-count-by-zip-code-20231.csv) | [CALF_VEHICLE_FUEL_TYPES.csv](benchmark/data/dml/csv/AV_AI.CALF_VEHICLE_FUEL_TYPES.csv) | `Public domain` | [CA Open Data Licenses](https://lab.data.ca.gov/licenses) | Listed directly on the CA Open Data dataset page. |
| `ITALY_WEEKLY_FUEL_PRICES` | [data.europa.eu](https://data.europa.eu) | [Download](https://energy.ec.europa.eu/document/download/906e60ca-8b6a-44e7-8589-652854d2fd3f_en?filename=Weekly_Oil_Bulletin_Prices_History_maticni_4web.xlsx) | [ITALY_WEEKLY_FUEL_PRICES.csv](benchmark/data/dml/csv/AV_AI.ITALY_WEEKLY_FUEL_PRICES.csv) | `Italian Open Data License (IODL) 2.0` | [IODL 2.0](http://www.dati.gov.it/iodl/2.0/) | This license allows viewing, extraction, downloading, copying, distribution, and the creation of derivative works (mashups). The user is required to cite the source (Ministero dell'Ambiente e della Sicurezza Energetica) and include a link to the IODL 2.0 license. |
| `MASS_WELL_DRILLING` | [data.mass.gov](https://data.mass.gov) | [Download](https://eeaonline.eea.state.ma.us/portal#!/search/welldrilling) | [MASS_WELL_DRILLING.csv](benchmark/data/dml/csv/AV_AI.MASS_WELL_DRILLING.csv) | `Massachusetts Public Records Law (G.L. c. 66)` | [Massachusetts G.L. c. 66](https://malegislature.gov/Laws/GeneralLaws/PartI/TitleX/Chapter66) | Public Records Law (G.L. c. 66) As a public document, this data is presumed to be open to all. The state cannot restrict your use of it (commercial or otherwise), but it disclaims all responsibility for the technical accuracy of drilling reports submitted by third parties (certified drillers). |
| `NYC_FOR_HIRE_VEHICLES` | [data.cityofnewyork.us](https://data.cityofnewyork.us) | [Download](https://data.cityofnewyork.us/api/views/8wbx-tsch/rows.csv?accessType=DOWNLOAD) | [NYC_FOR_HIRE_VEHICLES.csv](benchmark/data/dml/csv/AV_AI.NYC_FOR_HIRE_VEHICLES.csv) | `No restrictions on use via NYC Open Data` | [NYC Open Data FAQ](https://opendata.cityofnewyork.us/faq/) | NYC Open Data says there are no restrictions on the use of Open Data, while also stating that users are subject to NYC.gov terms of use and any additional agency terms. |
| `NYC_PAYROLL` | [data.cityofnewyork.us](https://data.cityofnewyork.us) | [Download](https://data.cityofnewyork.us/api/views/k397-673e/rows.csv?accessType=DOWNLOAD) | [NYC_PAYROLL.csv](benchmark/data/dml/csv/AV_AI.NYC_PAYROLL.csv) | `No restrictions on use via NYC Open Data` | [NYC Open Data FAQ](https://opendata.cityofnewyork.us/faq/) | NYC Open Data says there are no restrictions on the use of Open Data, while also stating that users are subject to NYC.gov terms of use and any additional agency terms. |
| `NYPD_SHOOTING_INCIDENTS_2024` | [data.cityofnewyork.us](https://data.cityofnewyork.us) | [Download](https://data.cityofnewyork.us/api/views/833y-fsy8/rows.csv?accessType=DOWNLOAD) | [NYPD_SHOOTING_INCIDENTS_2024.csv](benchmark/data/dml/csv/AV_AI.NYPD_SHOOTING_INCIDENTS_2024.csv) | `No restrictions on use via NYC Open Data` | [NYC Open Data FAQ](https://opendata.cityofnewyork.us/faq/) | NYC Open Data says there are no restrictions on the use of Open Data, while also stating that users are subject to NYC.gov terms of use and any additional agency terms. |
| `NYS_SPENDING_T` | [data.ny.gov](https://data.ny.gov) | [Download](https://data.ny.gov/api/views/4mpt-rfrw/rows.csv?accessType=DOWNLOAD) | [NYS_SPENDING_T.csv](benchmark/data/dml/csv/AV_AI.NYS_SPENDING_T.csv) | `OPEN-NY Terms of Use` | [OPEN-NY Terms of Use](https://data.ny.gov/api/views/77gx-ii52/files/ef0c1840-ad54-4240-92fd-6397c49fde46?filename=OPEN-NY_20Terms_20of_20Use.pdf) | OPEN-NY says users may freely download and use portal content as long as they comply with the terms of use. |
| `ONTIME_PERFORMANCE` | [transtats.bts.gov](https://transtats.bts.gov) | [Download](https://transtats.bts.gov/PREZIP/On_Time_Reporting_Carrier_On_Time_Performance_1987_present_2019_1.zip) | [ONTIME_PERFORMANCE.csv](benchmark/data/dml/csv/AV_AI.ONTIME_PERFORMANCE.csv) | `Public Domain (Domaine Public US)` | [USA.gov Federal Copyright Guidance](https://www.usa.gov/government-copyright) | As a work of the U.S. federal government, this dataset is in the public domain. It may be copied, modified, and distributed without permission, including for commercial purposes. Attribution to the BTS is, however, recommended for data transparency. |
| `SMALL_AREA_HEALTH_INSURANCE_ESTIMATES` | [www.census.gov](https://www.census.gov) | [Download](https://www2.census.gov/programs-surveys/sahie/datasets/time-series/estimates-acs/sahie-2015-csv.zip) | [SMALL_AREA_HEALTH_INSURANCE_ESTIMATES.csv](benchmark/data/dml/csv/AV_AI.SMALL_AREA_HEALTH_INSURANCE_ESTIMATES.csv) | `Federal government public-use/open data` | [Census Open Data](https://www.census.gov/about/policies/open-gov/open-data.html) | Census describes its public datasets as open/public-use data that users can analyze, extract, customize, and publish, subject to privacy protections and any non-government content exceptions. |
| `USDA_COUNTY_POPULATION` | [www.ers.usda.gov](https://www.ers.usda.gov) | [Download](https://www.ers.usda.gov/media/5499/population-estimates-for-the-united-states-states-and-counties-2020-23.csv?v=79463) | [USDA_COUNTY_POPULATION.csv](benchmark/data/dml/csv/AV_AI.USDA_COUNTY_POPULATION.csv) | `Public domain (USDA)` | [USDA Policies and Links](https://www.usda.gov/about-usda/policies-and-links) | USDA says most information on USDA websites is considered public domain and may be freely distributed or copied, with attribution requested. |


### Oracle DDL Data for Schema Creation

The `benchmark/data/ddl/` folder contains the schema-side artifacts (Table DDL, Analytic view DDL, Attribute dimension DDL and Hierarchy DDL) for the dataset:
- All table definitions belong to the `AV_AI` schema.
- The table-level DDL information is documented in [benchmark/data/ddl/DDL_README.md](benchmark/data/ddl/DDL_README.md).

### Oracle DML Data for Data Ingestion

The `benchmark/data/dml/` folder contains the generated insert payloads derived from the source CSV extracts.
- The full DML inventory is documented in [benchmark/data/dml/DML_README.md](benchmark/data/dml/DML_README.md).

## Natural Language Questions to Oracle AV SQL examples

The `benchmark/data/questions/` folder contains per-analytic-view NL2SQL task files in `.jsonl` format.
- The task-files are documented in [benchmark/data/questions/README.md](benchmark/data/questions/README.md).

# Contributors

- [Vasileios Trigonakis](https://github.com/trigonak)
- [Yassir Salmi](https://github.com/yassirsalmi)
- [Vlad Haprian](https://github.com/VladHaprian)
- Anne Murphy
- [Onur Kocberber](https://github.com/onurkocberber)
- [Sungpack Hong](https://github.com/hongsup2)
- Ekrem Soylemez

# Contributing

This project is not accepting external contributions at this time.

# Security

Please consult the [security guide](SECURITY.md) for our responsible security vulnerability disclosure process

# License

Copyright (c) 2026 Oracle and/or its affiliates. Released under the Creative Commons Attribution 4.0 International License (CC BY 4.0) as shown under https://creativecommons.org/licenses/by/4.0/
