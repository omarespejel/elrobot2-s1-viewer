# ElRobot2-S1 interactive 3D viewer

**Open the viewer: https://omarespejel.github.io/elrobot2-s1-viewer/**

A single self-contained HTML page (no build step, no server, no dependencies). It shows the CAD of a
Z-lift SCARA arm designed to restock cans and PET bottles through the rear of a convenience-store
cooler, plus six replays of a rigid-body simulation played back on that CAD.

What you can do in the page: drag to orbit, scroll to zoom, move the joint and carriage sliders,
pick a pick-and-place target, play any of the six simulation replays, and toggle the fixture, the
bottles, the column and the illustrative cooler outline.

## What this is and is not

* The arm, gripper, lane fixture and Z column are the real design geometry (STEP/STL tessellations).
  Purchased parts (extrusion, rail, screw, motor, camera) are drawn as approximate boxes.
* The replays are logged joint and bottle trajectories from a MuJoCo simulation with a simplified
  box fixture. Every second frame was re-posed on the real CAD and tested for overlaps; the arm and
  gripper never overlap the fixture, column or camera in any of the six replays.
* The pick-and-place preview (sliders and targets) is joint-space interpolation for looking at reach
  only: it has no collision or load checking.
* The cooler outline is illustrative. Its dimensions are assumed; no real cooler was measured.
* **Nothing in this design has been printed, built or measured.** Every dimension, mass and margin is
  CAD, hand calculation or simulation.

Renders with WebGL2, and falls back to a built-in software rasteriser if WebGL2 is unavailable.
