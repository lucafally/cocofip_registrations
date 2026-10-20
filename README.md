# Registered predictions of the conditional-coupling flow-matching studies

Public, self-contained record of the pre-registered predictions of a series of studies on
conditional couplings for flow matching (KR-AQCOT and its baselines) and on the flows used to
model Monte-Carlo simulations, run on the MPCDF clusters viper and raven. This repository holds
registrations only: prediction files, their registration stamps and the held-out audit files a
registration depends on. The code lives in a separate repository that will be published after
clean-up; nothing here depends on it.

## How the registrations work

Each study wrote its predictions (number, band, kill criterion) into a `*_registered.yaml`
before the graded runs were submitted. Two kinds of evidence back the dates:

- **Stamp jobs.** A scheduler job prints the sha256 of the prediction file and of the study
  configuration into its log before the first graded run is submitted; the stamp JSON, the log
  and the files it hashes are kept together (the sbibm benchmark below; the resynthesis + Reflow
  study's stamps are on raven and will be added).
- **Git history of the private code repository.** The commit that added or amended a prediction
  file. The commit hashes are listed below; once the code repository is public they can be checked
  directly. Until then they are claims, and the publication date of this repository
  (2026-10-20) is the public timestamp of everything in it.

Predictions are never deleted or silently reinterpreted: a graded prediction keeps its line with
its verdict, a fired kill criterion is reported as a result.

## sbibm benchmark (study A: the benchmark; study B: the audit's recipe ordering) — the public pre-registration

`sbibm_benchmark/` is byte-identical to the registration folder of the study and self-contained
(see its own README for the verification commands). Stamp A (job 12243703, 2026-10-16) freezes
P-A1–P-A6 before the full wave's flows were submitted; stamp B (job 12259551, 2026-10-17/18)
freezes P-B1–P-B2 together with the 64 held-out audit files in `public_B/`, computed before any
study-B flow was trained. This repository was published on 2026-10-20, before the study-B,
order-sensitivity and SLCP / Lotka–Volterra unlimited-track flows were trained: study B is a public
pre-registration; study A is internally stamped and publicly archived.

**History.** The first commit of this repository (`96ae992`, 2026-10-19 17:02 CEST) is the
registration commit of the private code repository's `registration` branch, carried over
unchanged (the same 76 files at the repository root); the second commit moves them into
`sbibm_benchmark/` and adds everything else. Both commits are authored by the study's author.

## Index of the other registered-prediction files

Each file is the version committed at `5a0e1c0` of the private repository (the registration is the
commit that added the file; later commits added predictions before their runs or recorded verdicts;
every prediction carries its own `registered:` date inside the file).

| file | study | first committed | last amended | sha256 (prefix) | note |
|---|---|---|---|---|---|
| `registered_predictions/scaling_predictions_registered.yaml` | toy scaling-law study | 2026-07-18 (`7fbdbcd`) | 2026-07-18 (`7fbdbcd`) | `2eb4c338c3076ba1…` | predictions frozen before wave W2 of the toy scaling-law study |
| `registered_predictions/scaling_predictions_real_registered.yaml` | real-MC scaling-law study | 2026-09-03 (`ae79f53`) | 2026-09-16 (`6be9e78`) | `f17f90330c217fe0…` | predictions frozen before wave RW2 of the real-MC scaling-law study |
| `registered_predictions/qot_real_mc_registered.yaml` | QOT coupling on real MC | 2026-09-15 (`83f13e1`) | 2026-10-02 (`36da1c9`) | `8f878908c3e7d19b…` | open predictions of the QOT-on-real-MC lane, graded rounds recorded in the lane |
| `registered_predictions/bernstein_dnf_scaling_registered.yaml` | Bernstein-DNF scale-up | 2026-09-17 (`c14c288`) | 2026-09-23 (`2ef5118`) | `6a0102e526445d4c…` | written before any job of the Bernstein-DNF scale-up study |
| `registered_predictions/dnf_scaling_study_registered.yaml` | DNF capacity × unique-samples scaling | 2026-09-23 (`2ef5118`) | 2026-10-15 (`a8b23d6`) | `ae5e8adae4335025…` | written before any job of the capacity × unique-samples study |
| `registered_predictions/coupler_audit_registered.yaml` | coupler calibration audit (real MC) | 2026-10-01 (`1cce2b0`) | 2026-10-13 (`353db76`) | `db7aa58f0689c2ad…` | predictions of the coupler calibration audit; the E7-RW-B2 design was added and stamped by commit 353db76 before its flows |
| `registered_predictions/response_cdf_route_registered.yaml` | CDF-route response | 2026-10-01 (`1cce2b0`) | 2026-10-01 (`1cce2b0`) | `1553560a5be82149…` | predictions of the CDF-route response lane (all graded) |
| `registered_predictions/resynthesis_reflow_registered.yaml` | resynthesis + Reflow (studies C, D) | 2026-10-16 (`20f15e6`) | 2026-10-16 (`20f15e6`) | `e3193a61d74b3d0c…` | stamp jobs on raven: 31188715 (A, 2026-10-15) and 31188827 (B, with the coupler audits); stamp files to be added from raven |

## How to verify

    cd sbibm_benchmark
    sha256sum sbibm_benchmark_registered.A.yaml study_v1.A.yaml        # compare with A_stamp.json / B_stamp.json
    python3 -c "import json,hashlib; d=json.load(open('B_stamp.json')); \
      bad=[k for k,h in d['audit_files'].items() if hashlib.sha256(open('public_B/'+k,'rb').read()).hexdigest()!=h]; \
      print('mismatching audit files:', bad)"

For the files under `registered_predictions/`, compare their sha256 with `git show <commit>:docs/<file> | sha256sum`
in the code repository once it is public.

## License and archive

Everything in this repository is published under the Creative Commons Attribution 4.0
International license (`LICENSE`, CC BY 4.0). Releases are archived on Zenodo through its GitHub
integration (`.zenodo.json` carries the deposit metadata); the DOI of the first release is added
here once it exists.

## Author

Luca Fallböhmer (GitHub: lucafally), Max-Planck-Institut für Kernphysik, Heidelberg.
