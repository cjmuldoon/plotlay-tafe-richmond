# Sample data

These tables are sample data. The figures are not about TAFE NSW Richmond or any real building. They show what a facilities team links to the buildings once each one is a keyed shape: the asset register, condition and remaining life, risk, work orders, planned maintenance, essential safety measures, the asbestos register, meters, space use and the capital works backlog, in the shape a CAFM or CMMS exports them.

How they were made, so anyone can make them again: one Python script draws everything from `random.Random(20261109)`, in ranges that look like a vocational campus’s, sized by each building’s footprint as drawn (`data/buildings/buildings.csv`; every building taken as one storey), with the same generator as the University High School showcase’s and its own seed. As at 31 October 2026. Every table has a `data_status` column that reads `Sample data` (the trend log, which a BMS writes in four columns, does not). They join on `building` (or `key`) to the building keys B01 to B38, and on `asset_id` to the register.

No real figure, chart or total in this repository uses them. The building outlines the sample sits on are the ones in `data/geometry/`.

`handover/` is the building handover pack Plotlay makes from the sample: the buildings taken from the sample buildings layer (each key a building, no rooms drawn) and the sample register in them, as COBie 2.4 (`COBie.xlsx`), IFC4 (`model.ifc`), Project Haystack (`haystack.json`, `haystack.zinc`) and Brick (`brick.ttl`), with a `README.txt`. Every building and asset keeps the same id from one export to the next.

## assets.csv

The asset register: one row per asset, a point inside its building (or in the grounds), as a CAFM exports it. Types and tag prefixes are Plotlay’s facilities asset types. 372 rows.

| Column | What it holds |
|---|---|
| `asset_id` | Tag: the type’s prefix and a number (AHU-001, EXT-014) |
| `building` | Building key, B01 to B38 (blank: in the grounds) |
| `level` | Ground, Level 1, Level 2 or Roof |
| `location` | Where on that level |
| `plotlay_type` | Plotlay asset type key (ahu, extinguisher, msb...) |
| `system` | Electrical, Fire, Egress, Mechanical, Hydraulic, BMS, Transport, Safety |
| `asset_class` | Class within the system |
| `asset_type` | Type |
| `install_year` | Year installed or last replaced |
| `expected_life_years` | Typical life of the type |
| `age_years` | 2026 less the install year |
| `remaining_life_years` | Expected life less age, plus or minus a tenth of the life for each step of condition above or below fair; not below 0 |
| `condition_1to5` | 1 very poor to 5 very good |
| `condition` | The same in words |
| `replacement_cost_aud` | Typical replacement cost of one, AUD |
| `standard` | The standard its routine service follows |
| `frequency_months` | Routine service every n months |
| `last_service` | Date of the last routine service |
| `next_due` | Last service plus the frequency |
| `service_status` | Current, Due within 30 days, or Overdue, at 31 October 2026 |
| `lat` | Latitude (WGS 84) |
| `lng` | Longitude (WGS 84) |
| `data_status` | Sample data |

## asset_risk.csv

Risk per asset: likelihood of failure times its consequence, each 1 to 5. 372 rows.

| Column | What it holds |
|---|---|
| `asset_id` | Tag, as in assets.csv |
| `building` | Building key |
| `likelihood_1to5` | 1 plus one each for condition fair or worse, poor or worse, no remaining life and service overdue; a step up or down at random for some |
| `consequence_1to5` | By type (life safety systems and main plant 4 or 5, a split system 2), one less for about a third of assets where a failure matters less |
| `risk_score` | Likelihood x consequence, 1 to 25 |
| `risk_rating` | Low (1 to 5), Medium (6 to 11), High (12 to 19), Extreme (20 to 25) |
| `risk_drivers` | What lifts it: past expected life, poor condition, service overdue, life safety system |
| `treatment` | Replace (capital works), Service now, Repair and monitor, Monitor or Accept |
| `assessed` | Date assessed |
| `data_status` | Sample data |

## work_orders.csv

Twelve months of work orders, November 2025 to October 2026: reactive jobs from the helpdesk and planned routine services from the maintenance schedule. 1,010 rows.

| Column | What it holds |
|---|---|
| `wo_id` | Work order number (WO-yymm-nnnn) |
| `raised` | Date raised |
| `building` | Building key |
| `asset_id` | The asset, where the job is on one |
| `wo_type` | Reactive or Planned |
| `source` | Helpdesk or PM schedule |
| `trade` | Trade |
| `priority` | P1 Emergency (1 day), P2 Urgent (2 days), P3 Routine (7 days), P4 Planned (28 days, or the service date) |
| `description` | What was asked for |
| `status` | Completed, Open, In progress, On hold (parts), Awaiting quote, or Cancelled (missed) |
| `target` | Date it should be done by |
| `completed` | Date completed |
| `completed_on_time` | Yes or No, for completed jobs |
| `age_days` | Days from raised to completed, or to 31 October 2026 while open |
| `overdue` | Yes for an open job past its target at 31 October 2026 |
| `cost_aud` | Cost of a completed job, AUD |
| `data_status` | Sample data |

## work_orders_monthly.csv

Each building at the end of each month, from work_orders.csv: the time axis for the map. 456 rows.

