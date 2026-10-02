# ElRobot2: design versions

Open the site from this repository's GitHub Pages link. The landing page shows the versions of a robot-arm
design that restocks cans and PET bottles through the rear of a convenience-store cooler, with the numbers
that separate them.

## Contents

* `index.html`: versions (S1 v1.1 as designed and simulated, S2 as a concept), version history, caveats.
* `viewer/index.html`: interactive 3D viewer of S1 v1.1 (a single self-contained page with six simulation replays).
* `report.html`: the design report (third round, 2 October 2026).
* `docs/`: parameters, test plan, risk register, review log, evidence table, S2 design brief, desk-research pack and module notes.
* `img/`: figures used in the report.

## What this is and is not

* The arm, gripper, lane fixture and Z column are drawn in CAD and tested in a MuJoCo rigid-body simulation with a
  simplified fixture. The viewer replays those simulations on the CAD.
* The cooler outline in the viewer is illustrative. Rack dimensions in the report are inferred from public
  catalogues (US manufacturers, Mexican resellers); no real cooler was measured.
* S2 (offset wrist with a neck-jaw for PET bottles and a flat clamp for cans) is a concept: it has not been
  drawn, simulated or tested.
* **Nothing in this design has been printed, built or measured.** Every dimension, mass and margin is CAD,
  hand calculation, simulation or desk research.
* Scope: aluminium cans and one-way PET bottles. Glass and returnable containers are out of scope.

The pages carry a noindex tag so that search engines are asked to skip them.
