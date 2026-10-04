Exact-mesh audit scripts (lead, independent of the Hand and Sim tracks), 2026-10-04.
Workspace layout expected: audit/ (these scripts + s2chk_viewer_meshes.py), viewer_s2/viewer_s2_test.html (or any S2 viewer HTML: the arm, Z-module and J3 kit meshes are read from its SCENE; set S1_VIEWER=<path>),
s2hand_final/frame (Hand-track frame meshes + manifest.json converted by the pipeline in elrobot2_s2_viewer_pipeline.tar.gz), s2can_in (Wrist CAN-mode meshes).
Run: S1_VIEWER=viewer_s2/viewer_s2_test.html python audit/s2chk_place.py s2hand_final/frame out.csv     (576 poses, about 4 min)
     python audit/s2chk_tote.py s2hand_final/frame out.csv; python audit/s2chk_clear.py s2hand_final/frame out.csv; python audit/s2chk_seat.py s2hand_final/frame out.csv; python audit/s2chk_can.py s2can_in out.csv
Every run prints positive controls first; criterion 1 mm3 overlap. Results of this run: place 576/576, tote 42/42, seat 65/65, clearance minima in s2chk_clear_summary_v2.csv; CAN mode retreat 216/216 collide.

Update 2026-10-04 (later): two additions.
1. s2chk_place.py now takes the release opening from the environment (S2CHK_RELEASE_GAP, default 49.0 mm cap-wall gap = open register 2469 ticks). The first run used 54.0 mm; a re-run at 49.0 mm gave the same result for all 576 poses (zero overlap), so s2chk_place_C_kit_v2.csv stands.
2. s2chk_release.py (new): release and retreat with the RELEASED BOTTLE as an obstacle, at the true release geometry (bottle touching the lane floor, hand at the seat height, then lowered by the 2.0 mm lip drop, jaws opened to 49.0 mm, horizontal retreat from the placement y to y = 288 mm in 8 steps, flat or lifted 12 mm).
   Run: S1_VIEWER=viewer_s2/viewer_s2_test.html python audit/s2chk_release.py s2hand_final/frame out.csv   (792 poses, about 2.5 min). Positive control: raising the hand 0.3 mm above the seat must give a lip-ring overlap (8.7 mm3 found).
   Result: 792 of 792 poses clear (touch, lip drop, open, retreat flat and lifted, arm and hand against bench, neighbours and the released bottle); carriage z_c 452 to 872 mm; hand-ceiling clearance during the lifted retreat at 0.287 m is 14.2 mm (600 mL) and 41 mm (500 mL), so the insertion pose (10.0 mm) remains the governing case.
   The earlier placement audit did not include the released bottle in its release and retreat phases; that gap is closed here.
