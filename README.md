# Introduction to GTFS

## History and Origin

The General Transit Feed Specification (GTFS) was born out of a practical frustration: in the early 2000s, there was no standardized way for transit agencies to share their schedule and route data with the outside world. Each agency maintained its data in proprietary formats, making it nearly impossible for software developers to build applications that worked across multiple transit systems, and equally difficult for riders to find reliable information in a single, familiar interface.

The solution emerged in 2005 through a collaboration between TriMet, the public transit agency serving the Portland, Oregon metropolitan area, and Google [1]. A TriMet employee named Chris Harrington worked with Google engineer Bibiana McHugh to format TriMet's transit data into an easily maintainable and consumable format that could be imported into Google Maps [1]. This transit data format was originally known as the **Google Transit Feed Specification (GTFS)**, and the first version of Google Transit was launched using TriMet's data covering the Portland Metro area [1].

The problems GTFS was designed to solve were both technical and organizational. On the technical side, transit agencies stored data in incompatible systems — ranging from paper timetables to proprietary scheduling software — with no common data model. On the organizational side, there was no mechanism for agencies to share this data with third-party developers. GTFS addressed both by defining a simple, open, file-based format that any agency could produce and any developer could consume. The specification was deliberately designed to be relatively simple to create and read for both people and machines, focusing on practical needs in communicating service information to passengers rather than attempting to model every operational detail of a transit system [1].

## GTFS Today

In 2010, the format was renamed the **General Transit Feed Specification** to accurately represent its use across many different applications beyond Google products [1]. This renaming was significant: it marked the transition of GTFS from a proprietary Google initiative to a true open standard, maintained by the broader transit and technology community.

Today, GTFS is the de facto global standard for public transportation data. It is currently used by over 10,000 transit agencies in more than 100 countries [2]. The standard is actively maintained by **MobilityData**, an independent nonprofit organization established in 2019, with continuous refinement driven by a global community of transit providers, app developers, consultants, and academic organizations [1] [2].

GTFS is actively used to power a wide range of applications, including trip planning, timetable creation, mobile data visualization, accessibility tools, and analysis for transit planning [1]. In Europe, the adoption of GTFS and the related NeTEx format is driven by EU delegated regulation 2017/1926, which requires member states to publish datasets on so-called National Access Points (NAPs) to facilitate application development and research on a multi-national scale [3]. Countries such as Finland, Ireland, the Netherlands, Norway, and Sweden already publish aggregated GTFS feeds covering their entire national transit networks [3].

The primary advantages of GTFS include its simplicity, backward compatibility, and the ability to describe complex transit scenarios such as loop routes, lasso routes, and branches [4]. The standard has also expanded beyond static schedule data: GTFS Realtime, introduced in 2011, enables live updates on vehicle locations, trip updates, and service advisories, significantly reducing wait times and improving the overall rider experience [2]. More recent extensions, such as GTFS-Flex, allow agencies to describe demand-responsive and flexible transit services.

## The Importance of Adoption

It is critical for more transit agencies to adopt the GTFS standard to realize the full benefits of data standardization. When agencies publish their data in GTFS format, they ensure consistency across regions, which simplifies travel planning for riders navigating multi-agency trips [2]. Standardization also lowers the barrier to entry for software developers, allowing agencies to reach a wider audience through a vast pool of third-party applications without the need for custom integrations [2].

> "GTFS provides a standardized format for transit data, ensuring consistency across agencies and regions, simplifying travel planning for riders." [2]

From a planning and analytical perspective, standardized GTFS data is invaluable. It enables the development of more accurate transit and predictive models. Researchers and planners use GTFS data combined with machine learning techniques to predict travel times, analyze spatial and temporal patterns of transit services, and assess transit reliability. The consistent structure of GTFS allows algorithms to be trained on data from one city and applied to another, accelerating the development of robust, generalizable models. Without a common data format, such large-scale cross-agency analysis and the development of sophisticated predictive tools would be prohibitively difficult.

Furthermore, the GTFS Best Practices Working Group — convened by the Rocky Mountain Institute and consisting of 17 organizations, including app providers, transit agencies, and consultants — has established that coordinated best practices are essential to prevent diverging requirements and application-specific datasets that reduce interoperability [4]. Agencies that adopt GTFS and follow its best practices contribute to a healthier, more interoperable transit data ecosystem that benefits riders, developers, and planners alike.

---

# 1. Data

## 1.1 Raw Data Collection

The foundation of any GTFS dataset is the raw transit data collected in the field. Before a single line of a GTFS file can be written, an agency must have accurate, reliable information about its routes, stops, schedules, and vehicle behavior. Transit agencies employ a variety of methods and technologies to gather this information, and the quality of the final GTFS output is directly dependent on the quality of the data collected at this stage.

**Automatic Vehicle Location (AVL)** is one of the most critical technologies in modern transit data collection. AVL systems utilize GPS receivers installed on transit vehicles combined with telecommunication networks to track the real-time position of each vehicle. This data is transmitted to a central dispatch system, providing the raw geographical and temporal information necessary to map route alignments and determine actual travel times between stops. AVL data is the primary input for generating accurate `shapes.txt` files (route alignments) and for validating the `arrival_time` and `departure_time` fields in `stop_times.txt`.

