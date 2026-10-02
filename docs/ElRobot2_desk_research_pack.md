# ElRobot2: desk-research pack on racks, containers and grip (2026-10-02)

Status: research summary and design decision. Nothing here was measured in a store or on hardware. Evidence ids (E01-E24) refer to cooler_envelope_evidence.csv; grades are A manufacturer or primary source, B vendor specification with numbers, C reseller, aggregator or forum, D inference.

## 0. Why this pack exists

On 2026-10-02 the user removed glass and returnable containers from the plan and stated that a store visit, a 1 g scale with real containers and a tilt-friction test are not available for now. The unknowns those three items were meant to settle (rack geometry, container size and mass, pad friction) were therefore researched from public sources, and the design was re-checked against the result. The check changed the design: the v1 top-down gripper cannot serve 600 mL or larger PET bottles in standard racks (Section 3), and the recommended next design is S2 (Section 4).

## 1. Findings

### 1.1 Racks

- The "tipo OXXO" chamber sold in Mexico is a reach-in cold room with glass doors of 24 x 75 or 26 x 75 in, 5 shelves per door (6 in some door sets, with 26 x 27 in shelves rated 100 kg), chambers 1 m, 1.5 m or 2 m deep, and a staff access door of 0.90 x 1.90 m (E09-E12). These are resellers' look-alike products (grade C), not OXXO specifications.
- The same door family is a US product line with manufacturer data: Anthony sells five shelves for doors shorter than 79 in and six for doors of 79 in or more, and its 79 in all-glass door comes with seven (E01, E02). A B-O-F gravity-feed rack for 30 in doors is 88 in tall with seven shelves (E05). Dividing the door height by the shelf count gives pitches from 0.287 m (7 in 79 in) through 0.318 m (6 in 75 in), 0.319 m (7 in 88 in) and 0.334 m (6 in 79 in) to 0.381 m (5 in 75 in). Shelf positions are adjustable, so this is a range, not a measurement.
- Beverage racks are gravity-flow and rear-entry: depths 24-42 in, door widths 24-30 in (E04); the shelf is installed with its rear end tilted up (E07). Lanes for 20-25 oz bottles are 74.6 mm wide with a 82.6 mm ring height (E06). The test fixture v1.1 uses 90 mm lanes.
- OXXO staff refill the coolers from the cold room behind them (E15, grade C). Japanese backrooms work the same way, with dividers every 2-3 columns instead of every column (E16).

### 1.2 The only deployed precedent

Telexistence's TX SCARA is a SCARA on a linear axis in the backyard, with joint axes and link length chosen for that space and no change to the room (E17). It moves in a narrow aisle with the arm going up and down and in and out of each shelf, uses a simple pincer, one hand for all beverage shapes, picks the grip point per beverage, and restocks above 98 % autonomously (E18). No dimensions are published, so it constrains the architecture (rear-side SCARA) but not the numbers.

### 1.3 Containers

- Nearly every soda and water bottle has a PCO-1881 neck (E20): finish height 17.0 mm, support-ring height 11.2 mm (E21). The support-ring outside diameter (about 33 mm) was not retrieved and is an assumption.
- Holding PET bottles at the neck is established factory practice. Air conveyors carry them by the neck ring, with the neck in a slot, the ring on the flange margins and the body hanging below (E26; empty bottles). Grippers for filled bottles close around the neck below the cap with pins carrying the weight, claw grippers under the cap head are rated to 15-17 kg, and round, wet or unstable containers are held best at the neck (E22).
- The Mexican 600 mL soda bottle is 235-239 mm tall (design_params source notes); the 1 L size remains an estimate. Mass: density x volume + pack, density spread 4 % (E23).

### 1.4 Pad friction

