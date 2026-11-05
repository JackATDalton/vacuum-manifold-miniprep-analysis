# Vacuum manifold miniprep analysis

Data and analysis for *An Open-Source 3D-Printed Vacuum Manifold for Automated DNA Isolation in
the Opentrons Flex and OT-2* (Dalton, Goddard and Lugagne; HardwareX, in revision). The notebooks
reproduce every statistic and figure in the yield-comparison section of the paper.

## Contents

```
data/
  C4JMiniprepYieldsOct2026.csv            controlled C4J experiment (raw, wide format)
  Miniprep.csv                            routine-use preps (raw, wide format)
  Supplementary_Data_S1_C4J.csv           C4J data, long format (Supplementary Data S1)
  Supplementary_Data_S2_RoutinePreps.csv  routine-use data, long format (Supplementary Data S2)
analysis/
  c4j_position.ipynb                      controlled comparison and manifold-port analysis
  miniprep.ipynb                          routine-use plasmids (>= 3 replicates per method)
plots/                                    figures written by the notebooks (SVG; combined figures also PNG)
```

## Data

All yields are in ng/µL, measured by NanoDrop/nanophotometer after elution, using the GeneJET
Plasmid Miniprep Kit (Thermo Fisher K0503).

**Controlled comparison (C4J).** One 150 mL overnight culture of plasmid C4J was split into 3
manual (centrifuge) preps and 3 runs of 24 preps on the manifold, one prep per port per run.
Manual preps were processed first, then runs 1–3 in sequence on the same day, by a single operator
with the same kit lot. In `C4JMiniprepYieldsOct2026.csv`, rows are manifold ports (A1–D6) and
columns are runs 1–3; the `Hand` row holds the three manual preps.

**Routine use.** Plasmids prepared during routine work. Rows are plasmids; columns `O1`–`O8` are
Opentrons culture rounds and `H1`–`H3` manual culture rounds. Each round is a separate culture and
rounds were not shared between methods. The paper analyses only plasmids with at least three
replicates by both methods (c1j, P1d, t1d, u1d, V37m); this filter is applied in `miniprep.ipynb`
and flagged in `Supplementary_Data_S2_RoutinePreps.csv`.

## Figures

| Paper | File | Notebook |
|---|---|---|
| Figure 3 (controlled comparison) | `plots/c4j/C4J_Combined.{svg,png}` | `c4j_position.ipynb` |
| Figure 4 (routine-use plasmids) | `plots/Triplicates_Combined.{svg,png}` | `miniprep.ipynb` |

Individual panels and supplementary analyses are written alongside them.

## Statistics

Means are shown ± SEM. CV is the sample SD divided by the mean; pooled geometric CV is
sqrt(exp(s²) − 1), with s² the within-plasmid variance of log yield. Mean yields are compared with
Welch's t-test (for the overall C4J comparison, each run's mean is one observation). Variability is
compared with an F test on log-yield variance. Manifold port is tested with an additive two-way
ANOVA (port × run) and a Friedman test, with variance components from the ANOVA mean squares.
Success rates use 95% Wilson intervals and Fisher's exact test.

## Running

```bash
conda env create -f environment.yml
conda activate vacuum-manifold-analysis
cd analysis
jupyter nbconvert --to notebook --execute --inplace c4j_position.ipynb miniprep.ipynb
```

The notebooks read from `../data` and write to `../plots`.

## Licence

Code: MIT (see `LICENSE`). Data: CC BY 4.0.

## Citation

If you use these data or code, please cite the paper (citation to be added on publication).
