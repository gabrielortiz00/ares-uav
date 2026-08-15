# ARES

ARES (**Autonomous Replanning and Estimation System**) is a personal engineering project exploring GPS-denied autonomous navigation for UAVs.

The goal is to build a UAV that can fly from Point A to Point B without using GPS/GNSS or prior knowledge of the environment. The UAV should be able to estimate its position using onboard sensors, detect obstacles in its path, update its understanding of its spatial environment, and replan its route when necessary.

Development will begin in simulation software, later transitioning to real UAV integration.

## Goals

The final vehicle should be able to achieve the following tasks:

- Navigate from a known starting position to an ending position
- Operate without use of GPS/GNSS for navigation
- Detect obstacles that were not known to the UAV before encountering them
- Estimate the vehicle's position using data from onboard sensors
- Build and continuously update a spatial model of the surrounding environment
- Plan an initial path from starting position to ending position
- Replan the path when the current path is blocked by an obstacle
- Perform navigation, localization, mapping, obstacle avoidance, and replanning using onboard computation
- Complete a real-world autonomous navigation demo

## Development

ARES is developed incrementally, starting with the implementation of navigation in simulation before introducing physical drone hardware.

### Phase 0 - Foundation

- [ ] Define the mission and scope of the project
- [ ] Write initial documentation and requirements
- [ ] Set up project structure and repository

### Phase 1 - Navigation in 2D

- [ ] Build a simple simulation environment in 2D
- [ ] Implement basic pathfinding algorithms
- [ ] Introduce obstacles
- [ ] Implement replanning around new obstacles
- [ ] Establish baseline metrics (path length, time, etc)


### Phase 2 - Navigation in 3D Simulation

- [ ] Set up 3D simulation environment and vehicle
- [ ] Implement path planning in the 3D environment
- [ ] Add simulated obstacles and sensing capabilities with realistic constraints
- [ ] Implement replanning logic around new obstacles
- [ ] Account for physical vehicle constraints when generating paths
- [ ] Measure and evaluate through established metrics (ex: path length, time, replanning time, collision rate, etc)

### Phase 3 - GPS-Denied Localization

- [ ] Remove access to GPS position data in simulation
- [ ] Introduce simulated sensor noise and drift
- [ ] Implement a method for estimating the vehicle's position without GPS
- [ ] Integrate localization estimates with the navigation system
- [ ] Measure localization error

### Phase 4 - Perception and Mapping

- [ ] Build a spatial model of the environment from simulated sensor data
- [ ] Detect obstacles only as they come into sensor range
- [ ] Continuously update the environment model as the vehicle moves
- [ ] Integrate perception and mapping with path planning and replanning
- [ ] Test navigation in several unknown environments of varying complexity

### Phase 5 - Full System Simulation

- [ ] Combine localization, perception, mapping, and path planning into a single system
- [ ] Integrate with a simulated flight controller/autopilot
- [ ] Run complete missions from start to destination without GPS
- [ ] Introduce failure cases and more difficult environments
- [ ] Evaluate overall system performance and identify limitations

### Phase 6 - Hardware Integration

- [ ] Select the UAV platform, companion computer, and sensors based on project requirements
- [ ] Build and configure the physical UAV
- [ ] Establish communication between the companion computer and flight controller
- [ ] Integrate real sensors and onboard computation
- [ ] Validate each subsystem individually

### Phase 7 - Physical Flight Testing

- [ ] Establish reliable manual flight
- [ ] Test onboard localization and perception through real flights
- [ ] Start with simple navigation tests with one obstacle in a safe environment
- [ ] Introduce more unknown obstacles and test replanning
- [ ] Collect flight data and compare flight testing results against simulation data

### Phase 8 - Final Demonstration

- [ ] Complete a fully autonomous flight from a known starting point to a specified destination
- [ ] Navigate without using GPS
- [ ] Detect previously unknown obstacles using onboard sensors
- [ ] Build and update a spatial model of the environment during flight
- [ ] Replan the route when obstacles block the current path
- [ ] Reach the destination using only onboard sensing and computation
- [ ] Document and evaluate the final system through recorded flight data and chosen metrics

## Repo Structure

```text
ares-uav/
├── docs/
├── sim/
├── src/
├── tests/
└── README.md
```

## Current Status

Phase 0