**Automated Passenger Counters (APC)** are sensors installed on vehicle doors that count the number of passengers boarding and alighting at each stop. While APC data is not strictly required for a basic GTFS schedule feed, it is essential for transit planning, capacity management, and for extensions such as GTFS-ride, which standardizes the representation of ridership data.

**Field Surveys and Manual Data Collection** remain an important complement to automated systems. Trained field personnel verify stop locations, confirm the physical accessibility of boarding facilities, and document the precise pedestrian right-of-way where passengers board. This is critical because GTFS Best Practices specify that stop locations should have an error of no more than four meters compared to the actual stop position, and should be placed very near to the pedestrian right of way [4]. Manual surveys are also used to document stop amenities, shelter availability, and parent-child station relationships.

**Automated Fare Collection (AFC) and Smart Card Systems** generate valuable data about passenger journeys, including origin-destination pairs and transfer patterns. While this data is not directly used in static GTFS, it informs the creation of accurate fare rules and attributes within `fare_attributes.txt` and `fare_rules.txt`, and is increasingly relevant for GTFS-Fares V2 implementation.

**Scheduling Software** is the operational backbone of most transit agencies. Systems like HASTUS, Trapeze, or Optibus are used to create and manage vehicle blocks, crew assignments, and timetables. These systems are typically the primary source of truth for the schedule data that eventually populates `trips.txt`, `stop_times.txt`, and `calendar.txt`. Establishing a reliable, automated data pipeline from the scheduling software to the GTFS export process is a key challenge for many agencies.

The following table summarizes the primary data collection technologies and their corresponding GTFS files:

| Technology | Primary Data Collected | Relevant GTFS Files |
| :--- | :--- | :--- |
| AVL (GPS) | Vehicle positions, route alignments | `shapes.txt`, `stop_times.txt` |
| APC | Passenger boardings and alightings | Supports `stop_times.txt` validation |
| Field Surveys | Stop coordinates, accessibility features | `stops.txt` |
| AFC / Smart Cards | Fare payment, journey patterns | `fare_attributes.txt`, `fare_rules.txt` |
| Scheduling Software | Timetables, trip blocks, service calendars | `trips.txt`, `stop_times.txt`, `calendar.txt` |

## 1.2 Data Storing

The way transit data is stored has a profound impact on the quality, consistency, and maintainability of the resulting GTFS feed. Historically, many transit agencies managed their schedule and stop data using legacy storage methods such as standalone CSV files or Microsoft Excel (XLSX) spreadsheets. While these methods are accessible and require no specialized infrastructure, they present significant and well-documented challenges when dealing with complex, interconnected transit networks.

The fundamental problem with flat file storage is the absence of enforced relationships between data entities. In a spreadsheet, nothing prevents a planner from entering a `route_id` in a trips file that does not correspond to any existing route, or from accidentally duplicating a `stop_id` with slightly different coordinates. These inconsistencies are difficult to detect manually and can propagate silently through the data pipeline, resulting in a GTFS feed that fails validation or, worse, contains subtle errors that mislead riders.

**Migrating to a centralized relational database** — such as PostgreSQL, MySQL, or Microsoft SQL Server — is strongly recommended for any agency managing a non-trivial transit network. A relational database offers substantial and measurable benefits over flat file storage:

| Feature | Legacy Storage (CSV/XLSX) | Relational Database (SQL) |
| :--- | :--- | :--- |
| **Data Consistency** | Prone to human error and duplicate entries. No enforcement of referential integrity. | Enforced through primary and foreign key constraints, preventing orphaned records. |
| **Version Control** | Difficult to track changes across multiple files. Requires manual file naming conventions. | Supports robust versioning, audit logs, and historical tracking via database triggers or tools like Liquibase. |
| **Collaboration** | Single-user access or cumbersome file sharing via email or shared drives. | Concurrent multi-user access for large teams of planners, with role-based permissions. |
| **Querying** | Limited filtering and complex data joining requires manual VLOOKUP or pivot tables. | Powerful SQL querying for complex analysis, including joins across multiple entities. |
| **Scalability** | Performance degrades rapidly with large datasets (e.g., millions of stop times). | Designed to handle large datasets efficiently with indexing and query optimization. |
| **Data Integrity** | No enforcement of data types or null constraints. | Strict enforcement of data types, null constraints, and custom check constraints. |

When large teams are working with transit data — including route planners, GIS specialists, scheduling analysts, and IT staff — a centralized database ensures that everyone is always accessing the same, authoritative "single source of truth." It eliminates the common and costly problem of disparate spreadsheets containing conflicting information about route alignments, stop coordinates, or service calendars. The investment in migrating to a relational database pays dividends not only in data quality but also in the efficiency of the GTFS export process.

## 1.3 Data Structure

To serve the GTFS standard optimally, the underlying data must be structured to align with the inherently relational nature of the specification. GTFS is, at its core, a relational database exported into a series of comma-separated text files. Understanding this structure is essential for designing an internal database schema that facilitates accurate and efficient GTFS production.

