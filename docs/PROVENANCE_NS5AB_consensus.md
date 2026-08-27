# Provenance of the NS5A–NS5B consensus sequences

This document records how the 56 consensus sequences in `sequences/` were
produced, from raw reads to the files deposited in GenBank.

---

## 1. Reads to alignments

Sequencing libraries were prepared from semi-nested RT-PCR products of the
NS5A and NS5B genes and sequenced paired-end on an Illumina MiSeq. Reads were
processed with a public Galaxy workflow, *Variant calling and consensus
construction from paired end short read data of non-segmented viral genomes*.

| Step | Tool | Version | Parameters |
|---|---|---|---|
| Quality trimming | fastp | 1.0.1 | defaults |
| Alignment | BWA-MEM | 0.7.19 | coordinate sorted, RG PL=ILLUMINA |
| Realignment | LoFreq viterbi | 2.1.5 | defaults |
| Primer trimming | iVar trim | 1.4.4 | min_qual 20, window 4, min_len 5, wiggle 0 |
| Variant calling | iVar variants | 1.4.4 | VCF, PASS records only |
| Consensus | iVar consensus | 1.4.4 | min_depth 20, sub-threshold positions masked N, min_indel_freq 0.8 |

Two workflow runtime parameters were left at their defaults deliberately, so
that the consensus reproduces what a routine programme workflow would
generate: **supporting read fraction to call a variant = 0.25** and **minimum
quality score = 20**.

Quality control used Samtools stats 2.0.7, Samtools view 1.21, QualiMap BamQC
2.3 and MultiQC 1.24.1.

---

## 2. Joining NS5A and NS5B

The Galaxy merge step joins amplicons by sequence overlap. That approach fails
for specimens in which the two amplicons do not overlap, and in this cohort it
produced truncated output for four specimens.

Consensus sequences were therefore built directly from the alignments at fixed
reference coordinates, using a purpose-written script
(`make_concat_consensus.py`):

- Reference: **DQ835760.1** (HCV subtype 6f)
- Coordinates: **NS5A nt 6255-7619** joined to **NS5B nt 7620-9401**, giving
  **3,147 nt**
- Each position is taken from whichever alignment covers it; where both cover
  a position, the alignment with greater depth is used
- Positions supported by fewer than 20 reads are masked as **N**
- The majority base is used at every position; **no IUPAC ambiguity codes are
  emitted**

The last choice is deliberate. Ambiguity codes at a codon carrying two
variable positions are the mechanism by which consensus interpretation
generates amino acids that exist on no viral molecule, and they contribute
nothing to phylogenetic inference. Base composition of the 56 deposited
sequences confirms this: A, C, G, T and N only.

NS5A ends at nt 7619 and NS5B begins at nt 7620, so the two genes are
adjacent rather than overlapping. The NS5B codon 162-211 window referred to
elsewhere in this project is the region where the NS5A amplicon runs into the
NS5B gene, a consequence of primer design rather than gene overlap.

Because every sequence is built on reference coordinates, the output is of
equal length across specimens and is already aligned.

---

## 3. Preparation for GenBank

Three operations were applied to the 3,147 nt sequences to produce the
deposited files:

**Terminal ambiguous bases removed.** Every sequence began with 4-19 N. After
trimming, deposited lengths range from 3,122 to 3,141 nt.

**`codon_start` recomputed per sequence.** Trimming shifts the reading frame
by the number of bases removed, so deposited sequences carry `codon_start` of
either 1 or 3.

**Sequence truncated at the terminal stop codon.** Each sequence contained the
genuine polyprotein stop codon followed by 6-9 nt of downstream sequence.
Retaining those bases would have placed a stop codon inside the annotated CDS.
Sequences were therefore truncated at the end of the stop codon, giving a CDS
that is partial at the 5' end and complete at the 3' end.

All 56 sequences were translated to verify that each contains exactly one stop
codon, positioned at the end.

Two sequences are exceptions. In **NP035B** and **NP044A** the terminal stop
codon fell inside a run of N that NCBI trimmed during submission processing.
Those two CDS features are therefore partial at both ends.

**Residual internal N.** Five sequences carry internal N runs at NS5B codons
162-211, a region not covered by either amplicon in those specimens: PB031B
(111 nt), NP035B (39), NP044A (36), NP027A (19) and LK016B. These are genuine
coverage gaps.

---

## 4. Relationship to the phylogenetic alignment

The alignment used for phylogenetic inference contains all 62 sequences at
3,147 reference-coordinate positions and is of uniform length. Deposited
GenBank sequences are shorter because terminal ambiguous positions are removed
at submission. The two are different artefacts of the same underlying data.

---

## 5. Conventions

Specimen identifiers carry a time-point suffix: **B** denotes the pre-treatment
specimen and **A** the specimen taken at virological failure. All coordinates
refer to DQ835760.1.

Sequences from one patient in this cohort (NP029) were deposited separately
under GenBank accessions PZ191478-PZ191483 and are not included in the 56
sequences here. The full paired cohort comprises 58 specimens; read alignments
for all 58 are deposited under BioProject PRJNA1519094.

---

## 6. Files

| File | Contents |
|---|---|
| `NS5AB_56_sequences.fsa` | 56 consensus sequences, named by specimen code |
| `NS5AB_56_features.tbl` | five-column feature table, gene and CDS `NS5AB` |
| `NS5AB_56_source.src` | source modifiers including collection dates |
| `NS5AB_56_assembly.cmt` | assembly structured comment |
| `SEQUENCE_INVENTORY.tsv` | specimen, patient, time point, length, `codon_start`, N content |
