# ElRobot2-S1 build sequence and test plan (v2.3, 2026-10-02)

Status: v2.3 (2026-10-02) changes the plan after the user removed glass and returnable containers from scope and stated that a store visit, a scale with real containers and a tilt-friction test are not available for now. (a) Glass and returnable containers are out of the plan (not deferred). (b) Phase S and tests A5-A7 become optional and non-blocking; the design runs on desk-research values (cooler_envelope_evidence.csv, design_params v0.4.3: shelf pitch 0.287-0.381 m with 0.318 m design-driving, lane 75 mm, wet pad friction 0.3 for printed TPU). (c) The v1 gripper stack stands 140 mm above the object and cannot serve a 600 mL or larger PET bottle in the standard racks found; Phases B and E therefore test the v1 gripper on fixture v1.1 (open racks, 0.44 m pitch) as a development tool only, and the new Phase H tests the S2 end effector (offset wrist, neck-jaw, flat clamp; S2_design_brief.md). Everything else equals v2.2 (which added Phase S, A7 and glass/returnable friction), v2.1 and the 2026-10-01 draft. It reflects the corrected clamp forces (34.7 N rated per finger), the Z module v0.6.1 (4040 column, tie rods, T8x4 screw, brake motor on 24 V, camera raised 38 mm), the arm and gripper as built in CAD (rev 4), fixture v1.1 (level pitch 0.44 m), the master assembly checks on the real CAD and the final MuJoCo rerun on fixture v1.1 (bay rule recommended; the wide-lane set is not). Numbers marked (sim) come from the MuJoCo model, (calc) from hand calculations, (FE) from the voxel finite-element checks, (CAD) from boolean checks on the real CAD meshes. Everything marked VERIFY is an assumption that a caliper, scale or bench test must confirm before it is trusted. Nothing in this project has been printed or built yet.

## Safety rules for every session

1. Hands stay out of the arm envelope while any rail is powered. The Z E-stop (mushroom button, normally closed) cuts the 24 V rail (stepper, brake); the 12 V servo bus stays alive on purpose so the arm holds its pose. Press it once with an empty carriage before the first loaded run (the brake must lock, the arm must hold).
2. First motion of each session at 20 % speed and 40 % torque limit. Servo torque limits: gripper 1.177 N*m (about 400 on the 0-1000 register, VERIFY the register scaling in test B2), J3 1.0 N*m, J1/J2 at most 40 % of their rated torque during the first runs.
3. Never test the Z column with a hand on the carriage. Never run it unattended until test C3 and the brake test have passed.
4. The column base (400 x 400 x 18 mm plywood) carries 6.5 kg (rear half) to 10.8 kg (centre) of ballast; bolt it to the common baseboard or clamp it to the table before the arm is extended.
5. Keep water away from the PSUs and terminals (wet bottles are part of the test plan). Fuse every rail (12 V rail at the supply, 24 V rail T3.15 A).

## Phase S (optional, not available now). Real cooler survey

| ID | Test | Procedure | Record | Pass |
|---|---|---|---|---|
| S1 | Survey of one real rear-loaded cooler (optional) | Fill in hoja_levantamiento_refrigerador_OXXO.pdf at one store if a visit becomes possible (shelf pitch A and clear gap B on three shelves, corridor F, access door G, glide lane width, photographs). Not required for any design step. | The filled sheet and the photographs | Replaces the desk-research pitch range 0.287-0.381 m with a measurement. The sheet's thresholds (430 / 410 / 310 mm) describe the v1 stack only; with S2 the thresholds are 287 mm (600 mL, neck-jaw), 322 mm (600 mL, flat clamp) and 208 mm (355 mL can, flat clamp). F at least 650 mm and G at least 450 mm wide: the robot fits. |

## Phase A. Coupons and measurements before the large prints (about 6 h of printing; A5-A7 optional)