Every published coefficient found for printed TPU 95A is a dry value against a test counter-surface, not against PET: about 0.2-0.44 in a belt-friction study, 0.47-1.0 in ball-on-disc tests against steel depending on infill, 0.50 for a gradient infill (E24). A patent claims soft cast polyurethane above 1.0 dry and 0.9 wet on floor surfaces (E28, not re-read). Nothing was found for wet PET at 4 C. The v1 assumption of 0.4 wet therefore has no support in the literature, and the design value of 0.3 for printed TPU 95A is an inference (dry 0.4-0.5 reduced by wetting), not a measurement. Cast polyurethane or silicone pads for the 1 L; form closure wherever possible.

## 2. Reference rack used for design

| Parameter | Value used | Range | Basis |
|---|---|---|---|
| Shelf pitch | 0.318 m (design-driving) | 0.287-0.381 m | E01, E02, E05, E09-E11; inferred by even division |
| Structure between bottle base and the shelf above (shelf frame + glide base) | 30 mm | 20-40 mm | ASSUMED; calibrated against the v1 planner (below) |
| Clearance margin | 5 mm | - | design choice |
| Lane width, 600 mL class | 75 mm | 74.6-90 mm | E06 |
| Lane width, 1 L | 90 mm | 85-95 mm | fixture v1.1 |
| Shelf depth | 686 mm (27 in) | 610-1067 mm | E01, E03, E04 |
| Corridor behind the shelves | chamber depth - 0.69 m | 0.3-1.3 m | E09, E12 (1.0-2.0 m chambers) |
| Slope | 6 deg, rear end up | not found | E07 and E06 give the direction (glides sit on angled shelves); the angle was not found |
| Rack height | 1.9-2.2 m | - | E05, E08; the Z range 0.37-1.06 m reaches 2-3 shelves |

## 3. What fits

Model: the smallest pitch at which an end effector fits above an object is p_min = H + T + m + R, where H is the object height, T the structure thickness, m the margin and R the height of end-effector structure above the object top that must enter the shelf tunnel. For the v1 stack R = 140 mm (CAD: L2 cover top 130 mm above the datum, bottle top 10 mm below it).

Calibration: the model with T = 30 mm and m = 5 mm reproduces the v1 planner's smallest level pitches (600 mL 0.412 against 0.407-0.434 m; 500 mL 0.385 against 0.380-0.407 m; 1 L 0.465 against 0.461-0.487 m; 355 mL can 0.298 against about 0.31 m), which is why T = 30 mm is used. Changing T by 10 mm moves every p_min by 10 mm (the error bars in Figure 9).

Gap above a 600 mL bottle (237 mm) and margin by concept, per rack configuration (mm; positive = fits):

| Configuration | Pitch (mm) | Gap above the bottle | v1 gripper (R 140) | S2 flat clamp (R 50) | S2 neck-jaw (R 15) | Push-in plate (R 0) |
|---|---|---|---|---|---|---|
| 7 shelves, 79 in door | 287 | 15 | -125 | -35 | 0 | 15 |
| 6 shelves, 75 in door | 318 | 46 | -94 | -4 | 30 | 46 |
| 7 shelves, 88 in rack | 319 | 47 | -93 | -3 | 32 | 47 |
| 6 shelves, 79 in door | 334 | 62 | -78 | 12 | 47 | 62 |
| 5 shelves, 75 in door | 381 | 109 | -31 | 59 | 94 | 109 |

Results:

1. The v1 gripper cannot serve a 600 mL or larger PET bottle in any of the five configurations (600 mL needs 0.412 m, 1 L 0.465 m). The 500 mL bottle needs 0.385 m, so it fits only in the 5-shelf door (0.381 m) and only if the structure is thinner than about 26 mm. It serves a 355 mL can down to 0.298 m and a tall can (157 mm) down to 0.332 m.
2. A flat body clamp with 50 mm of structure above the object serves a 600 mL bottle from 0.322 m, so 6 shelves in 79 in and 5 in 75 in but not the denser racks.
3. A neck-jaw that keeps 15 mm above the cap serves a 600 mL bottle from 0.287 m (all five configurations, with zero margin in the densest) and a 1 L from 0.340 m.
4. Cans: the flat clamp serves every configuration (355 mL from 0.208 m, tall cans from 0.242 m).
5. A 1.5 L and 2 L bottle need 0.361 and 0.370 m even with the neck-jaw: they sit on the roomier shelves of a rack.

