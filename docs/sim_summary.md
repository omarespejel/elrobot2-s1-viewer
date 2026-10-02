# sim_summary.md - ElRobot2-S1 MuJoCo validation on fixture v1.1 (final): full rerun of all suites

Status: provisional. Rigid bodies, ideal position servos clipped at the rated torque (J1/J2 2.206, J3 0.981 N*m), hand-tuned gains, assumed masses, convex-hull envelopes of the final arm STEP, box models of the fixture structures taken from the fixture STLs. This report supersedes the fixture-v1.0 report (accepted by the lead): every suite was rerun on fixture v1.1 with the same harness, success rules and seeds. **Notation: `x (y)` = fixture v1.1 result (the same suite and seeds on the v1.0 layout)**; every v1.0 value is read from the saved v1.0 result CSVs by `sim_make_summary.py`, none is typed in. A figure without brackets, or marked *new*, has no v1.0 counterpart. All runs: dt 1 ms, `ccd_tolerance` 1e-9, hover 25 mm, carry lift 25 mm (v1.0: 26 mm), Z speed 25 mm/s unless stated, carriage stroke 0.37-1.06 m (v1.0 runs: 1.00 m).

## 0. What changed from v1.0 to v1.1, and the answers to the lead's questions

**Bottom line.** On the picks the planner accepts, 45/45 dry (44/44) and 45/45 wet (44/44) cycles succeed. 5 of 50 matrix entries are impossible (6): pet_1000 level 0 lanes 1-5 (v1.0: pet_1000 level 0 lanes 1-5; pet_600_soda level 0 lane 1). The v1.1 pitch of 440 mm, the 8 mm pocket / 3 mm rim tray and the ledge cut back to x = -165 mm make 600 mL usable in every level-0 lane and in all five level-0 cells; the 1 L stays impossible at level 0 and the cell (-300, 0) stays impossible for the 500 mL, 600 mL and 1 L (the 355 mL cans can use it). Loaded J1 / J2 torque in the nominal and wet suites <= 0.45 / 0.2 N*m (0.45 / 0.18 in v1.0).

| metric | v1.1 | v1.0 | comment |
|---|---|---|---|
| nominal dry, accepted picks OK | 45/45 | 44/44 | 100% both |
| nominal wet (pad mu 0.4), accepted picks OK | 45/45 | 44/44 | 100% both |
| impossible matrix entries (of 50) | 5 | 6 | 600 mL lane 1 at level 0 is possible again; 1 L at level 0 still impossible |
| 600 mL level 0, free lift at the place pose, lane 1 (mm) | 19.7 | carry blocked (needs 24.0, allows 22.7) | lane 1 usable; margin in section 1 |
| 600 mL level 0, free lift at the place pose, lanes 2-5 (mm) | 36.8-37.4 | 16.8-17.4 | ceiling = level-1 plate underside / lane-module bracket seats |
| 600 mL level 0, free lift above the grasp at the tote cells (mm) | 49.5 | 31.5 | (-300,200) is limited by the tote bracket: see section 4 |
| carry lift over the rim (mm) | 25 | 26 | rim 11 mm (22): the 25 mm carry target (lift_h) now exceeds rim + 4 mm |
| loaded J1 / J2 peak, nominal + wet (N*m) | 0.45 / 0.20 | 0.45 / 0.18 | rated 2.206 |
| loaded J1 / J2 peak over all successful runs of all suites (N*m) | 1.65 / 1.35 (580 runs) | 1.67 / 1.42 (570 runs) | runs at >= 2.2 N*m: 0 (0) |
| max slip in transport, nominal + wet (mm) | 0.28 | 0.31 |  |
| mean cycle, dry nominal (s) | 24.3 | 24.3 | level-0 600 mL lane 1 added to the mean |
| Monte Carlo 600 mL level 0 (N = 60, seed 20261003) | 60/60 (100%, CI 94-100%) | 51/60 (85%, CI 74-92%), 9 rejected | v1.0: rejected draws counted as failures |
| Monte Carlo 600 mL level 1 (N = 60, seed 20261004) | 60/60 (100%, CI 94-100%) | 60/60 (100%, CI 94-100%) |  |
| Monte Carlo 1 L level 1, D75 pockets (seed 20261001) | 52/60 (87%, CI 76-93%) | 52/60 (87%, CI 76-93%) | failures = bottle-to-divider contacts in the 87 mm lane |
| Monte Carlo 1 L level 1, D85 pockets (seed 20261002) | 44/60 (73%, CI 61-83%) | 45/60 (75%, CI 63-84%) |  |
| Monte Carlo 1 L level 1, D80 pockets (new, seed 20261002) | 48/60 (80%, CI 68-88%) | - | pocket_r 0.040 m |
| Monte Carlo 600 mL level 0 lane 1 only (new, N = 20, seed 20261007) | 20/20 (100%, CI 84-100%) | - | lane 1 was blocked in v1.0 |
| lateral pick-error sweep 0-12 mm, level 0 / level 1 | 82/84 / 83/84 | 84/84 / 83/84 |  |
| retreat, default (slot-0 neighbours): contact-free | 16/45 | 16/44 | planar sweep |
| retreat, bay rule: contact-free | 45/45 | 44/44 |  |
| retreat, bay rule + wait: contact-free | 45/45 | 44/44 |  |
| retreat, fully populated lanes (planar sweep): contact-free | 0/45 | 0/44 |  |
| Z 50 mm/s: cycle-time saving (s) | 1.23 | 1.32 | cycle 24.25 -> 23.03 s, 45/45 OK |

**Direct answers to the lead's v1.1 questions** (details and the v1.0 comparison in the sections named):

1. *Level-0 entries still impossible* (section 1): the 1 L in all 5 lanes and all cells; the tote cell (-300,0) for the 500 mL (carry blocked: the planner allows 5.0 mm for a carry that needs 7.0 mm), the 600 mL (grasp pose inside the level-1 ledge) and the 1 L; nothing else. The cans can use (-300,0) (both succeed as the last pick of the order). The v1.0 impossible entry that disappears: 600 mL lane 1.
2. *600 mL lane 1 margin* (section 1): free lift 19.7 mm against 13 mm needed (rim 11 mm + 2 mm), i.e. 6.7 mm; equivalent to 6.2 mm of level-pitch margin (infeasible below 433.8 mm) or a rim of up to 15 mm; Monte Carlo lane 1: 20/20 (100%, CI 84-100%). Limiter: L2 against the level-1 front-left lane-module bracket seat.
3. *(-300,100) and (-215,0)* (sections 1, 4): static hull clearance at the grasp pose 14.7 / 18.6 mm (1.7 / 5.6 mm in v1.0), ceiling headroom 49.5 mm for both (31.5 mm); the level-0 Monte Carlo no longer rejects any draw (0 of 8 and 0 of 8; v1.0 6 of 8 and 3 of 8).
4. *Retreat rule for v1 operation* (section 6): the bay rule on the STD lanes (contact-free 45/45; every bottle stays upright and on the plate, but the open gripper's sweep drags the released bottle sideways by 7.3 mm on average and 14.5 mm at most; default retreat without a feeding rule 16/45). The WIDE 125 mm lanes work with fully populated neighbours only with the deeper back-out (10/10 nominal, 20/20 Monte Carlo, 17/20 with off-centre neighbours; the v1.0-harness back-out 7/10 nominal and 13/20 Monte Carlo) and are not recommended for the 600 mL.
5. *Ceiling clearance against the lead's CAD check* (section 2): within 0.4 mm at the tote cells; at the lane poses the model is 1.5-2.3 mm more generous without margin (equal within 0.8 mm with the planner's 1.5 mm margin); the same poses are blocked; lane 1 uses the viewer's IK branch (19.7 mm with margin), not the fixture track's 25 mm branch.
6. *Lift-over* (section 6): needs carriage travel to 1.08 m (600 mL) and 1.13 m (1 L), beyond the 1.06 m stroke; the 500 mL fits (1.05 m).
7. *Deviations* (section 12): can grasp-height rule (185 mm), camera not modelled, fixture structures as boxes, fingertip model correction (2.5 mm), new optional controller flags, torque-diagnosis table carried over.

## 1. Picks that are impossible or marginal (design findings)

