# Is it just luck? Allele imbalance in nanopore sequencing of a human genome

Course project for CB2330 (PRO1).

## What this is

Jain et al. 2018 (https://www.nature.com/articles/nbt.4060) sequenced a human genome on the MinION. At 46,098 heterozygous sites on chr20, where reads should split 50:50 between two alleles, they measured the deviation d = |0.5 - fraction of reads showing allele A| and reported mean d = 0.13, with 90% of sites at d <= 0.27.

Some deviation is expected just from having a finite number of reads. This project builds a generative model to ask how much and what extra per-site bias is needed to explain the rest.

Model, per site: n ~ Poisson(27.4) reads, p ~ Uniform(0.5 - w, 0.5 + w), a ~ Binomial(n, p), d = |0.5 - a/n|. One fitted parameter, w.

## What it found

The paper reports that the reads at a het site are on average 0.13 away from a 50:50 split, and that 90% of sites are within 0.27.

If every site were a perfectly fair coin, luck alone with 27.4 reads per site would give an average of 0.077 and a 90% value of 0.158. So luck explains a bit more than half of what the paper sees.

To match the paper's average of 0.13, the model needs w = 0.22. This means a site's real split is not exactly 0.5 but anywhere from 0.28 to 0.72. With w = 0.22 the model gives an average of 0.131 and a 90% value of 0.258, close to the paper's 0.27 even though that number was not used for fitting.

How sure are we about w = 0.22:
- When we simulate data with a known w (0.1, 0.2, 0.3) and fit it, we get the same w back.
- The paper rounds its average to 2 decimals. That alone moves w between 0.20 and 0.23.
- Using different random seeds gives w between 0.21 and 0.22.

The main weakness: w depends on how many reads each site has. We used 27.4, which is the paper's genome-wide average. If these sites really had about 10 reads each, pure luck would explain both of the paper's numbers and w would be 0. The paper does not report the coverage at these sites, so we cannot rule this out.
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