The core relational hierarchy of GTFS can be described as follows: an **Agency** operates one or more **Routes**. Each Route consists of one or more **Trips** (specific instances of a vehicle traveling a route on a given day). Each Trip is associated with a **Service Calendar** (defining the days of operation) and a sequence of **Stop Times** (the scheduled arrival and departure at each **Stop**). Routes may also have associated **Shapes** (geographic alignments) and **Fares**.

When creating database schemas for transit data, it is crucial to implement the following relational constraints:

**Primary Keys** must be defined for every entity table. For example, `stops` has `stop_id` as its primary key, `routes` has `route_id`, and `trips` has `trip_id`. These keys must be unique and stable across data iterations whenever possible [4].

**Foreign Keys** enforce referential integrity between related tables. Critical foreign key relationships include:
- `trips.route_id` → `routes.route_id`
- `trips.service_id` → `calendar.service_id`
- `stop_times.trip_id` → `trips.trip_id`
- `stop_times.stop_id` → `stops.stop_id`

**Null Validations** ensure that required fields are never left empty. For instance, `stops.stop_lat` and `stops.stop_lon` must always contain valid decimal degree values. Similarly, `stop_times.arrival_time` and `stop_times.departure_time` should specify time values whenever possible, including non-binding estimated or interpolated times between timepoints [4].

**Data Type Enforcement** prevents invalid values from entering the database. The `direction_id` field, for example, should be constrained to only accept the integer values 0 or 1. The `route_type` field should be constrained to a defined set of valid integer codes. Geographic coordinate fields should be stored as `DECIMAL` or `FLOAT` types with sufficient precision.

**Check Constraints** can enforce business rules beyond simple data types. For example, a constraint can ensure that `stop_times.departure_time` is always greater than or equal to `stop_times.arrival_time` for a given stop, or that `stop_times.stop_sequence` values within a trip are strictly increasing.

The following table illustrates the core GTFS files and their corresponding database table design considerations:

| GTFS File | Primary Key | Key Foreign Keys | Critical Not-Null Fields |
| :--- | :--- | :--- | :--- |
| `agency.txt` | `agency_id` | — | `agency_name`, `agency_url`, `agency_timezone` |
| `stops.txt` | `stop_id` | `parent_station` → `stop_id` | `stop_name`, `stop_lat`, `stop_lon` |
| `routes.txt` | `route_id` | `agency_id` → `agency.agency_id` | `route_type` |
| `trips.txt` | `trip_id` | `route_id`, `service_id` | `route_id`, `service_id` |
| `stop_times.txt` | (`trip_id`, `stop_sequence`) | `trip_id`, `stop_id` | `trip_id`, `stop_id`, `stop_sequence` |
| `calendar.txt` | `service_id` | — | All day-of-week fields, `start_date`, `end_date` |
| `shapes.txt` | (`shape_id`, `shape_pt_sequence`) | — | `shape_pt_lat`, `shape_pt_lon` |

Structuring the internal database to closely mirror the GTFS specification in this way greatly simplifies the export process and ensures compliance with GTFS Best Practices [4]. It also makes the database schema self-documenting for new team members, as the structure directly corresponds to the well-documented GTFS reference.

## 1.4 Data Management

Managing transit data over time is an ongoing operational challenge. Transit networks are not static: routes change, stops are added or removed, service frequencies are adjusted seasonally, and special service is operated for holidays and events. Robust data management practices are essential to ensure that the GTFS feed always accurately reflects the current and upcoming state of the transit network.

**Maintaining Persistent Identifiers** is one of the most important best practices for long-term data management. The `stop_id`, `route_id`, and `agency_id` fields should remain consistent across successive data iterations whenever possible [4]. Changing these identifiers can break integrations with downstream consumers, invalidate GTFS Realtime feeds that reference static GTFS IDs, and confuse analytics pipelines that track performance over time.

**Calendar and Service Management** requires careful attention. The `calendar.txt` file defines regular weekly service patterns, while `calendar_dates.txt` is used to describe exceptions such as holidays, special events, or emergency service changes. Best practice is to include both files for human readability and to ensure that expired or unused `service_id` values are removed from the feed [5]. Descriptive `service_id` values (e.g., `WEEKDAY_SUMMER_2025` rather than `SVC_001`) significantly improve the readability and maintainability of the data [5].

**Recommended Software and Architectural Patterns** for efficient transit data management include:

- **Dedicated Database Server:** A dedicated PostgreSQL or MySQL server, separate from application servers, provides the performance, reliability, and security required for production transit data. Cloud-managed database services (e.g., Amazon RDS, Google Cloud SQL) offer additional benefits in terms of automated backups, high availability, and scalability.

- **GTFS Data Management Platforms:** Commercial and open-source platforms such as IBI Group's TRANSIT Data Tools (GTFS Data Manager and GTFS Editor), Moovit's Transit Data Manager, and Arcadis's TRANSIT Data Suite provide graphical interfaces for editing GTFS feeds, managing feed versions, and running validation checks. These tools are particularly valuable for agencies that do not have dedicated software development resources.