| ID | Test | Procedure | Record | Pass |
|---|---|---|---|---|
| A1 | Servo pocket coupons (STS3215 gripper coupon, STS3095 arm coupon) | Print both coupons (arm_coupon_sts3095, gripper_coupon_sts3215). Seat a real servo in each. Measure body, shaft position from the horn-side face and the side wall, horn bolt pattern and spline depth with a caliper. | servo_dims.json, each value to 0.1 mm | Servo drops in by hand with no force and no rocking above 0.3 mm. Shaft offset within 0.5 mm of the model (STS3215 12.5 mm from the horn-side end, STS3095 17 mm). Otherwise edit the named parameter and rebuild (all parts are parametric). |
| A2 | Bearing seat coupons | fixture_coupon_bearing_seats (6806 / 6701 / 624) and a D22.1 ring for the 608. Seat diameters nominal +0.00 / +0.10 / +0.20 mm, production settings (6 walls, 0.2 mm layers). | Seat diameter per bearing | Light thumb pressure, spins freely, no perceptible radial play. The 6806 seat is the critical one (J1 and J2 carry the whole 12.2 N*m tilt moment). |
| A3 | Gear mesh coupon | Print the pinion (flange side down) and one rack at production settings. Turn the pinion by hand against the rack over the full 42.5 mm stroke. | Binding positions, backlash by dial indicator | Free over the whole stroke, backlash 0.1-0.4 mm. The CAD minimum flank gap is only 0.08 mm; printed gears usually need more. The servo pocket has +-2 mm adjusters to tune the centre distance; if it binds at the adjuster limit, thin the pinion teeth by 0.1 mm (named parameter). |
| A4 | Mass check | Weigh every printed part and assembly. | Mass table | Gripper 0.44 kg total, L1 side 0.627 kg, L2 side 0.303 kg, J1 turret 0.555 kg (Z-module estimate), carriage group 0.238 kg: each within 15 %. The printed gripper is 0.3355 kg against a 0.25 kg target (over budget, no lightening done). |
| A5 | Pad friction (optional, deferred) | Glue TPU pad stock to a flat board, tilt a clean dry PET bottle on it until it slips, repeat wet. Needs only a bottle and a board. | mu dry, mu wet | Dry at least 0.5 and wet at least 0.3. The v1 simulation assumes 0.8 dry and 0.4 wet; the desk-research design value for printed TPU 95A is 0.3 wet (an inference: published values are all dry, 0.2-1.0, on other surfaces; nothing found for wet PET at 4 C). Until A5 is done, PET at 1 L needs cast PU or silicone pads or the neck-jaw, and the clamp limits stay as designed (gripper_clamp_limits.csv, Figure 3, payload_mass_scenarios.csv). |
| A6 | Real SKU measurements (optional, deferred; cans and PET only) | Caliper D and H of any bottles and cans at hand (500 mL, 600 mL, 1 L, can) and, for S2, the PCO-1881 neck: support-ring outside diameter, ring thickness, free neck below the ring, cap top to ring underside. Weigh with any scale if available; density = (full - empty) / poured volume. Enter everything in container_data_sheet.csv. | bottle_sku_envelope.csv, container_data_sheet.csv | The 1 L diameter and base cup are estimates (D76-82; the simulation assumes a base cup 0.94 D that fits a D75 pocket only up to D 79.3 mm). The neck values replace the assumed ring OD 33 mm and the 4 mm free neck in the S2 brief (test H1). |
| A7 | Grip table from measured masses (optional, deferred) | Recompute payload_mass_scenarios.csv with measured masses and the A5 friction. | Updated payload_mass_scenarios.csv | Every friction-clamped SKU needs F = 2 m (g + 0.5 g) / (2 mu) at most 41.7 N per finger at the wet friction (about mu >= 0.353 m per kg); SKUs above that are rejected by the planner, not gripped harder. The 1 L is commanded at the cap when wet. |

## Phase B. Gripper bench (half a day; the v1 gripper is a development tool, see Phase H for PET in standard racks)

