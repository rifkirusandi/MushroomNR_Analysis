# NR Mushroom Spatial Analysis

This repository contains the geospatial telemetry and spatial analysis logic used to identify completely surrounded `NR21 Mushroom` network sites using K-Nearest Neighbors (KNN) algorithms.

## Features
- **Spatial Categorization**: Classifies sites into NR21 Only, NR26 Only, and NR21 & NR26.
- **KNN Geometric Surround Detection**: Determines if a site is geometrically surrounded by upgraded sites using Haversine distance.
- **Unified HTML Dashboard**: Exports a standalone interactive Map and Chart.js dashboard into a single file.

> Note: Base data (`.xlsx`, `.shp`) is ignored in this repository for privacy/security reasons.
