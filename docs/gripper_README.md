# ElRobot2-S1 gripper (GRIPPER track) - revision 4 (face 14 mm, pinion hub flange, can-fit fixes)

## 1. Revision 4 in five lines
1. Face width b = 14 mm for pinion and racks. Pinion tip-loading stall margin 0.89 (b = 12: 0.76); with the 1.2x-rated torque cap 1.27; pitch/HPSTC margins rated 2.74 / cap 2.28 / burst 2.38 / stall 1.60.
2. Pinion hub flange D24 x 3.5 mm carries the four horn-screw counterbores. It is on the SCREW-HEAD (floor) side of the gear, not the horn side: the heads must be reached from that side, so only a flange there can carry the counterbores. It runs in a D25 pocket in the base, 0.25 mm above the pocket floor / 624, and a neck D20 x 0.5 mm keeps its top 0.5 mm below the rack bottoms (the flange R12 passes under the rack tooth tips at y = 10.5).
   Walls: counterbore to flange edge 2.03 mm; M3 hole to tooth space 1.68 mm (rev 3: 0.48 mm). EXCEPTION: the 0.5 mm tall neck web is 1.23 mm wide at each hole (rule 1.6), compression only.
3. Stack: J3 interface plane = 45 + 14 + 3.5 + 0.5 = zeta 63.0 (rev 3: 57.0; expected 62.5 + 0.5 mm neck clearance). Body bottom -3.8 to J3 plane 63.0 = 66.8 mm.
4. Found and fixed with the new grasp-fit check (cylinders D57..D110, top at zeta -10, 320 mm long): the rev 3 finger head (6 mm inner thickening) and full-width yoke straps entered a can silhouette (1317 and 3747 mm3 at D66). Now: inner head 3 mm (clears a D57 can by 0.5 mm), yoke strap L-shaped (lower leg inner edge 7.5 mm inside the seat plane, full width only above zeta -2, R3 fillet). Overlap now 0 mm3 for all six diameters.
5. Re-run: 18/18 parts watertight, 15-body matrix at gap 70 and 20-position sweep all 0 mm3, positive controls 5 of 6 detected at one fixed 1 mm3 criterion (the sixth is a sensitivity limit, section 5), hand calcs, previews (6 PNG incl. a section through the pinion axis).

## 2. Stack (zeta mm, +up, origin on the J3/pinion axis) and grasp datums
| level | zeta | | datum | zeta | from body bottom |
|---|---|---|---|---|---|
| body bottom (floor underside) | -3.8 | | bottle top, rule z_g = z_floor + H + 10 mm | -10.0 | body bottom is 6.2 mm above it |
| 624 top = pocket floor | 3 | | finger top | -4.3 | 0.5 mm below |
| pinion flange | 3.25 .. 6.75 | | pad band top | -87 | **83.2 mm below** |
| neck / rack bed | 6.75..7.25 / 7 | | pad band centre | -112 | 108.2 mm below |
| racks + gear (b = 14) | 7.25 .. 21.25 | | pad band bottom | -137 | 133.2 mm below |
| cover plate | 21.5 .. 24.75 | | fingertip | -140 | 136.2 mm below |
| STS3215 (shaft down) | 24.75 .. 59.75 | | camera wedge | 30 .. 62 | |
| top plate / J3 plane | 60 .. 63 | | | | |
Gap = distance between the two pad V-apex lines; 40..125 mm = 42.5 mm per rack = 202.9 deg = 2309 ticks (gripper_gap_angle_ticks.csv). Hard stops (notch ends): 2.0 mm/rack (109 ticks) beyond gap 40, 0.75 mm/rack (41 ticks) beyond gap 125.