- **Canonical GTFS Validator:** MobilityData's open-source Canonical GTFS Validator is the standard tool for checking the compliance of any GTFS dataset with the specification before publication. Transit App explicitly recommends using this validator before releasing any dataset [5]. It checks for both errors (which would cause a dataset to be rejected by consumers) and warnings (which indicate potential data quality issues).

- **Version Control for Data:** While a relational database handles concurrent access, maintaining a version history of published GTFS feeds is also important. Storing archived ZIP files of each published feed, along with metadata about the publication date and the service period covered, enables agencies to audit historical data and roll back to a previous version if a newly published feed contains errors.

- **Monitoring and Alerting:** Automated monitoring of the published GTFS feed URL ensures that the feed remains accessible and up-to-date. Transit App, for example, automatically fetches new datasets published at a permanent URL within 4 hours [5]. Agencies should monitor their feed URL to ensure it remains stable and that the feed does not expire without a replacement being published.

## 1.5 Exporting Data

The final step in the transit data lifecycle is exporting the internal database records into the strict GTFS format: a ZIP archive containing a specific set of `.txt` files, each formatted as a comma-separated values (CSV) file with a defined header row. This step is where the quality of the upstream data management processes is ultimately validated.

**The Export Process** typically involves running a series of SQL queries against the relational database to extract the relevant data, transforming it into the exact format required by the GTFS specification (including correct field names, data types, and encoding), and packaging the resulting files into a single ZIP archive. This process should be fully automated to minimize the risk of human error and to ensure that the published feed is always current.

**Common Approaches to Extraction** vary by agency size and technical maturity:

1. **Custom ETL Scripts:** Many technically mature agencies develop custom Extract-Transform-Load (ETL) scripts, typically written in Python or Node.js, that query the database, apply any necessary transformations (e.g., formatting times in `HH:MM:SS` format, converting coordinate precision), and write the output files. Python libraries such as `partridge`, `gtfslib-python`, and `Make GTFS` can simplify this process. The `multigtfs` Django application provides a framework for importing and exporting GTFS within a web application context.

2. **Scheduling Software Integration:** Many commercial transit scheduling platforms (e.g., HASTUS, Trapeze) have built-in GTFS export functionality. However, the quality of these exports varies, and they may require post-processing to conform to GTFS Best Practices. Agencies should validate the output of any scheduling software export using the Canonical GTFS Validator before publication [5].

3. **Dedicated GTFS Management Platforms:** Platforms such as Arcadis's TRANSIT Data Suite and Moovit's Transit Data Manager provide end-to-end GTFS management, including a built-in export function that generates a validated, compliant GTFS feed directly from the platform's internal data model.

**Update Frequency and Seasonality** are important considerations for GTFS publication. The frequency of updates varies by agency but is typically tied to seasonal schedule changes, which may occur two to four times per year (e.g., Summer, Fall, Winter, and Spring service changes). However, GTFS Best Practices and Transit App guidelines both specify that new datasets must be published **at least 7 days in advance** of any new schedules taking effect [4] [5]. This lead time ensures that:

- Downstream consumers (trip planners, mobile apps) have sufficient time to ingest and process the new data.
- Riders can plan future trips using accurate information.
- Data quality issues can be identified and corrected before the new service begins.

For minor schedule revisions, updates may be processed on a more frequent basis, sometimes on the same day. In these cases, the GTFS Best Practices recommend expressing imminent service changes (within 7 days) through a GTFS Realtime feed rather than a static GTFS update [4].

**Publication Best Practices** dictate that the GTFS dataset should be published at a permanent, public URL that does not change between iterations [4] [5]. A good example of this practice is the STM (Société de transport de Montréal), which publishes its GTFS at a stable URL that is never modified, even when the content is updated [5]. The published dataset should be valid for at least the next 7 days, and preferably for 30 days or the entire service period [4] [5]. Expired calendars and unused `service_id` values should be removed from the feed before publication [5].

The following diagram illustrates the complete data lifecycle from raw collection to GTFS publication:

```
[Field Data Collection]
  AVL / APC / Field Surveys / AFC
          │
          ▼
[Scheduling Software]
  HASTUS / Trapeze / Optibus
          │
          ▼
[Centralized Relational Database]
  PostgreSQL / MySQL
  (with FK constraints, null validations, data types)
          │
          ▼
[ETL / Export Pipeline]
  Python / Node.js / Platform Export
          │
          ▼
[Canonical GTFS Validator]
  MobilityData Validator
          │
          ▼
[Published GTFS Feed]
  Permanent Public URL (gtfs.zip)
          │
          ▼
[Downstream Consumers]
  Google Maps / Transit App / Bing Maps / Custom Apps
```

By following this end-to-end process — from rigorous field data collection, through disciplined database management, to automated and validated GTFS export — transit agencies can ensure that their published feeds meet the quality standards expected by downstream consumers and, most importantly, provide riders with accurate and reliable transit information.

---

# 2. Repository Overview

This repository is organised as a small, dependency-light Python package named **PID-GTFS**, located under `PID-GTFS/`. The package provides a complete pipeline for working with GTFS data: validating raw records, persisting them in a referentially-consistent SQLite database, ingesting official GTFS ZIP feeds, exporting standards-compliant feeds back out, and checking the quality of those feeds with a pure-Python validator. The goal of the package is to embody the end-to-end data lifecycle described in Section 1 as a reusable library that any transit agency, app developer, or researcher can drop into their own project.

