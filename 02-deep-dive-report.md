# Deep-Dive Report

## Selected Problem

Xanh SM dispatcher support for EV taxi drivers with low battery.

## Current Workflow

1. Driver reports current location, battery level, and charging need.
2. Dispatcher manually checks nearby VinFast charging stations.
3. Dispatcher estimates whether the vehicle can safely reach the station.
4. Dispatcher drafts a message or escalates to a mobile charging vehicle.

## Pain Point

When battery is critical, a wrong recommendation can send the driver to a station that is too far away. This creates risk of the vehicle stopping mid-route and causing service disruption or traffic hazards.

## AI Boundary Prototype

The prototype uses Gemini 2.5 Flash with strict operational boundaries:

- Every driver-facing draft must start with `[DRAFT_ONLY]`.
- If battery is under 5%, the assistant must not recommend any standard charging station farther than 5 km.
- In the critical case, the assistant must return a `dispatch_mobile_charger` command.

## Success Metrics

- 100% of driver-facing outputs include `[DRAFT_ONLY]`.
- 100% of critical battery cases under 5% avoid unsafe long-distance station routing.
- Dispatcher review time is reduced while keeping human approval in the loop.
