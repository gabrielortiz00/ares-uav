# Phase 1 Requirements: Navigation in 2D

## Scope

Phase 1 builds the first working version of ARES's navigation loop, entirely in a 2D grid simulation. No real sensors, no 3D, no flight dynamics. The goal is to prove out path planning and replanning around obstacles that are only known once detected, and to start measuring how well that actually works.

## Environment Model

The environment is a 2D grid. Each cell is either free or blocked. The vehicle occupies one cell at a time and moves between adjacent cells.

Obstacles are not known in advance. A cell is only treated as blocked by the planner once it falls within a detection radius of the vehicle's current position. The detection radius is a configurable parameter, not a fixed constant, so it can be varied across test runs.

## Functional Requirements

**FR-1:** Given a grid with a start cell, a goal cell, and no detected obstacles, the system computes a valid path from start to goal using Dijkstra's algorithm.
*Verification:* `test_finds_path_no_obstacles` — path exists, starts at start, ends at goal, every step moves between adjacent cells.

**FR-2:** The system never returns a path that passes through a cell currently known to be blocked.
*Verification:* `test_path_avoids_known_obstacles` — for a grid with detected obstacles, assert no cell in the returned path is blocked.

**FR-3:** Obstacles outside the detection radius are not treated as blocked, even if they exist in the underlying grid.
*Verification:* `test_undetected_obstacles_ignored` — place an obstacle outside the detection radius and confirm the planner's path can pass through that cell.

**FR-4:** When the vehicle moves and a previously undetected obstacle enters the detection radius, the system replans a new path from the vehicle's current position to the goal, treating that obstacle as blocked.
*Verification:* `test_replans_on_new_detection` — step the vehicle toward a hidden obstacle, confirm a replan is triggered once it's within range, and the new path avoids it.

**FR-5:** If no path exists to the goal given currently known obstacles, the system reports failure instead of returning an invalid or partial path.
*Verification:* `test_reports_failure_when_unreachable` — construct a grid where the goal is fully enclosed by known obstacles, confirm the planner returns failure, not a path.

## Metrics

Every simulated run records:

- **Success rate** — whether the vehicle reached the goal without ever occupying a blocked cell, aggregated across many runs with different obstacle layouts.
- **Path length vs. optimal** — length of the path actually traveled, divided by the length of the shortest path computed with full knowledge of every obstacle from the start.
- **Planning time** — time to compute the initial path, and time to compute each replan.
- **Replan count** — number of times a new detection triggered a replan, per run.

These are collected as a baseline for Phase 1, not compared against a target yet. There's no prior number to compare against until this exists. Once A* and D* Lite are implemented, these same metrics are what make that comparison meaningful instead of anecdotal.

## Out of Scope for Phase 1

- 3D movement or altitude
- Any real sensor model, timing, or noise
- Vehicle dynamics or physical constraints on movement
- A* and D* Lite (planned as later work, evaluated against this baseline)