The package layout is deliberately flat and consists of five modules under `src/pid_gfts/` plus a test suite under `tests/`:

```
PID-GTFS/
├── pyproject.toml
├── README.md
└── src/pid_gtfs/
    ├── __init__.py        # public API surface and top-level aliases
    ├── schemas.py         # Pydantic v2 models for every GTFS table
    ├── database.py        # SQLite persistence layer (GtfsDatabase)
    ├── importer.py        # GTFS .zip → GtfsDatabase pipeline
    ├── exporter.py        # GtfsDatabase → GTFS .zip pipeline
    └── validator.py       # Pure-Python GTFS feed validator
```

The sub-sections below describe each module, why it matters within the broader GTFS lifecycle, and the main methods it exposes.

## 2.1 `schemas.py` — Typed GTFS Models

**Purpose.** This module defines Pydantic v2 models that mirror every standard GTFS table — plus the GTFS-ride extension — field-for-field. Each model encodes the data types, enumerations, and constraints documented in the GTFS specification, so that any record reaching the database has already been type-checked, range-checked, and cleaned of stray whitespace or empty strings.

**Importance.** Schemas are the first line of defence described in Section 1.3 (Data Structure). Without a typed layer, invalid values such as a `route_type` outside the permitted integer codes, a `direction_id` other than 0 or 1, or a malformed `HH:MM:SS` time can silently enter the database and propagate into the exported feed. By expressing these rules once, in Python, the package guarantees that every downstream operation (import, upsert, export) sees only well-formed data.

**Main exports.**
- `GTFS_TABLE_MODELS` — a `dict` mapping each table name (`"agency"`, `"stops"`, `"routes"`, `"trips"`, `"stop_times"`, `"calendar"`, `"calendar_dates"`, `"shapes"`, `"frequencies"`, `"transfers"`, `"feed_info"`, plus the GTFS-ride tables) to its corresponding Pydantic model class. This is the single source of truth used by the importer and the database layer.
- Model classes: `GtfsAgency`, `GtfsStop`, `GtfsRoute`, `GtfsTrip`, `GtfsStopTime`, `GtfsCalendar`, `GtfsCalendarDate`, `GtfsShape`, `GtfsFrequency`, `GtfsTransfer`, `GtfsFeedInfo`, `GtfsBoardAlight`, `GtfsRidership`, `GtfsRideFeedInfo`, `GtfsTripCapacity`.
- Enums: `LocationType`, `RouteType`, `DirectionId`, `WheelchairAccessible`, `BikesAllowed`, `PickupDropOffType`, `ContinuousPickupDropOff` — integer-coded to match the values accepted by the GTFS spec.

## 2.2 `database.py` — Persistence Layer (`GtfsDatabase`)

**Purpose.** A thin wrapper around a single SQLite file that mirrors GTFS with full referential integrity. The schema is created on demand, foreign keys are enforced (`PRAGMA foreign_keys = ON`), primary keys are defined for every table, and deletions happen in a foreign-key-safe order. The caller owns the path — the module has no knowledge of any particular project layout or data-lake convention.

**Importance.** This module is the concrete realisation of the "centralized relational database" recommended in Section 1.2. Storing GTFS data in a structured, constraint-enforced store — rather than in loose CSV or XLSX files — is what prevents orphaned `trip_id` references, duplicate `stop_id` entries, and the many other subtle inconsistencies that flat files allow. The module also provides an integrity-checking routine that reports violations, giving agencies an objective measure of data quality before the feed is exported.

**Main methods.**
- `GtfsDatabase(db_path)` — open (or create) a GTFS database at the given path; schema is created idempotently.
- `GtfsDatabase.from_slug(slug, root=None)` — convenience constructor that places the file at `<root>/<slug>.db`.
- `connect()` — open a raw `sqlite3.Connection` with FK enforcement enabled.
- `upsert(table_name, records)` — validate each record through the matching Pydantic model and insert-or-replace in a single transaction; returns an `InsertResult` with counts and error messages.
- `get_records(table_name, limit, offset)` — paginated read.
- `count(table_name)` — row count for a single table.
- `delete(table_name, ids)` — remove records by primary key (supporting composite keys via `"a|b"` notation).
- `clear(table_name)` / `clear_all()` — wipe a single table or the whole database in FK-safe order.
- `check_integrity()` — scan for orphaned foreign keys and return an `IntegrityReport`.
- `summary()` — dictionary with row counts, file size, schema version, timestamps, and integrity status.
- Module-level helper `upsert_records_on_conn(conn, table_name, records)` — used by the importer to batch many upserts inside one connection-scoped transaction.

## 2.3 `importer.py` — GTFS ZIP Ingestion

**Purpose.** Accepts a complete GTFS `.zip` feed (from a path on disk or a file-like object) and populates a `GtfsDatabase`. Members are never extracted to disk; each file is streamed via `ZipFile.open` and parsed with `csv.DictReader`. Zip-bomb and size guards run before any CSV parsing: archives larger than 500 MB compressed, individual members larger than 1 GB uncompressed, or archives with more than 50 members are rejected outright.

