# README: Raw Flight On-Time Performance Data (2025)
Includes data on flight delays: from January to December 2025, all raw, uncleaned data.

## 1. Source

The data come from the **Reporting Carrier On-Time Performance (1987–present)**
dataset published by the **U.S. Bureau of Transportation Statistics (BTS)**.
We downloaded the twelve monthly files covering **January–December 2025**
through the BTS TranStats field-selection download page.

Reference:
- Bureau of Transportation Statistics. *Reporting Carrier On-Time Performance:
  Table Information.*
- Bureau of Transportation Statistics. *Reporting Carrier On-Time Performance:
  Download.* Monthly data and field definitions.

## 2. File organization

The raw files are stored as monthly CSV files in the project folder
`Bureau of Transportation Statistics (202501-202512)`.
Each file corresponds to one calendar month of 2025 and is named
`T_ONTIME_REPORTING_2025MM.csv` (MM = 01–12).

Each row represents **one reported domestic nonstop flight operation**.
The raw files have **43 columns** (see Section 3).

## 3. Fields included

The 43 fields fall into the following groups.

### Time period
- `YEAR`, `QUARTER`, `MONTH`, `DAY_OF_MONTH`, `DAY_OF_WEEK`
- `FL_DATE` — flight date, stored as `M/D/YYYY 12:00:00 AM`

### Airline
- `OP_UNIQUE_CARRIER` — unique reporting carrier code (used for analysis)
- `OP_CARRIER_FL_NUM` — flight number

### Origin and destination
- `ORIGIN_AIRPORT_ID`, `ORIGIN_AIRPORT_SEQ_ID`, `ORIGIN`,
  `ORIGIN_CITY_NAME`, `ORIGIN_STATE_ABR`
- `DEST_AIRPORT_ID`, `DEST_AIRPORT_SEQ_ID`, `DEST`,
  `DEST_CITY_NAME`, `DEST_STATE_ABR`

### Departure performance
- `CRS_DEP_TIME` — scheduled local departure time (hhmm)
- `DEP_TIME` — actual local departure time (hhmm)
- `DEP_DELAY` — departure delay in minutes (negative = early)
- `DEP_DELAY_NEW` — departure delay, early departures set to 0
- `DEP_DEL15` — indicator, departure delay ≥ 15 minutes
- `DEP_TIME_BLK` — scheduled departure hour block

### Arrival performance
- `CRS_ARR_TIME` — scheduled local arrival time (hhmm)
- `ARR_TIME` — actual local arrival time (hhmm)
- `ARR_DELAY` — arrival delay in minutes (negative = early)
- `ARR_DELAY_NEW` — arrival delay, early arrivals set to 0
- `ARR_DEL15` — indicator, arrival delay ≥ 15 minutes
- `ARR_DELAY_GROUP` — arrival delay interval (15-minute groups)
- `ARR_TIME_BLK` — scheduled arrival hour block

### Cancellations and diversions
- `CANCELLED` — cancelled flight indicator (1 = yes)
- `CANCELLATION_CODE` — reason code for cancellation
- `DIVERTED` — diverted flight indicator (1 = yes)

### Flight summaries
- `CRS_ELAPSED_TIME` — scheduled elapsed flight time, minutes
- `ACTUAL_ELAPSED_TIME` — actual elapsed flight time, minutes
- `AIR_TIME` — time in the air, minutes
- `DISTANCE` — distance between origin and destination, miles

### Cause of delay (available for delayed flights only)
- `CARRIER_DELAY`, `WEATHER_DELAY`, `NAS_DELAY`,
  `SECURITY_DELAY`, `LATE_AIRCRAFT_DELAY` — delay minutes by cause

## 4. Known structural missingness

Missing values in the raw files are mostly **structural**, not data errors.

- `CANCELLATION_CODE` is only populated for cancelled flights.
- The five cause-of-delay fields are only populated for flights delayed
  by 15 minutes or more.
- `ARR_DELAY`, `ARR_TIME`, `ACTUAL_ELAPSED_TIME`, and `AIR_TIME` are
  missing for cancelled or diverted flights, which have no actual arrival.
- `DEP_DELAY` and `DEP_TIME` are missing for cancelled flights, which have
  no actual departure.

These fields should **not** be filled with zero. Arrival delay rates
should be computed only among non-cancelled, non-diverted flights with
valid arrival-delay observations. Cancellation and diversion rates use
all recorded scheduled flights as their denominator.

## 5. Coverage and limitations

- Coverage is limited to **reporting carriers** and eligible
  **U.S. domestic nonstop operations**.
- The files cover calendar year **2025** only.
- Delay and elapsed-time fields are supplied by BTS and should be used
  directly; local clock times should not be subtracted across time zones.
- Negative delays indicate early arrivals or departures and are retained.

## 6. Not included in this download

The following optional BTS fields were not included in our export and are
not required for this project: `FLIGHTS`, `DISTANCE_GROUP`,
`TAIL_NUMBER`, gate-return information, and the diverted-airport
detail fields (`DivAirportLandings`, `DivArrDelay`, `Div1`–`Div5`, etc.).
