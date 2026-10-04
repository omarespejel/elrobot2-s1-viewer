# S2 SIM replays (ElRobot2 S2-RC2, PET collar-jaw mode) - README

Produced with the FINAL hand (Hand-track manifest `s2hand_frame_manifest.json`, version b33681cb-3ff2-4bb9-8694-cf66b847b2be; convex decomposition of the STLs, see `s2sim_hand_pieces.json`), after the grip-clamp bug fix (the pre-fix replays were withdrawn), release variant **Vd2.0** (lip lowered 2.0 mm by the rack, then jaws opened, then retreat). Closing-distance policy active (window 29.2-32.5 mm, up to 3 re-grasps). Nominal-finish bottles (cap 30.5, ring 33/t2, neck 25.5, free neck 6 mm) for the four nominal cases; the two failure cases are Monte-Carlo draws from `s2sim_results_g4_core_Vd2.0.csv` (box G4).

## Cases

| case | CSV | rows | duration s | success (strict) | success_grip | reason | cap-wall gap range mm | q3 range deg |
|---|---|---|---|---|---|---|---|---|
| 500ml_L0_lane5_p0.318_C | `s2sim_replay_500ml_L0_lane5_p0.318_C.csv` | 576 | 23.04 | True | True | - | 30.20-49.57 | -29.6 to 31.1 |
| 600ml_L0_lane3_p0.318_C | `s2sim_replay_600ml_L0_lane3_p0.318_C.csv` | 557 | 22.28 | True | True | - | 30.22-49.61 | -63.8 to 35.2 |
| 600ml_L0_lane3_p0.318_F | `s2sim_replay_600ml_L0_lane3_p0.318_F.csv` | 576 | 23.04 | True | True | - | 28.46-49.54 | -63.8 to 35.2 |
| 600ml_L1_lane1_p0.318_C | `s2sim_replay_600ml_L1_lane1_p0.318_C.csv` | 885 | 35.40 | True | True | - | 30.22-49.65 | -112.3 to -10.1 |
| 600ml_tote_pocket1_pick_C | `s2sim_replay_600ml_tote_pocket1_pick_C.csv` | 104 | 4.16 | None | None | pick only (stop after the verified 3 mm lift) | 30.22-49.00 | -29.6 to -29.6 |
| fail_600ml_L0_lane3_F_draw34_not_in_lane | `s2sim_replay_fail_600ml_L0_lane3_F_draw34_not_in_lane.csv` | 626 | 25.04 | False | False | not_in_lane | 28.48-49.54 | -64.4 to 4.1 |
| fail_600ml_L0_lane3_C_draw44_collision | `s2sim_replay_fail_600ml_L0_lane3_C_draw44_collision.csv` | 590 | 23.60 | False | False | collision:bottle_body|div3_nose | 31.54-49.71 | -151.1 to -49.2 |

`success` (strict) = bottle upright, inside its lane and still upright after reaching the lane stop; `success_grip` = the same judged at the moment before the first contact with the lane stop (the 20 mm stop topples a 600 mL bottle that arrives faster than about 0.3 m/s on a low-friction lane, which is a bench property, not a gripper property; see summary section 7). The pick-only case ends after the verified 3 mm lift of a bottle from tote pocket 1 (success flags are not applicable).
Failure cases: `fail_600ml_L0_lane3_F_draw34_not_in_lane` = type F, the bottle is dragged back out of the lane when the open jaw retreats (gripper-attributable); `fail_600ml_L0_lane3_C_draw44_collision` = type C, lateral placement error 3.66 mm against a lane slack of 3.45 mm: the bottle body strikes the nose of divider 3 (placement-error-attributable, not gripper-attributable). Case metadata (draw index, errors, reason) is in `s2sim_replay_scene.json` -> cases.

## Format (S1 format, 25 Hz)

```
s2sim_replay.py - 25 Hz replay logs (s2sim_replay_<case>.csv) and the static scene (s2sim_replay_scene.json) in the S1 format for external rendering with the real CAD meshes.

CSV columns (one row per 0.04 s of simulated time, whole episode: init hold, drop-over, close, lift, carry, insert, lower, release, open, retreat, hold):
  t [s]; phase; z_carriage [m]; q1_deg, q2_deg, q3_deg; closing_axis_deg; wrist_x, wrist_y [m]; gripper_apex_gap_mm;
  then for the carried bottle ('bottle') and every neighbour (tn<k> = tote bottles, ln<lane>_s<slot> = bottles in the neighbouring lanes, slot 0 = at the lane mouth):  <name>_x,_y,_z,_qw,_qx,_qy,_qz;
  then (S2 additions, appended at the END so that S1 readers that index by name keep working): heading_deg, tendon_gap_mm.
CONVENTIONS (S1 conventions kept; differences for the S2 PET hand in capitals)
  World: x lateral, y toward the shelf, z up (m). J1 axis = world z through (0, 0). z_carriage = world z of the carriage / J1-horn-plane frame origin (slide joint z_lift, absolute); zeta = z_world - z_carriage + 0.150 m.
  q1: angle of link L1 from +x, CCW seen from above, deg; q2: L2 relative to L1; q3: HAND HEADING relative to L2:  q3 = 0 when the hand HEADING (direction J3 axis -> bore axis, hand +y) points along the forearm L2 outward
     (LEAD convention).  heading_deg = q1+q2+q3 wrapped to (-180, 180] = world direction of the hand heading.  closing_axis_deg = heading_deg - 90 (hand +x, jaw A side) wrapped: world direction in which jaw A moves out.
  gripper_apex_gap_mm = CAP-WALL FACE-TO-FACE DISTANCE of the two jaws in mm: 29.0 at the closed hard stop (rack s = -2.25 mm), 49.0 at the recommended drop-over opening (s = +7.75 mm);
     the viewer maps the travel of each jaw from the closed hard stop as (gap - 29) / 2 mm; jaw A moves +x, jaw B -x in the hand frame (hand frame: x closing axis, y heading, z = zeta, origin on the J3 axis).
     Type F has no cap-wall faces; the same number is reported (it is the virtual cap-wall distance of the common rack kinematics).  tendon_gap_mm = gripper_apex_gap_mm - 29.0.
  Pinion angle (for the viewer): -4.7746 deg per mm of rack travel s, s = -2.25 + (gap - 29)/2.
  Bottles: body frame origin = centre of the bottle's BASE plane, local +z along the bottle axis (towards the neck).  Quaternion = (w, x, y, z), body-to-world.
     Lane bottles are static (slots behind the mouth slot) or dynamic (mouth slot) and are leaned 6 deg so that their axis is normal to the lane plane; tote neighbours are upright and static.
  CAD hand: s2hand_frame_*.stl are in the hand frame; mount the hand frame at the J3 axis (wrist_x, wrist_y, z_world = z_carriage + (zeta - 150 mm)) rotated so that the hand +y points along heading_deg.
```

## Scene

`s2sim_replay_scene.json`: `static_geoms` + `layout` for cases with `spec.level == 0`, `static_geoms_level1` + `layout_level1` for `spec.level == 1` (shelf shifted up by the pitch 0.318 m); every case entry has `csv`, `spec`, success flags, release settings and the list of bottle bodies (column prefixes). Geometry kinds: box, convex_hull (nose/stop) with world coordinates in metres.