| Column | What it holds |
|---|---|
| `building` | Building key |
| `month` | yyyy-mm |
| `raised` | Raised in the month |
| `completed` | Completed in the month |
| `open_at_month_end` | Open at the month’s end |
| `overdue_at_month_end` | Open and past target at the month’s end |
| `reactive_raised` | Reactive jobs raised in the month |
| `data_status` | Sample data |

## pm_compliance.csv

Planned maintenance by building and the month each service fell due. 257 rows.

| Column | What it holds |
|---|---|
| `building` | Building key |
| `month` | yyyy-mm |
| `scheduled` | Services due |
| `on_time` | Done by the due date |
| `late` | Done after it |
| `missed` | Not done |
| `compliance_pct` | On time over scheduled, % |
| `data_status` | Sample data |

## esm_register.csv

Essential safety measures by building: each measure the building has, its inspections and defects. 131 rows.

| Column | What it holds |
|---|---|
| `building` | Building key |
| `measure` | The essential safety measure |
| `items` | How many assets it covers (1 for paths of travel) |
| `frequency_months` | Inspected every n months |
| `last_inspected` | Latest inspection |
| `next_due` | Next inspection |
| `status` | Current, Due within 30 days, or Overdue (an item overdue makes the measure overdue) |
| `open_defects` | Defects not yet fixed |
| `annual_report_due` | Date the annual essential safety measures report is due |
| `data_status` | Sample data |

## asbestos_register.csv

Asbestos-containing materials, presumed or sampled, in the buildings whose services date from before 1990 (sample). 20 rows.

| Column | What it holds |
|---|---|
| `acm_id` | Register number |
| `building` | Building key |
| `level` | Level |
| `location` | Where |
| `material` | The material |
| `friable` | Yes or No |
| `sampled` | Presumed, or Sampled, positive |
| `condition` | Good, Fair or Poor |
| `risk` | Low, Medium or High (friable and not good: High) |
| `labelled` | Yes or No |
| `last_inspected` | Latest inspection |
| `review_status` | Current, or Review due when last inspected before 2022 |
| `action` | Manage in place, Encapsulate, or Remove (capital works) |
| `data_status` | Sample data |

## meter_trends.csv

A BMS trend log, long form (timestamp, point, value, unit): hourly kWh from each metered building’s electricity meter and kL from its water meters, Monday 5 October to Sunday 1 November 2026. Points are named <building>-EM-01.kWh and <building>-WM-01.kL, so Plotlay’s BMS import puts each on its building. 16,800 rows.

| Column | What it holds |
|---|---|
| `timestamp` | Start of the hour, local time |
| `point` | Meter point name |
| `value` | Reading for the hour |
| `unit` | kWh or kL |

## space_use.csv

Teaching spaces by building and type (the buildings of 300 m² or more), and how much of the week they are timetabled and how full. 19 rows.

| Column | What it holds |
|---|---|
| `building` | Building key |
| `space_type` | Type of space |
| `spaces` | How many |
| `seats` | Seats in all |
| `periods_available` | 25 sessions a week per space |
| `periods_timetabled` | Sessions with a class |
| `frequency_pct` | Timetabled over available, % |
| `occupancy_pct` | Students over seats when in use, % |
| `utilisation_pct` | Frequency x occupancy, % |
| `data_status` | Sample data |

## capital_works.csv

The capital works backlog: asset renewals for high-risk assets near the end of their life, fabric work by condition, asbestos removal and accessibility. 82 rows.

| Column | What it holds |
|---|---|
| `project_id` | Project number |
| `building` | Building key |
| `element` | The element or system |
| `description` | What the work is |
| `driver` | Risk and end of life, Condition, Compliance or Accessibility |
| `priority` | 1 Within a year, 2 In 2 to 3 years, 3 In 4 to 5 years, 4 In 6 to 10 years |
| `target_year` | Year it is planned for |
| `estimate_aud` | Estimate, AUD, to the nearest 1,000 |
| `linked_assets` | Asset tags or register numbers it renews, separated by ; |
| `status` | Unfunded, Funded or In design |
| `data_status` | Sample data |

## building_summary.csv

One row per building: the tables above rolled up. What the sample maps colour. 38 rows.

| Column | What it holds |
|---|---|
| `key` | Building key |
| `fabric_condition_1to5` | Condition of the building fabric, 1 to 5 |
| `fabric_condition` | The same in words |
| `assets` | Assets in the register |
| `assets_past_life` | Assets with no remaining life |
| `risk_max` | Highest asset risk score |
| `risk_high_or_extreme` | Assets rated High or Extreme |
| `wo_open` | Work orders open at 31 October 2026 |
| `wo_overdue` | Of those, past target |
| `wo_reactive_12m` | Reactive work orders in the 12 months |
| `pm_compliance_pct` | Planned services done on time over the 12 months, % |
| `esm_overdue` | Essential safety measures overdue |
| `esm_measures` | Essential safety measures it has |
| `asbestos_items` | Asbestos register items |
| `backlog_aud` | Capital works backlog, AUD |
| `kwh_4wk` | Electricity in the four weeks of the trend log, kWh (buildings of 300 m² or more have a meter) |
| `kwh_per_m2_4wk` | The same over the footprint (one storey) |
| `after_hours_share_pct` | Share used outside 8 am to 4 pm on teaching days, % |
| `data_status` | Sample data |
