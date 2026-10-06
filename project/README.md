# Allele imbalance in a nanopore human genome: luck or bias?

Course project for CB2330 (PRO1), autumn 2026.

## What this is

Jain et al. 2018 (https://www.nature.com/articles/nbt.4060) sequenced a human genome on the MinION. At 46,098 heterozygous sites on chr20, where reads should split 50:50 between two alleles, they measured the deviation d = |0.5 - fraction of reads showing allele A| and reported mean d = 0.13, with 90% of sites at d <= 0.27.

Some deviation is expected just from having a finite number of reads. This project builds a small generative model to ask how much, and what extra per-site bias is needed to explain the rest.

Model, per site: n ~ Poisson(27.4) reads, p ~ Uniform(0.5 - w, 0.5 + w), a ~ Binomial(n, p), d = |0.5 - a/n|. One fitted parameter, w.

## What it found

| | mean d | 90th percentile |
|---|---|---|
| Paper | 0.13 | 0.27 |
| Pure luck (w = 0) | 0.077 | 0.158 |
| Fitted model (w = 0.22) | 0.131 | 0.258 |

- Sampling luck alone gives a bit more than half of the paper's d.
- Fitting w to the mean gives w = 0.22: a site's real A-fraction lies anywhere from 0.28 to 0.72. The 90th percentile was not used in the fit and comes out at 0.258 against 0.27.
- The fit recovers known values of w exactly (0.1, 0.2, 0.3). Rounding of the paper's mean gives w between 0.20 and 0.23. Different random seeds give 0.21 to 0.22.
- Main limitation: w depends on the assumed coverage. If coverage at these sites were about 10 reads instead of 27.4, pure luck explains both paper numbers and w = 0. The paper does not report coverage at these sites.

## How to run it

```
git clone <this repo>
cd <this repo>
pip install -r requirements.txt
jupyter notebook project.ipynb
```

Run all cells top to bottom. Takes about a minute. Only the Python standard library and matplotlib are used; random numbers come from a fixed-seed generator written in the notebook, so the output is the same every run.

## Files

- `project.ipynb`: the project card, then the work
- `data/paper_numbers.csv`: the four numbers taken from the paper
- `data/README.md`: where they came from and their shape