| ID | Test | Procedure | Pass |
|---|---|---|---|
| B1 | Gap calibration | Close on the calibration cylinders (D66, D71, D89, D105 dummies printed with the gripper) and on empty. Record servo ticks and caliper gap; fit a line (2309 ticks span 40-125 mm, 202.9 deg of pinion rotation). | Fit residual at most 1.0 mm over 40-125 mm; repeat closing to the same cylinder within 0.5 mm. |
| B2 | Clamp force and torque-limit register | Squeeze a luggage-scale or kitchen-scale force gauge at 20, 40, 60, 100 % of the commanded torque limit; find the register value that gives 34.7 N and 41.7 N. | Force at the rated setting within 20 % of 34.7 N per finger (calc); monotonic with the register; the 41.7 N cap (1.2 x rated) is the highest value ever commanded in normal use. Stall (104.2 N) is covered only by this cap: pinion tip-loading stall margin is 0.89 (FAIL) at b = 14 mm without it. |
| B3 | Hold test | 600 mL bottle at 25 N, can at 15 N, 1 L at 34 N (command limits); lift 0.2 m and shake horizontally at about 0.5 g 20 times; repeat wet. | At most 2 mm slip, no drop, 20 of 20 dry. Wet 1 L needs 39 N against the 34 N default limit (design_params v0.4.2 commands the wet 1 L at the 41.7 N cap, and the printed-pad design friction of 0.3 would need 53 N): record where it fails; the 1 L is a neck-jaw or cast-pad case. 1.5 L and 2 L are burst-only (47 / 63 N, above the cap) and are not part of v1 acceptance. |
| B4 | Finger deflection and backlash | Dial indicator on a fingertip while clamping the D105 dummy at 34.7 N. | Tip deflection at most 3 mm (calc 2.6 mm at rated). Rack backlash at most 0.5 mm. |
| B5 | Endurance of the gear train | 1000 open-close cycles at 25 N on the D71 dummy. | Backlash growth at most 0.2 mm, no tooth cracking at the pinion root or the 1.23 mm neck web (the neck web is the known thin section). |

## Phase C. Z module (one day, after the column is assembled)

| ID | Test | Procedure | Pass |
|---|---|---|---|
| C0 | Static checks | Measure the carriage plate flatness, rail parallelism to the column (dial indicator), screw straightness, coupler gap (1.0 mm designed between screw end and motor shaft end). | Rail parallel within 0.2 mm over 0.8 m; no binding by hand over the full stroke. |
| C1 | Speed ramp and step loss | Payload 4.5 kg total (arm mass plus a 2.1 kg dummy). Run the full stroke at 15, 25, 40 mm/s (never above 40) with 5 strokes loaded and empty each, 24 V supply. | Home switch repeats within 0.1 mm, no stall, no resonance band, motor case below 70 C. Set the cruise speed to 0.75 x the last good value. The simulation shows Z speed changes the cycle time by only 5 % (24.3 s at 25 mm/s, 23.0 s at 50 mm/s). |
| C2 | Repeatability | Dial indicator on the carriage at 0.37, 0.70, 1.00 m, approach each position 10 times from above. | Standard deviation at most 0.2 mm. |
| C3 | Power-off hold, both layers | (a) Brake released but motor unpowered, 4.5 kg on the carriage at mid-height, watch 5 minutes (detent plus thread friction layer, T8x4). (b) Brake engaged, same load, 5 minutes. | (a) Drift at most 2 mm; if it creeps the brake is the only holding layer and the arm must never run unattended. (b) No motion. The brake is the guaranteed layer; the T8x4 detent layer is an assumption (25 + 10 mN*m, margin 1.8 at mu 0.05). |
| C4 | Brake transient | Run at 40 mm/s, cut the 24 V rail with the E-stop. Inspect the carriage plate and nut ledge for cracking; repeat 10 times; then load the nut ledge statically to 170 N. | No cracks, no permanent offset. The scaled worst case is 108-172 N on the nut for 1-2 ms (plate 6.4-10.2 MPa across layers against a 5 MPa sustained allowable; not a dynamic FE). In normal stops the controller ramps to zero before the brake sets. |
| C5 | Column lean and stiffness | Hang 2 kg at 0.40 m from J1 with the carriage at the top of the stroke (1.06 m); dial indicators at the carriage and at the column top, forward and sideways, with and without the two tie rods. | Tip deflection at most 2.5 mm without rods, at most 1.5 mm with rods (calc: 4040 alone 2.45 mm worst case, 1.36 mm with rods; joint and foot stiffness are assumptions). Record the rod joint stiffness. |
| C6 | Overturning | Arm extended 0.385 m, 2.1 kg payload, push the carriage top sideways with about 20 N. | No lift-off of the base with the stated ballast (6.5 kg rear half); add ballast otherwise. |
| C7 | Camera clearance | Sweep J1 from -20 to 160 deg and J2 from -130 to +130 deg with no payload at three carriage heights, looking at the camera and bracket. | At least 3 mm clearance everywhere in the operating annulus. CAD (boolean sweep, v0.6.1): the lowest camera point is 7.3 mm above the L1 cover top and no gap below 3 mm was found between the operating arm chain and the Z module (smallest designed gap 4.0 mm, J1 pedestal top to the camera bracket foot). With the earlier v0.6 position the camera overlapped L1 at J1 = -20 deg. |

