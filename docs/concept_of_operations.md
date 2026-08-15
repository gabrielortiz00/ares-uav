# ARES Concept of Operations

## Scope

This document describes the concept of operations for ARES, a quadcopter designed to navigate from a known starting point to a known destination point without GPS, avoiding obstacles it discovers along the way. It covers how ARES is meant to operate, not how it's built. Design and implementation details are covered elsewhere as the project progresses.

## Background

Conventional autopilots like PX4 and ArduPilot fly a route by following waypoints loaded in before the flight. They know their orientation from an IMU and their position from GPS, but they have no awareness of what's actually in front of them. If an obstacle wasn't accounted for when the waypoints were planned, the autopilot won't see it and won't react to it.

ARES is made to address exactly that. It will fly and navigate without a GPS position fix, only reacting to obstacles as they are detected, not where they were assumed or known to be beforehand.

## Purpose

GPS-denied navigation is necessary in defense operations, indoor spaces, underground environments, and dense urban areas, where satellite navigation isn't available or can't be trusted. Solving it means building state estimation, perception, mapping, planning, and control that all work together.

## Operating Environment and Assumptions

ARES operates outdoors, in an open field, during daylight. GPS is available on the vehicle but deliberately disabled in software, standing in for the real-world causes of GPS denial: jamming, indoor spaces, underground environments, and any other case where a receiver can't get a usable fix. Because ARES doesn't use GPS as a navigation input at all, it doesn't matter which of these is the actual cause.

The vehicle flies at a low, fixed altitude and encounters a small number of obstacles placed along its path. Testing uses multiple course layouts and obstacle configurations, so the system actually has to solve the navigation problem instead of memorizing one path.

Night and low-light operation are out of scope. The sensors chosen for this project depend on visible light and ground texture, and would need different or additional hardware to work reliably after dark.

## System Concept

ARES is built on a quadcopter airframe that can hover and hold position, which makes it easier to control and safer to test iteratively than a fixed-wing aircraft.

The vehicle is assumed to carry:

- An IMU, for orientation
- A forward-facing depth/stereo sensor, to measure distance to obstacles directly, without relying on markers or tags
- A downward-facing optical flow sensor, paired with a rangefinder, to estimate horizontal velocity and, from that, position without GPS

These are described at the level of sensing capability, not specific hardware. Exact components are a Phase 6 decision. By then, the simulation phases (particularly state estimation in Phase 3) should show what drift, range, and update-rate the real sensors actually need to meet, so hardware can be chosen against real numbers instead of guesses.

Onboard compute is split into two roles: a companion computer, which runs perception, localization, mapping, and planning, and a flight controller, which handles low-level stabilization and executes the companion computer's commands. ARES integrates with an existing flight-controller stack (e.g. PX4 or ArduPilot) rather than writing one from scratch.

ARES operates in two modes: autonomous, where the companion computer plans and directs the flight, and manual, where I fly it directly by RC as a safety override during real-world testing. In simulation, only autonomous mode applies.

I'm the only person who builds, flies, and operates ARES.

## Operational Scenario

A typical mission starts with ARES sitting at a known point in the field, with a destination point and course boundary set ahead of time. GPS is disabled before takeoff. ARES takes off, climbs to its operating altitude, and starts flying straight toward the destination, since no obstacles are known yet.

As it flies, the forward depth sensor picks up an obstacle in its path. ARES marks that position as blocked and replans a path around it, then continues toward the destination. This repeats for each new obstacle it detects. Position throughout the flight comes from the IMU and downward optical flow, not GPS. The mission ends when ARES reaches the destination point and lands.

## Mission Success

The vehicle starts at a known point, flies to a known destination point at a fixed low altitude, and reliably reaches it without colliding with any obstacle in its path, without using GPS. This is tested across several different obstacle configurations and course layouts to ensure that the navigation system works effectively in different environments with no prior knowledge about obstacle locations.

Specific, measurable success thresholds (success rate, timing, tolerances) will be outlined in the requirements doc for each phase. This section only defines what "working" means conceptually rather than actual performance numbers.

## Constraints

- **Budget:** $500-1500 for the real vehicle and hardware.

## Alternatives Considered

**AprilTags for obstacle detection.** A similar project used printed markers on obstacles to make detection easy, since their obstacle camera had no way to measure distance to anything without a known reference size. ARES's depth sensor measures distance to any obstacle directly, so this wasn't needed, and it would have added a dependency on obstacles being marked in advance, which doesn't hold in a real environment.

**Visual-inertial odometry instead of optical flow.** A dedicated VIO tracking camera gives lower drift over distance than optical flow, at added cost and complexity. Optical flow paired with a rangefinder is well-proven for low, consistent-altitude flight, which matches ARES's mission profile. Any drift problems that come up are addressed through sensor fusion in Phase 3, not by changing hardware upfront.
