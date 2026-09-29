# PolyU Maps

<img width="200" height="200" alt="PolyU_Maps" src="https://github.com/user-attachments/assets/01510597-9be9-4718-9323-06531770c555" />

<a id="readme-top"></a>

A 3D interactive campus map of The Hong Kong Polytechnic University, developing with Flutter and support running on Android, iOS and web, later support(macOS, Windows, Linux). The map
is rendered with MapLibre GL (open source) and driven by official PolyU campus
data (331 POIs and 297 photographs).

---

## Abstract

This project builds a 3D interactive map of the PolyU campus. The official campus
map is a flat image laid over Google Maps [2] and the official wayfinding app
covers outdoor routes only [3]: neither shows how tall a building is, which floor a
room is on, or how to walk from one building to a room on a particular floor.
Building footprints from GeoJSON are extruded to their real height and divided
into floor levels; 331 official points of interest are shown with photographs; and
one search index covers building names, block codes, room codes and coordinates.
Outdoor positioning uses GPS with pedestrian routing, while indoor navigation runs
A* over a graph that spans floors and connected blocks. The two are joined so that
a single request can route from outside a building to a room on a given floor. 

## Today's official tools

**Official campus map (web) [2]** — a flat, stylised 2D campus image laid over
Google Maps, with the pins drawn on that image
<img width="1283" height="1769" alt="image" src="https://github.com/user-attachments/assets/b154e519-ced1-4c69-9571-2888c49021c5" />

**POLY:U:WAY (mobile app) [3]** — Same with website version
<img width="1284" height="2778" alt="WhatsApp Image 2026-09-22 at 13 14 20" src="https://github.com/user-attachments/assets/1b31061a-a2ed-45e1-9fa6-5503c51df26b" />

# Official App navigation demo
<img width="1284" height="2778" alt="WhatsApp Image 2026-09-22 at 13 14 21" src="https://github.com/user-attachments/assets/62037021-9efb-4bdd-8bc6-09b805557ef4" />
- Provide a longer path for user

<img width="1284" height="2778" alt="WhatsApp Image 2026-09-22 at 13 14 21 (1)" src="https://github.com/user-attachments/assets/16d1fd8a-6348-4933-b80c-d680cec2c365" />
- Give a incorrect path, e.g (out of the campus area)


## 1. Introduction

### 1.1 Background and Problem Statement

Two official tools exist today. Both stop at *where a building is*; neither answers
*how do I get to this room*.

**Official campus map (web) [2] & POLY:U:WAY (official mobile app) [3]** — the campus is drawn as a stylised 2D image over
Google Maps, with the pins placed on that image.

- The image is a picture, not a model: every building is flat, so **height, block
  and floor do not exist**. Pins sit on the campus image rather than on the real
  position in the Google base map, so the same place can look misplaced.
- A pin carries a name, address and photograph and little else. Services,
  departments and offices are pinned at building level, so a **room code cannot be
  resolved** from it.
