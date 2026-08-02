Apollo task1 competition scenarios:
- Scenario set path: `/home/ng/.apollo/resources/scenario_sets/69d52dd10e41e17bc62fda1a`
- All scenario JSON files in this set currently use `mapId: 69d51ffd0e41e1217f2fda19`.
- That map id is present in `/apollo_workspace/data/map_data/Xh_2026_contest/metaInfo.json`; the map files are under `/apollo_workspace/data/map_data/Xh_2026_contest` (`base_map.bin`, `routing_map.bin`, `sim_map.bin`).

Scenario JSON to problem mapping:
- `scenarios/69d420a3b58e96002b6da084.json`: `xh_2026_红绿灯场景_1` / traffic-light problem variant 1. Same map id. Start `(423254.5758, 4438082.2017)`, end `(423258.2216, 4438364.4335)`.
- `scenarios/69d520a3b58e96002b6da083.json`: `xh_2026_红绿灯场景_1` / traffic-light problem variant 1. Start `(423254.5758, 4438082.2017)`, end `(423254.2216, 4438364.4335)`.
- `scenarios/69d520a3b58e96002b6da084.json`: `xh_2026_红绿灯场景_2` / traffic-light problem variant 2. Start `(423258.9878, 4438081.5303)`, end `(423407.7767, 4438246.3800)`.
- `scenarios/69d520a3b58e96002b6da07d.json`: `xh_2026_变道场景` / lane-change problem. Start `(423492.5288, 4437626.4597)`, end `(423259.4830, 4437677.7580)`.
- `scenarios/69d520a3b58e96002b6da079.json`: `xh_2026_S弯场景_1` / S-curve problem variant 1. Start `(424042.26, 4437922.82)`, end `(424272.97, 4437924.33)`.
- `scenarios/69d520a3b58e96002b6da07a.json`: `xh_2026_S弯场景_2` / S-curve problem variant 2. Start `(424041.97, 4437922.99)`, end `(424273.2, 4437923.9)`.
- `scenarios/69d520a3b58e96002b6da07b.json`: `xh_2026_U形弯道场景_1` / U-turn problem variant 1. Start `(424040.2659, 4438479.6127)`, end `(424041.1517, 4438467.4628)`.
- `scenarios/69d520a3b58e96002b6da07c.json`: `xh_2026_U形弯道场景_2` / U-turn problem variant 2 with dynamic traffic. Start `(424040.2659, 4438479.6127)`, end `(424041.1517, 4438467.4628)`.
- `scenarios/69d520a3b58e96002b6da07e.json`: `xh_2026_施工区域通行_1` / construction-zone passing problem variant 1. Start `(423971.7136, 4437617.5600)`, end `(424319.0584, 4437616.7187)`.
- `scenarios/69d520a3b58e96002b6da07f.json`: `xh_2026_施工区域通行_2` / construction-zone passing problem variant 2. Start `(424017.3527, 4437610.0398)`, end `(424293.9127, 4437609.6576)`.
- `scenarios/69d520a3b58e96002b6da081.json`: `xh_2026_站点接驳场景_1` / station pickup/dropoff problem variant 1. Start `(423985.0506, 4438463.4640)`, middle waypoint `(424107.8213, 4438458.3624)`, end `(424170.7755, 4438463.3376)`.
- `scenarios/69d520a3b58e96002b6da082.json`: `xh_2026_站点接驳场景_2` / station pickup/dropoff problem variant 2. Start `(423985.0506, 4438463.4640)`, middle waypoint `(424107.8213, 4438458.3624)`, end `(424170.7755, 4438463.3376)`.
- `scenarios/69d520a3b58e96002b6da080.json`: `xh_2026_环岛让行场景_1` / roundabout yielding problem variant 1. Start `(423377.61, 4438035.97)`, middle waypoint `(423521.0824, 4438016.0632)`, end `(423544.0677, 4438146.1039)`.

Problems and scoring focus:
1. `xh_2026_交通灯场景`
   - Straight driving. On red light ahead, ego must stop before the stop line, 1.5 m to 2.0 m from the line, and must not cross it.
   - Right turn is allowed on red.
   - Penalty: stopping distance outside 1.5-2.0 m costs 20 points.
2. `xh_2026_变道场景`
   - Ego drives in current lane, encounters obstacle or occupied lane, and must change to adjacent lane after checking rear traffic in target lane.
   - Maintain safe distance to front/rear vehicles. Lane-change area speed limit is 30 km/h.
   - Failures/penalties: not completing task in time gives 0; collision gives 0; each 1 m/s overspeed costs 2 points per frame; continuous multi-lane change costs 40 points.
3. `xh_2026_S弯场景`
   - Ego enters narrow continuous S-curve, must pass smoothly, speed limit 30 km/h.
   - If obstacle ahead, assess safety and bypass while keeping safe lateral distance.
   - Failures/penalties: not completing in time gives 0; collision gives 0; each 1 m/s overspeed costs 2 points per frame; insufficient lateral clearance during bypass costs 20 points.
4. `xh_2026_U-Turn场景`
   - Ego enters intersection for U-turn, must complete U-turn inside designated area while yielding to oncoming traffic.
   - U-turn area speed limit 30 km/h. If U-turn lane is occupied by obstacle/construction, bypass and complete U-turn while keeping safe lateral clearance.
   - Failures/penalties: not reaching destination in time gives 0; collision gives 0; each 1 m/s overspeed costs 2 points per frame; lateral clearance below 1 m during bypass costs 20; not yielding to oncoming traffic costs 20.
5. `xh_2026_施工区域通行场景`
   - Ego drives forward, cones indicate road construction, must bypass. Speed limit is 30 km/h in construction area and lifted afterward.
   - Failures/penalties: entering construction area gives 0; passing without speed limit costs 2 points per frame for each 1 m/s overspeed.
6. `xh_2026_站点接驳场景`
   - Ego enters station pickup/dropoff area, must stop precisely in designated station parking spot.
   - Stop smoothly, remain stationary for simulated 5 seconds, then observe surroundings and leave safely.
   - Station area speed limit is 30 km/h.
   - Failures/penalties: not completing in time gives 0; not stopping at station parking spot gives 0; pressing/crossing parking line costs 20; leaving before waiting time costs 20; unsafe departure or collision gives 0; each 1 m/s overspeed costs 2 points per frame.
7. `xh_2026_环岛场景`
   - Ego enters roundabout, must stop at entrance and yield to vehicles inside.
   - After safe, enter roundabout, choose correct lane for target exit. Speed limit inside roundabout is 30 km/h.
   - Before exiting, turn signal must be enabled, rear traffic observed, then exit safely. If obstacle inside roundabout, bypass with safe lateral clearance.
   - Failures/penalties: not reaching destination in time gives 0; collision gives 0; not stopping/yielding at entrance gives 0; each 1 m/s overspeed costs 2 points per frame; lateral clearance below 1 m during bypass costs 20; no turn signal before exit costs 20; wrong exit costs 20.
