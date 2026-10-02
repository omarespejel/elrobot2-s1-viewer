# ElRobot2-S1 arm joints and links (track prefix `arm_`), status 2026-10-01

All 22 printed parts (the brief's list plus the rings, caps, spacers, covers and clamp it implies) and the PRINT-FIRST servo coupon (23 STL files) exist as watertight STEP (assembly pose, q1 = 90 deg, q2 = 0) + STL (print pose), saved as artifacts.
Everything below is CAD-derived or beam-theory; **no FEA, no slicer run, nothing printed yet**. Numbers marked PROVISIONAL depend on unverified servo/bearing dimensions
(named parameters at the top of arm_build.py) or on the coupon result.

## 1. Parts (STL = print pose, STEP = assembly pose; mass/time = analytic estimate, +-30 %)
| files | part | PETG/TPU | print time | print orientation |
|---|---|---|---|---|
| arm_coupon_sts3095.stl / arm_coupon_sts3095.step | PRINT FIRST: STS3095 pocket 65.5 x 30.5, stepped shaft-offset scales, 8 x M3 PCD 24 stub, 6806 / 6701 seats +0.0/+0.1/+0.2, insert/clearance ladder | 65 g | 1.9 h | flat, no supports (166 x 127 x 11.2 mm) |
| arm_j1_pedestal.stl / arm_j1_pedestal.step | carriage flange (6 x M3 inserts) + inverted-U tunnel + 2 x 6806 housing + STS3095 pocket | 232 g | 6.9 h | upright as assembled (flange plate vertical, servo pocket opening down, bearing bore vertical) |
| arm_j1_hub.stl / arm_j1_hub.step | tube OD29.95/ID18 + collar OD34 + 46x46 flange (identical part used at J2) | 28 g | 1.0 h | lying down: tube axis horizontal (along X), flange flat edge on bed (in-plane strength) |
| arm_j1_ring.stl / arm_j1_ring.step | OD34 ring between horn and hub tube (7 mm) | 5 g | 0.2 h | flat, axis vertical |
| arm_j1_cap.stl / arm_j1_cap.step | bearing cap, 6 x M3 on PCD 50 | 8 g | 0.2 h | flat, counterbores up |
| arm_j1_spacer.stl / arm_j1_spacer.step | 13 mm spacer between the 6806 outer races | 6 g | 0.2 h | flat, axis vertical |
| arm_j1_clamp.stl / arm_j1_clamp.step | servo clamp plate (z 45..48), slotted M3 holes | 13 g | 0.4 h | flat, flipped so counterbores face up |
| arm_l1.stl / arm_l1.step | U-channel 60 wide, floor z145..150, walls to 198, J2 pocket + 14 cover posts | 244 g | 7.0 h | upright as assembled (floor down, open top), rotated 45 deg about Z on the bed |
| arm_l1_cover.stl / arm_l1_cover.step | 3 mm structural cover (z 198..201), also the J2 servo clamp | 48 g | 1.4 h | flat (countersinks up), rotated 45 deg about Z on the bed |
| arm_j2_housing.stl / arm_j2_housing.step | 2 x 6806 housing, 60 x 60 flange bolted under the L1 floor | 43 g | 1.3 h | flange face down on the bed (flipped), bore opening up; lip printed first |
| arm_j2_hub.stl / arm_j2_hub.step | same part as the J1 hub, inverted; flange = L2 root interface | 28 g | 1.0 h | lying down: tube axis horizontal, flange flat edge on bed (same part as arm_j1_hub) |
| arm_j2_ring.stl / arm_j2_ring.step | OD34 ring (4 mm) | 3 g | 0.1 h | flat |
| arm_j2_cap.stl / arm_j2_cap.step | bearing cap | 8 g | 0.2 h | flat, flipped so counterbores face up |
| arm_j2_spacer.stl / arm_j2_spacer.step | 13 mm spacer | 6 g | 0.2 h | flat |
| arm_l2.stl / arm_l2.step | U-channel 52 wide, root plate z97.5..105.5, walls to 127, J3 pocket (servo top 127.3 uncovered) | 183 g | 5.3 h | upright as assembled (floor down), rotated 45 deg about Z on the bed |
| arm_l2_cover.stl / arm_l2_cover.step | 3 mm cover z127..130 over u = 36..161 | 21 g | 0.6 h | flat, countersinks up |
| arm_j3_housing.stl / arm_j3_housing.step | 2 x 6701 housing, 48 x 48 flange under the L2 floor | 19 g | 0.6 h | flipped: 48x48 flange face down on the bed, cylinder up, bore opening up (lip printed last: <=1.6 mm overhang) |
| arm_j3_pin.stl / arm_j3_pin.step | D11.95 pin, D14 collar, cruciform key pockets, D3.4 through bore | 2 g | 0.1 h | standing, collar end on the bed (brim recommended) |
| arm_j3_spacer.stl / arm_j3_spacer.step | 10 mm spacer between the 6701 outer races | 1 g | 0.1 h | flat, axis vertical |
| arm_j3_horn_plate.stl / arm_j3_horn_plate.step | plate z85.5..89 with key tabs, centring post, M3 pilot, 4 x M3 on 10 x 10 | 2 g | 0.1 h | flipped: top face on the bed, key tabs + centring post up |
| arm_j3_flange.stl / arm_j3_flange.step | 46 x 46 x 3 gripper interface (4 x M3 on 36 x 36), key tabs | 8 g | 0.2 h | flat, bottom on the bed, key tabs up |
| arm_cable_clips.stl / arm_cable_clips.step | 6 window grommets for the 14 x 10 cable windows | 4 g | 0.1 h | flat, lip down; 6 window grommets (14 x 10 windows in L1/L2 walls) |
| arm_tpu_shims.stl / arm_tpu_shims.step | 13 TPU 95A shims for the three servo pockets | 15 g | 0.4 h | flat; TPU 95A, 100% walls / 0.2 mm layers; 13 pieces: J1 (side x2, end x2, clamp pad), J2 (side x2, end x2), J3 (side x2, end x2) |

Total for one arm set (without coupon): 912 g PETG + 15 g TPU, about 27 h (estimate). Assembly: arm_assembly.step (62 solids: printed parts + servo / bearing / M3x50 / M3x20 dummies + TPU shims at compressed thickness);
section view: !section through x = 0.
Hardware: arm_hardware_list.csv (75 screws in 9 types, 50 heat-set inserts, 4 x 6806, 2 x 6701).

## 2. Design as built (changes vs the earlier README are marked *)
1. J1 and J2 are the same cartridge, inverted. Housing bore D42.1 for 2 x 6806 (spacer 13 mm between outer races), lip/cap hole D35.6, cap with 6 x M3x10 on PCD 50.
   Hub = tube OD29.95/ID18 + collar OD34 (loads the inner race) + 46x46 R6 flange; a separate ring (J1 7 mm, J2 4 mm) retains the other inner race. *Eight M3x50 on PCD 24 clamp
   L1 floor (J1) or L2 root plate (J2) + hub flange + collar + tube + ring to the horn (head seat z148.0 / z99.5, counterbore 2 mm, Belleville washer under each head). The hub bore is a blind pocket
   (closed by the horn), so no cable can pass through it: cables loop outside through the wall windows (grommets).
2. *J1 pedestal: flange plate 11 mm (y -110..-99, 6 x M3 inserts at x = +-30, z = 130/95/60), inverted-U tunnel (walls 4, ceiling 8), 10 mm front/rear walls, 45-degree gusset cone under the housing.
   STS3095 pocket 31 x 67 (shaft at y = 0, 17 + 1 mm from the front end), TPU shims, clamp plate z45..48 hangs below the tunnel (tunnel bottom z48).
3. L1: 60 x 257 mm U-channel, floor 5 mm (z145..150), walls 2.4 mm to z198, 3 mm cover z198..201 clamps the J2 servo. *The cover is structural (closed-box torsion): 14 posts per cover (7 per side, 31 mm pitch).
4. L2: 52 x 252 mm, root plate z97.5..105.5 under the J2 hub flange (screw access through a D34 hole in the floor), floor 4 mm (J3 pad 7 mm), walls to z127, cover z127..130 over u = 36..161.
5. J3 (lead update: interface plane zeta 62.5): housing D26 + 48x48 flange with 2 x 6701; *pin D11.95 with cruciform key pockets and ONE M3x20 countersunk screw from the flange underside
   through the pin bore into the horn plate pilot (assembly order: pin up from below, servo + plate (4 x M3x5 into the horn) down from above, flange + screw last).
   Servo torque limit <= 1.0 N m (pin tau 3.0 MPa; key bearing about 6 MPa).
6. J3 servo top (z127.3) stays uncovered to respect L2 <= 130; its upward retention relies on TPU shims only (no set screws modelled).

## 3. Key results (PROVISIONAL, from arm_verification.csv)
- Stack levels vs datum table: 37/37 rows within 0.5 mm (max 0.03 mm), J3/L2 rows against the table recomputed for J3 plane 62.5; one extra row explains the 68.07 mm area step on the J3 pin (key-pocket ceiling at 65.5 + 2.6, not the collar top at 67.1, which is isolated in a +-0.4 mm window) (arm_datum_table_v2.csv).
- Interference: 240 AABB-overlapping pairs tested with OCC booleans, 0 unintended overlaps; 17 intentional thread engagements (M3x50 in horns, M3x20 in plate pilot).
- Sweep J1 -20..160 x J2 -130..130 (10 deg): no contact with pedestal, carriage plane y < -110 or the 2040 column for any J3 position in front of the J1 axis (y >= 0), gripper aligned or at any J3 angle.
  First-contact radius for the full joint box (J3 behind the axis): 0.169 m at q2 = +-130 deg, larger r for q1 far from 90 deg (arm_sweep_first_contact.csv, arm_sweep_summary.csv).
  Closest approaches (y >= 0): L1 J2 flange to the plate plane 3.1 mm (q1 = -20 deg), gripper to tunnel 50 mm.
- Tip deflection at r = 0.385 m, 2.4 kg: L2 1.19 + L1 0.80 + L1 twist 0.27 = **2.3 mm**, pedestal tunnel +1.3 mm, total **3.6 mm (3.9 mm with cover-screw slip)**.
  This holds only if the cover acts as the lid of a closed box. With the cover not acting the open channel twists 22 deg: **36-43 mm**. Bond the cover (CA/epoxy bead on the wall tops) and measure.
- Bearing/bolt loads: J1 6806 radial 530 N each at the specified 10.6 N m, 611 N at the recomputed 12.2 N m (utilisation 0.22 of an assumed C0r 2.8 kN); pedestal screws 69-92 N tension (top row); hub tube 5.8-6.7 MPa (print lying down).
- **Mass is over the earlier budgets**: L1 side 0.627 kg (budget 0.485), L2 side 0.303 kg (0.195); J1 tilt moment 12.2 N m with the 0.44 kg gripper (spec 10.6). No lightening pass was done.

## 4. Open issues
1. Mass (above). 2. Cover-dependent torsion (above). 3. J3 servo retention and upper 6701 retention (press fit + spacer only). 4. Cable service loops at J1/J2 are not designed
(windows + grommets only; carriage plate needs a cable path). 5. Pedestal tunnel adds 1.3 mm of tip droop (thicker walls / no windows would recover about 0.4 mm). 6. L1 top is 201 mm (datum j2_clamp_top) vs 'free up to 200'.
7. Builders assume STS3095_AXIS = 'C48'; the 'A30' option is not implemented. 8. Previews/exploded view, cable-slack table, lightening, set screws: not done.

## 5. Verify with hardware (coupon first)
STS3095 shaft offset (17 mm default) and lateral offset, horn PCD / thickness (3.5) / thread depth (3.0), servo body 30 x 65 x 48 and STS3215 24.7 x 45.2 x 35, pocket clearance (+0.25 per side),
6806 / 6701 seat clearance and journal fit (J1 play up to 2.9 mm at the tip for 0.15 mm diametral total), insert hole D4.2, M3x50 length vs horn thread, STS3095 connector position vs pocket walls,
cover joint stiffness (23.5 N at the gripper, q2 = 31.5 deg, expect about 4 mm), J3 pull-out of the M3x20 in the plate pilot.

## 6. Interfaces / parameters other tracks must know
- J3_IFACE_ZETA = 62.5 (named parameter). Flange 46 x 46 x 3 covers |x| <= 23 only; M3x12 through both plates. The central M3x20 head is countersunk flush in the flange underside: the gripper top plate must be flat at the J3 axis.
- Carriage: 6 clearance holes M3 at x = +-30, z = zeta 130/95/60, plate face y = -110; screws M3x16 from behind; pedestal occupies |x| <= 45, zeta 45..145 (envelope checked). Cable path through/around the plate still to be defined.
- L1 floor bottom 145; L1 top 201; L2 top 130.0 (J3 servo top 127.3); L1 side 0.627 kg, L2 side 0.303 kg; J1 moment 12.2 N m.

## 7. Reproduce
`exec(open('arm_build.py').read()); exec(open('arm_verify.py').read()); build_all(); run_all_checks()` (about 2 min; verified in a clean process).
Scripts: arm_build.py, arm_verify.py; registry arm_registry.json.