- There is **no search** over buildings, block codes or room numbers, **no “where am
  I”** (the map never shows the user's own position), and no way to be routed from a
  pin to the place the user actually needs.
- **Indoor space is absent**: a visitor can read “PQ304” on a pin and still not know
  which block, which floor or which door to use.

— after a destination is chosen it shows a
recommended route, distance, walking time and street view on a *“PolyU look-alike
map”*, with a kiosk at the main entrance near Core P.

- Its stated coverage is *classrooms, theatres and buildings across campus* — that
  is, **outdoors**. There is no 3D view, no floor level, no indoor map, no indoor or
  cross-building route, and no accessible option that avoids stairs.
- Because the map is a *look-alike* image rather than a geo-referenced model, it can
  say which way to walk, but not what the destination looks like or which floor the
  room is on.

**The gap.** A guest standing on the campus asks three things: *which building,
which floor, and how do I walk there with the stairs I can actually use?* 

### 1.2 Aim

The aim is to replace the flat, image-based directory with one 3D, cross-platform
map that answers the whole question — *“where is this place, and how do I get
there?”* — from the street, into the building, up to the right floor and to the door
of the room: on any device, and with or without a network
connection.

### 1.3 Objectives

| # | Objective |
| :-- | :--- |
| O1 | Build a responsive 3D campus model from open GeoJSON footprints, with real building heights and per-floor levels. |
| O2 | Provide one search index over building names, block codes, room codes, points of interest and coordinates. |
| O3 | Deliver outdoor positioning and pedestrian routing on campus, and floor-aware indoor navigation inside connected buildings. |
| O4 | Join outdoor and indoor routing so one request covers the whole journey, door to door. |
| O5 | Offer routes that a user can actually walk, including an accessible mode that avoids stairs. |
| O6 | Keep the core map usable on every target platform and in 15 languages, without depending on a live network connection. |

### 1.4 What this project improves

| Observed today | What this project does instead |
| :--- | :--- |
| Campus drawn as a flat 2D image over Google Maps; height, block and floors invisible | 56 footprints extruded to their real height and split into floor slabs, so the model matches the building |
| Pins drawn on the image; part of the campus missing, placement not tied to real coordinates | 331 official POIs in 16 categories, all keyed to real coordinates, plus block codes (A / Z / VA / PQ …) |
| No search over buildings, block codes or room codes | One search index over building names, block codes, room codes, POI names and coordinates |
| No “where am I” | GPS position with accuracy circle, compass and follow-me mode |
| Address shown, but no way to walk there | Outdoor pedestrian routing, with a straight-line fallback when no road route exists |
| Indoor space missing; “PQ304” cannot be located | Floor plans for MMW / Core P / Core Q (9 levels) and Core T (6 levels); selecting a room shows its floor, photo and details |
| No indoor or cross-building route | A* over a graph keyed by level and node, crossing floors and connected blocks |
| Stairs are the only option | Accessible mode that removes stair edges and prefers lifts and ramps |
| Server-dependent app, offline during maintenance windows | Core map, search and indoor routing run from bundled data with no live network |

### 1.5 Scope

- **Core** (must ship): 3D campus view, floor levels, POIs, unified search,
  outdoor positioning, outdoor walking routes, indoor navigation,
  outdoor↔indoor handover.
- **Secondary** (if time allows): route summary, nearest-facility routing, more
  indoor venues, editor release, offline pack, login and user roles, map data
  server.
- **Out of scope**: Wi-Fi indoor positioning (accuracy cannot be guaranteed with
  consumer hardware, so it is covered by a feasibility note instead)

---

<!-- SCREENSHOTS -->
## Screenshots
<img width="1112" height="812" alt="Screenshot 2026-09-10 at 2 28 19 AM" src="https://github.com/user-attachments/assets/9b817187-8393-4867-88ce-726faff87938" />

<img width="1112" height="812" alt="Screenshot 2026-09-10 at 2 26 55 AM" src="https://github.com/user-attachments/assets/f8625aa7-74cc-4786-9145-8946f6889c9f" />

<img width="1112" height="812" alt="Screenshot 2026-09-10 at 2 29 11 AM" src="https://github.com/user-attachments/assets/11ab2652-9e27-45e3-a319-f174cb6af979" />

<img width="1112" height="812" alt="Screenshot 2026-09-10 at 2 27 27 AM" src="https://github.com/user-attachments/assets/85264b9c-4de5-4322-9693-b05ad4ecc567" />

## 2. Main Features

Status: ✅ done · 🚧 in progress · 🔜 to do · ⏳ planned later · ❌ out of scope

| # | Feature | What it provides | Status |
| :-- | :--- | :--- | :-- |
| 1 | 3D campus view | 56 footprints extruded to their real height, pan / zoom / tilt, block codes (A / Z / VA / PQ …) | ✅ |
| 2 | Floor levels | Every building split into floor slabs, per-floor picking and highlight | ✅ |
| 3 | Points of interest | 331 official POIs in 16 categories with 297 photographs, category filter, photo info card | ✅ |
| 4 | Unified search | Building names, block codes, room codes, POI names and coordinates in one search bar | ✅ |
| 5 | Outdoor positioning | GPS fix, accuracy circle, compass, follow-me mode (demo walk when there is no fix) | ✅ |
| 6 | Outdoor walking routes | On-campus pedestrian route, dashed straight-line fallback when the route leaves the campus | 🚧 |
| 7 | Indoor navigation | A* on a cross-floor graph — MMW / Core P / Core Q (9 levels) and Core T (6 levels); accessible mode avoids stairs | 🚧 |
| 8 | Inside + indoor view | 3D shell and indoor plan on one screen with one toggle button, shared floor bar and toolbar; picking a floor highlights the slab in 3D and switches the plan | ✅ |
| 9 | Outdoor↔indoor handover | One route from a point on campus to a room on a given floor, crossing buildings | 🚧 |
| 10 | Route summary | Distance, estimated walking time, turn-by-turn indoor steps | 🔜 |
| 11 | Nearest-facility routing | Quick route to the nearest toilet, water station, lift or canteen | 🔜 |
| 12 | Indoor data expansion | Further venues beyond MMW/PQ and Core T | 🔜 |
| 13 | Indoor editor | Floor-plan and graph authoring with JSON import/export and undo (development tool) | 🚧 |
| 14 | Offline pack | Tile and data cache after one download | 🔜 |
| 15 | Multi-language | 15 locales with switching at runtime | ⏳ |
| 16 | Login and user roles | Student, guest and administrator accounts | ⏳ |
| 17 | Map data server | Server-side storage of the GeoJSON and JSON files | ⏳ |
| 18 | Wi-Fi indoor positioning | Accuracy cannot be guaranteed with consumer hardware — documented as a feasibility note | 🔜 |

## 3. Work Plan (24 September 2026 – 21 April 2027)

### 3.1 Stage 1 — Navigation core (W1–W4, 24 Sep – 21 Oct)

| ID | Task | Weeks (dates) | Deliverable | Status |
| :-- | :--- | :--- | :--- | :-- |
| T-06 | 3D shell and indoor plan in one view: mode toggle, shared floor bar and toolbar, floor highlight in both | W1 (24–30 Sep) | `BuildingInsideView` | ✅ |
| T-07 | Outdoor positioning: real-device GPS validation, accuracy circle, compass and follow-me | W1 (24–30 Sep) | Live location layer | 🚧 |
| T-08 | Outdoor pedestrian routing: campus-bound validation, straight-line fallback, route styling | W1–W2 (24 Sep – 7 Oct) | Route layer + fallback | 🚧 |
| T-09 | Destination resolution: room code or POI → entry door and level | W2–W3 (1–14 Oct) | Room lookup service | 🔜 |


### 3.2 Stage 2 — Door-to-door routing (W5–W10, 22 Oct – 2 Dec)

| ID | Task | Weeks (dates) | Deliverable | Status |
| :-- | :--- | :--- | :--- | :-- |
| T-11 | Outdoor→indoor handover: one door-to-door route | W5–W7 (22 Oct – 11 Nov) | Cross-building route demo | 🔜 |
| T-12 | Route summary: distance, walking time, turn-by-turn indoor steps | W6–W8 (29 Oct – 18 Nov) | Route panel | 🔜 |
| T-13 | Nearest-facility routing: toilet, water station, lift, canteen | W8–W9 (12–25 Nov) | Quick-route menu | 🔜 |
| T-14 | Indoor data expansion: further venues and level review | W9–W10 (19 Nov – 2 Dec) | Extra venue JSON + tests | 🔜 |

### 3.3 Stage 3 — Content, offline use and hardening (W11–W15, 3 Dec – 6 Jan)

| ID | Task | Weeks (dates) | Deliverable | Status |
| :-- | :--- | :--- | :--- | :-- |
| T-15 | Editor release: JSON import/export, validation, undo | W11–W12 (3–16 Dec) | Editor release | 🔜 |
| T-16 | Offline pack: tile and data cache after one download | W12–W13 (10–23 Dec) | Offline mode | 🔜 |
| T-17 | Multi-device testing: Android, iOS, macOS, Windows, web | W13–W14 (17–30 Dec) | Test report and fixes | 🔜 |
| T-18 | Interim package: progress report, demo build and evaluation plan | W14–W15 (24 Dec – 6 Jan) | Interim report + demo | 🔜 |


## 4. Progress to Date (21 September 2026)

- **Data** — 331 official POIs in 16 categories with 297 photographs; 56 building footprints with heights.
- **3D view** — footprints extruded to their real height, floor levels, pan, zoom, tilt and block codes.
- **POIs** — category filter and an information box with photograph, name and address.
- **Search** — building names, block codes, room codes, POI names and coordinates.
- **Outdoor positioning** — GPS with accuracy circle, compass and follow-me mode.
- **Outdoor routing** — pedestrian route with a straight-line fallback when the route leaves the campus.
- **Indoor navigation** — floor-aware graph and A* routing across floors and connected blocks, with an accessible option that avoids stairs.
- **Indoor floor plans** — MMW / Core P / Core Q (9 levels) and Core T (6 levels), plus an editor for authoring plans and graphs.
- **Inside view** — 3D shell and indoor plan in one screen, sharing one floor bar and toolbar.
- **Languages** — 15 locales with switching at runtime.
- **Verification** — 10 offline test files (≈ 1,500 lines) covering data parsing, the floor model, the editor and the indoor router.

## 5. Project Structure

```
lib/
  main.dart                     app entry, localisation setup
  ContentView.dart              shell and navigation between views
  Widgets/Maps/
    MapLibre3DView.dart         3D campus map: buildings, POIs, search, GPS, outdoor routing
    BuildingInsideView.dart     3D shell + indoor floor plan in one screen
    BuildingFloors.dart         floor model and per-building overrides
    BuildingFloorEditor.dart    floor count / floor height editor
    BuildingShortCodes.dart     block codes (A / Z / VA / PQ …)
    PolyUCampusData.dart        POI data, categories and room codes
    Indoor/
      IndoorModels.dart         venue, level, area, node and route models
      IndoorRepository.dart     asset + override loader, room-to-POI lookup
      IndoorRouter.dart         A* over the level|node graph
      IndoorFloorMapView.dart   floor plan, room picking, indoor navigation
      Editor/                   floor-plan and graph editor
assets/
  polyu_buildings.geojson       56 footprints with heights
  polyu_pois_full.json          331 POIs with photographs
  indoor/<venue>/building.json  indoor venue data per level
  poi_images/, translations/    POI photos, 15 locales
  sounds/
tools/
  gen_indoor_skeleton.py        indoor venue skeleton generator
  indoor_geom.py                geometry helpers
  preview_venue.py              preview a venue JSON in the browser
test/                           10 offline test files (≈ 1,500 lines)
```

---

## Built With

- flutter_map: ^8.2.2
- flutter_map_vector_tiles: ^2.8.1
- latlong2: ^0.9.0
- permission_handler: ^12.0.1
- geolocator: ^14.0.2
- flutter_mjpeg: ^2.0.4
- flutter_tts: ^4.2.5
- window_manager_plus: ^1.0.5
- cupertino_icons: ^1.0.8
- speech_to_text: ^7.0.0
- dio: ^5.4.0
- audioplayers: ^6.5.1
- image_picker: ^1.0.7
- dartssh2: ^2.13.0
- file_picker: ^10.3.10
- shared_preferences: ^2.2.2
- path_provider: ^2.1.4
- flutter_webrtc: ^1.3.1
- flutter_blue_plus: ^1.34.5
- local_auth: ^2.3.0
- flutter_secure_storage: ^9.2.2
- easy_localization: ^3.0.7
- invert_colors: ^0.0.4
- flutter_rfb: ^0.6.2
- maplibre: ^0.3.3+2
- maplibre_webview: ^0.3.5
- flutter_polyline_points: ^3.1.0
- webview_flutter: ^4.14.1

## Data Sources

- Official PolyU campus map — https://www.polyu.edu.hk/campus-map/
  - POI API: `https://www.polyu.edu.hk/public-data/POIs?channel=campus-map&category=…`
  - Photos: `https://www.polyu.edu.hk/campus-map/img/…`
- Building footprints: `assets/polyu_buildings.geojson`
- POI data (with images): `assets/polyu_pois_full.json`
- Photos: `assets/poi_images/<category>/…`

## References

[1] README template — https://github.com/othneildrew/Best-README-Template/tree/main

[2] The Hong Kong Polytechnic University, “PolyU Campus Map.” — https://www.polyu.edu.hk/campus-map/ (accessed 21 Sep 2026)

[3] PolyU Information Technology Services Office, “Campus Wayfinding Service” (POLY:U:WAY, route, distance, walking time and street view on a PolyU look-alike map; maintenance windows). — https://www.polyu.edu.hk/its/it-services/campus-wayfinding-service/ (accessed 21 Sep 2026)

[4] POLY:U:WAY on Google Play — 1,000+ downloads, 7 reviews, 4.4★. — https://play.google.com/store/apps/details?id=com.wayfind (accessed 21 Sep 2026)

[5] POLY:U:WAY on the App Store, id 1437595452 — 6 ratings, 4.3★, version 1.10.0, first released 2018. — https://apps.apple.com/hk/app/poly-u-way/id1437595452 (accessed 21 Sep 2026)

[6] PolyU Campus Map POI data service. — https://www.polyu.edu.hk/public-data/POIs?channel=campus-map

[7] MapLibre GL — open-source mapping library. — https://maplibre.org/