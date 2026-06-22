# NR Mushroom Spatial Analysis

This repository contains advanced geospatial telemetry and spatial analysis logic used to identify completely surrounded `NR21 Mushroom` network sites. The script processes site coordinates, performs spatial joins with clutter data, and uses K-Nearest Neighbors (KNN) alongside angular bearing calculations to mathematically verify continuous coverage umbrellas.

## Algorithm Logic & Rules

The analysis operates through a sophisticated, multi-stage spatial pipeline:

### 1. Dynamic Data Ingestion & Categorization
The algorithm dynamically loads network datasets via wildcard scanning (`*.xlsx`) and categorizes sites purely based on the strings found in their `TECHNOLOGY` column:
* **NR21 Only:** Contains `NR2100` (but no NR2600).
* **NR26 Only:** Contains `NR2600`, `NR26G`, or `NR26`.
* **NR21 & NR26:** Contains both.

### 2. MapInfo Clutter Spatial Join
To overcome missing or inaccurate spreadsheet data, the script loads raw MapInfo Vector Polygon files (`*.TAB`). It takes the GPS coordinates of every network site and performs a mathematical **Spatial Join (Point-in-Polygon)** overlay.
* This precisely assigns every single site its true `Morpho` classification (e.g., `DENSE URBAN`, `RURAL`, `SUB URBAN`).
* Any duplicate intersections caused by topographically overlapping sliver polygons are automatically dropped to maintain dataset integrity.

### 3. Continuous Coverage Enforcement (3GPP Thresholds)
To ensure that an `NR21 Only` site is actually covered by the surrounding `NR26` infrastructure, the algorithm enforces a **Maximum Distance Threshold (Radius)**. It calculates the Haversine distance from the central site to all surrounding neighbors and rejects any neighbor that is too far away to provide continuous RF coverage based on its clutter type:
* **DENSE URBAN:** Max 500 meters
* **URBAN:** Max 1,000 meters
* **SUB URBAN:** Max 2,000 meters
* **RURAL:** Max 5,000 meters

### 4. Geometric Surround Detection (KNN Angular Gap)
Finally, to guarantee the site is surrounded in a 360-degree manner (and not just sitting on the edge of a cluster):
1. Finds the **20 Nearest Neighbors**.
2. Filters down to the neighbors that are upgraded (`NR21 & NR26` or `NR26 Only`) and within the physical **Continuous Coverage** distance threshold.
3. If there are at least 3 valid neighbors, it mathematically calculates the **Azimuth/Bearing Angles** from the central site to each neighbor.
4. It sorts the angles and calculates the angular gaps between them. 
5. If the **Maximum Angular Gap** is `<= 210 degrees`, the site is officially flagged as an **`NR21 Mushroom`**.

## Output Features
- **Unified Master Dashboard:** The script merges a high-performance Canvas Folium map, Chart.js KPI graphs, and an interactive Grid.js Data Table into a single, fully portable `.html` file.
- **Export Capabilities:** The dashboard allows for one-click CSV exporting of the actionable Mushroom sites.

> **Note:** Base data, shapefiles, and MapInfo layers (`.xlsx`, `.shp`, `.TAB`) are explicitly ignored in this repository for privacy/security reasons.