## Phase D. SCARA arm (one day, on the carriage or bolted to a bench)

| ID | Test | Procedure | Pass |
|---|---|---|---|
| D1 | Range and cable check | Command J1 from -20 to 160 deg, J2 +-130 deg, J3 +-180 deg slowly with no payload. | No collision, no cable strain, no tight spot in the bearings. Cable routing is not designed in CAD (open item): plan the loops at the J1/J2/J3 windows. |
| D2 | Open-loop accuracy | 3 x 3 grid (80 mm pitch) in the reach annulus; photograph the tip with the ArUco board. | RMS error at most 4 mm after fitting link lengths and zero offsets. Backlash of 0.5 deg is 3.4 mm at 0.385 m. The lane funnel tolerates about 10 mm (sim). |
| D3 | Link stiffness and cover bond | Hold 2.4 kg at full reach 0.385 m with the arm folded straight (q2 = 0) and with q2 = 60 deg. Dial indicator at the gripper body. | Vertical droop at most 4 mm straight and at most 6 mm at q2 = 60 deg. Calc: 3.6 mm if the L1 cover acts as a bonded lid, 36-43 mm if not. The independent FE (lead) gives a closed-box twist of 1.16 mrad per N*m against at least 24 mrad per N*m for the open U (21 times softer; the open value is not converged). Bond the cover with a CA or epoxy bead plus all 14 screws; if the droop exceeds the limit, add stiffening before any bottle test. |
| D4 | Current and temperature | 100 pick-place moves with a 600 mL bottle (or a 0.65 kg dummy). Log servo temperature, load and supply current. | Servo temperature below 55 C, no overload faults, peak 12 V supply current below the fuse rating (modelled peak 4.4 A, STS3095 stall 9.8 A each, so keep the torque limits). |
| D5 | Camera and J1 low limit | Command J1 to 0 deg and -20 deg with L2 folded; photograph the clearance to the Orbbec camera. | At least 3 mm (see C7). |

## Phase E. Integrated pick-and-place (two days; v1 gripper on fixture v1.1, open racks at 0.44 m pitch)

These tests show that the arm, Z module, planner and v1 gripper work together on the test fixture. They say nothing about a real cooler: with the desk-research shelf pitch of 0.287-0.381 m the v1 stack cannot enter a shelf tunnel over a 600 mL or larger PET bottle (Figure 9 of the report), which is why Phase H exists.

