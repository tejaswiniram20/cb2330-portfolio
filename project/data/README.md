# data/

`paper_numbers.csv` holds the only four numbers this project uses. They were typed by hand from the text of

> Jain M. et al. *Nanopore sequencing and assembly of a human genome with ultra-long reads.* Nature Biotechnology 36, 338-345 (2018). https://www.nature.com/articles/nbt.4060

No raw reads are downloaded. The paper reports summary statistics of d only, so the model is fitted to those summaries.

## Shape

4 rows, 4 columns: `name, value, unit, source`.

| name | value | unit | where in the paper |
|---|---|---|---|
| n_het_sites | 46098 | sites | Online Methods: heterozygous SNP positions on chr20, from the Illumina Platinum Genomes VCF for GM12878, at least 10 bp apart |
| mean_d | 0.13 | fraction of reads | Online Methods: "Averaged over all evaluated heterozygous SNPs, d = 0.13" |
| p90_d | 0.27 | fraction of reads | Online Methods: "90% of SNPs have d <= 0.27" |
| mean_coverage | 27.4 | reads per site | Fig. 1c legend: Poisson lambda = 27.4 (mean coverage excluding 0-coverage positions = 27.41, s.d. 64.98) |

d = |0.5 - (allele A reads) / (allele A reads + allele B reads)| at one heterozygous site.

## Caveats

- `mean_d` and `p90_d` are rounded to 2 decimals in the paper.
- `mean_coverage` is the whole-genome figure for all reads. The d values were measured on Scrappie base calls on chr20 only, and the paper does not give the coverage at those sites. The notebook tests how much this matters.