## 3. Force chain, allowables, margins
T = 2 F_t r_p, F_t = T/(2 r_p), clamp per finger = 0.85 F_t (r_p 12 mm): rated 0.981 N*m -> 34.7 N, 1.2x cap 1.177 N*m -> 41.7 N, stall 2.942 N*m -> 104.2 N. Allowables (lead): in-plane PETG 20 MPa repeated / 35 MPa single event, across layers 0.5x (assumption). Pinion stress = k F_t / b with the lead's FEM k (2.5 pitch/HPSTC, 4.5 tip, MPa per N/mm of face; generated outline, m 1.5, z 16, x = 0), taken proportional to 1/b (not re-run at b = 14).
| item (MPa, margin) | rated 34.7 N | cap 41.7 N | burst 70 N | stall 104.2 N |
|---|---|---|---|---|
| pinion tooth, pitch/HPSTC loading | 7.3 (2.74) | 8.8 (2.28) | 14.7 (2.38) | 21.9 (1.60) |
| pinion tooth, tip loading | 13.1 (1.52) | 15.8 (1.27) | 26.5 (1.32) | 39.4 (0.89) **FAIL** |
| rack tooth (Lewis x Kt 1.5) | 6.0 (3.32) | 7.2 (2.77) | 12.1 (2.89) | 18.1 (1.94) |
| finger bending (as built) | 4.6 (4.32) | 5.5 (3.60) | 9.3 (3.75) | 13.9 (2.52) |
| yoke leg bending (net of hole) | 5.5 (3.67) | 6.5 (3.06) | 11.0 (3.19) | 16.3 (2.14) |
| finger-head screws, hole bearing | 3.8 (5.29) | 4.5 (4.41) | 7.6 (4.60) | 11.3 (3.09) |
| tab bearing in yoke slot (across layers) | 3.3 (3.00) | 4.0 (2.50) | 6.7 (2.61) | 10.0 (1.75) |
| tab root bending (w_eff 20 mm estimate) | 9.4 (2.12) | 11.3 (1.77) | 19.0 (1.84) | 28.2 (1.24) |
| hard stop (tab on notch end) | 1.8 (10.86) | 2.2 (9.05) | 3.7 (9.44) | 5.5 (6.34) |
Max clamp with all margins >= 1: 52.9 N repeated, 92.6 N single event (pinion tip loading). Stall is covered ONLY by the torque cap: set the STS3215 torque limit <= 1.2x rated = 1.177 N*m (about 400 on a 0-1000 register; VERIFY scaling). Pad deflection at rated 2.6 mm. 624-2RS 11.2 vs 500 N; J3 inserts 45 vs 300 N.

## 4. Clamp command limits per finger (supersede the param file)
| class | mass kg | clamp needed N (mu 0.5, SF 2, 0.5 g) | command limit N | torque N*m | % rated | % stall | min mu | within 1.2x cap |
|---|---|---|---|---|---|---|---|---|
| can 355 mL | 0.382 | 11.2 | 15 | 0.424 | 43% | 14% | 0.37 | yes |
| PET <= 600 mL (500/600 mL) | 0.650 | 19.1 | 25 | 0.706 | 72% | 24% | 0.38 | yes |
| PET 1 L | 1.072 | 31.5 | 34 | 0.960 | 98% | 33% | 0.46 | yes |
| PET 1.5 L | 1.605 | 47.2 | 47 | 1.327 | 135% | 45% | 0.50 | NO |
| PET 2 L | 2.130 | 62.7 | 63 | 1.779 | 181% | 60% | 0.50 | NO |
1.5 L / 2 L need 47-63 N at mu 0.5: above rated torque and the cap, burst-only (steel M1.5 gears, smaller r_p or STS3250 for nominal use).

