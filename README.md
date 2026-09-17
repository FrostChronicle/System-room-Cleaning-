# 🧹 Smart Hostel Cleaning & Resource Optimization System

A web application that models a data-structures-and-algorithms approach to real-world facility management. It schedules hostel room cleaning using priority queues, optimizes staff routes with graph traversal (BFS/Dijkstra), and surfaces predictive analytics from historical cleanliness data — replacing manual, paperwork-driven coordination with an automated, real-time system.

> Built as an educational project to demonstrate practical DSA applications: max-heaps for scheduling, graphs for pathfinding, FIFO queues for fair staff assignment, and hash maps for O(1) analytics lookups.

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Data Structures & Algorithms Used](#data-structures--algorithms-used)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage Guide](#usage-guide)
- [Documentation](#documentation)
- [Attributions](#attributions)
- [License](#license)

## Overview

The system models a hostel with:
- **32 rooms** across **4 floors** (8 rooms per floor), each tracked for cleanliness score, occupancy, and last-cleaned time
- **6 staff members** who are assigned tasks fairly via a FIFO queue
- **Urgency-based scheduling**, where every room's cleaning priority is computed from its cleanliness score, time since last cleaned, occupancy, and any active student requests
- **Graph-based room adjacency**, letting the system compute efficient cleaning routes and shortest paths between rooms
- **Historical tracking**, enabling degradation-rate calculations and predictive maintenance recommendations

All state is generated as mock data on load and persisted to the browser's `localStorage`, so the app is fully self-contained — no backend or database required.

## Key Features

- **Real-Time Dashboard** — live priority queue, staff availability, and system-wide stats, with an optional auto-simulation mode
- **Room Graph Visualization** — interactive, color-coded (green/yellow/red) floor grid with BFS path highlighting between rooms
- **Student Request Portal** — students can flag a room for cleaning, boosting its urgency score
- **Staff View** — staff can pick up tasks, log a new cleanliness score, and complete jobs (with optional QR-code verification)
- **Analytics Dashboard** — floor-wise cleanliness stats, cleaning trends over time, cleanliness distribution, and most-cleaned rooms
- **Predictive Analysis** — flags high-risk rooms based on degradation rate and recommends pre-emptive cleaning
- **Auto-Simulation Mode** — runs the whole system autonomously (task assignment, completion, requests, degradation) at adjustable speed

## Data Structures & Algorithms Used

| Structure / Algorithm | Where | Purpose | Complexity |
|---|---|---|---|
| Priority Queue (max-heap) | `utils/data-structures.ts` | Urgency-based task scheduling | O(log n) insert/extract |
| Graph (adjacency list) | `utils/data-structures.ts` | Models rooms and their neighbors | — |
| BFS | `RoomGraph.bfsCleaningPath()` | Optimal cleaning path through adjacent dirty rooms | O(V + E) |
| Dijkstra's Algorithm | `RoomGraph.dijkstraPath()` | Shortest path between any two rooms | O((V + E) log V) |
| FIFO Queue | `utils/data-structures.ts` | Fair, in-order staff assignment | O(1) |
| Hash Map | `CleaningHistoryMap` | Per-room cleaning history and analytics | O(1) lookup |

Urgency score formula:
```
urgency = (100 − cleanlinessScore)
        + (hoursSinceLastCleaned × 2)
        + (occupiedBonus: 20)
        + (requestBoost: 30)
```

See [`DSA_DOCUMENTATION.md`](./Smart%20Hostel%20Cleaning%20System/DSA_DOCUMENTATION.md) for the full implementation write-up.

## Tech Stack

- **React 18** + **TypeScript**
- **React Router 7** for navigation
- **Vite 6** for tooling/dev server
- **Tailwind CSS v4** for styling
- **Radix UI** primitives + **shadcn/ui** components
- **Recharts** for analytics charts
- **QRCode** for staff task verification
- **Sonner** for toast notifications

## Project Structure

```
System-room-Cleaning-/
├── LICENSE
├── README.md                          # you are here
└── Smart Hostel Cleaning System/       # the application
    ├── src/
    │   ├── app/
    │   │   ├── components/
    │   │   │   ├── Dashboard.tsx        # main dashboard
    │   │   │   ├── RoomGraph.tsx        # graph visualization
    │   │   │   ├── StudentRequest.tsx   # student interface
    │   │   │   ├── StaffView.tsx        # staff interface
    │   │   │   ├── Analytics.tsx        # analytics & charts
    │   │   │   ├── QuickGuide.tsx       # in-app user guide
    │   │   │   ├── SimulationControls.tsx
    │   │   │   ├── AuthPage.tsx         # login / register
    │   │   │   ├── Layout.tsx
    │   │   │   ├── WelcomeDialog.tsx
    │   │   │   └── ui/                  # shadcn/ui components
    │   │   ├── context/
    │   │   │   ├── HostelContext.tsx    # core app state
    │   │   │   └── AuthContext.tsx      # auth state
    │   │   ├── utils/
    │   │   │   ├── data-structures.ts   # heap, graph, queue, hash map
    │   │   │   ├── storage.ts           # localStorage persistence
    │   │   │   └── mock-data.ts         # mock room/staff generation
    │   │   ├── routes.tsx
    │   │   └── App.tsx
    │   ├── styles/
    │   └── main.tsx
    ├── DSA_DOCUMENTATION.md
    ├── ATTRIBUTIONS.md
    ├── package.json
    └── vite.config.ts
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) 18+
- [pnpm](https://pnpm.io/) (recommended — a `pnpm-workspace.yaml` is included), or `npm`/`yarn`

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/System-room-Cleaning-.git
cd "System-room-Cleaning-/Smart Hostel Cleaning System"

# Install dependencies
pnpm install
# or: npm install
```

### Run the dev server

```bash
pnpm dev
# or: npm run dev
```

The app will be available at the local URL Vite prints (typically `http://localhost:5173`).

### Build for production

```bash
pnpm build
# or: npm run build
```

## Usage Guide

1. **Sign in** — the login screen accepts any name/password combination (this is a demo auth flow with no real backend); choose **Student** or **Staff**, or use quick access.
2. **Dashboard** — view the live priority queue and staff availability, assign tasks manually, or toggle auto-simulation.
3. **Room Graph** — click a room to inspect it, view its neighbors, and see the BFS cleaning path from it. Colors indicate cleanliness: green (clean), yellow (moderate), red (dirty).
4. **Student Request** — submit a cleaning request for a room to boost its urgency score by +30.
5. **Staff View** — pick a staff member and room, log the new cleanliness score and task duration, and mark it complete (optionally scanning a QR code).
6. **Analytics** — review cleanliness trends, floor-wise stats, and predictive high-risk room flags.

## Documentation

- [`Smart Hostel Cleaning System/DSA_DOCUMENTATION.md`](./Smart%20Hostel%20Cleaning%20System/DSA_DOCUMENTATION.md) — detailed breakdown of every data structure and algorithm used, with complexity analysis
- [`Smart Hostel Cleaning System/README.md`](./Smart%20Hostel%20Cleaning%20System/README.md) — app-level README with feature and architecture details

## Attributions

This project uses UI components from [shadcn/ui](https://ui.shadcn.com/) (MIT licensed) and photos from [Unsplash](https://unsplash.com) (used under the Unsplash license). See [`ATTRIBUTIONS.md`](./Smart%20Hostel%20Cleaning%20System/ATTRIBUTIONS.md) for details.

## License

Released under the [MIT License](./LICENSE).

---

*An educational project demonstrating practical DSA applications in facility management — contributions adding new algorithms (A*, MST, DP-based scheduling) or features are welcome.*
