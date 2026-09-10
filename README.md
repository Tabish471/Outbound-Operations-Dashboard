


**Prompt**

Create a single self-contained HTML file for a professional, interactive logistics operations analytics dashboard called **“Outbound Operations — Ground Team View.”**

The application must run locally by simply opening the `.html` file in a browser.

## 1. Dashboard Objective

Build a decision-support dashboard for analysing outbound shipment operations. The dashboard should help ground/operations teams understand:

* Overall shipment volume
* Critical and high-priority shipments
* SLA-risk shipments
* Shipment ageing
* NTC-wise shipment distribution and status
* Putaway pending backlog
* Long-pending shipments
* Bag-level shipment details
* Destination/connection availability
* Vehicle modes and route cutoffs
* Client allocation and data quality

The dashboard should focus on **operational exceptions and actionable numbers rather than charts**.

The application should feel like a professional internal logistics control tower rather than a generic analytics dashboard.

## 2. Technology & Architecture

Build the entire application as one standalone HTML file containing:

* HTML
* CSS
* Vanilla JavaScript
* Client-side CSV parsing using Papa Parse via CDN
* No backend
* No database
* No build process
* No installation requirement

All operational data must be processed locally in the browser.

The dashboard must work by simply opening the HTML file in Chrome/Edge.

## 3. Visual Design

Use a professional enterprise logistics/operations theme:

* Dark navy header
* White content cards
* Light grey page background
* Blue primary accent
* Red for critical/P0 issues
* Orange/amber for warnings/P1
* Yellow for moderate attention
* Green for normal/healthy status
* Rounded cards
* Subtle borders and shadows
* Compact operational tables
* Clear hierarchy between KPIs, filters and detailed data

Use:

* Inter for normal UI text
* Space Grotesk for headings and KPI values
* IBM Plex Mono for timestamps and numerical operational values

Use a maximum content width of approximately 1400–1450px.

Make the interface responsive for desktop, tablet and smaller screens.

## 4. Header

Create a dark navy header containing:

**Outbound Operations — Ground Team View**

Subtitle:

**Shipment ageing, NTC status, put-away pending & bag-level search — no charts, straight to the numbers**

Display operational metadata such as:

* Data source
* Number of records loaded
* Last refreshed timestamp

Provide:

**Upload new CSV (refresh data)**

When a new CSV is uploaded:

* Parse the file in the browser
* Validate required columns
* Replace the current dataset
* Recalculate all metrics
* Refresh every dashboard section
* Display the upload timestamp
* Show success/error notifications
* Report invalid/skipped rows
* Detect duplicate Bag IDs

## 5. Outbound Data Upload

Allow users to upload an outbound shipment CSV.

Use Papa Parse for client-side parsing.

The application should validate the uploaded file and gracefully handle:

* Missing columns
* Empty files
* Invalid rows
* Invalid values
* Duplicate Bag IDs

Show a user-friendly toast message after processing.

Example:

**Loaded X bags from filename.csv**

If rows were skipped:

**X row(s) skipped**

If duplicates exist:

**X duplicate Bag IDs**

Do not reload the browser after upload.

## 6. Global Filters

Create a filter area below the header.

All dashboard modules must respect these filters.

### Priority

Create multi-select chips:

* P0
* P1
* P2
* P3

### Status

Create dynamically populated status chips.

Expected statuses:

* expected
* yard
* dock
* in_center

### MOT

Create dynamically populated MOT chips.

### NTC

Dropdown:

* All NTCs
* Dynamically populated NTCs

### Product Type

Dropdown:

* All Product Types
* Dynamically populated product types

### Client

Provide a text search field:

**Search client…**

### Reset Filters

Provide a button to restore all filters to their default state.

Filtering must happen instantly without page reload.

## 7. Snapshot KPI Section

Create a section titled:

**Snapshot**

Display six KPI cards.

### KPI 1 — Total Bags

Show:

* Number of bags in current filtered view
* Percentage/count context where useful

### KPI 2 — P0 Critical

Count all shipments with:

`Priority = P0`

Display percentage of filtered shipments.

### KPI 3 — P1 High

Count all shipments with:

`Priority = P1`

Display percentage of filtered shipments.

### KPI 4 — SLA-Risk Bags

Count shipments where:

`Priority = P0 OR P1`

AND:

`Status = expected OR dock`

Subtitle:

**P0/P1 stuck in expected/dock**

### KPI 5 — Put-Pending

