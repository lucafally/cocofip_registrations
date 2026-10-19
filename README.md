# Registration record of the sbibm benchmark study (KR-AQCOT on the public sbibm tasks)

Self-contained: every file needed to verify the pre-registration sits in this folder. The code
repository it came from is private; nothing here depends on it.

## What is registered

- `sbibm_benchmark_registered.A.yaml`, `sbibm_benchmark_registered.B.yaml` — the registered
  predictions (P-A1–P-A6: study A, the benchmark; P-B1–P-B2: study B, the held-out audit's recipe
  ordering), with every band amendment and its date written into the file (`pilot_freezes`).
  Both copies are byte-identical (one yaml serves both stamps).
- `study_v1.A.yaml` — the study configuration at stamp A (tasks, arms, budgets, model, coupler
  recipes, evaluation), byte-identical to the file whose hash the stamps record.
- `public_B/*.audit.json` — the held-out audit of every KR coupler of the unlimited track at stamp
  B (the study-B recipes on SLCP and Lotka–Volterra, the production recipe on every task, the
  coupler-variant and reversed-order couplers), computed before any study-B flow was trained.

## The stamps

- `A_stamp.json` (2026-10-16 13:17 UTC) and `B_stamp.json` (2026-10-17 17:34 UTC): sha256 of the
  registered yaml and of the configuration, the job id of the stamp job, the private repository's
  HEAD at the time; `B_stamp.json` also lists the sha256 of every audit file in `public_B/`.
- `stamp_A.12243703.out`, `stamp_B.12259551.out`: the stamp jobs' logs on the MPCDF cluster viper
  (job start 2026-10-16 18:14 CEST and 2026-10-18 01:23 CEST): `sha256sum` of both files, the
  git HEAD, and the full content of the registered yaml as the job saw it.

Timeline, stated plainly: study A's bands were amended from a two-task pilot (documented in
the yaml) and frozen by stamp A before the full wave's flows were submitted; the public archive of
this folder postdates those submissions, so A is internally stamped and publicly archived. Study
B's flows were trained only after this folder was published, so B is a public pre-registration.

## How to verify

    sha256sum sbibm_benchmark_registered.A.yaml study_v1.A.yaml      # compare with A_stamp.json / B_stamp.json
    python3 -c "import json,hashlib; d=json.load(open('B_stamp.json')); \
      bad=[k for k,h in d['audit_files'].items() if hashlib.sha256(open('public_B/'+k,'rb').read()).hexdigest()!=h]; print('mismatching audit files:', bad)"

Grading: `scripts/build_sbibm_bench_figs.py grade` in the code release writes `grading.json`
with every prediction's measured value and verdict against the bands of the registered yaml.
