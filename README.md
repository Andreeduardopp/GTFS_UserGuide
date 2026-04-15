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

## References

[1] MobilityData. "About - General Transit Feed Specification." *GTFS.org*. [https://gtfs.org/about/](https://gtfs.org/about/)

[2] MobilityData. "Why use GTFS? - General Transit Feed Specification." *GTFS.org*. [https://gtfs.org/getting-started/why-use-gtfs/](https://gtfs.org/getting-started/why-use-gtfs/)

[3] "European transport feeds." *EU National Access Points Feed Registry*. [https://eu.data.public-transport.earth/](https://eu.data.public-transport.earth/)

[4] MobilityData. "GTFS Schedule Best Practices." *GTFS.org*. [https://gtfs.org/documentation/schedule/schedule-best-practices/](https://gtfs.org/documentation/schedule/schedule-best-practices/)

[5] Transit App. "Guidelines for Producing GTFS Static Data for Transit." *Transit Partners Resources*. [https://resources.transitapp.com/article/458-guidelines-for-producing-gtfs-static-data-for-transit](https://resources.transitapp.com/article/458-guidelines-for-producing-gtfs-static-data-for-transit)
