# SealSNP: Classifying Gray Seal Colonies from SNP Genotypes

by **Annie Tran** 

Bioinformatics course project (University of Texas at Arlington).

## Research question

Can supervised machine learning classify Northwest Atlantic gray seals (*Halichoerus grypus atlantica*) to their breeding colony using SNP genotypes?

## Data

The data came from Cammen et al. (2019), archived on Dryad: <https://doi.org/10.5061/dryad.fg52075>. The study was conducted to examine whether the seals' DNA in the Northwest Atlantic region reflects a population crash period and whether genetic variety returns as their population grows afterwards. The original paper investigated population history with statistics and simulations while this repository uses the same data to predict colony from genotypes. A noteworthy finding from the study above is that there were no significant genetic structure among colonies so colony classification may prove difficult.

Files used by the notebook (gray seal files):

| File | Description |
|---|---|
| `Hg_genepop_r80p8.gen.txt` | SNP genotypes in genepop format: 252 seals, 3,282 markers, 8 population blocks |
| `Hg.catalog.snps.csv` | SNP catalog (locus ID, position, major and alternate alleles) |
| `Hg_consensus_seq_all.fa` | Consensus sequence for each RAD locus (FASTA) |

*The data files are not stored in this repository.*

Sample sizes: 

| Gulf of St. Lawrence (2015, n = 32) | 
|Muskeget Island (2002, n = 33; 2015-16, n = 34) |
|Sable Island (1973-74, n = 28; 1985, n = 29; 1998, n = 32; 2004, n = 32; 2015, n = 32)|

## Repository contents

```
SealSNP/
├── README.md            # this file
├── notebooks/
│   └── SealSNP.ipynb    # full analysis (parsing, QC, encoding, PCA, modeling)
└── docs/
    └── ai_usage.md      # AI usage statement and verification log
```

## How to run

1. Download the three files listed above from the Dryad link into local machine.
2. Open `notebooks/SealSNP.ipynb` in Google Colab.
3. Run the cells from top to bottom. The first cell asks you to upload the three data files; answer `no` when all files have been selected/uploaded and you wish to stop the upload.

The notebook uses a fixed random seed (`random_state=42`), so results should be reproducible.

**Software:** Python 3.13 (Google Colab), Biopython 1.88, NumPy 2.4.6, pandas 2.2.3, matplotlib 3.10.0, scikit-learn 1.6.1

## Workflow summary

1. **Inspect and parse:** the genepop file was transformed into a seals × markers matrix (one malformed row, `Sab98_7X3`, was corrected).
2. **Quality control:** examined duplicate IDs, genotype code lengths, missingness per seal and per marker, allele counts, allele codes checked against the SNP catalog, and FASTA position numbering checked against the catalog.
3. **Encode** biallelic SNPs were encoded as alternate-allele dosage (0, 1, 2; missing as NaN). One tri-allelic marker (`32903_25`) is excluded to limit model sensitivity and complexity, leaving 3,281 SNPs.
4. **Label** each seal was matched to its corresponding colony and cohort using its ID prefix.
5. **Explore** initial colony visualization was conducted with PCA.
6. **Experiment 1:** classify the three colonies with a stratified 80/20 split, majority baseline, logistic regression, and random forest. Imputation and scaling happen inside scikit-learn pipelines, and hyperparameters are tuned by 5-fold stratified cross-validation on the training set.
7. **Experiment 2:** cohort-held-out (group-aware) validation on Sable and Muskeget Islands (Gulf of St. Lawrence was excluded due to insufficient cohort amount). Imputation, scaling, and models used is similar to experiment 1. Hyperparameter tuning was done using a leave-one-out validation on the training set. 

Metrics: accuracy, balanced accuracy, macro-F1, per-class precision and recall, and confusion matrices.

## Status

- [x] Parsing, quality control, encoding, labeling, PCA
- [x] Experiment 1: colony classification
- [ ] Experiment 2: cohort-held-out validation
- [ ] Final report

# Preliminary results
**Experiment 1:** (held-out test set of 51 seals) tuned logistic regression reached accuracy 0.549 and balanced accuracy 0.372, against a naive baseline's balanced accuracy of 0.333 and a majority-class accuracy of about 0.61. Random forest matched the majority baseline so no real prediction was done beyond assigning guesses based on class distribution. In PCA, the three colonies overlap, which is consistent with the original finding of no strong colony structure.

## AI usage

See [`docs/ai_usage.md`](docs/ai_usage.md).

## References

Cammen, K. M., Schultz, T. F., Bowen, W. D., Hammill, M. O., Puryear, W. B., Runstadler, J., Wenzel, F. W., Wood, S. A., & Kinnison, M. (2018). Genomic signatures of population bottleneck and recovery in Northwest Atlantic pinnipeds. *Ecology and Evolution, 8*(13), 6599-6614. <https://doi.org/10.1002/ece3.4143>

Cammen, K. M., Schultz, T. F., Bowen, W. D., Hammill, M. O., Puryear, W. B., Runstadler, J., Wenzel, F. W., Wood, S. A., & Kinnison, M. (2019). *Data from: Genomic signatures of population bottleneck and recovery in Northwest Atlantic pinnipeds* [Data set]. Dryad. <https://doi.org/10.5061/dryad.fg52075>
