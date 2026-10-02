# ElRobot2-S1 fixture, test coupon and assembly drawings (track `fixture_`)

Revision v1.1 of 2026-10-01 (late), after CHANGE REQUEST v1.1 (boolean check of the FINAL arm + gripper rev 4 against this track's CAD): lane entrance plane y = 270, **level pitch 440 (floors z = 100 / 540)**, tote tray pocket 8 mm deep with a 3 mm rim (**rim top 11 mm above the pocket floor**), tote ledge top at floor - 6 mm,
reach annulus r 170 ... 385 mm. All envelope sweeps were re-run with the final arm envelope (L1 link zeta 145 ... 201, J2 housing 115 ... 145, L2 85.3 ... 130 incl. cover, J3 housing top 127.3; gripper rev 4: body bottom zeta -3.8, footprint 149.5 x 60.3, fingertip -140, grasp rule z_g = z_floor + max(H + 10, 143)). Everything below is generated from `fixture_build.py` and the result CSVs; nothing in the tables is typed by hand.

## 1. Frame of reference and key coordinates (mm)

World frame: J1 axis at the origin, x lateral, y toward the shelf, z up, baseboard top face z = 0. The lane floors slope **down toward +y by 6 deg**, obtained by tilting the 12 mm shelf plates on printed wedge shims (the lane modules are flat).

| item | value |
|---|---|
| lane entrance plane | y = 270 |
| lane floor reference at the entrance | z = 100 (level 0), 540 (level 1); **level pitch 440** |
| lane centres (default 5-lane set) | x = -180, -90, 0, 90, 180; pitch 90, free width 87 (3 mm dividers, 35 mm high) |
| lane target (bottle centre) | y = entrance + D/2 + 1.5: **307 for D71**, 326.5 for D110 |
| WIDE lane set (level 1 only) | x = -125, 0, 125; pitch 125, **free width 120.5** (4.5 mm dividers, 45 mm high, 10 x 60 mm stop) |
| small tote (6 pockets D75 x 8 deep) | tray 185 x 305 x 17 (base 14, rim 3); tray surface 8 mm and **rim top 11 mm above the pocket floor** (= lane floor reference z); pocket centres x = -300, -215, **y = 0, 100, 200** (pitch 85 along x, 100 along y); closing axis along y |
| D120 tote insert (2 pockets) | same outer size and heights, pockets D120 x 8 deep; centres (**-257.5, 30**) and (**-257.5, 170**), pitch 140 |
| tote ledge plates | x -370 .. -165, y -62.5 .. 252.5, **6 mm ply**; top at z = floor - 6 (94 / 534), underside z = 88 / 528; the pocket floor is the lane floor reference; three stops per ledge: two on the front edge, one on the LEFT (-x) edge |
| posts 30 x 30 | x = +-278.0; front y = 299.8 (s = 30 along the slope), rear y = 548.5 (s = 280); **lengths 599.6 / 573.4**; bracket seat tops at 6 deg (front / rear): level 0 69.6 / 43.4, level 1 509.6 / 483.4 |
| shelf plates | 500 x 440 x 12, front edge 15 mm before the entrance, tilted 6 deg (4 .. 10 deg with shims 04 .. 10) |
| tote stand | 18 mm plywood panel 280 wide x 576 tall (inner face x = -385), base plate x -620 .. -370, 2 knee gussets 200 x 200; tote brackets at y = 0 and 200 |

## 2. What changed in this revision (CHANGE REQUEST v1.1)

| item | before (rev. 2026-10-01 pm) | now (v1.1) | reason / consequence |
|---|---|---|---|
| level pitch | 420 (floors z 100 / 520) | **440 (floors z 100 / 540)** | lead request: room under the level-1 plate for the final arm (L2 cover top at zeta 130). Posts 579.6 / 553.4 -> **599.6 / 573.4**; level-1 bracket seat tops 489.6 / 463.4 -> **509.6 / 483.4** (level 0 unchanged: 69.6 / 43.4); tote-stand panel 556 -> **576** tall; level-1 ledge underside 510 -> **528** |
| tray pocket depth (D75 and D120) | 10 mm | **8 mm** | the pocket floor stays at z = floor (100 / 540): tray base 14 - pocket 8 = 6 mm, so the ledge top is at floor - 6 (was floor - 4) and the ledge underside at floor - 12 |
| tray rim | 12 mm above the tray surface (rim top 22 mm above the pocket floor) | **3 mm (rim top 11 mm above the pocket floor)** | lift to clear the rim is 11 mm + margin instead of 22 mm; tray height (bbox z) 26 -> 17 mm, parts `fixture_tote_tray_*` and `fixture_tote_tray_d120_*` re-exported |
| tote ledge right edge, side stop | ledge x -370 .. -152; side stop on the right edge of the tray | **ledge x -370 .. -165 (flush with the tray); side stop on the LEFT edge (x -358 .. -350)** | NOT REQUESTED, see the note below the table |
| envelope model in the sweeps | box model: L1 zeta 145 .. 163 (nominal) or 135 .. 200 (conservative), L2 10 .. 118, J3 block 55 .. 118, body 150 x 62 x 55 | **final CAD envelope** (section 7) | the nominal / conservative pair is obsolete: the final L1 link is 145 .. 201 |
| grasp height | z_g = z_floor + max(H + 5, 143) | **z_g = z_floor + max(H + 10, 143)** | gripper rev 4 datum (body bottom at zeta -3.8, 6.2 mm above the bottle top); the can lift rule is unchanged (+5 mm lane / +10 mm tote; minimum +4 / +8 mm with the 3 mm margin) |
| level-0 tote cell (-300, 0) | usable, hover-limited for 500 / 600 mL | **UNUSABLE for 500 and 600 mL at level 0** (usable for the three can SKUs; level 1 keeps all six cells) | the L1 link top (zeta 201) reaches the level-1 tray / ledge / stop there (section 8); the pocket is kept |
| closed-form headroom, drawings | stack top zeta 118, footprint 150 x 62 | **L2 cover top zeta 130, footprint 149.5 x 60.3** | final CAD |

Note on the ledge and the side stop (a deviation from the request): with the final arm the corner of the level-0 L1 link (zeta 201) passes within 2 - 3 mm (plan view) of the old ledge edge x = -152 when the arm serves tote cells (-300, 100) and (-215, 0); both failed the check with the 3 mm margin (the first also collided nominally for 600 mL). The ledge only needed to extend past the tray edge to carry the right-hand stop, so the stop moved to the tray's left edge and the ledge now ends flush with the tray (x = -165): 11 mm of plan clearance, both cells pass with a lift >= 40 mm. `fixture_tote_stop` itself is unchanged.

Earlier changes of the same day (lead corrections 2026-10-01 pm), still valid:

| item | before | now | reason |
|---|---|---|---|
| small tote pocket pitch along y | 85 mm (y 20 / 105 / 190) | **100 mm (y 0 / 100 / 200)** | the gripper finger stack reaches 11.5 mm beyond the pad V-apex plane: with D71 gripped (apex gap 73.5) the outer face is 48.25 mm from the bottle axis, the neighbour surface was 49.5 mm away (1.25 mm); now 16.25 mm |
| D120 insert pitch | 130 mm (y 40 / 170) | **140 mm (y 30 / 170)** | same rule: 16.56 mm for D110 |
| tray outer size | 185 x 275 | **185 x 305**, split at tray-local y = -50 (front piece 102.5 mm, rear piece 202.5 + 15 mm tongues) | pockets 100 mm apart; both pieces fit the 250 mm bed |
| tote brackets / stand / gussets | y 5 / 205, stand y -35 .. 245 | y 0 / 200, stand y -40 .. 240 | centred on the new tote |
| front posts | s = 0 (y 270) | **s = 30 (y 299.8)** | the 305 mm tray reaches y = 252.5 (it hit the bracket by 2.5 mm) and the 149.5 mm gripper body at tote cell (-300, 200) reaches y = 274.75: the post front face is at y 284.8, 10.1 mm behind the body |
| shelf plate | 500 x 450, front margin 25 | **500 x 440, front margin 15** (rear end unchanged) | at 10 deg the plate front edge touched the tray rear edge; now the edge is at least 2.6 mm behind it at every slope step |
| STD exit stop | 3 x 20 mm | **5 x 20 mm** | hand-calc S7: the 3 mm wall had 0.76x at mu 0.07 |
| WIDE free width | 122 (design file) | **120.5** | 125 pitch minus the 4.5 mm divider; 5.25 mm per side for D110 |

## 3. Files

All files are in `out_fixture/`. Parametric source: `fixture_build.py` (parts, parameters at the top), `fixture_assembly.py` (assembly poses, interference, sweeps), `fixture_cutlist.py`, `fixture_closedform.py`, `fixture_handcalcs.py`,
`fixture_sweep_envelope.py`, `fixture_slope_envelope.py`, `fixture_drawings.py`, `fixture_previews.py`, `fixture_docs.py` (this README and the hardware list). Re-run order: build, assembly (`FIXTURE_SLOPE=1`), sweep_envelope (one run: descent sweep and lift-limit scan), slope_envelope (lift limits for 4 ... 10 deg, optional, 4 min), cutlist, closedform, handcalcs, drawings, previews, docs.

* Printed parts: `fixture_<part>.stl` / `.step` for the 27 parts of section 4.
* Verification: `fixture_verification.csv`, `fixture_interference_pairs_{STD,WIDE}.csv`, `fixture_interference_matrix_{STD,WIDE}.csv`, `fixture_interference_summary.csv`, `fixture_sweep_dovetail_{STD,WIDE}.csv`, `fixture_sweep_slope.csv`, `fixture_sweep_envelope_summary.csv` (+ pose level: `fixture_sweep_envelope_nominal_rules.csv` all cases with the can lift rule, `fixture_sweep_envelope_nominal.csv` can_355_std without the rule, `fixture_interface_scan.csv` 1 mm lift scan of every IK branch; **`fixture_interface_limits.csv`** best branch per target and SKU with verdict; `fixture_interface_limits_vs_slope.csv` the same scan at slopes 4 ... 10 deg; `fixture_sweep_envelope_conservative_rules.csv` is obsolete and holds only a note), `fixture_handcalcs_frame.csv`, `fixture_wide_stability_tstand.csv`, `fixture_assembly_transforms.csv`.
* Closed-form tables: `fixture_reach_targets.csv`, `fixture_reach_targets_wide.csv`, `fixture_headroom_sku.csv`, `fixture_headroom_vs_slope.csv`, `fixture_sku_config.csv`, `fixture_lane_flow.csv`.
* Drawings: `fixture_drawing_topview.png`, `fixture_drawing_sideview.png`, `fixture_drawing_frame.png`; cut list and holes: `fixture_cutlist.csv`, `fixture_plate_holes.csv`, `fixture_post_drill_table.csv`; hardware: `fixture_hardware_list.csv`.
* Previews: `fixture_preview_lane_module.png`, `fixture_preview_tote_tray.png`, `fixture_preview_frame.png`, `fixture_preview_coupon.png`, `fixture_preview_exploded.png`.

## 4. Printed parts (gate 1)

PETG, 0.4 mm nozzle, 0.2 mm layers, 6 walls. Infill 22 % gyroid for lane segments, trays and the coupon (25 %); **100 % for brackets, shims, feet and stops** (load-bearing). No part needs supports (largest unsupported narrow span 5 mm <= 12 mm bridge limit). Every part fits the 250 x 250 x 255 mm bed.
Mass = 6 x 0.42 mm walls + 22 % (or stated) infill on the remaining volume, 1.27 g/cm3. Default build set (without WIDE, D120, wheel options and spare shims, coupon): 6.69 kg; all listed quantities incl. options: 9.21 kg.

| part | qty | set | bbox (mm) | volume (mm3) | mass each (g) | infill | print orientation | supports |
|---|---|---|---|---|---|---|---|---|
| lane_entry | 8 | default | 90 x 214 x 43.2 | 160440.4 | 186.6 | 0.22 | underside on bed, walls up, tongue at +y end | no |
| lane_entry_R | 2 | default | 93 x 214 x 43.2 | 186153.2 | 233.2 | 0.22 | underside on bed | no |
| lane_exit | 8 | default | 90 x 200 x 43.2 | 166081.3 | 195.0 | 0.22 | underside on bed, stop wall up | no |
| lane_exit_R | 2 | default | 93 x 200 x 43.2 | 192001.3 | 241.5 | 0.22 | underside on bed | no |
| tote_tray_front | 2 | default | 185 x 102.5 x 17 | 189046.6 | 182.2 | 0.22 | bottom on bed, rim up | no |
| tote_tray_rear | 2 | default | 185 x 217.5 x 17 | 394229.6 | 355.6 | 0.22 | bottom on bed, rim up | no |
| bracket_main_L | 4 | default | 40 x 108 x 53 | 65541.0 | 83.2 | 1.0 | leg back face (against the post) on the bed, seat arm stands up; slots vertical | no |
| bracket_main_R | 4 | default | 40 x 108 x 53 | 65541.0 | 83.2 | 1.0 | as _L (mirror image) | no |
| bracket_tote | 4 | default | 60 x 76 x 70 | 113103.8 | 143.6 | 1.0 | leg back face on the bed, seat arm stands up | no |
| lane_entry_wide | 2 | WIDE option | 125 x 214 x 53.2 | 229016.7 | 252.2 | 0.22 | underside on bed | no |
| lane_entry_wide_R | 1 | WIDE option | 129.5 x 214 x 53.2 | 276405.7 | 316.7 | 0.22 | underside on bed | no |
| lane_exit_wide | 2 | WIDE option | 125 x 200 x 68.2 | 299855.3 | 307.7 | 0.22 | underside on bed | no |
| lane_exit_wide_R | 1 | WIDE option | 129.5 x 200 x 68.2 | 348410.3 | 371.1 | 0.22 | underside on bed | no |
| tote_tray_d120_front | 1 | D120 option | 185 x 152.5 x 17 | 300602.1 | 262.1 | 0.22 | bottom on bed, rim up | no |
| tote_tray_d120_rear | 1 | D120 option | 185 x 167.5 x 17 | 315240.2 | 271.4 | 0.22 | bottom on bed, rim up | no |
| shim_04deg | 0 | shim spare | 41 x 40 x 8.4 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| shim_05deg | 0 | shim spare | 41 x 40 x 8.75 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| shim_06deg | 8 | default | 41 x 40 x 9.1 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| shim_07deg | 0 | shim spare | 41 x 40 x 9.46 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| shim_08deg | 0 | shim spare | 41 x 40 x 9.81 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| shim_09deg | 0 | shim spare | 41 x 40 x 10.17 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| shim_10deg | 0 | shim spare | 41 x 40 x 10.53 | 11257.4 | 14.3 | 1.0 | flat bottom on the bed (wedge up) | no |
| lane_entry_wheel | 0 | wheel option | 90 x 214 x 43.2 | 115800.5 | 147.1 | 0.22 | underside on bed | no |
| lane_exit_wheel | 0 | wheel option | 90 x 200 x 43.2 | 122961.1 | 156.2 | 0.22 | underside on bed | no |
| post_foot | 4 | default | 90 x 90 x 20 | 47337.8 | 60.1 | 1.0 | flange on bed | no |
| tote_stop | 6 | default | 25 x 8 x 12 | 2291.1 | 2.9 | 1.0 | flat on bed | no |
| coupon_bearing_seats | 1 | coupon | 180 x 130 x 10 | 176791.1 | 176.0 | 0.25 | flat on bed | no |

Watertight (trimesh): 27/27; positive volume: 27/27; fits the bed: 27/27; parts needing supports: 0.

## 5. Hardware per part (exact sizes and quantities)

Per part: lane segments (20 default): 2 x heat-set insert M3 x 5 x 4 + 2 x M3 x 20 SHCS + washer. Main brackets (8): 2 x M6 x 50 (through the 30 mm post) + 2 wood screws 4 x 30 (seat and shim to the plate). Wedge shims (8): fixed by the same 4 x 30 screw. Tote brackets (4): 2 x M6 x 35 into the 18 mm panel + 4 x wood screw 4 x 20 into the 6 mm ledge.
Post feet (4): 4 x 5 x 25 to the baseboard + 3 x 5 x 20 through the socket walls. Tote stops (6): 1 x 3.5 x 16. Trays: CA/epoxy on the dovetail seam. Full list (also `fixture_hardware_list.csv`):

| item | qty | used for | status |
|---|---|---|---|
| Ruthex M3 x 5 x 4 brass heat-set insert | 40 | STD lane segments: 2 per segment x 20 segments (10 entry + 10 exit) | on hand/ordered (uses 40 of the stock) |
| M3 x 20 socket head cap screw | 40 | lane segment to shelf plate, from below through the 12 mm plate into the insert | BUY |
| M3 washer | 40 | under the plywood, one per M3 screw | BUY |
| Ruthex M3 x 5 x 4 brass heat-set insert (WIDE set) | 12 | WIDE lanes: 3 lanes x 2 segments x 2 inserts (level 1 swap, OPTIONAL set) | on hand/ordered |
| M3 x 20 socket head cap screw + M3 washer (WIDE set) | 12 | WIDE segments to the level-1 plate (OPTIONAL set) | BUY |
| M6 x 50 socket head cap screw | 16 | main bracket leg to post: 8 main brackets x 2 bolts (through the 12 mm leg + 30 mm wood post) | BUY (ASSUMPTION: M6 hardware, brief allowed M5/M6) |
| M6 hex nut + 2 x M6 washer (18 mm OD) | 16 nuts + 32 washers | main bracket bolts (nut and washer on the far face of the post) | BUY |
| alt: M6 x 16 socket head cap screw + M6 T-nut | 16 | same 16 main-bracket joints if 3030 aluminium posts are used instead of wood | BUY (alternative, replaces the 16 x M6 x 50 + nuts) |
| M6 x 35 socket head cap screw | 8 | tote bracket leg to the 18 mm plywood stand panel: 4 tote brackets x 2 bolts | BUY |
| M6 hex nut + 2 x M6 washer (18 mm OD) | 8 nuts + 16 washers | tote bracket bolts (nut and washer on the outer face of the panel) | BUY |
| 4 x 30 countersunk wood screw | 16 | main bracket seat + wedge shim to the 12 mm shelf plate, from below: 2 per bracket x 8 | BUY |
| 4 x 20 countersunk wood screw | 16 | tote bracket seat (16 mm) to the 6 mm ledge plate: 4 per bracket x 4 | BUY |
| 3.5 x 16 wood screw | 6 | tote stops to the ledge plates (3 per ledge) | BUY |
| 5 x 25 wood / chipboard screw | 16 | post foot flange to baseboard: 4 per foot x 4 feet | BUY |
| 5 x 20 wood screw | 12 | post retaining screws through the foot socket walls: 3 per foot x 4 | BUY |
| 4 x 40 wood screw + wood glue | 24 | tote stand knee gussets: 6 screws per edge x 2 edges x 2 gussets | BUY (stand joints are a PROPOSAL, not in the brief) |
| 4 x 50 wood screw + wood glue | 6 | tote stand panel to base plate | BUY (PROPOSAL) |
| 5 x 30 wood screw | 4 | tote stand base plate to the baseboard (far corners) | BUY (PROPOSAL) |
| PTFE or UHMW-PE adhesive tape, 19 mm (3/4 in) wide, about 0.2 mm thick | 15 m | lane floors: 3 strips x 0.4 m x 10 lanes = 12.0 m net | BUY |
| PTFE or UHMW-PE adhesive tape, 19 mm wide (WIDE set) | 4.5 m | WIDE lanes: 3 strips x 0.4 m x 3 lanes = 3.6 m net (OPTIONAL set) | BUY |
| Skate-wheel strips, 17 mm wide | 30 | OPTIONAL alternative lane floor (parts fixture_lane_entry_wheel / fixture_lane_exit_wheel, 3 channels per segment) | OPTIONAL |
| CA or 2-part epoxy | 1 | tote tray dovetail seams (2 STD trays + 1 D120 tray) | BUY |
| Birch plywood (or acrylic) 12 mm, 500 x 440 | 2 | lane shelf plates, level 0 and level 1 | cut |
| Birch plywood 6 mm, 205 x 315 | 2 | tote ledge plates (extension beyond the brief) | cut |
| Birch plywood 18 mm: panel 576 x 280, base plate 250 x 280, 2 gussets 200 x 200 | 1 + 1 + 2 | tote stand (replaces the two tote posts) | cut |
| Post 30 x 30 mm wood batten or 3030 aluminium, 599.6 mm | 2 | frame | cut |
| Post 30 x 30 mm wood batten or 3030 aluminium, 573.4 mm | 2 | frame | cut |
| Baseboard >= 12 mm plywood, at least 960 x 620 | 1 | common test bed; its top face is world z = 0 and must equal the robot base z = 0 | user supplied |
| Ruthex M3 inserts, M3 screws, 6806-2RS / 6701-2RS / 624-2RS bearings (coupon test fit only) | 4 + 4 + 1 each | fixture_coupon_bearing_seats.stl calibration: seats, insert holes, M3 clearance and counterbore holes, slide-fit slots | on hand / on order (6806 must be bought) |

Cut list (`fixture_cutlist.csv`; hole positions in `fixture_plate_holes.csv`):

| item | qty | material | length_mm | width_mm | thickness_mm |
|---|---|---|---|---|---|
| lane shelf plate, level 0 | 1 | 12 mm birch plywood (or acrylic) | 500.0 | 440.0 | 12.0 |
| lane shelf plate, level 1 | 1 | 12 mm birch plywood (or acrylic) | 500.0 | 440.0 | 12.0 |
| tote ledge plate, level 0 and level 1 | 2 | 6 mm birch plywood | 205.0 | 315.0 | 6.0 |
| post 30x30, front (x=+-278, y=299.8) | 2 | wood batten or 3030 aluminium | 599.6 | 30.0 | 30.0 |
| post 30x30, rear (x=+-278, y=548.5) | 2 | wood batten or 3030 aluminium | 573.4 | 30.0 | 30.0 |
| tote stand side panel | 1 | 18 mm birch plywood | 576.0 | 280.0 | 18.0 |
| tote stand base plate | 1 | 18 mm birch plywood | 250.0 | 280.0 | 18.0 |
| tote stand knee gusset (right triangle 200 x 200) | 2 | 18 mm birch plywood | 200.0 | 200.0 | 18.0 |
| baseboard (user supplied), minimum | 1 | >= 12 mm plywood | 960.0 | 620.0 | 12.0 |

Post and panel drilling heights (default 6 deg; the bracket legs have an 80 mm vertical slot, +-40 mm of height adjustment, so 4 .. 10 deg need no re-drilling):

| post | level | y_mm | bracket_seat_top_z_mm | bolt_lower_z_mm | bolt_upper_z_mm |
|---|---|---|---|---|---|
| front (left and right) | 0 | 299.8 | 69.6 | 98.1 | 123.1 |
| front (left and right) | 1 | 299.8 | 509.6 | 538.1 | 563.1 |
| rear (left and right) | 0 | 548.5 | 43.4 | 71.9 | 96.9 |
| rear (left and right) | 1 | 548.5 | 483.4 | 511.9 | 536.9 |
| tote stand panel (y=0 and y=200) | 0 | 0; 200 | 88.0 | 106.0 | 136.0 |
| tote stand panel (y=0 and y=200) | 1 | 0; 200 | 528.0 | 546.0 | 576.0 |

## 6. Assembly order

1. Print `fixture_coupon_bearing_seats` first (about 15 min) and measure it: insert holes, M3 clearance/counterbore, the four slide-fit slots (0.15 / 0.20 / 0.25 / 0.30 mm), bearing seats. Edit the named parameters at the top of `fixture_build.py` if a fit is off, then rebuild.
2. Print the lane segments, trays, brackets, shims, feet and stops; cut the plywood and the four posts per the cut list; drill the plate and ledge holes (`fixture_plate_holes.csv`) and the post and panel holes (`fixture_post_drill_table.csv`).
3. Press the 40 heat-set inserts into the underside holes of the lane segments (soldering iron 220-230 C for PETG, until flush), apply the PTFE/UHMW tape strips (3 per segment) on the ribs.
4. Glue the tray halves along the dovetail seam (CA or epoxy), keep them flat on a table until set.
5. Fix the baseboard so that its top face is the robot's z = 0. Screw the four post feet to it (positions from section 1), insert the posts and lock them with the 3 retaining screws per foot.
6. Build the tote stand: base plate on the baseboard (4 x 5 x 30), panel on the base plate (glue + 6 x 4 x 50 from below), the two knee gussets (glue + 24 x 4 x 40). Bolt the four tote brackets to the panel at the heights of the drill table.
7. Bolt the eight main brackets to the posts at the seat heights of the drill table (M6 x 50 through the 30 mm post; slots allow +-40 mm), then lay the wedge shims (default `fixture_shim_06deg`) on the seats.
8. Lay the two shelf plates on the shims and screw them from below (4 x 30, one per bracket); check the slope with an inclinometer: 6 deg, down toward +y.
9. Fit the lane modules: exit segment slides onto the entry segment's dovetail tongue from above, then 2 x M3 x 20 per segment from below through the plate.
10. Screw the tote ledge plates onto the tote brackets (4 x 20), screw on the three tote stops per ledge (two on the front edge, one on the left (-x) edge of the tray position), set the tray on the ledge against the stops.
11. For the WIDE set (level 1): remove the five STD modules of level 1, fit the three wide lanes (12 inserts, 12 x M3 x 20), swap the tray for the D120 insert.
12. Run the VERIFY list (section 9) before the first robot run.

## 7. Verification results

### Gate 2 and 3: interference matrix, sweeps

* Interference matrix (real boolean intersections, manifold3d): STD 66 instances, 149 pairs with overlapping bounding boxes, 129 mating, **0 FAIL**, largest overlap 0.0967 mm3; WIDE 62 instances, 128 pairs, **0 FAIL**, largest 0.0498 mm3. Non-mating pairs overlap 0.0 mm3. Mating pairs (module on plate, dovetail, shim on seat, bracket on post, tray on ledge, stop on ledge, post in foot) touch with overlaps below 0.3 mm3 (numerical contact); screws, inserts and bolts are not modelled, so there are no designed overlaps.
* Dovetail assembly sweep (exit segment onto entry tongue, tray front onto rear piece; 9 lift positions 20 ... 0 mm; STD and WIDE; both levels): 72 checks, 0 FAIL, largest 0.0018 mm3.
* Slope adjustment sweep (posts fixed, brackets slide in the slot, matching shim, whole assembly re-posed and re-intersected, 7 positions 4 ... 10 deg):

| slope_deg | shim | n_pairs_bbox_overlap | n_fail | max_overlap_mm3 | min_bolt_slot_margin_mm | lowest_seat_bottom_z_mm | verdict |
|---|---|---|---|---|---|---|---|
| 4 | fixture_shim_04deg | 145 | 0 | 0.331 | 7.34 | 39.28 | PASS |
| 5 | fixture_shim_05deg | 149 | 0 | 0.3423 | 12.26 | 34.36 | PASS |
| 6 | fixture_shim_06deg | 149 | 0 | 0.0967 | 17.2 | 29.42 | PASS |
| 7 | fixture_shim_07deg | 149 | 0 | 0.0235 | 17.77 | 24.46 | PASS |
| 8 | fixture_shim_08deg | 153 | 0 | 0.1023 | 18.34 | 19.47 | PASS |
| 9 | fixture_shim_09deg | 159 | 0 | 0.4574 | 16.22 | 14.44 | PASS |
| 10 | fixture_shim_10deg | 159 | 0 | 0.0921 | 11.17 | 9.39 | PASS |

* Robot-envelope sweep (`fixture_sweep_envelope_nominal_rules.csv`, summary `fixture_sweep_envelope_summary.csv`): FINAL arm + gripper rev 4 envelope (`ENV` in `fixture_assembly.py`: bounding boxes of the CAD solids of `arm_assembly.step` / `gripper_assembly_gap70.step`, footprint 149.5 x 60.3 from the lead's boolean check): gripper body (zeta -3.8 .. 63), the two finger guide rails below it (to zeta -46), both fingers (13 x 44.4, zeta -140 .. -4.3, outer face at D/2 + 12.75 from the bottle axis), J3 flange + housing (zeta 62.5 .. 85.3), L2 link 52 wide (zeta 85.3 .. 127.3) with its cover (to 130), J2 housing r 30 (zeta 111 .. 145) and hub, L1 link 60 wide (zeta 145 .. 201), intersected with every fixture part of both levels and with same-SKU neighbour bottles. 127 target x SKU cases (level 0: 5 SKUs x 11 targets; level 1: 5 SKUs x 11 + pet_1000 on the 5 STD lanes; WIDE level 1: pet_1500 and pet_2000 on 3 lanes + 2 D120 pockets, pet_1000 on the 2 D120 pockets), every IK branch inside the joint limits, closing axis along y, 8 heights from 30 mm above the grasp pose down to 0, each at nominal size and with +3 mm on all envelope boxes: 1504 poses.
  **124 of 127 cases are collision-free over the whole descent with the 3 mm margin; 2 reach the grasp pose but the hover is limited; 1 fail at the grasp pose.** This holds with the can lift rule of section 8. Without it `can_355_std` collides at 22 of its 22 targets (near finger on the sloping floor in the lanes, fingertip on the pocket web in the tote).
  Cases that are not full hover: pet_500_water at tote_x0y0 (level 0): hover <= 0 mm with margin, <= 4.29 mm nominal; pet_600_soda at lane1 (level 0): hover <= 21.43 mm with margin, <= 21.43 mm nominal; pet_600_soda at tote_x0y0 (level 0): fails at the grasp pose. The vertical lift that is free at every target (1 mm scan, `fixture_interface_limits.csv`) is tabulated in section 8; the limits come from the L1 link (zeta 201) under the level-1 tray / ledge and from the L2 link under the level-1 brackets.

### Gate 4: hand calculations

Loads: 25 kg per shelf level (STD 5 lanes x 5 x 600 mL = 20.3 kg, WIDE 3 lanes x 3 x 2 L = 22.7 kg) with a dynamic factor of 1.5; tote 5.5 kg. Printed PETG design allowables as in the brief (E = 1.5 GPa, 10 MPa in-plane, 5 MPa across layers, includes safety factor ~3); Kt = 1.5 at the seat/leg fillets; birch ply 10 MPa bending, E = 8 GPa; pine post 10 MPa, E = 9 GPa. Smallest stress margin of the table (row B2): **1.55x**, failures: 0 (row L1 is the load definition, not a margin). The lane, stop, divider, stability and tote-stand checks for 2 L bottles are in `fixture_wide_stability_tstand.csv` (static tip angle 20.9 deg, divider 4.5 x 45 mm 1.4x, stop 10 x 60 mm 1.73x, tote-stand plywood 7.3x, hold-down 15x); that file also lists the rejected variants for comparison (3 mm divider 0.8x and 3 mm stop 0.31x for a 2 L bottle), which is why the WIDE set has 4.5 x 45 dividers and a 10 x 60 stop. Lane flow (`fixture_lane_flow.csv`): at 6 deg a bottle starts and covers 0.3 m in under 5 s for mu <= 0.10 (3.5 s at mu 0.10).

| id | check | value | allowable | margin (x) | verdict |
|---|---|---|---|---|---|
| L1 | design load per shelf level | 20.3 kg STD / 22.7 kg WIDE | 25 kg used (rounded up) | 1.1 | PASS |
| L2 | support reactions of one level (load centroid at s = 200 mm along the slope) | W 368 N; rear pair 250 N, front pair 118 N | per rear bracket = half of the pair |  | PASS |
| P1 | shelf plate bending at the rear supports (12 mm birch ply, 500 mm wide) | 0.73 MPa (M 8.79 N*m, a 145 mm) | 10 MPa | 13.65 | PASS |
| P2 | shelf plate overhang deflection | 0.080 mm | < 0.5 mm (bottle entry tolerance) | 6.23 | PASS |
| B1 | main bracket seat root bending (across layers: arm is printed standing up) | 3.02 MPa (R 125 N) | 5 MPa across layers | 1.66 | PASS |
| B2 | main bracket leg bending at the seat junction (leg printed flat: in-plane) | 6.45 MPa | 10 MPa in-plane | 1.55 | PASS |
| B3 | M6 bolts: tension from the seat moment (leg rotates about its bottom edge), upper bolt | 60 N (shear 63 N per bolt) | M6 8.8 tension 10 kN; pull-through of 18 mm washer in pine ~ 0.9 kN | 14.98 | PASS |
| B4 | bolt bearing on the 12 mm PETG leg (slot 6.6 mm) | 0.79 MPa | 10 MPa | 12.66 | PASS |
| B5 | wedge shim contact pressure | 0.076 MPa | 2 MPa (PETG creep limit) | 26.22 | PASS |
| T1 | tote load per ledge | 5.2 kg -> 76 N with x1.5 |  |  | PASS |
| T2 | tote bracket seat root bending (across layers) | 2.85 MPa (M 4869 N*mm) | 5 MPa across layers | 1.75 | PASS |
| T3 | tote bracket M6 bolt tension (upper bolt) / pull-through in 18 mm birch ply | 89 N | 1.5 kN (18 mm washer, 15 MPa bearing) | 16.87 | PASS |
| T4 | ledge plate bending beyond the seat edge (6 mm birch ply) | 2.32 MPa | 10 MPa | 4.3 | PASS |
| F1 | post (30x30 wood) axial + eccentric bracket moment | 1.47 MPa | 10 MPa | 6.8 | PASS |
| F2 | frame sway (posts as fixed-fixed columns between the shelves), 20 N at level 1 | 342 N/mm -> 0.06 mm | < 1 mm | 17.12 | PASS |
| S10 | STD exit stop 5 x 20 mm, 600 mL bottle at the lane end, mu 0.1 | F 11 N, 0.9 MPa | 5 MPa across layers | 5.57 | PASS |
| S7 | STD exit stop 5 x 20 mm, 600 mL bottle at the lane end, mu 0.07 | F 30 N, 2.4 MPa | 5 MPa across layers | 2.1 | PASS |
| S3 | STD divider 3 x 35 mm, lateral 0.1 m/s 600 mL bottle | demand 6.6 N | 17 N | 2.58 | PASS |
| S4 | lane segment E/X retention against the stop impact (WIDE 97 N) | 97 N | 3.6 kN | 37.11 | PASS |

### Reach, clearances and headroom (task item 5)

All 16 targets lie inside the annulus r 170 ... 385 mm: the tightest is tote cell (-300, 200) at r = 360.6 mm (24.4 mm inside r_max), then lanes 1 and 5 at r = 355.9 (29.1 mm).
Gripper footprint (149.5 x 60.3, closing axis along y) at the lane targets: 13.35 mm to each divider, 24.4 mm to the neighbouring D71 bottle (40 mm pads: 23.5 / 34.5). WIDE lanes: 30.1 / 39.9 mm.
Tote (closing axis along y): outer finger face to the neighbouring D71 bottle 16.25 mm along y (pitch 100), lateral clearance of the finger stack 27.3 mm along x (pitch 85); D120 insert, D110 bottle: 16.56 mm. At the two end rows the rim inner face is 49.5 mm from the pocket axis, 1.25 mm outside a D71 finger stack: harmless because the fingertips of PET bottles are far above the 3 mm rim (its top is 11 mm above the pocket floor) and, for cans, the lifted grasp height of section 8 puts the fingertips 13 mm above the pocket floor, 2 mm above the rim top (3.75 mm clearance to the rim for D66 cans).

| target | x_mm | y_mm | r_mm | margin_to_rmax385_mm | margin_to_rmin170_mm | ik_feasible_solutions | q1_q2_deg |
|---|---|---|---|---|---|---|---|
| lane1 | -180.0 | 307.0 | 355.9 | 29.1 | 185.9 | 2 | 93.2/+54.3; 147.5/-54.3 |
| lane2 | -90.0 | 307.0 | 319.9 | 65.1 | 149.9 | 2 | 69.5/+73.8; 143.2/-73.8 |
| lane3 | 0.0 | 307.0 | 307.0 | 78.0 | 137.0 | 2 | 50.1/+79.7; 129.9/-79.7 |
| lane4 | 90.0 | 307.0 | 319.9 | 65.1 | 149.9 | 2 | 36.8/+73.8; 110.5/-73.8 |
| lane5 | 180.0 | 307.0 | 355.9 | 29.1 | 185.9 | 2 | 32.5/+54.3; 86.8/-54.3 |
| tote_x0y0 | -300.0 | 0.0 | 300.0 | 85.0 | 130.0 | 1 | 138.6/+82.8 |
| tote_x0y1 | -300.0 | 100.0 | 316.2 | 68.8 | 146.2 | 1 | 123.8/+75.5 |
| tote_x0y2 | -300.0 | 200.0 | 360.6 | 24.4 | 190.6 | 1 | 120.7/+51.3 |
| tote_x1y0 | -215.0 | 0.0 | 215.0 | 170.0 | 45.0 | 1 | 122.5/+115.0 |
| tote_x1y1 | -215.0 | 100.0 | 237.1 | 147.9 | 67.1 | 1 | 101.4/+107.3 |
| tote_x1y2 | -215.0 | 200.0 | 293.6 | 91.4 | 123.6 | 1 | 94.3/+85.5 |

| target | x_mm | y_mm | r_mm | margin_to_rmax385_mm | margin_to_rmin170_mm | ik_feasible_solutions | q1_q2_deg |
|---|---|---|---|---|---|---|---|
| wide_lane1 | -125.0 | 326.5 | 349.6 | 35.4 | 179.6 | 2 | 81.9/+58.1; 140.0/-58.1 |
| wide_lane2 | 0.0 | 326.5 | 326.5 | 58.5 | 156.5 | 2 | 54.7/+70.6; 125.3/-70.6 |
| wide_lane3 | 125.0 | 326.5 | 349.6 | 35.4 | 179.6 | 2 | 40.0/+58.1; 98.1/-58.1 |
| d120_pocket_front | -257.5 | 30.0 | 259.2 | 125.8 | 89.2 | 1 | 123.8/+99.2 |
| d120_pocket_rear | -257.5 | 170.0 | 308.6 | 76.4 | 138.6 | 1 | 107.0/+79.0 |

Level-0 headroom (L2 cover top, zeta 130 above the gripper zeta-0 plane, must stay >= 15 mm under the upper plate underside; upper plate underside at the entrance 519.7 mm at 6 deg). **600 mL: 42.7 mm at the lane target, 39.5 mm 30 mm further along the slope, 51 mm (L2 cover) under the level-1 tote ledge.** The L1 link (top at zeta 201) is -20 mm relative to the level-1 ledge underside at a level-0 tote cell (negative = above it: it collides wherever the link passes under the ledge or the tray, section 8). SKUs that fit level 0: can_355_std, can_355_slim, can_473_tall, pet_500_water, pet_600_soda. Level 1 is open above. Carriage range 370 ... 1000 mm (z_c = z_g + 150; the offset is UNVERIFIED for the rev-4 datum table): z_c **exceeds 1000 mm for pet_1500 1006.2 (lane) / 1011 (D120 tote); pet_2000 1014.1 (lane) / 1020 (D120 tote); pet_2500 1038.8 (lane) / 1045 (D120 tote); pet_3000 1023 (lane) / 1030 (D120 tote)**; the largest value inside the range is 990 mm. These SKUs were inside the range at pitch 420 and grasp gap 5 (pet_2000: 995 mm at the D120 tote), so the pitch and gap changes of this revision push them out.

| sku | H_mm | D_max_mm | L0_lane_clearance_axis_mm | L0_lane_clearance_L2tip30_mm | L0_tote_clearance_mm | L0_headroom_ge15 | L1_zc_mm | L1_zc_tote_mm | L1_carriage_ok_370_1000 |
|---|---|---|---|---|---|---|---|---|---|
| can_355_std | 122.7 | 66.0 | 146.7 | 143.5 | 155.0 | True | 829.4 | 833.0 | True |
| can_355_slim | 156.0 | 57.0 | 123.7 | 120.5 | 132.0 | True | 852.8 | 856.0 | True |
| can_473_tall | 157.2 | 66.0 | 122.5 | 119.3 | 130.8 | True | 853.6 | 857.2 | True |
| pet_500_water | 210.0 | 68.0 | 69.7 | 66.5 | 78.0 | True | 906.3 | 910.0 | True |
| pet_600_soda | 237.0 | 71.0 | 42.7 | 39.5 | 51.0 | True | 933.1 | 937.0 | True |
| pet_1000 | 290.0 | 82.0 | -10.3 | -13.5 | -2.0 | False | 985.5 | 990.0 | True |
| pet_1500 | 311.0 | 89.0 | -31.3 | -34.5 | -23.0 | False | 1006.2 | 1011.0 | False |
| pet_2000 | 320.0 | 110.0 | -40.3 | -43.5 | -32.0 | False | 1014.1 | 1020.0 | False |
| pet_2500 | 345.0 | 115.0 | -65.3 | -68.5 | -57.0 | False | 1038.8 | 1045.0 | False |
| pet_3000 | 330.0 | 130.0 | -50.3 | -53.5 | -42.0 | False | 1023.0 | 1030.0 | False |

| sku | D_max_mm | H_mm | lane_config | std_lane_free_per_side_mm | wide_lane_free_per_side_mm | tote_insert | level0_ok | level1_carriage_ok_tote | note |
|---|---|---|---|---|---|---|---|---|---|
| can_355_std | 66.0 | 122.7 | STD 5-lane (L0+L1) | 10.5 | 27.25 | 6 x D75 | True | True |  |
| can_355_slim | 57.0 | 156.0 | STD 5-lane (L0+L1) | 15.0 | 31.75 | 6 x D75 | True | True |  |
| can_473_tall | 66.0 | 157.2 | STD 5-lane (L0+L1) | 10.5 | 27.25 | 6 x D75 | True | True |  |
| pet_500_water | 68.0 | 210.0 | STD 5-lane (L0+L1) | 9.5 | 26.25 | 6 x D75 | True | True |  |
| pet_600_soda | 71.0 | 237.0 | STD 5-lane (L0+L1) | 8.0 | 24.75 | 6 x D75 | True | True |  |
| pet_1000 | 82.0 | 290.0 | STD 5-lane (L1 only) | 2.5 | 19.25 | 2 x D120 (L1) | False | True | D is an ESTIMATE in the SKU table: measure |
| pet_1500 | 89.0 | 311.0 | WIDE 3-lane (L1 only) |  | 15.75 | 2 x D120 (L1) | False | False | D is an ESTIMATE in the SKU table: measure |
| pet_2000 | 110.0 | 320.0 | WIDE 3-lane (L1 only) |  | 5.25 | 2 x D120 (L1) | False | False | D is an ESTIMATE in the SKU table: measure |
| pet_2500 | 115.0 | 345.0 | none (D > 110) |  | 2.75 | 2 x D120 (L1) | False | False | D is an ESTIMATE in the SKU table: measure |
| pet_3000 | 130.0 | 330.0 | none (D > 110) |  |  | none | False | False | D is an ESTIMATE in the SKU table: measure |

## 8. Interface rules for the robot and gripper tracks (found by this track's sweeps, FINAL arm + gripper rev 4)

* **Closing axis along y at the tote and in the lanes.** Tote cells: x = -300, -215, y = 0, 100, 200; D120 pockets (x = -257.5) y = 30, 170. Closing along x would leave 1.25 mm to the neighbour bottle.
* **Grasp height** z_g = z_floor + max(H + 10, 143) (`gripper_datum_clearances.csv`): z_g is the height of the gripper zeta-0 plane; the body bottom (zeta -3.8) is then 6.2 mm above the bottle top and the fingertip (zeta -140) 3 mm above the floor for cans. z_floor is the lane floor at the bottle axis (it falls 0.105 mm per mm of y: 96.1 at y = 307, level 0) or the pocket floor in the tote.
* **Cans (`can_355_std`, H 122.7): raise the grasp height** by 5 mm in the lanes and 10 mm in the tote pockets (both verified with the 3 mm margin; the minimum is +4 mm and +8 mm). At 6 deg the floor rises 4.8 mm under the outer face of the near finger stack (nominal standoff 3 mm), and in the tote the fingertips must clear the 8 mm pocket web and, at the two end rows, the rim (top 11 mm above the pocket floor). Other SKUs are unaffected (their fingertips start >= 13 mm above the floor). The lane rule depends on the slope: +5 mm is valid up to 8 deg with the 3 mm margin; use 45.75 mm x tan(slope) (+7.3 mm at 9 deg, +8.1 mm at 10 deg) above that.
* **Tote rim.** The rim top is 11 mm above the pocket floor (8 mm pocket + 3 mm rim): lift a bottle by 11 mm plus a margin (14 mm with the 3 mm margin used here) before moving it sideways out of a cell, and carry it at least that high over the tray when it approaches lane 1 of level 0 (the tray's rear-right corner, x -350 ... -165, y <= 252.5, lies in front of lane 1). Relative to the lane floor at the lane-1 target (z 96.1) the rim top (111) is 14.9 mm high, so the carry height there is 17.9 mm with the margin. The lift that is free at every usable level-0 target is at least 22 mm with the 3 mm margin (table below); level 1 has at least 40 mm (scan cap) at every target.
* **Vertical lift limits at level 0** (`fixture_interface_limits.csv`; mm of pure vertical lift above the grasp pose that stay collision-free along the whole path; nominal envelope / envelope grown by 3 mm; '>=40' = scan cap reached, 'blocked' = collision already at the grasp pose; best IK branch; can lift rule applied):

| target (x, y) | can_355_std | can_355_slim | can_473_tall | pet_500_water | pet_600_soda |
|---|---|---|---|---|---|
| lane1 (-180, 307) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | 25 / 22 |
| lane2 (-90, 307) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / 38 |
| lane3 (0, 307) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / 38 |
| lane4 (90, 307) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / 38 |
| lane5 (180, 307) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / 39 |
| tote_x0y0 (-300, 0) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | 7 / 4 | blocked / blocked |
| tote_x0y1 (-300, 100) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 |
| tote_x0y2 (-300, 200) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | 37 / 34 |
| tote_x1y0 (-215, 0) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 |
| tote_x1y1 (-215, 100) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 |
| tote_x1y2 (-215, 200) | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 | >=40 / >=40 |

* **Level-0 tote cell (-300, 0): UNUSABLE for 500 mL and 600 mL.** The L1 link (top at zeta 201) reaches the level-1 tray, ledge and stop there (J2 at about (-150, 132)): for 500 mL the link top is 7 mm below the ledge underside (z 528), so only a 7 mm lift (4 mm with margin) is possible against the 11 mm + margin needed; 600 mL collides at the grasp pose (link top 20 mm above the ledge underside). The pocket is kept (level 1 uses it; the three can SKUs have a lift >= 40 mm there). Outside this track: a level pitch of at least 447 mm would make it usable for 500 mL (474 mm for 600 mL), a lower L1 link top (the cover is the top 3 mm) would help in the same way.
* **Other level-0 limits.** 600 mL at lane 1 (x = -180): lift 25 mm nominal / 22 mm with margin, stopped by the L2 link against the level-1 front-left bracket seat (seat bottom z = 495.6 at 6 deg; it moves with the slope shim); 600 mL at tote cell (-300, 200): 37 mm nominal / 34 mm with margin, stopped by the L2 link against the level-1 tote bracket at y = 200. Every other usable level-0 target allows >= 38 mm with the margin.
* **Level 1** is open above: lift >= 40 mm (scan cap) at every STD and WIDE target and SKU.
* **Slope range 4 ... 10 deg** (`fixture_interface_limits_vs_slope.csv`, `fixture_slope_envelope.py`: the whole fixture is re-posed per slope step, posts fixed, matching shim): the level-0 limits above hold at every slope. Lowest lift with the 3 mm margin of any usable 500 / 600 mL target over the range: 21 mm (pet_600_soda at lane1, 4 deg; 24 mm nominal); tote cell (-300, 0) stays unusable for 500 / 600 mL at every slope. The fixed can lane rule (+5 mm) clears the nominal envelope at every slope but loses the 3 mm margin at 9, 10 deg; the slope-scaled rule extra = 45.75 mm x tan(slope) (4 deg +3.2, 5 deg +4.1, 6 deg +4.9, 7 deg +5.7, 8 deg +6.5, 9 deg +7.3, 10 deg +8.1) keeps the 3 mm margin at all seven slopes (verified).
* The 15 mm shelf rule is met by the L2 cover with room: with 42.7 mm at the lane target (39.5 mm at +30 mm) the L2 top may reach zeta 154.5 (not 130) at the 600 mL level-0 lane. The closed-form number counts the L2 cover only; the 3D sweep above also counts the L1 link and the brackets.
* Front posts are at y = 299.8; the gripper body at tote cell (-300, 200) ends at y = 274.75 (10.1 mm to the post). Use the fixture coordinates of section 1 for keep-out volumes.
* The side stop of the tote is on the tray's LEFT edge and the ledge ends flush with the tray's right edge (section 2): do not assume ledge or stop material at x > -165.
* Z-lift carriage at level 1: the pitch of 440 mm and the 10 mm grasp gap move z_c above 1000 mm for the SKUs listed in section 7 (headroom paragraph).

## 9. VERIFY WITH CALIPER BEFORE PRINTING (unverified dimensions; all are named parameters at the top of `fixture_build.py`)

1. `tape_t` 0.2 mm and `rib_w` 19 mm: measure your PTFE/UHMW tape thickness and width (the ribs are 1 mm high, 19 mm wide, 3 per segment).
2. `insert_d` 4.2 x `insert_depth` 5.5 and `slide_clear` 0.25: print the coupon, test an insert and the four slide-fit slots; the dovetail and tray joints use 0.25 mm per side.
3. `bolt_d` 6.6 (M6 clearance), `cbore_d` 11 x 6.5 deep: check with your M6 SHCS and washers (M5 is possible: change `bolt_d` to 5.5 and `cbore_d` to 9.5).
4. `post` 30.0 and `foot_socket` 30.6: measure the real batten or 3030 profile (wood battens are often 28-30 mm; resize the socket and the post lateral positions `post_gap`).
5. `plate_t` 12, `ledge_t` 6 and the 18 mm panel: measure the plywood actually bought (birch ply is often 11.5 - 12.4 mm); thickness errors shift the seat heights and the 15 mm headroom.
6. `wheel_chan_w` 18 / strip 17 mm (wheel option only): UNVERIFIED.
7. SKU diameters and heights of `pet_1000`, `pet_1500`, `pet_2000`, `pet_2500`, `pet_3000` are estimates in the SKU table: measure before relying on the WIDE and D120 sets.
8. Lane friction: PTFE/UHMW on PET assumed mu 0.10 (kinetic). Measure with a slide-angle test; stops and the 6 deg slope were sized for 0.07 ... 0.10.
9. `pocket_depth` 8, `tray_rim_h` 3, `tray_base_h` 14, `ledge_t` 6: the pocket floor must sit exactly at the lane floor reference (z = 100 / 540): measure the printed tray (6 mm of material under each pocket), the ledge plywood (6 mm) and the tote bracket seat (16 mm); every mm of error moves the pocket floor and eats into the 3 mm margins of section 8. The 3 x 3 mm rim is a thin lip: if it chips in use it can be removed without changing any clearance reported here (the tray surface is 8 mm above the pocket floor).
10. Arm and gripper envelope (`ENV` in `fixture_assembly.py`): bounding boxes read from `arm_assembly.step` and `gripper_assembly_gap70.step` (assembly pose, J1 axis at the origin, links along +y) plus the lead's footprint 149.5 x 60.3 (the STEP shows 144 x 60.2); the finger box (13 x 44.4, outer face D/2 + 12.75) uses the V-apex rule of the lead. Links with rounded corners (R about 10 mm) are boxes, conservative by up to 3 mm at the corners. Confirm with the real stack before trusting the 3 mm margins.
11. Z-lift carriage reach at level 1 for 1.5 L and 2 L bottles (section 7): z_c = z_g + 150 is the earlier convention; the rev-4 datum table puts the L1 floor bottom at zeta 145.

On the assembled hardware: slope 6.0 deg (inclinometer); posts plumb; baseboard flat, top = robot z 0; wiggle test of the frame (the sway estimate of 0.05 mm per 20 N ignores the bracket joint flexibility); gripper-to-lane and gripper-to-tote clearances with the real gripper; ledge sag under a loaded tote.

## 10. Known limits and open issues

* Envelope sweep: 3 cases are not full hover (section 7): the level-0 tote cell (-300, 0) for 500 / 600 mL (the L1 link top meets the level-1 ledge and tray) and 600 mL at lane 1. The fixture side is at its limit (ledge plate 6 mm, tray base under the pocket 6 mm); remedies are on the arm side (L1 link top lower) or in the level pitch, or procedural (use another cell for 500 / 600 mL).
* The tote ledge is flush with the tray rear edge (y = 252.5) and the lane plate front edge is only 2.6 mm behind it (y = 255.1 at 6 deg): do not raise the tray by more than 1 mm.
* Printed PETG across-layer allowable (5 MPa) is a design value from the brief; the tote bracket seat (1.75x) and main bracket seat (1.66x) have the smallest margins: a 100 N load test on the first printed bracket is recommended.
* The tote stand joints (glue and wood screws), the foot retaining screws and the M6 length choices are proposals (the brief did not specify them).
* Interference uses the bracket, shim, foot and post meshes without fasteners; hole positions were checked in the tables only, not by boolean cuts of the plywood.
* The WIDE lanes and the D120 insert replace the STD lanes and the 6-pocket tray on level 1 (section 6 step 11); they are not usable together with the STD set on the same level.
