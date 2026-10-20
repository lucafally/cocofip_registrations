# Pre-registration of the sbibm benchmark study (AQCOT and CoCoFIP)

Public, self-contained record of the registered predictions of the sbibm benchmark study of the
AQCOT / CoCoFIP paper. Study A benchmarks the conditional coupling AQCOT (its nested-sort
instance, KR-AQCOT) against the independent coupling and the published conditional-coupling
baselines on the public sbibm tasks, scored with sbibm's own metrics against its reference
posteriors. Study B tests whether CoCoFIP, the truth-free statistic computed from a pretrained
coupler and held-out simulations alone, orders coupler recipes the way their trained flows do.
This repository holds the registration only; the code lives in a separate repository that will
be published after clean-up, and nothing here depends on it.

**Terminology.** The registered files predate the names used in the paper: they call CoCoFIP
"the held-out audit" (`*.audit.json`, `worst_axis_imprint`) and KR-AQCOT `kr_qot`. The files
are hash-locked by the stamps and are therefore not reworded.

## What is here

`sbibm_benchmark/` is byte-identical to the registration folder of the study and self-contained;
its own README lists every file and the verification commands. It holds

- the registered predictions P-A1–P-A6 (study A) and P-B1–P-B2 (study B), with every band
  amendment and its date written into the file;
- the study configuration at stamp A;
- the 64 CoCoFIP files of every coupler of the unlimited track (`public_B/`), computed before
  any study-B flow was trained;
- the two stamps and the logs of the stamp jobs.

## The stamps and the timeline

- **Stamp A** (2026-10-16 13:17 UTC, job 12243703 on the MPCDF cluster viper): sha256 of the
  predictions and of the configuration, recorded before the full wave's flows were submitted.
- **Stamp B** (2026-10-17 17:34 UTC, job 12259551): the study-B predictions together with the
  sha256 of the 64 CoCoFIP files, recorded before any study-B flow was trained.
- **Publication:** this repository was pushed on 2026-10-20 at 13:30 CEST. The gated flows of
  study B, of the order-sensitivity test and of the SLCP / Lotka–Volterra unlimited track were
  submitted after the push, the first at 13:30:34 CEST (job 12294797 onward).

Stated plainly: study B is a public pre-registration. Study A's bands were amended from a
two-task pilot (documented in the yaml), frozen by stamp A before its flows, and publicly
archived here after those flows ran.

## How to verify

    cd sbibm_benchmark
    sha256sum sbibm_benchmark_registered.A.yaml study_v1.A.yaml        # compare with A_stamp.json / B_stamp.json
    python3 -c "import json,hashlib; d=json.load(open('B_stamp.json')); \
      bad=[k for k,h in d['audit_files'].items() if hashlib.sha256(open('public_B/'+k,'rb').read()).hexdigest()!=h]; \
      print('mismatching files:', bad)"

## History of this repository

- 2026-10-19, `96ae992`: the registration commit of the private code repository (the same 76
  files at the repository root), carried over unchanged as the first commit.
- 2026-10-20, `f61f2b8`: the files moved into `sbibm_benchmark/`; license and Zenodo metadata
  added; registered-prediction files of other studies of the same code base, which are not part
  of this paper, were included as well.
- 2026-10-20, `8fcd3b2` (later the same day): those files removed; the repository holds the sbibm
  registration only.

## License and archive

Everything in this repository is published under the Creative Commons Attribution 4.0
International license (`LICENSE`, CC BY 4.0). Releases are archived on Zenodo through its GitHub
integration (`.zenodo.json` carries the deposit metadata); the DOI of the first release is added
here once it exists.

## Author

Luca Fallböhmer (GitHub: lucafally), Max-Planck-Institut für Kernphysik, Heidelberg.
