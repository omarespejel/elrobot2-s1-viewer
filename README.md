# ElRobot2: design versions

Open the site from this repository's GitHub Pages link. The landing page shows the versions of a robot-arm
design that restocks cans and PET bottles through the rear of a convenience-store cooler, with the numbers
that separate them.

## Contents

* `index.html`: versions (S1 v1.1, S2-RC2 release candidate), version history, caveats.
* `viewer/index.html`: interactive 3D viewer of S1 v1.1 (a single self-contained page with six simulation replays).
* `viewer-s2/index.html`: interactive 3D viewer of the S2-RC2 neck-grip hand on the S1 arm (a single self-contained page with seven simulation replays).
* `report.html`: the S1 design report (third round, 2 October 2026).
* `report-s2.html`: the S2-RC2 design report (4 October 2026).
* `docs/`: parameters, test plan, risk register, review log, evidence table, measurement sheet, track reports (Hand, Wrist, Simulation), desk-research pack and module notes.
* `img/`: figures used in the reports.

## What this is and is not

* S1 (top-down gripper) and S2-RC2 (collar jaws that hold a bottle by the neck) are drawn in CAD, checked with exact-mesh
  interference tests and finite-element or closed-form strength checks, and simulated in MuJoCo on a stand-in bench.
  The viewers replay those simulations on the CAD.
* The cooler outline in the viewers is illustrative. Rack dimensions in the reports are inferred from public
  catalogues (US manufacturers, Mexican resellers); no real cooler was measured. The S2-RC2 bottle neck is assumed (PCO-1881) until it is measured.
* **Nothing in this design has been printed, built or measured.** Every dimension, mass and margin is CAD,
  hand calculation, simulation or desk research.
* Scope: aluminium cans (S1) and one-way PET bottles (S2-RC2: 500 and 600 mL verified, 1 L conditional). Glass and returnable containers are out of scope.

The pages carry a noindex tag so that search engines are asked to skip them.