**Importance.** Reliable ingestion is essential both for agencies that receive feeds from upstream systems (scheduling software, contractors, aggregated national feeds) and for analysts who need to query third-party GTFS data. The importer gives callers explicit control over how new data should interact with existing data via four import modes, so it can serve both initial-load and incremental-update workflows.

**Main API.**
- `preview_gtfs_zip(source) -> GtfsZipPreview` — inspect an archive without writing anything: which tables are present, which are missing, row counts, and unknown files.
- `import_gtfs_zip(db, source, *, mode=ImportMode.REPLACE, chunk_size=5000) -> GtfsImportResult` — perform the import. Tables are loaded in FK-safe order (`agency → feed_info → calendar → … → stop_times → GTFS-ride tables`).
- `ImportMode` — `REPLACE` (wipe then load), `MERGE` (insert-or-replace over existing data), `MERGE_PARTIAL` (like `MERGE` but accepts archives that don't carry every required file, for incremental updates), `ABORT_IF_NOT_EMPTY` (safety mode that refuses to write over non-empty databases).
- `GtfsImportResult` — per-table counts (`inserted_by_table`, `failed_by_table`), cleared/skipped tables, capped error messages, and total duration.
- `GtfsImportError` — raised for non-recoverable failures (bad zip, guard-rail breach, missing required files).

## 2.4 `exporter.py` — GTFS Feed Publication

**Purpose.** Reads from a `GtfsDatabase` and produces a standards-compliant `.zip` archive of `.txt` CSV files ready for publication. The exporter writes UTF-8 content with CRLF line endings and minimal quoting (matching GTFS conventions), omits columns that are entirely null, and emits files in the canonical GTFS order. A pre-export validation step and a 0–100 feed-completeness score are included.

**Importance.** Exporting is the final step of the data lifecycle described in Section 1.5. The exporter enforces the rules that make the feed usable by downstream consumers: required tables must be populated, at least one of `calendar`/`calendar_dates` must have records, and the output is written atomically to a single file. The completeness score provides an objective, weighted summary (required 60% / recommended 25% / optional & GTFS-ride 15%) that agencies can track over time as a data-quality KPI.

**Main API.**
- `validate_before_export(db) -> ValidationResult` — block export if any required table is empty; surface warnings for missing recommended tables (`feed_info`, `shapes`).
- `compute_feed_completeness(db) -> FeedCompleteness` — weighted score plus per-table breakdown.
- `export_gtfs_feed(db, out_path, *, include_ride=True, validate=True) -> ExportResult` — write the `.zip` file; returns the path, the list of included files, the total record count, the completeness score, and any warnings.

## 2.5 `validator.py` — Pure-Python Feed Validator

**Purpose.** A JVM-free alternative to the official MobilityData Canonical GTFS Validator. It reads an exported GTFS `.zip` (no database required) and runs a battery of spec checks: required file presence, required field presence, primary-key uniqueness, referential integrity (`trips ↔ routes`, `trips ↔ service`, `stop_times ↔ trips`, `stop_times ↔ stops`), time/date/coordinate/color format checks, logical checks such as `start_date ≤ end_date` and strictly-increasing `stop_sequence` per trip, and `route_type` validation.

**Importance.** As described in Section 1.4, validation before publication is a non-negotiable best practice. Bundling a lightweight Python validator directly in the package means this check can be run as part of automated CI pipelines, pre-commit hooks, or ad-hoc Python scripts, without needing to install and invoke a separate Java tool. It complements — it does not replace — the Canonical Validator for final certification before publication.

**Main API.**
- `validate_gfts_feed(zip_path) -> GtfsValidationReport` — run every check and return a structured report.
- `GtfsValidationReport` — aggregate with `is_valid`, `error_count`, `warning_count`, `info_count`, and a list of `GtfsValidationIssue` entries (severity, file, field, row, message).

## 2.6 Tests (`tests/`)

The test suite under `PID-GTFS/tests/` exercises the three main moving parts of the package:

- `test_schemas.py` — round-trip validation of each Pydantic model, covering required fields, enum values, and type coercion.
- `test_database.py` — upsert / delete / clear / integrity-check behaviour, including FK enforcement and the `InsertResult` / `IntegrityReport` contracts.
- `test_importer_exporter.py` — end-to-end pipeline tests: import a GTFS `.zip`, verify per-table counts, export it back out, and assert the resulting archive is byte-compatible with the expectations of the validator and downstream consumers.
- `conftest.py` — shared fixtures that build temporary GTFS archives and `GtfsDatabase` instances on a per-test basis.

## 2.7 Public API Summary

All the symbols most users need are re-exported from the package root (`pid_gtfs/__init__.py`) so that typical usage is a single import:

```python
from pid_gtfs import (
    GtfsDatabase,
    import_gtfs_zip, preview_gtfs_zip, ImportMode,
    export_gtfs_feed, validate_before_export, compute_feed_completeness,
)

db = GtfsDatabase("feeds/agency_x.db")
preview = preview_gtfs_zip("feeds/agency_x.zip")
import_gtfs_zip(db, "feeds/agency_x.zip", mode=ImportMode.REPLACE)
export_gtfs_feed(db, "out/agency_x_gtfs.zip")
```

The top-level aliases `preview_zip`, `import_zip`, and `export_zip` are also provided for brevity.

---

# 3. Building a GTFS Feed from Scratch

## 3.1 Introduction
GTFS (General Transit Feed Specification) is the global standard for public transportation schedules and geographic information. A GTFS feed is fundamentally a relational database exported as a series of comma-separated values (CSV) files contained within a .zip archive.

Standardizing transit data into this format is essential for everything from routing engines (like Google Maps) to feeding clean, structured inputs into long-term forecasting models.

A valid GTFS feed requires a minimum of 6 text files:

*   `agency.txt` - Your transit operator details.
*   `stops.txt` - The geographic positions of your stops.
*   `routes.txt` - A list of your bus/train routes.
*   `calendar.txt` - The operational days and service windows.
*   `trips.txt` - Specific journeys made on your routes (linking routes to schedules).
*   `stop_times.txt` - The arrival and departure times at each stop for each specific trip.

## 3.2 Practical Example: Two Intersecting Routes
Let's imagine your transit agency is OptiBus. You operate two routes:

*   **Route 1**: The "Downtown Loop" (Runs Monday–Friday).
*   **Route 2**: The "University Express" (Runs Weekends).

Here is how you map this multi-route system into the 6 required GTFS files.

### Step 1: Define Your Agency (`agency.txt`)
This file establishes the operator. If your feed contains data from multiple transit companies, you would list all of them here and use the `agency_id` to link routes to their specific operators.

```csv
agency_id,agency_name,agency_url,agency_timezone
optibus,OptiBus,https://www.optibus.example.com,Europe/Lisbon
```

### Step 2: Define Your Stops (`stops.txt`)
This file acts as your spatial nodes. We map them using latitude and longitude. Let's define three stops. Stop A will act as a transfer hub used by both routes.

```csv
stop_id,stop_name,stop_lat,stop_lon
STOP_A,Downtown Transfer Hub,41.442530,-8.291770
STOP_B,South Commercial District,41.432530,-8.295770
STOP_C,University Campus,41.453530,-8.281770
```

### Step 3: Define Your Routes (`routes.txt`)
This file catalogs your lines. The `route_type` is crucial for routing engines to know the mode of transport (3 represents a standard bus, 0 is a tram, 2 is rail).

```csv
route_id,agency_id,route_short_name,route_long_name,route_type
R_1,optibus,1,Downtown Loop,3
R_2,optibus,2,University Express,3
```

### Step 4: Define Service Days (`calendar.txt`)
Instead of hardcoding dates into trips, GTFS uses "service IDs" as a scheduling abstraction. This makes it easy to apply broad operational rules (e.g., a standard weekday schedule vs. a weekend schedule) across thousands of trips.

```csv
service_id,monday,tuesday,wednesday,thursday,friday,saturday,sunday,start_date,end_date
WEEKDAY_SERVICE,1,1,1,1,1,0,0,20260101,20261231
WEEKEND_SERVICE,0,0,0,0,0,1,1,20260101,20261231
```
> [!NOTE]
> 1 means active, and 0 means inactive. The dates format is YYYYMMDD. This allows you to plan seasonal route changes well in advance.

### Step 5: Define Your Trips (`trips.txt`)
A route is just an abstract concept; a trip is a physical vehicle executing that route at a specific time. You must generate a unique `trip_id` for every single run.

```csv
route_id,service_id,trip_id
R_1,WEEKDAY_SERVICE,TRIP_R1_0800
R_1,WEEKDAY_SERVICE,TRIP_R1_0900
R_2,WEEKEND_SERVICE,TRIP_R2_1000
```

### Step 6: Define Your Stop Times (`stop_times.txt`)
This is the heaviest file in the feed. It maps the timeline of every stop on every trip. Notice how the trips navigate through the stops defined in Step 2.

```csv
trip_id,arrival_time,departure_time,stop_id,stop_sequence
TRIP_R1_0800,08:00:00,08:00:00,STOP_A,1
TRIP_R1_0800,08:15:00,08:15:00,STOP_B,2
TRIP_R1_0900,09:00:00,09:00:00,STOP_A,1
TRIP_R1_0900,09:15:00,09:15:00,STOP_B,2
TRIP_R2_1000,10:00:00,10:00:00,STOP_C,1
TRIP_R2_1000,10:30:00,10:30:00,STOP_A,2
```
> [!IMPORTANT]
> The `stop_sequence` guarantees the strict topological ordering of the route, which is vital for routing algorithms. Arrival and departure times can differ if you want to model a vehicle laying over at a transit hub.

## 3.3 Adding Ridership Data: The GTFS-ride Extension
While standard GTFS dictates where and when your transit network operates, it does not record how many people actually use it. To bridge the gap between scheduled service and actual human demand—which is an essential step when feeding historical data into time-series prediction and demand forecasting pipelines—you can use the GTFS-ride extension.

GTFS-ride is an open data standard designed specifically for storing, sharing, and analyzing fixed-route transit ridership data. It functions as an add-on to your standard GTFS feed, utilizing the exact same `trip_id` and `stop_id` keys to link passenger counts directly to your operational schedules.

### Required and Optional Files
To implement GTFS-ride, you generate additional CSV text files and include them in the same `.zip` archive as your standard GTFS files.

*   `ride_feed_info.txt` **(Required)**: Provides metadata specific to the source and attributes of your supplementary ridership files.
*   `board_alight.txt` **(Optional)**: Tracks boardings and alightings (passengers getting on and off) at the individual stop-level.
*   `trip_capacity.txt` **(Optional)**: Identifies the maximum passenger capacities of the specific vehicles used for service.
*   `ridership.txt` **(Optional)**: Supplies ridership counts aggregated at various levels.
*   `rider_trip.txt` **(Optional)**: Contains anonymized data regarding specific individual rider trips.

### Practical Example: `board_alight.txt`
Let's assume you have Automated Passenger Counter (APC) data from the 8:00 AM "Downtown Loop" trip (`TRIP_R1_0800`) established in Step 5 of the main guide. You recorded that 15 people boarded at Stop A, and 5 got off at Stop B.

#### 1. Define the Feed Metadata (`ride_feed_info.txt`)
```csv
ride_files,ride_start_date,ride_end_date,gtfs_ride_version
6,20260101,20261231,v1.2
```

#### 2. Map the Boardings and Alightings (`board_alight.txt`)
Notice how seamlessly the transit usage maps against your established spatial and schedule data.
```csv
trip_id,stop_id,stop_sequence,record_use,schedule_relationship,boardings,alightings,current_load
TRIP_R1_0800,STOP_A,1,0,0,15,0,15
TRIP_R1_0800,STOP_B,2,0,0,2,5,12
```
*   **boardings**: The number of passengers entering the vehicle at the stop.
*   **alightings**: The number of passengers exiting the vehicle at the stop.
*   **current_load**: A field calculating either the percentage or integer count of the passenger load at that specific stop.

### Data Engineering Integration
From an engineering perspective, standardizing your collection around GTFS-ride creates a robust, highly structured data standard perfect for long-term strategic planning.

In your ETL pipeline, you would extract raw APC sensor data, transform it using Pandas to join against your internal transit schedules, and load the aggregated passenger flows into `board_alight.txt`. This resulting dataset serves as the ideal, clean historical context required to train machine learning models and MLOps architectures for predicting future urban transit demand.

## 3.4 Assembly and Validation
*   **Directory Setup**: Create an isolated directory (e.g., `gtfs_export_v1`).
*   **File Generation**: Save the datasets above with their exact `.txt` filenames.
*   **Encoding**: Ensure all files are strictly formatted as CSVs with **UTF-8 encoding** (this prevents character corruption in stop names).
*   **Compression**: Select only the text files and compress them into `gtfs.zip`. Do not zip the parent folder, or feed validators will fail to read the root directory.

## 3.5 Engineering Implementation & ETL Automation
Manually typing GTFS files is impossible at scale. In a modern data engineering context, generating a GTFS feed is typically handled as the final loading step of an automated ETL pipeline:

*   **Extract**: Pull the raw operational data from an upstream relational database (e.g., PostgreSQL/PostGIS) or an external API hub.
*   **Transform (Python/Pandas)**: 
    *   Clean and normalize the raw data.
    *   Use Pandas DataFrames to perform table joins and map internal primary keys to GTFS-compliant string formats (e.g., `agency_id`, `stop_id`).
    *   Generate the strict topologies required for the `stop_sequence` column.
*   **Load (Export)**: Export the DataFrames using `df.to_csv(filename, index=False)`.
*   **Archive & Distribute**: 
    *   Utilize Python's built-in `zipfile` module to package the generated `.txt` files into an in-memory zip archive (using `io.BytesIO`).
    *   Serve the resulting `.zip` directly via an API endpoint (e.g., a FastAPI route) or upload it to a cloud storage bucket for consumption by downstream analytics applications. 

---

## References

[1] MobilityData. "About - General Transit Feed Specification." *GTFS.org*. [https://gtfs.org/about/](https://gtfs.org/about/)

[2] MobilityData. "Why use GTFS? - General Transit Feed Specification." *GTFS.org*. [https://gtfs.org/getting-started/why-use-gtfs/](https://gtfs.org/getting-started/why-use-gtfs/)

[3] "European transport feeds." *EU National Access Points Feed Registry*. [https://eu.data.public-transport.earth/](https://eu.data.public-transport.earth/)

[4] MobilityData. "GTFS Schedule Best Practices." *GTFS.org*. [https://gtfs.org/documentation/schedule/schedule-best-practices/](https://gtfs.org/documentation/schedule/schedule-best-practices/)

[5] Transit App. "Guidelines for Producing GTFS Static Data for Transit." *Transit Partners Resources*. [https://resources.transitapp.com/article/458-guidelines-for-producing-gtfs-static-data-for-transit](https://resources.transitapp.com/article/458-guidelines-for-producing-gtfs-static-data-for-transit)
