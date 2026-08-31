# Cross-cohort IVW meta-analysis of the seven replicated ME/CFS risk loci

Code and input data reproducing the fixed-effect inverse-variance-weighted (IVW)
meta-analysis and forest plot of the seven replicated ME/CFS risk loci.

## Contents

| File | Description |
|---|---|
| `pooled_meta.ipynb` | Annotated Julia notebook. Runs the full analysis and writes the figure. |
| `data/UKB1_Downsampled_Results.csv` | UK Biobank discovery cohort (UKB1) summary statistics: variants significant at FDR < 5%. |
| `data/UKB2_Downsampled_Results.csv` | Disjoint UK Biobank replication cohort (UKB2) summary statistics. |
| `data/aou_full_sumstats_support_filtered.csv` | All of Us (AoU) replication cohort summary statistics. |

## Requirements

Julia 1.9 or later:

```julia
import Pkg; Pkg.add(["CSV", "DataFrames", "CairoMakie"])
```

For the notebook, also install IJulia (`Pkg.add("IJulia")`).

## Running

```bash
jupyter notebook pooled_meta.ipynb  # notebook
```

Both read from `data/` by default and write `replication_meta.png` and
`replication_meta.pdf` to the working directory. Override the paths with the
`META_DIR` and `META_OUT` environment variables.

## Method summary

Effects are **per-genotype risk differences** estimated with TarGene (weighted TMLE), not
odds ratios. The three cohorts are disjoint, so their estimates are independent and pooled
under a fixed-effect model; between-cohort heterogeneity is reported as I². All cohorts were
downsampled to a common case fraction (~1.11%, ~89 controls per case) before estimation, so
the risk-difference estimates share a scale and need no rescaling.

TarGene estimates each genotype transition separately rather than assuming an additive dose
response. Six loci are pooled on beta1 (major homozygote to heterozygote); **rs261902 is
pooled on beta2** (heterozygote to minor homozygote), the transition driving its discovery
association. Estimates are harmonised to a common effect allele before pooling, using UKB1
as the orientation anchor.

## Expected output

| Locus | Transition | Pooled risk difference | 95% CI | I² | Replicated in |
|---|---|---|---|---|---|
| rs115186419 | TT->TC | -0.00467 | [-0.00629, -0.00305] | 33% | AoU |
| rs73175505 | CC->CT | -0.00504 | [-0.00651, -0.00356] | 50% | UKB2 |
| rs261902 | AG->AA | -0.00446 | [-0.00650, -0.00242] | 0% | AoU |
| rs117553493 | CC->CT | -0.00475 | [-0.00639, -0.00311] | 69% | UKB2 |
| rs72741654 | CC->CT | -0.00468 | [-0.00629, -0.00307] | 29% | AoU |
| rs74963073 | CC->CT | -0.00471 | [-0.00643, -0.00299] | 72% | AoU |
| rs76847656 | TT->TC | -0.00437 | [-0.00595, -0.00280] | 58% | AoU |