## 5. Verification (gripper_parts_verification.csv, _interference_matrix_gap70.csv, _sweep_checks.csv, _grasp_fit_checks.csv, _positive_controls.csv)
Parts: 18/18 watertight, consistent winding, positive volume, fit the 250 x 250 x 255 bed. Matrix: 105 pairs, max overlap 0.0 mm3. Sweep: 20/20 positions (gap 40..125 in 5 mm steps + both overtravel stops) 0 mm3, footprint x 144..149.5 mm (<= 150), y 60.3 mm (< 62).
Grasp-fit: D57, 66, 71, 89, 105, 110 at their V-flank contact gap, cylinders from zeta -10 down 320 mm: 0 mm3 against every printed part. 2D gear mesh (453 positions): overlap 0, min flank gap 0.082 mm, contact ratio 1.74.
Positive controls (gripper_positive_controls.csv, one criterion fixed in advance: overlap > 1 mm3, the same as the PASS limit): pinion +0.6 mm in x 29.0 mm3; pinion +1.0 mm in z 1.9 mm3; pinion -0.3 mm in z 13.6 mm3; rev 3 yoke strap vs D66 3747 mm3; rev 3 finger head vs D66 1317 mm3 - detected. Pinion +0.6 mm in z (0.1 mm flange/rack penetration) gives 0.37 mm3 and is NOT detected: overlap grows ~3.7 mm3 per mm of penetration there, so the check resolves ~0.27 mm; it certifies nominal CAD clearance (0.5 mm flange to rack), not printed tolerances.

## 6. Print table (PETG 0.4 nozzle, 0.2 layer, 6 walls, 22% gyroid unless stated; supports none by design)
| part | qty | material | print orientation | infill | bbox mm | est. g each | supports |
|---|---|---|---|---|---|---|---|
| gripper_base | 1 | PETG | floor down, channels up | 22% gyroid + 6 walls | 144.0x58.3x25.3 | 72.3 | none |
| gripper_carrier | 1 | PETG | cover plate face down, tower up (pocket open up) | 22% gyroid + 6 walls | 144.0x58.3x38.5 | 68.8 | none |
| gripper_top_plate | 1 | PETG | flat | 22% gyroid + 6 walls | 64.0x43.5x3.0 | 10.3 | none |
| gripper_pinion | 1 | PETG | flipped: horn face down, journal pointing up, 100% infill; flange underside bridges 2 mm over the 0.5 mm neck gap | 100% | 27.0x27.0x23.0 | 9.0 | none |
| gripper_rack | 2 | PETG | flipped: top face (tab side) down, teeth flat, 100% infill | 100% | 75.0x19.6x14.0 | 10.8 | none |
| gripper_yoke | 2 | PETG | outer face down (strap lies flat) | 100% | 40.0x73.4x8.0 | 16.9 | none |
| gripper_finger | 2 | PETG | on its long side (strap-side end face up), 6 walls | 22% gyroid + 6 walls | 13.0x135.7x44.3 | 43.4 | none |
| gripper_pad | 2 | TPU 95A | back face down, V + ribs up, 100% infill, slow | 100% | 50.0x40.0x7.9 | 10.6 | none |
| gripper_camera_plate | 1 | PETG | back face down | 22% gyroid + 6 walls | 32.0x36.0x25.4 | 11.7 | none |
| gripper_coupon_sts3215 | 1 | PETG | flat (15-minute fit test) | 22% gyroid + 6 walls | 35.0x80.0x8.0 | 17.9 | none |
| gripper_dummy_D66 | 1 | PETG | upright, open top | 100% | 66.0x66.0x120.0 | 105.0 | none |
| gripper_dummy_lid_D66 | 1 | PETG | flat | 100% | 66.0x66.0x3.0 | 12.8 | none |
| gripper_dummy_D71 | 1 | PETG | upright, open top | 100% | 71.0x71.0x120.0 | 112.3 | none |
| gripper_dummy_lid_D71 | 1 | PETG | flat | 100% | 71.0x71.0x3.0 | 14.8 | none |
| gripper_dummy_D89 | 1 | PETG | upright, open top | 100% | 89.0x89.0x120.0 | 139.4 | none |
| gripper_dummy_lid_D89 | 1 | PETG | flat | 100% | 89.0x89.0x3.0 | 23.4 | none |
| gripper_dummy_D105 | 1 | PETG | upright, open top | 100% | 105.0x105.0x120.0 | 164.9 | none |
| gripper_dummy_lid_D105 | 1 | PETG | flat | 100% | 105.0x105.0x3.0 | 32.7 | none |
Printed gripper mass (no servo, inserts, screws): 335.5 g vs 250 g target: OVER (base 72, carrier 69, fingers 2x43, yokes 2x17); no lightening done. Pinion: flange underside bridges 2 mm over the 0.5 mm neck gap - check the slit stays open.

