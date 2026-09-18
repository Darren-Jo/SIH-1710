# Smart India Hackathon Workshop
# Date: 18/09/2026
## Register Number: 212225230039
## Name: Darren Joseph K
## Problem Title
SIH 1710: Enhancing Navigation for Railway Station Facilities and Locations
## Problem Description
Background: Railway stations are complex environments with numerous facilities and locations such as ticket counters, platforms, restrooms, food courts, and waiting areas. Passengers often face difficulties in navigating these spaces, especially in large or unfamiliar stations. Efficient and user-friendly navigation systems are crucial for improving passenger experience, reducing congestion, and ensuring timely travel connections. Description: The problem involves developing a comprehensive navigation solution for railway stations that assists passengers in locating various facilities and destinations within the station premises. This includes creating detailed maps, providing real-time directions, and integrating features such as accessibility options for individuals with disabilities. The solution should be intuitive, easy to use, and accessible via multiple platforms, including mobile devices and digital kiosks. Key challenges include updating navigation information in real-time, ensuring accuracy, and accommodating the diverse needs of all passengers. Expected Solution: The expected solution is a multi-platform navigation system that provides detailed, real-time directions to all facilities and locations within a railway station. This system should include: A mobile application with 3D interactive maps and step-by-step navigation. Digital kiosks located throughout the station with touch-screen interfaces. Voice-guided navigation for visually impaired passengers. Regular updates to reflect changes in station layout and facility locations. Integration with existing railway apps and services for seamless user experience. The solution should enhance the overall passenger experience by reducing confusion, saving time, and improving accessibility within the station.

## Problem Creater's Organization
Ministry of Railway

## Idea
# Setu
Setu treats time-to-departure, not just distance from point A to point B, as the primary input to every route.

### 1 Time-Critical Routing — Scanning a ticket QR/PNR surfaces platform, live ETA, the least-congested walking route, and a countdown buffer; the route recalculates automatically if the platform changes.
### 2 Beacon-Free Positioning — Sparse QR/NFC anchors at decision points, plus Wi-Fi RSSI fingerprinting off existing station Wi-Fi, plus on-device dead reckoning, fused with a lightweight Kalman filter — no BLE beacon network to install or maintain.
### 3 Crowd-Aware Routing — Anonymized Wi-Fi probe density builds a live congestion heatmap; routing steers passengers away from the worst bottlenecks.
### 4 Accessibility as a Routing Graph — Separate edge weights for stairs, escalators, ramps, and lifts so wheelchair and low-mobility routes are physically valid, paired with distinct haptic vibration patterns alongside voice guidance.
### 5 Zero-Install Adoption — A single Progressive Web App opened via QR/NFC at any gate or kiosk; the same codebase runs on phones and kiosk touchscreens.
### 6 Live-Editable Digital Twin — A staff CMS lets the station master's office push layout or closure changes that go live on every phone and kiosk within seconds.


## Proposed Solution / Architecture Diagram
<img width="1797" height="589" alt="image" src="https://github.com/user-attachments/assets/00bfacff-748d-4137-91ff-61785abfd7b3" />


## Use Cases
| Actor / Scenario | Situation | How Setu Responds |
|---|---|---|
| Passenger with a tight connection | Scans ticket QR on arrival, 12 minutes to departure | Shows platform, live ETA, and the fastest low-crowd route with a countdown; re-routes instantly if the platform changes |
| First-time visitor, no time pressure | Wants a restroom, ATM, or food court | Switches to facility-finder mode; shortest walking route to the nearest matching facility |
| Visually impaired passenger | Needs full guidance without reading a screen | Turn-by-turn voice plus distinct haptic vibration patterns per turn/arrival, using only accessible routing edges |
| Wheelchair user / heavy luggage | Cannot use stairs | Routing graph excludes stair edges entirely; guided via ramps and lifts only |
| Elderly passenger, unfamiliar with apps | Approaches a station kiosk | Same PWA runs on the kiosk touchscreen with a large-text, simplified UI — no phone, no install |
| Passenger in a crowded concourse (peak hours / festival rush) | Live Wi-Fi density heatmap shows a bottleneck ahead | Route is automatically shifted around the worst congestion, cutting both time and crowd-crush risk |
| Passenger whose train's platform changes mid-walk | Live ETA adapter detects a platform reassignment | Route recalculates instantly with one calm voice + haptic alert, instead of silently going stale |
| Passenger in a basement/underground zone with no signal | Connectivity drops mid-route | Dead reckoning plus the pre-cached station graph keeps guidance working offline; syncs back once reconnected |

## Technology Stack
| Layer | Technology | Why |
|---|---|---|
| Client | React + Vite PWA, 2D Canvas/Leaflet renderer | Fast load on low-end phones; works offline via service worker; avoids the slow-loading 3D/WebGL problem that sinks many rival proposals |
| Positioning | TensorFlow Lite on-device fingerprint matcher, optional Web Bluetooth fallback | Runs entirely on-device; beacon support only where a station already has them |
| Routing | Node.js/Express, custom A*/Dijkstra graph engine | Fast recomputation on live ETA or crowd changes |
| Real-time data | Adapter for an NTES/PRS-style live train feed (simulated feed for the prototype, pending official Railways API access), Redis for live cache | Keeps the demo unblocked while integration is pursued |
| Database | PostgreSQL + PostGIS | Spatial station graph, per-station editable |
| Staff CMS | React admin panel, role-based access | Lets station staff push layout/closure updates instantly |
| Voice / Haptic | Web Speech API with bundled offline TTS per station, Vibration API | Works without connectivity in basement/underground zones |


## Dependencies
### 1 Station Wi-Fi infrastructure (RailWire access points) — required; already deployed at most target stations, so no new install
### 2 Live train ETA/PRS-NTES data access from Indian Railways — pending approval; the prototype runs on a simulated feed until this is granted
### 3 Station floor plans / physical walkthrough for graph digitization — 2–3 weeks per station
### 4 Wi-Fi fingerprint data collection & calibration — 1 week per station
### 5 QR/NFC anchor stickers (printing + placement at decision points) — 2–3 days per station
### 6 Staff CMS onboarding & training for the station master's office — 2 days per station
### 7 Indicative pilot budget (one station): ₹1.5–2.5 lakh — covers anchor printing, fingerprint calibration effort, and kiosk software licensing; excludes new hardware, since existing station Wi-Fi is reused
