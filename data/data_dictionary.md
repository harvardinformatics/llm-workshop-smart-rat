# Data Dictionary

A short guide to every file in `data/raw/`: what it is, what one row means, and what you can do
with it. Read this before you point an LLM at the data — good prompts start with knowing what
you're looking at.

**Provenance.** Both datasets come from the [City of Cambridge Open Data
Portal](https://data.cambridgema.gov/), which runs on Socrata. The smart box data is collected by
**Modern Pest Services**, the city's pest-control contractor (dataset `78bs-b5ig`); the sighting
data comes from **SeeClickFix / Commonwealth Connect**, the system behind Cambridge's 311 service
requests (dataset `uz4w-baz5`). There is **no paper behind this data** — it's municipal
administrative data, published as a by-product of running the program rather than of answering a
research question. That's worth remembering: nobody designed it to answer yours.

**The files in `data/raw/` are a frozen snapshot, pulled in July 2026**, and deliberately not kept
current. Re-running an analysis against a fresh pull is the Day 2 exercise in
[`../orientation/05_claude_organization.md`](../orientation/05_claude_organization.md).

---

## Files at a glance

| File | Grain (one row = ) | Use it for |
|---|---|---|
| `raw/smart_rat_boxes.csv` | one device *deployment* | where the city intervened, and how much each device caught |
| `raw/rodent_311.csv` | one citizen complaint | where and when residents reported rodents |

---

## The files

### `raw/smart_rat_boxes.csv` — the intervention

One row per **deployment**, not per device: a device moved to a new site gets a new row, so there
are more rows in this file than there are distinct `serial_number` values.

| Column | Type | Description |
|---|---|---|
| `serial_number` | text | Hardware ID of the device. **Not unique** — a redeployed device appears again under the same serial |
| `device_name` | text | Human-readable location label. Informal; may name a park, street, or building |
| `address` | text | Street address of deployment. May be approximate; some entries are intersections |
| `latitude` | number | Decimal degrees latitude. Missing on a few rows |
| `longitude` | number | Decimal degrees longitude. Missing on the same rows |
| `coordinates` | point | GeoJSON point (lng, lat) — combines `latitude` and `longitude` |
| `active_site` | boolean | The city's flag for whether the device is currently deployed |
| `device_type` | text | `Box` (above-ground) or `Pipe` (sewer) |
| `total_since_install` | number | Cumulative catches since install |
| `prior_to_2023` | number | Catches recorded before 2023. Blank wherever the device post-dates 2023 — NULL, not `0` |
| `total_2023` | number | Total catches in 2023 |
| `total_2024` | number | Total catches in 2024 |
| `total_2025` | number | Total catches in 2025 |
| `total_2026` | number | Total catches in 2026 (year to date) |
| `january_2026` … `july_2026` | number | Monthly catches in 2026 — the only month-level detail in the file |
| `august_2026` … `december_2026` | number | Months after the snapshot date. A zero here means *not yet recorded*, not *no catches* |
| `participatory_budgeting` | text | `Yes` / `No` — whether the device was funded through Cambridge's participatory budgeting program |


### `raw/rodent_311.csv` — what residents reported

One row per citizen complaint, pre-filtered by the city to rodent-related tickets. The file has
more physical lines than it has records, because descriptions contain line breaks.

| Column | Type | Description |
|---|---|---|
| `ticket_id` | text | SeeClickFix ticket ID |
| `city` | text | Always `Cambridge`. Constant; carries no information |
| `issue_type` | text | Category as entered by the filer. Nearly all are `Rodent Sighting`, followed by a long free-text tail |
| `ticket_status` | text | `Archived`, `Open`, `Acknowledged`, `Closed`. Overwhelmingly `Archived` — see the gotchas |
| `issue_description` | text | Free-text description by the filer. Sometimes blank, and may contain line breaks |
| `ticket_closed_date_time` | datetime | When the ticket was resolved. Sometimes blank. Format `MM/DD/YYYY hh:mm:ss AM/PM` |
| `ticket_created_date_time` | datetime | When the ticket was submitted. **Never blank.** Format `MM/DD/YYYY hh:mm:ss AM/PM` |
| `ticket_last_updated_date_time` | datetime | Last status change. **Different format:** `YYYY Mon DD hh:mm:ss AM/PM` |
| `address` | text | Address of the complaint. Free text, inconsistently formatted |
| `lat` | number | Latitude of the complaint |
| `lng` | number | Longitude of the complaint |
| `location` | point | GeoJSON point (lng, lat) — combines `lat` and `lng` |
| `image` | text | URL to a user-submitted photo, where the filer attached one |


---

## Analysis gotchas (the things that quietly change your answer)

- **A zero catch count does not mean no rats.** A zero can mean the device caught nothing, or
  malfunctioned, or went offline, or was installed last week. The city's own metadata says it
  plainly: *"A report of 0 caught does not mean there are no rats."* Nothing in the file
  distinguishes these cases.
- **`issue_description` contains line breaks inside quoted fields.** The file has substantially
  more physical lines than records. Anything that splits on newlines — `wc -l`, `readLines`, a
  hand-rolled parser — will silently mangle those records and invent hundreds of fragments that
  look like rows. Use a real CSV parser (`pandas.read_csv`, `readr::read_csv`, `csv.DictReader`),
  and check that the row count it gives you matches the record count the portal reports.
- **Two date formats in one file.** `created` and `closed` are `MM/DD/YYYY hh:mm:ss AM/PM`;
  `last_updated` is `YYYY Mon DD hh:mm:ss AM/PM`. Parsing all three with one format string will
  fail, or worse, succeed on some rows and produce `NaT` on others.
- **The 2014–2015 hole.** A stray record in 2013, then nothing at all until 2016. A trend computed
  from the first row of the file is measuring a reporting-system change, not rodent activity.
- **Blank is NULL, not zero.** An empty cell parses as `NA` / `NaN` / `None` — a *missing value*,
  which is a different fact about the world than the number 0. `prior_to_2023` is blank wherever
  the device did not exist yet; "we have no value" is not "it caught nothing." The practical
  consequence cuts two ways, and only one of them is a trap:
  **summing is fine** — most tools skip NULLs, and the city's own arithmetic already does too
  (`total_since_install` equals the sum of the year columns).
  **Averaging is not** — the denominator is the number of devices that actually existed before
  2023, not the number of rows in the file. Average over every row and you have quietly averaged
  in devices that hadn't been installed.