## 7. Hardware (gripper only; dummies add 3 inserts + 3 M3x10 each)
| item | qty | where |
|---|---|---|
| STS3215 C018 with horn | 1 | tower pocket, shaft down |
| 624-2RS (4 x 13 x 5) | 1 | base seat, flush with pocket floor |
| M3 heat-set insert (hole 4.2 x 5.5) | 30 | base 4, carrier 12 (J3 4, plate 2, servo-adjust 4, camera 2), racks 2, fingers 12 |
| M3 x 18 (buy) | 4 | pinion to horn, heads in the flange counterbores (was M3 x 12) |
| M3 x 12 (buy) | 4 | J3 hub to tower inserts |
| M3 x 10 | 22 | carrier to base 4, top plate 2, servo-adjust 4, yoke to finger 8, pad to finger 4 |
| M3 x 8 (buy) | 2 | yoke to rack-tab insert |
| M3 x 16 (buy) | 2 | camera wedge |
| M2 x 12 + M2 nut (buy) | 4 + 4 | camera board |

## 8. Assembly order
1. Press all inserts. 2. Print the coupon; check pocket, shaft window, 624 seat, inserts. 3. Press the 624 into the base seat. 4. Horn on the STS3215 shaft; servo into the carrier from the top (horn through the D25 cover window), adjust screws loose. 5. Turn the carrier over; bolt the pinion to the horn from below (4 x M3 x 18). 6. Racks into the base channels, tabs through the notches, closed to the stops (gap 40).
7. Lower the carrier onto the base (journal into the 624, flange into the pocket, gear between the racks), 4 x M3 x 10; tune the adjust screws until the pinion turns freely. 8. Yokes on the tabs (M3 x 8), yokes on the finger heads (4 x M3 x 10), TPU pads (2 x M3 x 10). 9. Top plate, camera wedge, J3 hub (4 x M3 x 12). 10. Set the torque cap.

## 9. VERIFY BEFORE PRINTING / weak points
Caliper (named parameters at the top of gripper_build.py): SV_SHAFT_FROM_END 12.5, servo 45.2 x 24.7 x 35, HORN_STANDOFF 3.5, HORN_OD 22, horn thread depth >= 3.2 mm (M3 x 18 engagement), 624 fit, journal 3.95, insert hole 4.2 x 5.5, M3 head vs counterbore 5.8, TPU pad fit, tick sign, Torque_Limit scaling, PETG vs 20/35 MPa.
Weak points: (a) stall with tip loading 0.89 unless the torque cap is set; (b) neck web 1.23 mm and the 0.5 mm slit between flange and gear (if it fuses the flange top rubs the rack bottoms: file it); (c) tab-root bending uses an estimated 20 mm spread width (stall margin 1.24); (d) mass over target; (e) no FEA and no test print; the lead's FEM was not re-run at b = 14.

## 10. Deviations from the brief / lead request
Face 14 (brief 10); J3 plane 63.0 (brief 55); flange on the floor side with a 0.5 mm neck (expected horn side, 62.5); body-bottom datum -3.8; finger 44.3 wide with a 3 mm inner head; yoke/tab finger mount with an L-shaped strap; servo pocket +-2 mm with adjust screws; J3 inserts in tower walls; housing 144 mm; 202.9 deg (not 191); r_p 9 mm and lightening not touched (gripper_rp9_variant_estimate.csv is regenerated by the script only).