Order of configurations: (1) level 1 only, 5 lanes (90 mm pitch) with the bay rule, 600 mL; (2) level 0 with the bay rule; (3) mixed SKUs on both levels; (4) optional, only if you want to learn from it: the 3-lane wide set (125 mm pitch) with the deeper back-out, which the simulation does not recommend for 600 mL. Bay rule = drop a bottle into a lane only while slots 0 and 1 (the first 160 mm) of both neighbouring lanes are empty; at most lanes 1, 3, 5 are then full and lanes 2, 4 hold 3 bottles (21 of 25 slots).
Level 0 limits (CAD boolean and MuJoCo agree within 0.4 mm): free lift at the place pose is 21.6 mm in lane 1 and 38.5-39.2 mm in lanes 3 and 5 for the 600 mL; the 11 mm tray rim plus 2 mm needs 13 mm. Tote cell (-300, 0) is unusable for 500 mL and 600 mL (works for cans as the last pick). The 1 L is impossible at level 0 (the L2 cover enters the level-1 plate; it needs a level pitch of at least 461 mm in lanes 2-5 and 486 mm in lane 1).

| ID | Test | Procedure | Pass |
|---|---|---|---|
| E1 | ArUco calibration | Markers 20-23 (80 mm) on the fixture; solve camera-to-base at three Z heights. | Marker centre error at most 3 mm after calibration. |
| E2 | Ten-cycle 600 mL, level 1 | Tote to lane 3 on the 5-lane set, bay rule. | At least 9 of 10 placed in the correct lane; no contact on retreat; cycle time at most 30 s (sim 19.9 s with the bay rule, 24.3 s with the default retreat, Z at 25 mm/s). |
| E3 | Retreat rule check | Same on the 5-lane set: (a) bay rule, (b) default retreat with a bottle in slot 0 of the neighbour lane, (c) fully populated lanes. | (a) at least 9 of 10 clean retreats (sim 45/45). (b) expected to touch the neighbour in most cases (sim 16/45 contact-free). (c) expected to fail: the open gripper lies inside the footprint of the slot-1 bottle (sim 0/45, pad stall forces up to 1.2 kN in the model); do not run (c) with a full lane load. |
| E4 | Level 0 | 600 mL into lanes 1-5, tote cells except (-300, 0); cans into every cell. | At least 9 of 10 (sim 60/60 Monte Carlo, lane 1 20/20); the rim of the tote tray (11 mm above the pocket floor) is cleared with the 25 mm carry lift (lane 1 allows 19.7 mm with the planner's margin). |
| E5 | Mixed 12-bottle run, two levels | 4 cans, 4 x 500/600 mL at level 0 and 1; 1 L only at level 1 (lane clear width 87 mm leaves only 2.5-5.5 mm per side: expect divider contacts, sim 52/60 at 3 mm pick error). | At least 11 of 12 placed, zero drops, zero lane jams. |
| E6 | Wet bottles | Bottles taken out of a fridge so that they sweat. | At least 9 of 10 for 600 mL; record the largest SKU that holds (wet 1 L needs 39 N, above its 34 N limit). |
| E7 | Lane flow | Slide five 600 mL bottles into an empty lane at 6 deg with PTFE tape on the floor. | Every bottle reaches the end stop within 5 s without toppling. Simulation: only mu 0.10 at 6 deg flows without toppling (3.4 s); mu 0.05 topples at every slope, mu 0.2 does not flow. Measure the lane friction first and choose the shim angle (4-10 deg available) from it. |
| E8 | Fault recovery | Topple one bottle in the tote and one in a lane. | Safe stop; an operator clears it in under 2 minutes using the leader arm. |

## Phase H. S2 end effector (offset wrist, neck-jaw for PET, flat clamp for cans)

Start after the S2 CAD and the S2 simulation exist (S2_design_brief.md, section 7). Hardware needed: Z module and arm (Phases C and D), the S2 parts, a rack mock-up with adjustable pitch (0.29-0.44 m in 10 mm steps) and 75 mm lane inserts, and PET bottles from any shop (500 mL, 600 mL, 1 L). All H tests run with the 20 % speed rule.

| ID | Test | Procedure | Pass |
|---|---|---|---|
| H1 | Neck geometry | Print a PCO-1881 neck stub (ring 33 mm, assumed). Measure 5-10 real bottles: ring outside diameter, ring thickness, free neck below the ring, cap top to ring underside. Adjust the jaw notch (30 mm assumed) and the shims. | Every bottle: ring OD 32-34 mm, free neck below the ring at least 4 mm (3 mm jaw). Bottles that fail are listed and sent to the flat clamp. |
| H2 | Neck-jaw hold | Lift a 600 mL and a 1 L bottle by the ring 10 times at 0.3 g, carry 0.3 m, wet and dry. | No slip off the ring ledges, swing amplitude below 5 deg, no cap or ring damage, 10 of 10. |
| H3 | Place into a 75 mm lane | Rack mock-up at pitch 0.318 m, 600 mL bottle, wrist-camera feedback, 10 attempts per lane in lanes 1, 3 and 5. | At least 9 of 10 placed without touching the lane dividers or the shelf above (clearance 3.6-5 mm per side against servo backlash 3.4 mm at 0.385 m). If below 9 of 10 after two design iterations, build the push-in transfer plate. |
| H4 | Tote to lane cycle | Single-row tote tray (pitch at least 85 mm), 500 mL and 600 mL, 20 cycles at pitches 0.318 m and 0.287 m. | No drop, no collision with the tote tray, cycle time recorded. At 0.287 m the margin is zero by the model: record the minimum pitch that works. |
| H5 | J3 moment | Hang 1.3 kg at 100 mm from the J3 axis (1.25 N*m, includes the tool) for 10 minutes; dial indicator at the bar tip. | Deflection at most 1 mm, play at most 0.3 deg, no loosening of the bearing housing. |
| H6 | Flat clamp, cans | 355 mL and tall cans into 75 mm lanes at pitch 0.318 m, 20 cycles. | No drop, no collision. Gate: if neither the neck-jaw (H3) nor the flat clamp passes at 0.318 m, build the push-in plate before any further CAD. |

## Phase F. Cold environment (one evening)

| ID | Test | Procedure | Pass |
|---|---|---|---|
| F1 | Fridge soak | Gripper, one STS3215 and the Gemini 2 in a household fridge (about 4 C) for 2 h, operate there. | Camera streams after the soak, servo moves without error, wet pad friction at least 0.3. |
| F2 | Condensation | Move the cold camera to a warm room, watch lens and connector for 30 minutes. | Know the fogging time. The camera is specified 0 to 40 C non-condensing: keep it in the cold or enclose it. |

## Phase G. Data and teleoperation (later)

| ID | Test | Procedure | Pass |
|---|---|---|---|
| G1 | Teleop with the leader arm | Map leader joints to task-space targets (x, y, z, yaw, gripper gap). | An operator completes a pick-place with 3D camera feedback. |
| G2 | Dataset | Record 50 episodes in LeRobot format with the Gemini 2 and the wrist camera. | Dataset replays; ACT or SmolVLA baseline can be trained later. |

## Order of work

1. Order the long-lead items on the first batch (rail, 4040 column, screw, brake motor, 24 V and 12 V supplies, 6806 bearings, TMC2209 drivers) the same day; print the Phase A coupons A1-A4 in the evening and measure. A5-A7 and the store survey (Phase S) are optional and do not gate anything.
2. Build the Z column on the common baseboard and run Phase C; the brake and the speed ramp decide whether unattended operation is ever allowed.
3. Print the arm (927 g printed, about 27 h +-30 %), bond the L1 and L2 covers, run Phase D.
4. Optional: print the v1 gripper (335 g) as a development tool and run Phase B; it serves cans in open racks and Stage 0 demos with the AM-ARM200.
5. Fixture: print only the level-1 half (about 3.5 kg, 5-lane set at 90 mm, tote tray, brackets, feet). Hold the level-0 half and the 90 mm lane plates until the rack mock-up with adjustable pitch and 75 mm lane inserts exists.
6. S2: CAD and simulation of the offset wrist, neck-jaw and flat clamp (next design step), then Phase H.
7. Integrate (Phase E on the fixture, Phase H on the mock-up), then F. Only then spend time on G.
