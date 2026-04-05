# Delivery Route Optimization Simulator

A last-mile delivery route optimizer that implements and visualizes three classic routing algorithms on an interactive map — all running client-side in the browser.

![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-3-38bdf8?logo=tailwindcss)
![Leaflet](https://img.shields.io/badge/Leaflet-1.9-199900?logo=leaflet)

## Live Demo

Deployed on Vercel — no setup needed, runs entirely in the browser.
https://delivery-route-optimization-simulat.vercel.app/

## Features

- **Interactive map** — delivery stops plotted as numbered nodes over a real dark tile map
- **Three algorithms** implemented in pure TypeScript:
  - **Dijkstra's Shortest Path** — visits stops in order of shortest distance from the warehouse
  - **Nearest Neighbor TSP** — greedy heuristic that always picks the closest unvisited stop
  - **2-opt Local Search** — improves the NN result by reversing route segments
- **Side-by-side metrics** — total distance (km), estimated delivery time, and % improvement over the naive route
- **Step-by-step trace panel** — shows exactly how each algorithm built or improved the route
- **Configurable** — choose 4–15 stops and any stop as the warehouse; regenerate random layouts instantly

## How to Use

| Control | Action |
|---|---|
| **Stops slider** | Set number of delivery stops (4–15) |
| **Warehouse dropdown** | Choose which stop is the starting depot |
| **Regenerate** | Randomize stop locations |
| **Run All Algorithms** | Execute all 3 algorithms and compare |
| **Metric cards** | Click to highlight that algorithm's route on the map |
| **Trace tabs** | Switch between algorithm step-by-step traces |

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Next.js 16 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS (dark theme) |
| Map | Leaflet.js + react-leaflet |
| Deployment | Vercel |

## How the Algorithms Work

### Dijkstra's Shortest Path
Runs Dijkstra from the warehouse on a fully connected graph (edges weighted by Haversine distance). Stops are visited in the order they are **settled** — ascending order of shortest distance from the warehouse. The trace shows each settlement and which distances were relaxed.

### Nearest Neighbor TSP
A greedy heuristic for the Travelling Salesman Problem. Starting at the warehouse, at each step it moves to the **closest unvisited stop**. Simple and fast — the trace shows each greedy choice.

### 2-opt Local Search
Takes the Nearest Neighbor route and iteratively tries all pairs of edges `(i, j)`. If **reversing the segment** between them reduces total distance, the swap is kept. Repeats until no improvement is possible. The trace shows every swap and the distance saved.

### Distance Formula
All distances use the **Haversine formula** for accurate great-circle distances in km.

### Naive Baseline
The "naive" route visits stops in the order they were randomly generated. All `% improvement` figures are relative to this baseline.

## Project Structure

```
app/
  layout.tsx          Root layout
  page.tsx            Main page — state and controls
  globals.css         Tailwind + Leaflet overrides

components/
  MapWrapper.tsx      Dynamic import shell (ssr: false)
  MapInner.tsx        Leaflet map, markers, route polylines
  MetricsPanel.tsx    Algorithm comparison cards
  TracePanel.tsx      Step-by-step trace with timeline

lib/
  types.ts            Shared TypeScript interfaces
  haversine.ts        Distance formula + matrix builder
  generateStops.ts    Random stop generation (Chicago area)
  algorithms/
    dijkstra.ts
    nearestNeighbor.ts
    twoOpt.ts
    index.ts
```