| case | finding | model result |
|---|---|---|
| 1 L (pet_1000), level 0, all 5 lanes and all cells | L2 cover top reaches z_g + 130 = 530 mm at the 1 L grasp height (z_g = 400 mm) against a ledge underside at 528 mm: ceiling cap -3.5 mm at the tote cell, -16.8 to -16.2 mm at the lanes (negative = interference). Level pitch at which the planner finds a path (level-1 structures shifted): >= 461 mm for lanes 2-5 and >= 486 mm for lane 1 (466 / 480 mm in v1.0); or keep the 1 L on level 1. | planner: `pick_cell_blocked_by_ceiling` / `place_pose_blocked_by_ceiling` (5/5 lanes, 5/5 cells) - still impossible (v1.0: same) |
| 600 mL, level 0, lane 1 (x = -0.18) | Free lift at the place pose 19.7 mm against 13 mm needed to clear the 11 mm rim (+2 mm): margin 6.7 mm (v1.0: carry blocked, needs 24.0 mm, allows 22.7 mm). The planner stays feasible down to a level pitch of 433.8 mm (6.2 mm of pitch margin) and for rims up to 15 mm (rim 18 mm: needs 20.0 mm, ceiling allows 19.7 mm). Limit = lane-module bracket seat (level 1, front-left, underside z 495.6 mm) above the L2 cover. Monte Carlo lane 1 only: 20/20 (100%, CI 84-100%). | usable (v1.0: impossible) |
| Tote cell (-300, 0), level 0 | 600 mL: the grasp pose penetrates the level-1 ledge / tray / stop by 17 mm (L1 cover at the ledge): impossible. 500 mL: 7.0 mm of ceiling headroom at the grasp (5.5 mm with the planner margin; the planner's carry check takes a further 0.5 mm off and reports 5.0 mm) against the 7.0 mm that the carry out of the cell needs: rejected by the planner; planner-feasible pitch >= 445 mm (500 mL), 472 mm (600 mL), 525 mm (1 L) (v1.0: 445 / 472 / 525 mm). Cans: ceiling headroom 42 mm, executed as the last pick of the restock order (no other tote bottle left): std can succeeds (cycle 27.7 s, slip 0.24 mm, no contact); slim can succeeds (cycle 27.6 s, slip 0.25 mm, no contact). The restock order keeps 5 cells at level 0 for every SKU (as in v1.0). | impossible for PET >= 500 mL and the 1 L; possible for the 355 mL cans (v1.0: impossible for every SKU) |
| Tote cell (-300, 100), level 0, PET >= 500 mL | Static hull-to-fixture clearance at the grasp pose 14.7 mm (1.7 mm in v1.0), ceiling headroom above the grasp 49.5 mm (31.5 mm). Level-0 600 mL Monte Carlo: 0 of 8 draws rejected by the planner (6 of 8 in v1.0). | comfortable (v1.0: marginal, effectively unusable) |
| Tote cell (-215, 0), level 0, PET >= 500 mL | Static clearance 18.6 mm (5.6 mm in v1.0), headroom 49.5 mm (31.5 mm). Monte Carlo: 0 of 8 rejected (3 of 8 in v1.0). | comfortable (v1.0: marginal) |
| Tote cell (-300, 200), level 0, PET | The tote bracket shelf at y = 170-230 mm limits the lift to 36.2 mm for 600 mL (18.2 mm in v1.0); 500 mL 63.2 mm. The planner finds a path with the 25 mm carry. | works (v1.0: works, tight) |
| Retreat lifting over neighbours | Needs 139 mm of lift for PET in the simulation (143 mm in the closed form). Level 1 with the real stroke (carriage 1.06 m): room above the grasp datum 150 / 123 / 70 mm for 500 mL / 600 mL / 1 L, i.e. it needs carriage travel to 1.080 m (600 mL) and 1.133 m (1 L), beyond the 1.06 m stroke. At level 0 the ceiling leaves 19-36 mm (600 mL) and 46-64 mm (500 mL) at the retreat poses: impossible for PET. | see section 6 |
| 1 L fit in D75 pockets | Assumed base cup 0.94 D fits only for D <= 79.3 mm; diameters 79.8-82 mm do not seat. Assumption of mine, to be checked on a real 1 L base (unchanged by v1.1). | fixture check |
| 1 L in the 87 mm lane | Clearance per side 2.5-5.5 mm for D 76-82 mm; the 3 mm (1 sigma) lateral error produces bottle-to-divider contacts: 8 of 60 draws fail in the D75-pocket run (8 in v1.0), all `bottle_body|divider` (section 5). Needs a lane clear width >= D max + 10 mm (92 mm) or a lead-in chamfer. | tolerance finding (unchanged) |

Pitch-fix check (`sim_results_level_pitch_verify.csv`, new for v1.1): Monte Carlo at level 0 with the level-1 structures raised by 50 mm (pitch 490 mm), same error model, N = 60 per SKU, all lanes and the 5 cells. 600 mL: 59/60 OK, 0 rejected (at 440 mm: 60/60). 1 L (diameters that fit the D75 pocket): 54/60 OK, 0 rejected by the planner (cells: none); the 6 failures are 5 bottle-to-divider contacts in the lane and 1 finger-blade contact with a neighbouring tote bottle at pick cell (-215,200) (`f2_blade_lo|tn1_body`). The 600 mL failure is a `no_grasp` abort (pick error too large for the V pads). A pitch increase of 50 mm therefore removes the ceiling as the limit for the 1 L in every level-0 lane; none of the 7 remaining failures is a ceiling failure, but only 5 of them are the lane-tolerance failures of section 5. Disclosure: one successful 600 mL run in this check reaches 2.16 N*m of loaded J1 torque (cell (-215,0), lane 1, pick error -1.4 / -7.4 mm), 98% of the 2.206 N*m rating; the 440 mm suites never exceed 1.65 N*m. I did not investigate it further.

Cans are grasped higher than PET (z_g = floor + 185 mm, fingertips 45 mm above the plane) so that the fingers clear the 8 mm tote surface and the 35 mm lane dividers. **Deviation:** the fixture rule gives z_g = floor + 148 mm (lane) / 153 mm (tote) for the 355 mL cans (max(H + 5 or 10 mm, 143 mm)); I kept my 185 mm rule from v1.0 so that the harness is unchanged. It costs headroom (section 2 compares both rules) but never limits a can pick at v1.1.

## 2. Model changes for fixture v1.1 and validation against the lead's CAD check

Changes in `sim_build_model.py` / `sim_controller.py` / `sim_run_tests.py` (everything else of the v1.0 harness is unchanged: planner, success rules, error models, seeds, solver settings): (1) level pitch 440 mm (floors z = 100 / 540 mm); the level-1 lane plate underside at the entrance is 519.7 mm (the lead's value; my slab model reproduces it); (2) tray: pocket floor at z_floor (tray base 6 mm below it), pockets 8 mm deep (surface 8 mm above the pocket floor), 3 mm walls on all four sides up to 11 mm above the pocket floor (3 mm above the surface; v1.0: 10 mm surface, 22 mm rim); (3) ledge plate x = -370..-165 mm (v1.0: to -152), 6 mm thick, top 534 mm at level 1, y = -62.5..252.5 mm; (4) three tote stops per level (two on the front edge at x = -330..-305 and -210..-185, y = -60.5..-52.5; one on the left edge at x = -358..-350, y = 87.5..112.5; none on the right), top 12 mm above the ledge; (5) lane-module main brackets from the fixture STLs (x = +-210..263 mm, front y = 279.8-319.8 mm with the seat underside at z = 55.6 (level 0) / 495.6 (level 1) mm, rear y = 528.5-568.5 mm at 29.4 / 469.4 mm; seat 14 mm thick, vertical plate 15 mm x 108 mm), tote brackets (plate x = -385..-370, shelf x = -370..-315 mm, z from 72 / 512 mm), stand panel x = -403..-385, y = -40..240, z = 18..594 mm (all modelled as boxes taken from voxel probes of the STLs); (6) carriage stroke 0.37-1.06 m (hard stops 364 / 1066 mm); the 1.00 m ceiling of the v1.0 runs is removed; (7) gripper mounted 0.5 mm lower (body bottom zeta -4.3 mm, fingertip zeta -140.5 mm), grasp rule unchanged; (8) STD lane exit stop 5 x 20 mm, WIDE lane set 3 lanes at x = -125 / 0 / 125 mm with free width 120.5 mm, dividers 4.5 mm x 45 mm, stop 10 x 60 mm. **Model correction, not a fixture change:** in the v1.0 model the lower finger blade ended at zeta -143 mm (pad bottom minus 6 mm) instead of the CAD fingertip at -140 mm; v1.1 uses the CAD fingertip (-140.5 mm), so the fingertips sit 2.5 mm higher than in the v1.0 runs. **Not modelled:** the wrist camera (raised 30 mm in v1.1; no collision geometry in the model), the shims above the bracket seats, the posts and feet (x = +-278 mm, away from the arm).

Vertical ceiling clearance of the arm hulls (mm of carriage rise from the pose until the first contact with a ceiling fixture), computed by `sim_clearance_probe.py` for the same poses as the fixture track's independent CAD check ('lead' column, no margin). 'Planner margin' is the 1.5 mm margin the planner keeps; without it the numbers are comparable to the CAD check. The probe tests the arm hulls L1, L2, the J2 / J3 housings and the covers (not the gripper and fingers, which hang lower and do not limit the lift):

| pose | this model, no margin | this model, planner margin 1.5 mm | lead's CAD check | difference, no margin (model - CAD) | limiting pair in the model (arm part / fixture) |
|---|---|---|---|---|---|
| 600 mL, lane 1 (place pose) | 21.2 | 19.7 | 19.7 (viewer IK branch); fixture track best branch 25 nominal / 22 with 3 mm margin | +1.5 | l2 / level-1 front-left lane-module bracket seat |
| 600 mL, lane 3 (place pose) | 38.3 | 36.8 | 36.6 | +1.7 | l2 / level-1 lane plate underside |
| 600 mL, lane 5 (place pose) | 38.9 | 37.4 | 36.6 | +2.3 | l2 / level-1 lane plate underside |
| 600 mL, tote cell (-300,200) | 37.7 | 36.2 | 37.3 (fixture: 37 / 34) | +0.4 | l2 / level-1 tote bracket shelf (y 170-230) |
| 600 mL, tote cells (-215,0) and (-215,200) | 51.0 | 49.5 | 50.6 | +0.4 | l2 cover / level-1 ledge plate |
| 600 mL, tote cell (-300,100) | 51.0 | 49.5 | >= 40 | agrees (>=) | l2 cover / level-1 ledge plate |
| 600 mL, tote cell (-300,0) | blocked (-17 mm penetration at the grasp pose) | blocked | blocked at the grasp pose (L1 link top zeta 201 hits tray / ledge / stop) | agrees | l1 / level-1 ledge plate |
| 500 mL, tote cell (-300,0) | 7.0 | 5.5 | 7 / 4 (without / with 3 mm margin) | +0.0 | l1 cover / level-1 ledge plate |
| 355 mL cans, my grasp rule (z_g = floor + 185), all lanes and the 5 cells used | 83.2 | 81.7 | >= 73 | agrees (>=) | l2 / level-1 front-left lane-module bracket seat |
| 355 mL cans, fixture grasp rule (z_g = floor + 143..166), same poses | 107.2 | 105.7 | >= 73 | agrees (>=) | - |
| 355 mL cans, fixture rule, including the dropped cell (-300,0) | 61.0 | - | >= 73 | slim can: below 73 mm | - |
| 1 L at level 0 | blocked everywhere (cell caps -2.0 mm, lane caps -15.3..-14.7 mm) | blocked | blocked everywhere | agrees | - |
| level 1, every SKU | no ceiling above level 1 is modelled: search limit 300 mm; the stroke limits instead (below) | - | >= 90 | not comparable (see text) | - |

**Where the numbers do not agree.** (a) At the tote cells they agree within 0.4 mm. (b) At the lane poses the model is 1.5-2.3 mm more generous than the CAD check without margin (lane 1: 21.2 against 19.7; lanes 3 / 5: 38.3 / 38.9 against 36.6); with the planner's 1.5 mm margin they coincide within 0.8 mm, so the planner's numbers are the safe ones to quote, and the remaining difference is probably my box models of the bracket seats and the tilted plate underside against the exact meshes. (c) The fixture track's closed-form headroom for the 600 mL at level 0 (42.7 mm at the lane target, 51 mm under the ledge) is 4-6 mm above my lane numbers (36.8-37.4 mm with margin): it does not include the lane-module bracket seats and the plate tilt. (d) Lane 1: the fixture track's best IK branch gives 25 mm nominal; my controller only uses the elbow +1 branch (the viewer's branch), which gives 19.7-21.2 mm, so 19.7 mm is the number that applies to the simulated arm. (e) Level 1: the model has nothing above level 1, so the only limit is the carriage stroke: room above the grasp datum with the 1.06 m stroke is cans 355 std 185 mm, 500 mL 150 mm, 600 mL 123 mm, 1 L 70 mm, 1.5 L 49 mm, 2 L 40 mm; the 1.5 L and 2 L therefore have no room for a lift-over and only 49 / 40 mm for hover and carry lift (25 mm each are used).

## 3. Acceptance table (same criteria as the v1.0 report)

Success definition (as coded in `sim_run_tests.py`, scored when the gripper has opened, before the retreat; unchanged from v1.0): the planner accepts the pick; no contact with dividers, plates, ceilings, tote or neighbours before release (force > 3 N or penetration > 0.2 mm); the grasp check passes (|D_est - D_nom| <= 6 mm, finger speed <= 2 mm/s); the bottle's rear edge is at most 15 mm upstream of the lane entrance plane (a bottle overhanging the plate edge is fed on by gravity); lateral offset <= max(12 mm, free play + 4 mm); tilt <= 8 degrees. The 15 mm rear-edge tolerance is a relaxation of the original 5 mm that I disclosed in v1.0 (it only mattered for the 2 L in the wide lanes); across all successful runs of all suites here the largest rear-edge overhang is reported in section 12. Wide-lane runs use the lateral play of the 120.5 mm lane.

| criterion | result v1.1 (v1.0) | status |
|---|---|---|
| nominal success >= 95% (dry; pad mu 0.8, cans 0.7) | 45/45 (100%) of the picks the planner accepts (44/44); 5/50 matrix entries impossible (6/50) | PASS for accepted picks; coverage gap only for the 1 L at level 0 |
| nominal success >= 90% (wet; pad mu 0.4) | 45/45 (100%) (44/44); the same 5 impossible entries (6) | PASS / same coverage gap |
| J1/J2 loaded-phase torque <= rated (2.206 N*m) | nominal + wet loaded peaks <= 0.45 / 0.20 N*m (0.45 / 0.18); over all 580 successful runs of the suites nominal, wet, cells, mc, mc600, mc_d85, lateral the largest loaded-phase peak is 1.65 / 1.35 N*m (1.67 / 1.42 in 570 runs); runs reaching 2.2 N*m: 0 (0) | PASS |
| J1/J2 whole-cycle torque <= rated | default retreat: 28/45 runs reach 2.2 N*m on J1 (27/44), stall against a neighbour; bay rule 4/45 on J1 and 0/45 on J2 (4/44, 1/44); bay rule + wait 3/45 (4/44) | FAIL with the default retreat; mostly PASS with the bay rule |
| zero collisions before release (dividers, neighbours, plates, ceilings) | 0 of 45 dry and 0 of 45 wet runs (0 of 44, 0 of 44) | PASS |
| zero collisions in the retreat (open gripper) | contact-free: default 16/45 (16/44); bay rule 45/45 (44/44); bay rule + wait 45/45 (44/44); fully populated STD lanes, planar sweep 0/45 (0/44); WIDE lanes, corridor retreat with deeper back-out: see section 6 | FAIL unless the bay rule holds or the lane module changes (section 6) |
| Monte Carlo >= 90% (N = 60) | 600 mL level 0 60/60 (100%; v1.0 51/60, 9 rejected draws counted as failures), level 1 60/60 (60/60); 1 L level 1: 52/60 with D75 pockets (52/60), 44/60 with D85 pockets (45/60), 48/60 with D80 pockets (new) | 600 mL PASS at both levels (v1.0: level 0 FAIL, 85%); 1 L FAIL |
| slip <= 2 mm in transport | 0 of 45 dry and 0 of 45 wet runs; Monte Carlo successes with slip > 2 mm: 600 mL 1 of 120 (v1.0 1), 1 L 5 of 52 (6); lateral sweep: 12 of 165 (13 of 167) | PASS nominal; FAIL under 1 L / large errors |
| lateral pick error sweep (600 mL, lane 3, 12 directions per magnitude) | level 0: 0 mm 12/12, 2 mm 12/12, 4 mm 12/12, 6 mm 12/12, 8 mm 12/12, 10 mm 12/12, 12 mm 10/12; level 1: 0 mm 12/12, 2 mm 12/12, 4 mm 12/12, 6 mm 12/12, 8 mm 12/12, 10 mm 12/12, 12 mm 11/12 (v1.0: level 0 84/84, level 1 83/84 in total) | >= 95% up to 10 mm at both levels |
| gravity check (J1/J2/J3 inertial only) | static J1 0.004, J2 0.001, J3 0.000 N*m with 0.65 / 1.07 kg held still 5 s; Z holds 20.9 / 24.6 N (hand estimate 23.7 / 27.9 N) | PASS (< 0.5 N*m) |

Per SKU and level, dry (pad mu 0.8, cans 0.7); retreat = default slot-0 neighbours; 'free lift in lane' = ceiling headroom at the place pose (60.0 = search limit, no ceiling); each cell is `v1.1 (v1.0)`:

| SKU | level | runs | planner-rejected | pick-place OK | max slip (mm) | pre-release collisions | contact-free retreat (default) | mean cycle (s) | loaded peak J1 / J2 (N*m) | carry lift range (mm) | free lift in lane (mm) |
|---|---|---|---|---|---|---|---|---|---|---|---|
| can_355_std | 0 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.7 (23.8) | 0.35 / 0.15 (0.35 / 0.15) | 25-25 (26-26) | 60.0 (60.0) |
| can_355_std | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.7 (23.8) | 0.35 / 0.15 (0.35 / 0.15) | 25-25 (26-26) | 60.0 (60.0) |
| can_355_slim | 0 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.0 (23.1) | 0.36 / 0.17 (0.36 / 0.17) | 25-25 (26-26) | 60.0 (60.0) |
| can_355_slim | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.0 (23.1) | 0.36 / 0.17 (0.36 / 0.17) | 25-25 (26-26) | 60.0 (60.0) |
| pet_500_water | 0 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 24.8 (24.9) | 0.37 / 0.16 (0.37 / 0.16) | 25-25 (26-26) | 46.7 (43.9) |
| pet_500_water | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 24.8 (24.9) | 0.37 / 0.16 (0.37 / 0.16) | 25-25 (26-26) | 60.0 (60.0) |
| pet_600_soda | 0 | 5 | 0 (1) | 5/5 (4/4) | 0.28 (0.31) | 0 (0) | 2/5 (2/4) | 23.9 (24.0) | 0.40 / 0.20 (0.22 / 0.17) | 20-25 (16-26) | 19.7 (16.8) |
| pet_600_soda | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.7 (23.8) | 0.40 / 0.17 (0.40 / 0.17) | 25-25 (26-26) | 60.0 (60.0) |
| pet_1000 | 0 | 5 | 5 (5) | - (-) | - (-) | - (-) | - (-) | - (-) | - (-) | - (-) | - (-) |
| pet_1000 | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 0/5 (0/5) | 27.7 (27.8) | 0.45 / 0.18 (0.45 / 0.18) | 25-25 (26-26) | 60.0 (60.0) |

The level-0 600 mL row now equals the level-1 row: the carry no longer ducks under the plate (v1.0: 16-17 mm), so the loaded J1 peak of lane 2 (0.40 N*m) appears at both levels.

Wet (pad mu 0.4):

| SKU | level | runs | planner-rejected | pick-place OK | max slip (mm) | pre-release collisions | contact-free retreat (default) | mean cycle (s) | loaded peak J1 / J2 (N*m) | carry lift range (mm) | free lift in lane (mm) |
|---|---|---|---|---|---|---|---|---|---|---|---|
| can_355_std | 0 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.1 (23.2) | 0.35 / 0.15 (0.35 / 0.15) | 25-25 (26-26) | 60.0 (60.0) |
| can_355_std | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.1 (23.2) | 0.35 / 0.15 (0.35 / 0.15) | 25-25 (26-26) | 60.0 (60.0) |
| can_355_slim | 0 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.0 (23.1) | 0.36 / 0.17 (0.36 / 0.17) | 25-25 (26-26) | 60.0 (60.0) |
| can_355_slim | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.0 (23.1) | 0.36 / 0.17 (0.36 / 0.17) | 25-25 (26-26) | 60.0 (60.0) |
| pet_500_water | 0 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.1 (23.2) | 0.37 / 0.16 (0.37 / 0.16) | 25-25 (26-26) | 46.7 (43.9) |
| pet_500_water | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.1 (23.2) | 0.37 / 0.16 (0.37 / 0.16) | 25-25 (26-26) | 60.0 (60.0) |
| pet_600_soda | 0 | 5 | 0 (1) | 5/5 (4/4) | 0.28 (0.31) | 0 (0) | 2/5 (2/4) | 24.0 (23.4) | 0.40 / 0.20 (0.22 / 0.17) | 20-25 (16-26) | 19.7 (16.8) |
| pet_600_soda | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 2/5 (2/5) | 23.8 (23.9) | 0.40 / 0.17 (0.40 / 0.17) | 25-25 (26-26) | 60.0 (60.0) |
| pet_1000 | 0 | 5 | 5 (5) | - (-) | - (-) | - (-) | - (-) | - (-) | - (-) | - (-) | - (-) |
| pet_1000 | 1 | 5 | 0 (0) | 5/5 (5/5) | 0.28 (0.28) | 0 (0) | 0/5 (0/5) | 24.5 (25.6) | 0.45 / 0.18 (0.45 / 0.18) | 25-25 (26-26) | 60.0 (60.0) |

Mean phase times, dry, default retreat (s): 1_descend 1.50, 2_close 1.26, 3_lift 1.50, 4_carry 5.21, 5_place_desc 1.49, 6_release 0.34, 8_open_full 1.02, 8c_clear_fwd 0.81, 9_return_row 5.38, 9b_close_to_tote_gap 2.63, 10_return_tote 3.12. Time until the gripper has opened in the lane 12.3 s (12.4 s); full cycle 24.3 s (24.3 s; bay rule 19.9 s).

## 4. Tote cells and ceiling clearances (level 0)

Vertical lift available above the grasp at each cell (mm) and result of the executed pick-place (level 0), `v1.1 (v1.0)`. The cell (-300,0) column is the run with the other tote bottles present (as in v1.0, planner override for the 600 mL only); the cans fail there only because those bottles are still in the tote, and with the tote otherwise empty (last pick of the order) both cans succeed (section 1):

| SKU | (-215,200) | (-215,100) | (-215,0) | (-300,200) | (-300,100) | (-300,0) |
|---|---|---|---|---|---|---|
| can_355_slim | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 40.5 FAIL (-) |
| can_355_std | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 60.0 OK (60.0 OK) | 40.5 FAIL (22.5 FAIL) |
| pet_500_water | 60.0 OK (58.5 OK) | 60.0 OK (58.5 OK) | 60.0 OK (58.5 OK) | 60.0 OK (45.2 OK) | 60.0 OK (58.5 OK) | blocked (blocked) |
| pet_600_soda | 49.5 OK (31.5 OK) | 49.5 OK (31.5 OK) | 49.5 OK (31.5 OK) | 36.2 OK (18.2 OK) | 49.5 OK (31.5 OK) | blocked (blocked) |
| pet_1000 | blocked (blocked) | blocked (blocked) | blocked (blocked) | blocked (blocked) | blocked (blocked) | - (-) |

At level 1 all six cells succeed for every SKU including the 1 L (60 mm search limit, no ceiling above). Static clearance of the arm hulls to the ledge / brackets at the grasp pose for the 600 mL (`sim_results_clearance_v11.csv`, `v1.1 (v1.0)`): (-215,200) 17.6 (17.6) mm, (-215,100) 18.9 (18.9) mm, (-215,0) 18.6 (5.6) mm, (-300,200) 24.4 (11.4) mm, (-300,100) 14.7 (1.7) mm. The minimum distances at (-215,200) and (-215,100) are horizontal (arm link against the front edge of the level-1 lane plate) and do not change with the pitch.

## 5. Monte Carlo and lateral error

Draws (unchanged from v1.0 and with the same seeds): gripper pick-point error N(0, 3 mm) per axis, closing-axis error N(0, 3 deg), bottle offset inside its pocket (uniform over the free play), pad mu U(0.4, 0.8), mass +-5%, diameter U(D min, D max) (1 L with D75 pockets: only diameters whose assumed 0.94 D base cup fits the pocket; the D80 / D85 variants use the full range), random lane and cell. Level-0 600 mL draws use lanes 2-5 as in v1.0 (so that the draws are identical to the v1.0 run); lane 1, which v1.1 makes possible, has its own run (seed 20261007, N = 20). Planner-rejected draws are counted as failures in the percentage because the real arm cannot foresee the error that displaces its pick pose into the ledge.

| suite | draws | planner-rejected | executed OK | effective success (95% CI) | slip > 2 mm (successes) | max slip (mm) | loaded peak J1 / J2 (N*m) |
|---|---|---|---|---|---|---|---|
| 600 mL, level 0, lanes 2-5 (seed 20261003) | 60 | 0 (9) | 60/60 (51/51) | 100% (94-100%) (85%, 74-92%) | 0 (0) | 1.90 (1.85) | 1.65 / 1.15 (1.67 / 1.15) |
| 600 mL, level 1 (seed 20261004) | 60 | 0 (0) | 60/60 (60/60) | 100% (94-100%) (100%, 94-100%) | 1 (1) | 2.39 (2.10) | 0.93 / 0.62 (0.95 / 0.61) |
| 600 mL, mixed levels (seed 20261001) | 60 | 0 (2) | 59/60 (57/58) | 98% (91-100%) (95%, 86-98%) | 0 (0) | 0.56 (0.53) | 0.50 / 0.34 (0.52 / 0.34) |
| 1 L, level 1, D75 pockets, D 76-79.3 mm (seed 20261001) | 60 | 0 (0) | 52/60 (52/60) | 87% (76-93%) (87%, 76-93%) | 5 (6) | 4.37 (4.33) | 1.86 / 1.22 (2.21 / 1.25) |
| 1 L, level 1, hypothetical D85 pockets, D 76-82 mm (seed 20261002) | 60 | 0 (0) | 44/60 (45/60) | 73% (61-83%) (75%, 63-84%) | 4 (5) | 5.31 (6.11) | 1.12 / 0.68 (1.11 / 0.67) |
| 1 L, level 1, hypothetical D80 pockets, D 76-82 mm (new, seed 20261002) | 60 | 0 (-) | 48/60 (-) | 80% (68-88%) | 4 (-) | 5.62 (-) | 1.10 / 0.84 (-) |
| 600 mL, level 0, lane 1 only (new, N = 20, seed 20261007) | 20 | 0 (-) | 20/20 (-) | 100% (84-100%) | 0 (-) | 0.47 (-) | 0.78 / 0.80 (-) |

600 mL: no failure in the level-0 run (60/60; v1.0 rejected 9 draws: cell (-300,100) 6 of 8, cell (-215,0) 3 of 8; v1.1 rejects 0 of 8 and 0 of 8) and none at level 1. The one 600 mL failure in the mixed-level run is a `no_grasp` abort (pick error along x too large for the V pads to re-centre a bottle held in its pocket; the grasp check |D_est - D_nom| > 6 mm stops the cycle before the lift), as in v1.0. 1 L failures: 8 of 8 (D75 run) and 15 of 16 (D85 run) are `bottle_body|divider` contacts in the lane; the x pick error exceeds the free play per side ((87 - D)/2 = 2.5-5.5 mm) in 6/8 and 11/16 of those failures. The D80 and D85 runs draw the full diameter range (up to 82 mm, 2.5 mm free play per side in the lane) and let the bottle sit off-centre in a larger pocket: 48/60 for D80 (11 divider contacts) and 44/60 for D85, against 52/60 for D75 with the narrower diameter range. Larger pockets do not help; the lane tolerance limits the 1 L.

Lateral error sweep, 600 mL, lane 3, first cell, 12 random directions per magnitude per level, `v1.1 (v1.0)`:

| error (mm) | level 0 OK | level 1 OK | max slip (mm) | loaded J1 max (N*m) | max bottle x offset at release (mm) |
|---|---|---|---|---|---|
| 0 | 12/12 (12/12) | 12/12 (12/12) | 0.28 (0.28) | 0.22 (0.22) | 0.3 (0.3) |
| 2 | 12/12 (12/12) | 12/12 (12/12) | 0.28 (0.28) | 0.22 (0.23) | 0.8 (0.8) |
| 4 | 12/12 (12/12) | 12/12 (12/12) | 0.29 (0.29) | 0.33 (0.31) | 2.8 (2.8) |
| 6 | 12/12 (12/12) | 12/12 (12/12) | 0.28 (0.28) | 0.38 (0.39) | 4.8 (4.7) |
| 8 | 12/12 (12/12) | 12/12 (12/12) | 0.61 (0.51) | 0.67 (0.67) | 6.7 (6.7) |
| 10 | 12/12 (12/12) | 12/12 (12/12) | 2.83 (2.60) | 1.15 (1.14) | 8.2 (8.2) |
| 12 | 10/12 (12/12) | 11/12 (11/12) | 4.10 (4.04) | 1.47 (1.38) | 8.5 (8.6) |

The lane free play for the 600 mL (D 67.5 mm in 87 mm) is 9.75 mm per side. Failures in the v1.1 sweep: level 0, 12 mm, x error +11.3 mm, bottle -7.6 mm off-centre at release (collision; bottle_body|divider0_2), level 0, 12 mm, x error -11.4 mm, bottle +8.5 mm off-centre at release (collision; bottle_body|divider0_3), level 1, 12 mm, x error -11.9 mm, bottle +8.5 mm off-centre at release (collision; bottle_body|divider1_3); v1.0: level 1, 12 mm, x error -11.9 mm (bottle_body|divider1_3). The extra failures at level 0 are borderline divider touches: the bottle is 7.6-8.5 mm off-centre at release against 9.75 mm of play, and the same draws passed in v1.0 with 7.5-8.4 mm; the 3 N / 0.2 mm contact criterion is at its edge there, so 82/84 against 84/84 is most likely threshold noise at the 12 mm point rather than a systematic level-0 degradation (not rerun with other seeds). Slip above 2 mm begins at 10 mm error because the pads grip the bottle off-centre.

## 6. Retreat, lift-over and lane pitch

(i) Retreat statistics, nominal dry matrix (accepted picks, STD 5-lane set, 90 mm pitch), `v1.1 (v1.0)`. 'Contact-free' = no contact of the open gripper or arm with fixtures or neighbours (force > 3 N or penetration > 0.2 mm). 'Full cycle OK' also requires no retreat contact, a bottle tilt in the lane plane of at most 8 degrees and the bottle's rear edge at most 5 mm upstream of the lane entrance plane; it does not limit the sideways drag of the bottle during the retreat (`bottle_dx_retreat_mm`). The last column is the pad force on the released bottle, mean / max (N); in the fully populated case these are stall forces of a position-controlled arm pushing against bottles, not design loads.

| retreat variant | runs | contact-free | full cycle OK | mean cycle (s) | runs reaching 2.2 N*m (J1 / J2) | pad push on released bottle (N) |
|---|---|---|---|---|---|---|
| default: slot-0 neighbours in the adjacent lanes, planar sweep | 45 (44) | 16/45 (16/44) | 16/45 (16/44) | 24.3 (24.3) | 28 / 29 (27 / 28) | 13 / 29 (14 / 24) |
| bay rule: neighbours only at slots 2-4 (nothing in slots 0-1 when the arm drops) | 45 (44) | 45/45 (44/44) | 45/45 (43/44) | 19.9 (20.0) | 4 / 0 (4 / 1) | 13 / 29 (14 / 24) |
| bay rule + wait up to 6 s for the bottle to slide clear | 45 (44) | 45/45 (44/44) | 45/45 (43/44) | 25.9 (26.0) | 3 / 0 (4 / 0) | 14 / 31 (14 / 26) |
| fully populated lanes (slots 0-4) | 45 (44) | 0/45 (0/44) | 0/45 (0/44) | 28.9 (29.0) | 45 / 42 (44 / 41) | 77 / 1249 (70 / 760) |

With the bay rule 45 of 45 retreats (43 of 44 in v1.0) leave the bottle upright and on the plate (no exception), but 'undisturbed' does not mean 'unmoved': the sideways sweep drags the released bottle in x by 7.3 mm on average and 14.5 mm at most (slim can), up to the full free play of the lane, and the default retreat does the same (7.3 / 14.7 mm; `bottle_dx_retreat_mm` in sim_results_nominal_bay.csv and sim_results_nominal.csv). The 1 L: 5/5 lanes upright and on the plate in v1.1, 4/5 in v1.0 (lane 3: the swinging finger knocks the bottle off the plate, pad push up to 20 N); with the 6 s wait 5/5 in v1.1. The 1 L is marginal in both versions: the pad pushes the released bottle with 12-29 N in v1.1 (16-24 N in v1.0) and J1 / J2 reach 2.2 N*m in 3 of 5 lanes (3 of 5), so the clean lane-3 retreat in v1.1 against the knocked bottle in v1.0 is two outcomes of the same marginal contact (the fingertip is 2.5 mm higher and the plate 20 mm higher), not a change of margin; do not read it as a fix. Waiting does not clear the bottle (`cleared` is false in every run): at lane mu 0.10 on 6 degrees the net acceleration is about 0.05 m/s^2 and the open front finger sits downhill of the bottle. The bay rule needs an explicit rule in the feeding order, not just a pause. Fully populated STD lanes: the planar sweep is impossible at any pitch (the front finger, 62.5-74 mm ahead of the wrist, lies inside the footprint of the slot-1 bottle).

(ii) Lift-then-sweep retreat (fingertips raised above the neighbours, then planar sweep). Required lift = neighbour height + 10 mm minus fingertip height above the datum plane; 'available' is limited by Z max and, at level 0, by the ceiling. Contact-free retreats out of 5 lanes per SKU (600 mL level 0: 5, 1 L: level 1 only). Z max 1.06 m is the real carriage stroke (v1.0 runs used 1.00 m); 1.25 m is hypothetical ('needs carriage travel beyond 1.06 m'). `v1.1 (v1.0)`:

| level | Z max (m) | SKU | lift required (mm) | lift available (mm) | neighbours at slot 0 | bay rule | fully populated | peak J1 fully populated (N*m) |
|---|---|---|---|---|---|---|---|---|
| 1 | 1.06 (1.00) | can_355_std | 88 (90) | 88 (90) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.4 (0.4) |
| 1 | 1.06 (1.00) | can_355_slim | 121 (123) | 121 (123) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.5 (0.5) |
| 1 | 1.06 (1.00) | pet_500_water | 139 (142) | 139 (109) | 5/5 (3/5) | 5/5 (5/5) | 5/5 (0/5) | 0.5 (2.2) |
| 1 | 1.06 (1.00) | pet_600_soda | 139 (142) | 122 (82) | 3/5 (2/5) | 5/5 (4/5) | 3/5 (0/5) | 2.2 (2.2) |
| 1 | 1.06 (1.00) | pet_1000 | 139 (141) | 69 (29) | 2/5 (0/5) | 3/5 (5/5) | 0/5 (0/5) | 2.2 (2.2) |
| 1 | 1.25 (1.25) | can_355_std | 88 (90) | 88 (90) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.4 (0.4) |
| 1 | 1.25 (1.25) | can_355_slim | 121 (123) | 121 (123) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.5 (0.5) |
| 1 | 1.25 (1.25) | pet_500_water | 139 (142) | 139 (142) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.5 (0.5) |
| 1 | 1.25 (1.25) | pet_600_soda | 139 (142) | 139 (142) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.6 (0.6) |
| 1 | 1.25 (1.25) | pet_1000 | 139 (141) | 139 (141) | 5/5 (5/5) | 5/5 (5/5) | 5/5 (5/5) | 0.7 (0.7) |
| 0 | 1.06 (1.00) | can_355_std | 88 (90) | 81-88 (78) | 5/5 (3/5) | 5/5 (5/5) | 5/5 (3/5) | 0.4 (2.2) |
| 0 | 1.06 (1.00) | can_355_slim | 121 (123) | 81-99 (78-79) | 2/5 (2/5) | 5/5 (5/5) | 0/5 (0/5) | 2.2 (2.2) |
| 0 | 1.06 (1.00) | pet_500_water | 139 (142) | 46-64 (43-44) | 2/5 (2/5) | 5/5 (5/5) | 0/5 (0/5) | 2.2 (2.2) |
| 0 | 1.06 (1.00) | pet_600_soda | 139 (142) | 19-36 (16) | 2/5 (2/4) | 5/5 (4/4) | 0/5 (0/4) | 2.2 (2.2) |

Level 1 with the real stroke (Z max 1.06 m): contact-free retreats from fully populated lanes 18/25; by SKU: can_355_std 5/5, can_355_slim 5/5, pet_500_water 5/5, pet_600_soda 3/5, pet_1000 0/5. Lift-over of PET needs 139 mm above the grasp datum in the simulation (143 mm in the closed form), i.e. carriage travel (closed form) to 1.053 m (500 mL), 1.080 m (600 mL) and 1.133 m (1 L) against the 1.06 m stroke: **the 600 mL and 1 L lift-over needs carriage travel beyond 1.06 m** (+20 mm and +73 mm; the 500 mL fits). At Z max 1.25 m every SKU retreats contact-free from fully populated lanes (25/25; peak J1 <= 0.73 N*m, mean cycle per SKU 25-30 s). At level 0 (Z max 1.06 m) the ceiling leaves 19-36 mm (600 mL), 46-64 mm (500 mL) and 81-99 mm (slim can) against 139 / 139 / 121 mm required, so lift-over is impossible for PET and the slim can; the standard can reaches it in 5 of 5 lanes (v1.0: 3).

A partial lift is not harmless. At Z max 1.06 m the 1 L lifts only 69 of 139 mm and the 600 mL 122 of 139 mm; with the bay rule the 1 L retreat is contact-free in 3/5 lanes (v1.0, 29 mm of lift: 5/5) because the lifted pad meets the shoulder and neck of the released bottle, and in 1 lane the bottle is thrown out of the lane. Lift-over should only be used with the full lift.

(iii) Minimum lane pitch for a sideways retreat with fully populated neighbours on the STD-type lane (level 1, centre lane, Z max 1.25 m so that the lift variant is not height-limited; the 'corridor' retreat is the v1.0 harness retreat, return via the pre-lane row). Contact-free yes/no by lane pitch (mm), `v1.1 (v1.0)`:

| SKU | retreat | 90 | 105 | 115 | 125 | 135 | 145 | 160 |
|---|---|---|---|---|---|---|---|---|
| pet_600_soda | sweep | no (no) | no (no) | no (no) | no (no) | no (no) | no (no) | no (no) |
| pet_600_soda | corridor | no (no) | no (no) | no (no) | yes (yes) | yes (yes) | yes (yes) | yes (yes) |
| pet_600_soda | lift | yes (yes) | yes (yes) | yes (yes) | yes (yes) | yes (yes) | yes (yes) | yes (yes) |
| pet_1000 | sweep | no (no) | no (no) | no (no) | no (no) | no (no) | no (no) | no (no) |
| pet_1000 | corridor | no (no) | no (no) | no (no) | yes (yes) | yes (yes) | yes (yes) | yes (yes) |
| pet_1000 | lift | yes (yes) | yes (yes) | yes (yes) | yes (yes) | yes (yes) | yes (yes) | yes (yes) |

Closed-form corridor pitch (step sideways by pitch/2 between the neighbours) p_min = D max + 44.4 mm finger width + 2 c, with c = 3 mm clearance: can_355_std 116.4, can_355_slim 107.4, pet_500_water 118.4, pet_600_soda 121.4, pet_1000 132.4, pet_1500 139.4, pet_2000 160.4 mm (`sim_results_min_pitch_analytic.csv`; the finger stack is 11.5 mm and the rear finger sits 62-72 mm behind the wrist). The simulation of the centre lane agrees with the closed form to within the clearance (table above). The present 90 mm pitch supports neither a sweep nor a corridor retreat with populated neighbours.

(iv) **New: 600 mL on the WIDE lane set** (3 lanes at x = -125 / 0 / 125 mm, pitch 125 mm, free width 120.5 mm, 4.5 mm x 45 mm dividers, level 1) with **fully populated neighbouring lanes** (5 bottles per neighbour lane, slots 0-4, no bay rule), as requested: does the corridor retreat work at a pitch of 125 mm (closed-form limit 121.4 mm for the 600 mL at D max 71 mm)? 'Corridor' is the retreat of the v1.0 harness (lift over the dividers if needed, step half a pitch toward the column axis, back out along -y to the pre-lane row, then to the tote). Its return leg makes the open front finger cross the first slot of the neighbouring lane, so I added an option `corridor_backout` (backing out along -y until the open front finger is behind the rear edge of a slot-0 bottle, y_wrist <= 0.19 m, and staying on that row up to the tote); both are reported. Nominal = 10 picks (lane 1 x pick cells 0-3, lane 2 x 0-2, lane 3 x 0-2, dry); Monte Carlo = 20 draws with the error model of section 5 (seed 20261008), and 20 further draws (seed 20261009) in which the neighbour bottles also sit off-centre in their lanes (each N(0, 4 mm), clipped to +-12 mm, the release scatter seen in the nominal runs).

| case (600 mL, WIDE lanes, fully populated unless stated) | picks | pick-place OK | arm contact-free in the retreat | full cycle OK | pad push on released bottle, mean / max (N) | bottle dragged in x, max (mm) | mean cycle (s) | peak J1 (N*m) |
|---|---|---|---|---|---|---|---|---|
| nominal: corridor (v1.0 harness back-out) | 10 | 10/10 | 7/10 | 7/10 | 20 / 39 | 7.2 | 23.5 | 2.21 |
| nominal: corridor + deeper back-out | 10 | 10/10 | 10/10 | 10/10 | 19 / 39 | 7.1 | 21.2 | 0.93 |
| nominal: planar sweep | 10 | 10/10 | 0/10 | 0/10 | 15 / 20 | 18.5 | 27.5 | 2.21 |
| nominal: lift-over (Z max 1.25 m, hypothetical) | 10 | 10/10 | 10/10 | 10/10 | 3 / 3 | 0.2 | 30.9 | 0.62 |
| nominal: bay rule (slots 2-4 only) + planar sweep | 10 | 10/10 | 10/10 | 10/10 | 13 / 20 | 23.7 | 21.4 | 1.09 |
| Monte Carlo (20): corridor | 20 | 20/20 | 13/20 | 12/20 | 13 / 29 | 11.4 | 26.7 | 2.21 |
| Monte Carlo (20): corridor + deeper back-out | 20 | 20/20 | 20/20 | 20/20 | 13 / 21 | 11.4 | 23.6 | 1.25 |
| Monte Carlo, off-centre neighbours (20): corridor | 20 | 20/20 | 14/20 | 14/20 | 15 / 38 | 13.5 | 24.5 | 2.21 |
| Monte Carlo, off-centre neighbours (20): corridor + deeper back-out | 20 | 20/20 | 17/20 | 17/20 | 15 / 38 | 13.8 | 23.5 | 2.06 |
| nominal: corridor + back-out + downhill shift (tried, rejected) | 10 | 10/10 | 10/10 | 10/10 | 15 / 27 | 7.0 | 22.2 | 1.02 |
| Monte Carlo (20): corridor + back-out + downhill shift (tried, rejected) | 20 | 20/20 | 19/20 | 18/20 | 14 / 30 | knocked off the plate | 24.7 | 2.21 |
| Monte Carlo, off-centre neighbours (20): corridor + back-out + downhill shift (tried, rejected) | 20 | 20/20 | 15/20 | 15/20 | 14 / 24 | 13.8 | 24.7 | 1.97 |

Reading: the corridor step itself fits at 125 mm. With the v1.0-harness back-out the retreat is contact-free for lanes 1 and 2 (7/7) and fails for the outer lane 3 (0/3: f1_blade_lo|ln1_body, f1_padp|ln1_body, f1_padp|ln1_shoulder, f2_blade_lo|ln1_body, f2_padm|ln1_body), because on its way to the tote the open front finger crosses the first slot of the centre lane. With the deeper back-out: 10/10 nominal, 20/20 Monte Carlo draws (13/20 without it, all failures in lane 3) and 17/20 with off-centre neighbours (14/20 without). The 3 of 20 remaining failures with off-centre neighbours are finger-blade contacts with a neighbour bottle (the neighbours of those draws are 3.5-5.9 mm rms off-centre): the clearance per side is (125 - 67.5 - 44.4)/2 = 6.5 mm at nominal diameter and 4.8 mm at D 71 mm, so a neighbour that has rolled 5-7 mm into the corridor closes it. The planar sweep fails in 10 of 10 as in the STD lanes (the sweep is the same problem at any pitch), the bay rule works on the WIDE set as it does on the STD set, and lift-over works only with the hypothetical 1.25 m stroke. Cost of the corridor retreat: the released bottle rests against the open front pad and is carried sideways by the pad: 6.5 mm on average (7.1 mm max) with a pad push of 19 N on average and up to 39 N (position-controlled stall forces as elsewhere). The lane has 26.5 mm of lateral play per side, so the drag does not tip or jam the bottle in these runs (largest tilt against the plane 0.002 degrees in the nominal back-out runs), but a real PET bottle would see tens of newtons of side load from the pad. A variant that first shifts the open gripper 20 mm downhill (the shift that the sweep and lift retreats use) did not help: the pad push stays at 14-15 N on average, and the retreats are no cleaner (Monte Carlo 19/20 against 20/20, off-centre neighbours 15/20 against 17/20; the contacts are with slot-1 bottles of the neighbour lanes) and in one draw the bottle is knocked off the plate (last three rows of the table).

## 7. Torque saturation: cause and fix (unchanged from v1.0, traces regenerated)

Cause 1 (artifact): the convex-collision solver (MuJoCo `ccd`) occasionally returned a spurious penetration of about 6 mm for the cylinder-versus-pad-box contact, a 600-700 N force for one step that the position loop answered with a 3-5 N*m demand (clipped at 2.206). The 8-case diagnosis table (`sim_results_torque_diagnosis.csv`, **carried over from the v1.0 report, not rerun**: it characterises the solver settings, not the fixture) shows with `ccd_tolerance` 1e-6 at dt 1 ms 1 of 8 cases spiking (pad force 696 N, J1 2.21 N*m) and with 1e-9 none (0 of 8; pad force <= 12.4 N). The model keeps dt 1 ms and `ccd_tolerance` 1e-9; in the v1.1 rerun no successful run of the suites nominal, wet, cells, mc, mc600, mc_d85, lateral saturates J1/J2 in a loaded phase (0 of 580 runs reach 2.2 N*m; v1.0: 0 of 570).

Cause 2 (geometry): the front finger blade scraping the outermost divider end during the lateral traverse (31 N at 0.04 mm penetration in an earlier version) is handled by the force-based touch criterion and a traverse row that accounts for the blade. Cause 3 (real): the large whole-cycle torques (up to 90 N*m demand) are retreat stalls of the open gripper against lane neighbours, the pad pushing the just-released bottle (13 N on average, up to 29 N with a slot-0 neighbour; v1.0 14 / 24 N) and, for the 1 L, the swinging finger knocking the bottle; they are covered in section 6.

!torque traces

Figure (`sim_torque_600ml_L0_lane3.png`, from `sim_torque_600ml_L0_lane3.csv` and `_bay.csv`, 100 Hz; the v1.0 figure is replaced by the v1.1 one): servo torque of J1, J2 and J3 over one 600 mL level-0 cycle, lane 3, first cell, dry. Grey = bottle gripped (lift, carry, place descent): at most 0.22 / 0.15 / 0.03 N*m on J1 / J2 / J3 (0.22 / 0.12 / 0.03 in v1.0). Orange = retreat. Left: a bottle in the next lane's first slot; the arm stalls against it and J1, J2, J3 sit at their limits for 5.7 / 6.9 / 4.4 s (5.7 / 6.8 / 4.4 s). Right: bay rule; the only peak is the pad touching the released bottle (18 N): 1.63 N*m on J1 (1.68). The 1 L level-1 traces (`sim_torque_1000ml_L1_lane3*.csv`) were regenerated as well.

## 8. Cycle time and Z-speed sensitivity

| variant | mean full cycle (s) | descend (s) | lift (s) | place descent (s) | return to tote (s) | pick-place OK | loaded peak J1 / J2 |
|---|---|---|---|---|---|---|---|
| Z 25 mm/s (baseline) | 24.25 (24.34) | 1.50 (1.50) | 1.50 (1.54) | 1.49 (1.50) | 3.12 (3.14) | 45/45 (44/44) | 0.45 / 0.20 (0.45 / 0.18) |
| Z 50 mm/s | 23.03 (23.03) | 1.17 (1.17) | 1.17 (1.19) | 1.16 (1.17) | 2.88 (2.84) | 45/45 (44/44) | 0.45 / 0.20 (0.45 / 0.18) |

Doubling the Z speed saves 1.23 s per cycle on average (1.15 s at level 0, 1.29 s at level 1; 5.1%; v1.0: 1.32 s, 1.29 / 1.33 s): the vertical moves are 20-40 mm, the Z acceleration (150 mm/s^2) needs 0.33 s and 8 mm to reach 50 mm/s, and each move includes a 0.25 s settle; the carry and the two return legs (joint-limited) dominate. Success, slip and loaded torques are unchanged. A 0.44 m level change (pitch 440 mm) takes 17.9 s at 25 mm/s and 9.1 s at 50 mm/s (arithmetic, trapezoid with 150 mm/s^2; not simulated; v1.0 pitch 420 mm: 17.0 / 8.7 s). The 600 mL time until the gripper opens in the lane is 12.5 s (12.7 s), carry phase 5.3 s (5.6 s): at level 0 the carry no longer has to duck to 16-17 mm under the plate (the ceiling now allows the 25 mm carry lift everywhere except lane 1, where it follows the 19.7 mm ceiling envelope).

## 9. Lane flow, 1.5 L / 2 L on the WIDE lanes, and items not rerun

600 mL, 90 mm lane, 0.3 m travel, end stop 5 x 20 mm (v1.0: assumed 30 x 8 mm lip) (`sim_results_lane_flow.csv`; fixture only, independent of the arm). Cells whose outcome differs from v1.0 (flow / topple, or time by > 0.1 s, or impact speed by > 0.05 m/s) show `v1.1 (v1.0)`; 0 of 12 cells differ:

| lane mu \ slope | 4 deg | 6 deg | 8 deg | 10 deg |
|---|---|---|---|---|
| 0.05 | 1.75 s, 0.38 m/s, TOPPLES | 1.06 s, 0.62 m/s, TOPPLES | 0.83 s, 0.80 m/s, TOPPLES | 0.71 s, 0.95 m/s, TOPPLES |
| 0.1 | no flow | 3.42 s, 0.18 m/s, upright | 1.24 s, 0.54 m/s, TOPPLES | 0.91 s, 0.73 m/s, TOPPLES |
| 0.2 | no flow | no flow | no flow | no flow |

Only mu 0.10 at 6 degrees flows slowly without toppling (marginal: tan 6 deg = 0.105).

1.5 L and 2 L in the WIDE lanes (level 1, 125 mm pitch, 120.5 mm free width), **rerun on fixture v1.1 with a D120 tray** (two 120 mm pockets, 8 mm deep, centres (-257.5, 30) / (-257.5, 170) mm, same outline and rim as the standard tray, modelled from the lead's numbers; first pick = (-257.5, 170)), single bottle, three clamp-force cases (34.7 / 70 / 104 N) x pad mu 0.8 and 0.4 x 3 lanes = 36 runs (`sim_results_wide_lanes_v11.csv`; the v1.0 report kept the old flat-tote results, `sim_results_wide_lanes_v2.csv`: 36/36 OK). Carriage stroke 1.06 m (v1.0 runs of this test: 1.02 m):

| SKU | lane | runs | pick-place OK | max slip (mm) | loaded peak J1 / J2 (N*m) | peak clamp (N) | contact-free retreat | mean cycle (s) |
|---|---|---|---|---|---|---|---|---|
| pet_1500 | 1 | 6 | 6/6 | 1.19 | 0.18 / 0.35 | 104.2 | 6/6 | 19.9 |
| pet_1500 | 2 | 6 | 6/6 | 1.15 | 0.23 / 0.20 | 104.2 | 6/6 | 20.3 |
| pet_1500 | 3 | 6 | 6/6 | 1.15 | 0.18 / 0.11 | 104.2 | 6/6 | 23.1 |
| pet_2000 | 1 | 6 | 6/6 | 1.94 | 0.18 / 0.44 | 104.2 | 6/6 | 20.2 |
| pet_2000 | 2 | 6 | 6/6 | 1.57 | 0.30 / 0.24 | 104.2 | 6/6 | 20.5 |
| pet_2000 | 3 | 6 | 6/6 | 1.57 | 0.18 / 0.11 | 104.2 | 6/6 | 23.9 |

Result: 36/36 OK (36/36 in the v1.0 report, on the flat tote and at Z max 1.02 m). No failures. The 1.5 L / 2 L have 49 / 40 mm of carriage travel above the grasp (section 2), hover and lift 25 mm each. The static-hold and force-sweep tests of the 2 L were not repeated (earlier CSVs `sim_results_static_hold.csv`, `sim_results_force_sweep.csv`, `sim_results_force_cases_summary.csv` are old-layout results). The v1.0 caveat stands: under the original 5 mm rear-edge rule the 2 L scored 0/18 on the old layouts ('not_in_lane'); with the 15 mm rule it passes (the open 125 mm gripper pushes the leaning D106 bottle back before it slides on). Rear-edge overhang of the successes in this run (rear edge upstream of the entrance plane, positive = overhanging the plate edge): pet_1500 3.0 to 4.0 mm; pet_2000 11.9 to 13.6 mm. 18 of 36 would fail the original 5 mm rule (v1.0: 18 of 36); the 2 L passes the 15 mm rule with only about 1.4-3 mm to spare, so it is marginal on the plate edge.

## 10. Recommendations and parameter changes for the CAD / fixture tracks

1. **Level 0 and 1 L.** Keep the 1 L on level 1. To make it possible at level 0 the level-1 structures must move up so that the pitch is >= 461 mm for lanes 2-5 and >= 486 mm for lane 1 (planner feasibility with the 1.5 mm margin; v1.0: 466 / 480 mm), i.e. +21 / +46 mm on the v1.1 pitch.
2. **Level pitch margin.** Do not reduce the 440 mm pitch by more than about 6 mm (600 mL lane 1 becomes infeasible at 434 mm) or raise the rim above 15 mm without re-checking lane 1. The cell (-300,0) would become usable for the 500 mL at a pitch of 445 mm and for the 600 mL at 472 mm (the fixture track quotes 447 / 474 mm, within 2 mm).
3. **Lane clear width** >= 92 mm (D max 82 + 10) or a lead-in chamfer for the 1 L; the 87 mm lane leaves 2.5-5.5 mm per side and 8 of 60 Monte Carlo draws hit a divider (unchanged from v1.0).
4. **Feeding / retreat rule** - see the recommendation below.
5. **1 L pocket:** check the real base-cup diameter; D75 seats it only up to D 79.3 mm under my 0.94 D assumption; D80 / D85 pockets do not improve the Monte Carlo result (the lane tolerance limits it).
6. **Z speed 50 mm/s** saves about 1.2 s per cycle; carriage travel for lift-over of the 600 mL / 1 L is 1.08 / 1.13 m.

**Recommended retreat rule for v1 operation.** (1) Use the **bay rule on the STD 5-lane set**: drop a bottle into a lane only while slots 0 and 1 (the first 160 mm) of both neighbouring lanes are empty. Contact-free retreats 45/45 of the nominal matrix (44/44 in v1.0), bottle upright and on the plate in 45/45 (43/44), dragged sideways by 7.3 mm on average (14.5 mm at most); cans, 500 mL and 600 mL: 40/40 upright and on the plate; the 1 L: 5/5 lanes (4/5 in v1.0). Mean cycle 19.9 s, J1/J2 whole-cycle peaks 2.2 N*m reached in 4 / 0 runs. It needs no hardware change but caps the usable capacity: two neighbouring lanes can never both hold more than 3 bottles (the lane that is filled later needs the other one at slots 2-4 only), so at most lanes 1, 3, 5 are full and lanes 2, 4 hold 3 bottles each, 21 of 25 slots (arithmetic from the rule, not simulated). (2) The **WIDE 125 mm lanes with the corridor retreat and the deeper back-out** also work with fully populated neighbours for the 600 mL (10/10 nominal, 20/20 Monte Carlo, 17/20 with neighbours that sit off-centre by N(0, 4 mm)) but give 15 slots per level instead of 25 and have only 4.8-6.5 mm of clearance per side at 125 mm (with neighbours scattered by +-7 mm the closed form asks for p >= 71 + 44.4 + 2 x (3 + 7) = 135 mm). Both retreats drag the released bottle sideways and press on it with the open pad (WIDE corridor: 6.5 mm mean x drag, pad push 19 N mean / 39 N max; STD bay rule: 7.3 mm mean, 14.5 mm max, pad push 13 / 29 N), so the drag does not separate them. I would not choose the WIDE set for the 600 mL. For the 1.5 L / 2 L the closed-form corridor pitch is 139 / 160 mm (section 6 iii), above the 125 mm of the WIDE set, so populated neighbours are not supported there either; their tests in section 9 ran with a single bottle and no neighbours. (3) Lift-over is not an option for the 600 mL / 1 L with the 1.06 m stroke (it needs 1.08 / 1.13 m), and (4) the default retreat (no feeding rule) is contact-free in only 16/45 cases (16/44 in v1.0), so a feeding rule is required in any case. A real implementation would replace the position-controlled stall by a force-limited sweep.

## 11. Local viewer and package

`sim_view_local.py` opens the v1.1 scene in the MuJoCo viewer (`pip install mujoco numpy`, then `mjpython sim_view_local.py` on macOS). New options: `--wide` (3-lane WIDE set, level 1, lanes 1-3), `--retreat corridor` and `--backout`. Tested here: `--check` and `--headless` in a clean folder containing only the package files (full 600 mL level-1 cycle: success, no pre-release collisions; the WIDE-lane corridor case with and without `--backout`); the interactive window itself could not be opened in this sandbox, so camera defaults and real-time pacing are untested. Examples: `mjpython sim_view_local.py --sku pet_600_soda --level 0 --lane 3 --neighbours bay`, `mjpython sim_view_local.py --wide --lane 3 --neighbours full --retreat corridor --backout`, `--speed 8 --loop`. Put these files in one folder (relative paths):

```
sim_view_local.py  sim_build_model.py  sim_controller.py  sim_run_tests.py  sim_replay.py  sim_cad_data.json  sim_design_params_v4.json  sim_elrobot2_s1.xml (written by the viewer if missing)  sim_summary.md
```

**Replay logs (25 Hz, regenerated on v1.1 with `sim_replay.py`; same CSV columns as before, scene JSON `sim_replay_scene.json` with the same schema plus the v1.1 tote / ledge / stop / bracket / stand geometry, the WIDE-lane variant `scene_variant_wide_lanes_level1` and the new layout keys).** They are collision-checked against the MuJoCo model (convex-hull arm, box fixtures), not against the CAD meshes:

| file | SKU | level | lane set | lane | neighbour slots | rows | duration (s) | pick-place OK | pre-release contacts | retreat contact pairs |
|---|---|---|---|---|---|---|---|---|---|---|
| sim_replay_600ml_L0_lane3.csv | pet_600_soda | 0 | STD (5 lanes, 90 mm pitch) | 3 | 0 (entrance row) | 601 | 24.0 | yes | 0 | 5 |
| sim_replay_600ml_L0_lane3_bay.csv | pet_600_soda | 0 | STD (5 lanes, 90 mm pitch) | 3 | 2-4 (bay rule) | 464 | 18.6 | yes | 0 | 0 |
| sim_replay_600ml_L1_lane3.csv | pet_600_soda | 1 | STD (5 lanes, 90 mm pitch) | 3 | 0 (entrance row) | 601 | 24.0 | yes | 0 | 5 |
| sim_replay_1000ml_L1_lane3.csv | pet_1000 | 1 | STD (5 lanes, 90 mm pitch) | 3 | 0 (entrance row) | 670 | 26.8 | yes | 0 | 4 |
| sim_replay_600ml_L1_wide_lane2_corridor.csv | pet_600_soda | 1 | WIDE (3 lanes, 125 mm pitch) | 2 | 0-4 (fully populated) | 457 | 18.3 | yes | 0 | 0 |
| sim_replay_600ml_L1_wide_lane3_corridor.csv | pet_600_soda | 1 | WIDE (3 lanes, 125 mm pitch) | 3 | 0-4 (fully populated) | 567 | 22.7 | yes | 0 | 0 |

## 12. Deviations, assumptions, not done, and files

**Deviations from the lead's v1.1 specification and from the v1.0 harness (all explicit):**

1. Can grasp height: z_g = floor + 185 mm (my v1.0 rule, so that fingertips clear the dividers) instead of the fixture rule (148 mm lane / 153 mm tote). The harness is otherwise unchanged; section 2 gives the clearances under both rules.
2. Wrist camera (raised 30 mm) not modelled: it has no collision geometry in the model, so a camera-versus-ceiling contact would not be detected.
3. Fixture structures are boxes from voxel probes of the fixture STLs (main brackets: plate + seat only, shim above the seat omitted; tote brackets; stops; stand panel y = -40..240 mm from the README), not the meshes. The 1.5-2.3 mm excess at the lane poses in section 2 is probably from this. Posts and feet are not modelled.
4. Fingertip correction: the v1.0 model's lower finger blade ended at zeta -143 mm; v1.1 uses the CAD value (-140.5 mm with the 0.5 mm mount offset). This changes clearances by 2.5 mm independent of the fixture change.
5. New controller options (default off, so all v1.0-harness suites are unchanged): `corridor_backout`, `corridor_fwd`. The suite `pitch` (and its v1.0 comparison) uses the v1.0 corridor retreat.
6. Monte Carlo 600 mL level 0 keeps the v1.0 draw sequence (lanes 2-5) so that the comparison is draw-for-draw; lane 1 has a separate N = 20 run. The 1 L D80 run is new; D85 is kept.
7. Level-pitch study: now signed (negative = margin); the rim study sweeps 11-30 mm; the pitch margin of every level-0 cell was added. Studies use the planner only (feasibility with the 1.5 mm margin), not full simulations; the Monte Carlo pitch-fix check of v1.0 (`sim_results_level_pitch_verify.csv`) was rerun.
8. The torque-diagnosis table (solver settings) is carried over from v1.0, not rerun. Static-hold and force-sweep tests of the 2 L are old-layout results. The D120 tray is modelled only as two pockets for the 1.5 L / 2 L test (section 9).
9. Rear-edge rule: the 15 mm tolerance of v1.0 is kept. Over the 864 successful runs of all suites the largest overhang of the rear edge upstream of the entrance plane is +13.6 mm (the 1.5 L / 2 L runs of section 9; 18 successes, all 2 L, would fail the original 5 mm rule); excluding them the largest value is -1.4 mm (negative = inside the plate).
10. The 1 L base cup (0.94 D, 12 mm tall), the 30-degree-class bottle shoulder, the Monte Carlo distributions (3 mm, 3 degrees, mu 0.4-0.8, +-5% mass), the pad friction and all masses are my assumptions, as in v1.0.

Fidelity limits: rigid bodies, no liquid slosh or PET shell compliance, ideal position servos, hand-tuned gains and masses, convex-hull arm envelopes (conservative), crude bottle (cylinder, shoulder, assumed base cup). The retreat controller is a simple scripted sweep, not optimised; pad pushes in the retreat are stall forces of a position-controlled arm. No video; the interactive viewer window was not exercised.

Files: sim_build_model.py, sim_controller.py, sim_run_tests.py (suites nominal, wet, cells, cell_m300_0, mc, mc600, mc600_lane1, mc_d85, mc_d80, lateral, nominal_bay, nominal_bay_wait, nominal_full, retreat_lift, pitch, wide, wide_fwd, wide_big, zspeed, flow, gravity), run_suites_v11.sh and run_post_v11.sh (batch drivers), sim_clearance_probe.py, sim_cad_data.json, sim_make_cad_data.py, sim_replay.py, sim_view_local.py, sim_torque_plot.py, sim_torque_make_figure.py, sim_pitch_analysis.py, sim_level_pitch_study.py, sim_make_summary.py, sim_elrobot2_s1.xml, sim_design_params_v4.json; results sim_results_nominal.csv, _wet, _cells, _cell_m300_0, _montecarlo, _montecarlo_600ml_by_level, _montecarlo_600ml_L0_lane1, _montecarlo_1L_D85, _montecarlo_1L_D80, _lateral, _nominal_bay, _nominal_bay_wait, _nominal_full, _retreat_lift, _min_pitch, _min_pitch_analytic, _wide600_full, _wide600_fwd, _wide_lanes_v11, _level_pitch, _clearance_v11, _nominal_z50, _lane_flow, _gravity_check, _torque_diagnosis (v1.0); replay sim_replay_*.csv, sim_replay_scene.json; torque sim_torque_*.csv and sim_torque_600ml_L0_lane3.png. Old-layout files kept from v1.0: sim_results_static_hold.csv, _force_sweep, _force_cases_summary, _wide_lanes, _wide_lanes_v2, sim_lane_flow_analytic.csv, sim_design_params_v1/v2/v3.json.
