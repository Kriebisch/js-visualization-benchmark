# JavaScript Visualization Benchmark

This project was developed as part of my bachelor thesis and focuses on reproducible, browser-native performance measurement without external benchmarking tools. It provides a custom benchmarking environment to evaluate the performance of different JavaScript visualization libraries.

## Overview

This project implements a custom benchmarking framework to evaluate the performance of different JavaScript visualization libraries.

The benchmark focuses on measuring:
- Rendering time
- Data processing time
- Memory consumption (RAM usage)

Tested libraries include:
- D3.js
- ECharts
- Chart.js

The benchmark supports multiple chart types:
- Bar Charts
- Scatter Plots
- Time-Series Charts

## Key Idea

Instead of using external automation tools, a custom "blackbox" benchmarking approach was implemented:

- Each chart is rendered in an isolated browser window
- Performance is measured using the native `performance` API
- Memory usage is captured via `performance.memory` (Chrome)
- Results are aggregated over multiple runs

This ensures more realistic and reproducible measurements.

## Architecture

The benchmark is based on a custom-built, browser-native measurement system.

### 1. Benchmark Units ("Cheat Files")

Each benchmark scenario is implemented as a standalone HTML file.

Responsibilities:
- Load and parse dataset (CSV via PapaParse)
- Process and transform data
- Render chart using a specific library
- Measure:
  - Data loading time
  - Data processing time
  - Render time
  - Memory usage

Measurements are done using:
- `performance.mark()` / `performance.measure()`
- `requestAnimationFrame()` for accurate render timing
- `performance.memory` (Chrome only)

Each run sends results back to the main controller via `postMessage`.

---

### 2. Benchmark Handler

The central orchestration component:

- Opens benchmark windows programmatically
- Executes multiple runs per configuration
- Collects results across runs
- Calculates overhead (window handling vs. actual render time)
- Aggregates results

After completion:
- Results are displayed
- Box plots are generated
- Data can be exported as JSON


All libraries are stored locally in the project (`/lib`) to ensure consistent results.

### 3. Evaluation (Comparison)

A dedicated evaluation HTML file allows comparison across:

- Libraries
- Chart types
- Dataset sizes

Features:
- Aggregates multiple benchmark result files
- Visualizes distributions using box plots
- Uses logarithmic scaling for better comparability

⚠️ Current limitation:
- Benchmark results must be manually downloaded and re-linked (hardcoded)

---

### 4. Time-Series Data Preparation

Time-series charts required a separate preprocessing step.

Problem:
- Raw dataset timestamps were too expensive to aggregate at runtime
- Caused performance issues and browser instability

Solution:
- Preprocessing step generates:
  - Hourly data
  - Daily data
  - Monthly data

This is done via a dedicated preparation script:
- Parses timestamps using Luxon
- Aggregates values into time buckets
- Exports JSON datasets

---

## Features

- Multi-run benchmarking
- Comparison across different data sizes (e.g. 10 / 100 / 1000 data points)
- Separation of:
  - Data loading time
  - Data processing time
  - Rendering time
- Memory usage measurement
- Automatic result export (JSON)
- Visualization via box plots

## How It Works

1. The handler starts a benchmark run
2. A browser window is opened for a specific chart
3. The chart renders and measures performance
4. Results are sent back to the handler
5. Process repeats for all libraries and configurations
6. Final results are aggregated and visualized

## Running the Project

### Requirements

- Node.js
- A local web server (e.g. `http-server`)
- Google Chrome (required for memory measurement)

## Setup Libraries

The following libraries are required and must be placed in `/lib`:

- D3.js
- ECharts
- Chart.js
- PapaParse
- Luxon

### Start data server

cd public/data
http-server --cors -c-1

### Start Chrome with memory tracking enabled

Start-Process "C:\Program Files\Google\Chrome\Application\chrome.exe" -ArgumentList "--enable-precise-memory-info", "--js-flags=--expose-gc"

- Make sure no other Chrome instance is running.

## Why No External Benchmark Tools?

Initially, tools like Puppeteer were evaluated.

However:
- Browser automation introduced noise
- Measurements were not reproducible
- Execution behavior differed from real user environments

Final decision:
→ Implement a custom in-browser benchmark system

Advantages:
- Full control over execution
- Deterministic measurement flow
- No external interference

---

## Run Benchmark

1. Open the benchmark handler HTML file in Chrome  
2. Select the number of runs  
3. Start the benchmark  
4. Download results if needed  

## Screenshots

Example outputs and benchmark results can be found in:


/docs/screenshots/


## Notes

- Data files are not included in this repository
- Paths to datasets may need to be adjusted manually
- Benchmark is designed for local execution
- Chrome is required for memory measurements (`performance.memory`)

---

## External Libraries

The required visualization libraries (D3.js, ECharts, Chart.js, Luxon, PapaParse) are not included in this repository.

They must be downloaded manually and placed into the `/public/benchmark/lib` directory.

This is intentional to:
- Reduce repository size
- Avoid licensing issues
- Ensure compliance with project restrictions

## Limitations & Future Improvements

- No automated upload/import of benchmark results
- Manual linking of JSON result files required
- Time-series preprocessing is external (not integrated into pipeline)

Potential improvements:
- UI for uploading result files
- Automated pipeline for evaluation
- More advanced statistical analysis
- Support for additional libraries

---

## Use of AI

Some utility functions (e.g. data aggregation and visualization helpers) were developed with the assistance of AI.

The core benchmark architecture, measurement logic, and system design were fully implemented by me.

---

## Context

This project was developed as part of a bachelor thesis focusing on performance evaluation of web-based data visualization libraries.