Figure 9 (fig_clearance_vs_pitch.png) shows the smallest pitch per SKU and concept against the vendor pitch range. Figure 10 (fig_s2_vs_v1_side_view.png) shows the 0.318 m case to scale.

## 4. Decision: S2 = offset wrist + low-profile end effectors

Concepts compared:

| Concept | R (mm) | For | Against | Verdict |
|---|---|---|---|---|
| v1 top-down gripper | 140 | built in CAD, simulated, friction clamp for cans | cannot serve 600 mL or larger PET in standard racks | bench tool only |
| S2 flat body clamp | 50 | small step from v1, serves all cans, 600 mL at 6/79 in and 5/75 in | still friction-limited; marginal at 0.318 m | cans and bench |
| S2 neck-jaw | 15 | form closure on the support ring (no friction needed), industrial practice, serves 600 mL in all five configurations | depends on PCO-1881 geometry and free neck below the ring; bottle swing; PET only | primary for PET |
| Push-in from a transfer plate | 0 | any container, insensitive to pitch | extra mechanism on the carriage, alignment to a sloped shelf, pushing over the lip | fallback |
| Suction on the cap | about 30 (inference) | thin | wet and cold surfaces, can lids, pump and tubing | not pursued |

S2 keeps the Z module, the base, J1, J2, L1, L2 and the electronics. It adds an offset bar L3 (100 mm, allowed 85-130 mm) so that J3 and the end of L2 stay in the corridor (stack front edge at y = 230 mm against the tunnel mouth at 253 mm), and a tool that enters the tunnel only with a thin neck-jaw or flat clamp. Reach check: with L3 = 100 mm the J3 axis lies 205-273 mm from J1 for lane pitches of 75 and 90 mm, inside the 170-385 mm reach annulus (L3 above 130 mm leaves the annulus). Static moment at J3: 0.83 N*m for a 600 mL bottle and 1.25 N*m for a 1 L (bearing pair 30 mm apart: 28-42 N per bearing); a 20 x 10 mm PETG bar deflects 2.5 mm under 1.27 kg, so use a 20 x 16 mm section (0.6 mm) or aluminium. Details: S2_design_brief.md.

## 5. The plan without a store visit, a scale or a friction test

1. Design values are the conservative ones in Section 2 and the friction value of 0.3 for printed pads; every one is recorded in design_params.json (v0.4.3) with its grade.
2. Deferred, not blocking: Phase S (store survey), A5 (pad friction), A6 (real SKU measurements) and A7 (grip table from measured masses). They refine values; the design no longer depends on them.
3. Hardware that can proceed now because it does not depend on pitch: Z column and base (Phase C), J1/J2 turret, L1 and L2 (Phase D), electronics, camera calibration (E1), the AM-ARM200 for Stage 0 demos. Hold the level-0 fixture half and the 90 mm lane plates; hold the v1 gripper unless a development tool is wanted for level-1 tests.
4. New in the test plan: Phase H (S2 end effector), starting with a printed PCO-1881 neck stub so that the first jaw test needs no store and no scale.

## 6. What desk research cannot close

- The actual pitch, glide type and slope of OXXO's racks (the vendor range is 0.287-0.381 m).
- The free neck below the support ring and the ring outside diameter on the bottles that matter (jaw thickness 3 mm needs at least 4 mm).
- Wet friction on PET at 4 C and the 1 L size.
- The structure thickness T (assumed 30 mm).

Each of these is closed by a measurement of minutes once a bottle, a caliper or a rack is at hand; Phase H lists them in order.
