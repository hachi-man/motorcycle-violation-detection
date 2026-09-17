# Phase 1 — Problem Definition

## Project Title
Motorcycle Traffic Violation Detection System

## 1. What objects will the system detect?
The system detects four classes of objects from road footage:
- **helmet** — a safety helmet worn by a motorcycle rider
- **no_helmet** — a motorcycle rider's bare head without a helmet
- **motorcycle_normal** — a motorcycle carrying 1 or 2 riders (compliant load)
- **motorcycle_overloaded** — a motorcycle carrying 3 or more riders (violation)

## 2. Why is this problem useful?
Motorcycle traffic violations are a major cause of road accidents and fatalities in Pakistan. Two of the most common violations — riding without a helmet and triple riding (3+ people on one motorcycle) — are currently monitored manually by traffic police at checkpoints. Manual monitoring is inefficient, inconsistent, and limited in coverage. An automated detection system can process camera footage continuously, flag violations in real time, and reduce the human effort required for enforcement.

Punjab's Safe City Authority (PSCA) and PITB are actively developing AI-based e-challan features to automate violation detection. Helmet non-compliance and motorcycle overloading are both explicitly listed as planned detection targets on their enforcement roadmap. This project builds a prototype that directly addresses these planned features.

## 3. Where can this system be applied?
- Roadside fixed cameras at intersections and checkpoints
- Punjab Safe City camera network
- Traffic police monitoring stations
- Motorway and highway surveillance systems
- School and hospital zone safety monitoring

## 4. What are the target classes?
| Class ID | Class Name | Description |
|---|---|---|
| 0 | helmet | Safety helmet on a rider's head |
| 1 | motorcycle_normal | Motorcycle with 1–2 riders (compliant) |
| 2 | motorcycle_overloaded | Motorcycle with 3+ riders (violation) |
| 3 | no_helmet | Rider's bare head without a helmet |

## 5. What will be the final output?
The system produces annotated images and videos with:
- Color-coded bounding boxes drawn around each detected object
- Class label displayed above each bounding box
- Confidence score shown alongside each label
- For video: persistent object tracking with unique IDs per motorcycle across frames

Violations (no_helmet, motorcycle_overloaded) are highlighted in distinct colors to make them immediately visible to monitoring personnel. The system can process both static images and live or recorded video footage.