Count shipments where:

`Status = in_center`

AND:

`Putaway Location is blank`

### KPI 6 — Put-Pending > 4 Days

Count put-pending shipments older than:

**96 hours**

Display the percentage of total put-pending shipments.

All KPI values must update whenever filters change.

## 8. Shipment Ageing by Product Type

Create:

**Shipment Ageing — by Product Type**

Calculate shipment ageing using:

`Current Time - Incoming Time`

Ignore shipments without a valid incoming timestamp.

Add a Product Type selector:

* All Product Types
* Individual product types

Display:

| Ageing Bucket         | Shipment Count | % of Bags | Download     |
| --------------------- | -------------: | --------: | ------------ |
| Greater than 30 hours |          Count |         % | Download CSV |
| Greater than 40 hours |          Count |         % | Download CSV |
| Greater than 48 hours |          Count |         % | Download CSV |

Ageing buckets must be cumulative.

For example:

* > 30 hours includes all shipments older than 30h
* > 40 hours includes all shipments older than 40h
* > 48 hours includes all shipments older than 48h

Use increasing severity indicators:

* > 30 = normal/attention
* > 40 = warning
* > 48 = critical

Each bucket must have a **Download CSV** action that exports only the matching shipments.

## 9. Top 5 NTCs by Ageing

Create:

**Top 5 NTCs — by Product Type & Ageing Bucket**

Controls:

* Product Type
* Ageing bucket

Ageing bucket options:

* All shipments
* Greater than 30 hours
* Greater than 40 hours
* Greater than 48 hours

Aggregate shipments by NTC.

Rank NTCs by shipment count and show the top five.

Columns:

| NTC | Shipment Count | Weight (Tons) | % of Bucket Total | Download |
| --- | -------------: | ------------: | ----------------: | -------- |

Calculate:

* Shipment count
* Total weight in tons
* Percentage of selected ageing population

Allow each NTC to be downloaded as CSV.

## 10. NTC-wise Shipment Status

Create:

**NTC-wise Shipment Status — Top 25**

Aggregate the current filtered dataset by NTC.

Display the top 25 NTCs.

Show both shipment count and shipment weight.

Use count columns:

* expected
* yard
* dock
* in_center

Use weight columns:

* in_center
* dock
* yard
* expected

Table should include:

| NTC | Total Count | Total Wt (Tons) | expected | yard | dock | in_center | in_center Wt | dock Wt | yard Wt | expected Wt |

Allow users to:

* Search NTCs
* Sort by any column
* Toggle ascending/descending order

Default sorting should be:

**Total Count descending**

If `in_center` weight exceeds:

**8 tons**

highlight that weight cell as a critical operational attention point.

## 11. Bag-Level Shipment Search

Create:

**Shipment Search**

Provide a search field:

**Search Bag ID…**

Search behaviour:

* Case-insensitive
* Partial matching
* Minimum 3 characters
* Maximum 100 results
* Must respect all global filters

Display:

| Bag ID | Product Type | Priority | NTC | Status | Putaway Location | Ageing (hrs) | Client | Weight (kg) |

If putaway location is missing, display:

**— pending —**

If incoming timestamp is unavailable, display:

**—**

This module should allow operations users to quickly locate individual shipments.

## 12. Connection & Cutoff Search

Create a separate module:

**Connection & Cutoff Search**

This module uses a route database.

Allow users to upload a route database CSV.

Required fields:

* `oc`
* `cn`
* `vmode`
* `vname`
* `cutoff_departure`
* `eta`
* `tat`
* `vehicle_size`
* `operating_days`
* `lane_type`
* `active`

The application should only retain active routes where:

`oc = Gurgaon_Pathradi_H`

Treat the origin as fixed.

After upload, display:

* Number of connections
* Number of unique CNs
* Loaded filename

Provide:

**Search CN**

Minimum search length:

**2 characters**

Search should be partial and case-insensitive.

Add a vehicle mode filter:

* All Modes
* Dynamically generated vehicle modes

Display matching routes using:

| Origin | CN | Mode | Vehicle / Vendor | Cutoff Departure | ETA | TAT | Vehicle Size | Operating Days | Lane |

Sort results by:

1. CN
2. Cutoff departure

Show a clear message when no matching connection exists.

## 13. Putaway Pending

Create:

**Putaway Pending**

Definition:

A shipment is considered putaway-pending when:

`Status = in_center`

AND:

`Putaway Location is blank`

Display two KPIs:

### Total Put-Pending Shipments

Count of all qualifying shipments.

### Pending > 4 Days

Count of shipments older than:

**96 hours**

Also show the percentage of total pending shipments.

## 14. Putaway Ageing

Create a table:

**Not-Put Ageing Buckets**

Buckets:

* Greater than 2 hours
* Greater than 4 hours
* Greater than 120 hours / 5 days

Columns:

| Ageing Bucket | Count | % of Pending | Download |
| ------------- | ----: | -----------: | -------- |

Buckets are cumulative.

Provide a CSV download for each bucket.

Use:

* > 2h = attention
* > 4h = warning
* > 120h = critical

## 15. Putaway Pending by NTC

Create:

**Put-Pending by NTC — Top 15**

Rank NTCs by pending shipment count.

Display:

| NTC | Pending | >4 Days | Oldest (hrs) |

For each NTC calculate:

* Total pending shipments
* Number pending for more than 4 days
* Oldest shipment ageing

Highlight NTCs with long-pending shipments.

## 16. Data Cleaning

During outbound data processing:

* Convert blank clients to `Unassigned`
* Standardise priorities to P0/P1/P2/P3
* Treat blank putaway location as pending
* Detect duplicate Bag IDs
* Handle missing timestamps
* Handle invalid numeric values
* Ignore unusable rows without crashing

Display a data-quality note such as:

**Data cleaned: blank clients → Unassigned; priority mapped to P0–P3; put-pending = in_center with no putaway location.**

Also display:

* Duplicate Bag IDs found
* Unassigned client bags

## 17. CSV Export

Provide CSV download functionality for:

* Shipment ageing buckets
* Top NTC ageing buckets
* Putaway ageing buckets
* NTC-specific shipment groups where applicable

Export columns:

* Bag ID
* Product Type
* Priority
* NTC
* Status
* Putaway Location
* Ageing (hrs)
* Client
* Weight (kg)

Correctly escape commas, quotes and special characters.

Use descriptive filenames such as:

`ageing_gt48h_All.csv`

`top_ntc_<NTC>_gt48h_All.csv`

`put_pending_gt120h.csv`

## 18. Application State

Maintain a central application state containing:

* Active priorities
* Active statuses
* Active MOTs
* Selected NTC
* Selected Product Type
* Client search
* NTC search
* Bag ID search
* Ageing Product Type
* Ageing Bucket
* Route database
* CN search
* Vehicle mode
* Current dataset

Create a central rendering/update mechanism so that changing a filter refreshes all dependent modules consistently.

Avoid duplicated calculation logic.

## 19. Initial Sample Data

The HTML should contain an embedded sample dataset so that the dashboard works immediately when opened.

The sample should demonstrate:

* P0/P1/P2/P3 shipments
* Multiple statuses
* Multiple NTCs
* Multiple product types
* Different ageing levels
* Putaway-pending shipments
* Long-pending shipments
* Different clients
* Duplicate Bag IDs
* Unassigned clients
* Multiple route connections
* Multiple vehicle modes

Also include embedded sample route data.

## 20. User Experience

The dashboard should feel like a **ground-operations command center**.

Prioritise:

* Exceptions
* Ageing
* SLA risk
* Backlog
* NTC performance
* Shipment-level visibility
* Route cutoffs

Do not make charts the centrepiece.

The primary interaction model should be:

**Upload → Filter → Identify exceptions → Search → Investigate → Download actionable shipment list**

Use compact tables and strong KPI hierarchy so an operations user can understand the current situation within seconds.

## 21. Error Handling

Provide clear toast notifications for:

* Successful upload
* Failed upload
* Missing columns
* Empty dataset
* Invalid rows
* Duplicate Bag IDs
* Route upload failure
* No matching CN
* No matching Bag ID
* Successful CSV download

Never expose raw JavaScript errors to the user.

## 22. Final Requirements

The final deliverable must be:

* One `.html` file
* Fully functional
* Locally runnable
* No backend
* No database
* No installation
* No build process
* No external data processing server
* Client-side CSV processing
* Responsive
* Data-driven
* Modular and maintainable

Do not create a static visual mockup.

Every KPI, percentage, table, ageing value, search result, filter, sorting operation and CSV download must be dynamically calculated from the currently loaded dataset.

The final application should resemble an **enterprise outbound logistics control tower designed for daily ground-team decision making**.
