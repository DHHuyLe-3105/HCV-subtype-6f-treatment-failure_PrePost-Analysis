# HCV subtype 6f treatment failure - data deposit

Data accompanying: *Paired molecular analysis before treatment and at failure
reveals a fixed drug-target landscape in HCV subtype 6f failing
sofosbuvir/velpatasvir.*

Twenty-nine patients with hepatitis C virus subtype 6f who failed
sofosbuvir/velpatasvir in a province-wide test-and-treat programme in
Phetchabun, Thailand. NS5A and NS5B were sequenced to high depth in paired
pre-treatment and failure specimens, and every substitution reported by
consensus interpretation was re-adjudicated against individual sequencing
reads.

## What is here

Read-level adjudication output, per-position coverage and detection limits,
two-amplicon concordance measurements, the lineage catalogue of all 1,047
codons, and the source data behind every figure and table.

## What is not here

Sequence read alignments are in the NCBI Sequence Read Archive under
BioProject PRJNA1519094. Consensus sequences are in GenBank under accessions
PZ897562-PZ897617. Analysis code is available from the corresponding author on
reasonable request.

## Conventions

Specimen identifiers carry a time-point suffix: **B** denotes the
pre-treatment specimen and **A** the specimen taken at virological failure.
So LK008B is the baseline specimen and LK008A the failure specimen of the same
patient.

The two-letter prefix denotes the district of origin: LK Lom Kao, LS Lom Sak,
NP Nong Phai, PB Mueang Phetchabun, ST Si Thep.

All coordinates refer to hepatitis C virus subtype 6f reference DQ835760.1.
NS5A codon 1 begins at nucleotide 6255 and NS5B codon 1 at nucleotide 7620.

## Files

### data/read_level

| File | Contents |
|---|---|
| `cohort_report.csv` | 1,087 read-level determinations across 58 specimens: every position reported by consensus interpretation, with the adjudication outcome |
| `g2p_hcv_data.csv` | raw geno2pheno[HCV] output, 58 specimens. This is the record showing what the consensus-based system reported, including the two calls absent from the reads |

### data/coverage

| File | Contents |
|---|---|
| `coverage.csv` | read depth at the eight codons the interpretation system reports for this regimen, 58 specimens |
| `lod_report.csv` | 95% limit of detection per position, computed as 3/depth |
| `overlap_concordance.csv` | amino-acid comparisons between the two amplicons where they overlap in NS5B |
| `threshold_robustness.csv` | resistance calls re-derived at minor-variant thresholds from 1% to 15% |

### data/figures

| File | Contents |
|---|---|
| `GT6f_FigureData.xlsx` | source data for the figures and tables. `clinical_master` carries only the variables published in Supplementary Table S13 |
| `A1_ras_trajectories.csv` | 203 paired measurements: resistance-residue frequency at each of the seven NS5A drug-target positions, per patient, at both time points |
| `A2_paired_tests.csv` | paired test at each drug-target position |
| `A3_target_vs_background.csv` | drug-target positions compared with the rest of the two genes |
| `A4_ras_burden.csv` | summed resistance burden per patient |
| `A5_lineage_catalogue.csv` | lineage residue at every codon of NS5A and NS5B |
| `TableS9_full_lineage_catalogue.csv` | the same catalogue formatted for the supplementary table, all 1,047 codons |
| `table3_consensus_vs_read.csv` | consensus calls set against read-level adjudication |
| `table3_summary.csv`, `table3_headline_metrics.csv` | derived summaries |

### data/reference

| File | Contents |
|---|---|
| `REF_6f_6a_1a.txt` | the reference genomes used for alignment and for the phylogeny |
| `ns5ab_tree_1000bootstrap.nwk` | maximum-likelihood tree, 62 sequences, 1,000 bootstrap replicates |

## Licence

CC BY 4.0. If you use these data, please cite the paper.
