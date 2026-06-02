---
name: epigenetic-annotataion
description: >-
  Runs annotation and interpretation of epigenetic NGS datasets including ATAC-seq, ChIP-seq, CUT&Tag, CUT&RUN, and differential region analyses. The workflow performs nearby-gene annotation, genomic feature assignment, reporting, visualization, and GSEA-ready export generation.
license: MIT
compatibility: >-
  Requires Python 3.10+ with bedtools, pybedtools, pandas, numpy, scipy, matplotlib, seaborn, scikit-learn, plotly, python-kaleido, xlsxwriter, and adjusttext. Needs outbound HTTPS network access to maayanlab.cloud (Enrichr). Writes outputs under a user-specified directory.
metadata:
  author: Hasan Al Reza <hasan.al.reza.bd@gmail.com>
  version: "1.0.0"
  status: stable
  last_reviewed: "2026-06-02"
---

# SKILL: Epigenetic NGS Dataset Annotation Pipeline

## Overview

This skill provides a unified workflow for annotating epigenetic NGS datasets including:

- ATAC-seq
- ChIP-seq
- CUT&Tag
- CUT&RUN
- Differential accessibility analyses
- Differential peak analyses
- Differential methylation region analyses

The workflow is executed through a single Python wrapper:

```text
run_epigenetic_annotation.py
```

The wrapper orchestrates:

1. Nearby-gene annotation
2. Genomic feature annotation
3. Result aggregation
4. Visualization
5. GSEA-ready output generation
6. Conda environment management
7. Helper-script validation

---

# Project Structure

```text
project/
├── run_epigenetic_annotation.py
│
├── scripts/
│   ├── voom2anno.sh
│   ├── annotateGenomicFeatures.py
│   ├── OrganizeAnnotationResults.py
│   ├── wcn.sh
│   ├── tabit.sh
│   ├── tabnNA.sh
│   ├── region2bed.sh
│   ├── bed2region.sh
│   ├── winandgroup.sh
│   └── gene2nomicro.awk
│
├── annotations/
│   ├── gencode.v31.hg38.gtf.bed.sorted.tss
│   ├── gencode.v19.hg19.bed.tss
│   ├── gencode.vM22.mm10.gtf.bed.tss
│   ├── gencode.vM17.mm9.gtf.bed.tss
│   ├── sacCer3.shiftedBy125.flank375.bed.tss
│   ├── hg38/
│   ├── hg19/
│   ├── mm10/
│   ├── mm9/
│   └── sacCer3/
│
└── environment/
    └── epi_anno_env.yml
```

---

# Main Pipeline

## File

```text
run_epigenetic_annotation.py
```

## Responsibilities

The wrapper:

- detects input type automatically
- selects the correct voom2anno mode
- runs gene annotation
- runs genomic feature assignment
- generates plots and reports
- validates helper scripts
- injects helper scripts into PATH
- manages conda environments
- supports dry-run execution

---

# Supported Inputs

| Input Type | voom2anno Mode |
|------------|----------------|
| `*.bed` | `bed6h1` |
| `*.vout` | `pktesth1` |

No manual mode selection is required.

---

# Workflow

```text
Input File
      ↓
voom2anno.sh
      ↓
*.anno
      ↓
annotateGenomicFeatures.py
      ↓
Feature-annotated .anno
      ↓
OrganizeAnnotationResults.py
      ↓
Final reports + GSEA outputs
```

---

# Genome Support

| Genome | Annotation File |
|----------|----------|
| hg38 | gencode.v31.hg38.gtf.bed.sorted.tss |
| hg19 | gencode.v19.hg19.bed.tss |
| mm10 | gencode.vM22.mm10.gtf.bed.tss |
| mm9 | gencode.vM17.mm9.gtf.bed.tss |
| sacCer3 | sacCer3.shiftedBy125.flank375.bed.tss |

---

# Genomic Feature Annotation

Expected structure:

```text
annotations/
└── hg38/
    ├── 2kb.promoter.up.bed
    ├── 2kb.promoter.down.bed
    ├── 2kb.exon.bed
    ├── 2kb.intron.bed
    ├── 2kb.tes.bed
    ├── 2kb.dis5.bed
    ├── 2kb.dis3.bed
    └── 2kb.intergenic.bed
```

Equivalent structures are expected for all supported genomes.

---

# Helper Script Validation

The wrapper validates:

```text
wcn.sh
tabit.sh
tabnNA.sh
region2bed.sh
bed2region.sh
winandgroup.sh
gene2nomicro.awk
```

and automatically prepends:

```text
scripts/
```

to PATH during execution.

---

# Conda Environment Support

The workflow ships with a default environment YAML:

```text
environment/epi_anno_env.yml
```

Default environment name:

```text
epi_anno_env
```

Create and use the bundled environment:

```bash
python run_epigenetic_annotation.py \
    --input-dir peaks \
    --create-conda-env \
    --run
```

Use a custom environment YAML:

```bash
python run_epigenetic_annotation.py \
    --input-dir peaks \
    --conda-yaml /path/to/custom_env.yml \
    --conda-env custom_epi_env \
    --create-conda-env \
    --run
```

Use an existing conda environment prefix:

```bash
python run_epigenetic_annotation.py \
    --input-dir peaks \
    --conda-prefix /path/to/env \
    --run
```

---

# Common Usage

## Dry Run

```bash
python run_epigenetic_annotation.py \
    --input-dir peaks \
    --dry-run
```

## BED Files

```bash
python run_epigenetic_annotation.py \
    --input-dir peaks \
    --genome hg38 \
    --run
```

## Differential Peak Files

```bash
python run_epigenetic_annotation.py \
    --input-dir differential_results \
    --genome hg19 \
    --run
```

## Custom Output Directory

```bash
python run_epigenetic_annotation.py \
    --input-dir peaks \
    --out-dir annotation_results \
    --run
```

---

# Command Line Arguments

| Argument | Description |
|-----------|-----------|
| `--input-dir` | Directory containing `.bed` and/or `.vout` files |
| `--out-dir` | Output directory |
| `--bed-glob` | BED file pattern |
| `--vout-glob` | VOUT file pattern |
| `--copy-inputs` | Copy instead of symlink inputs |
| `--base-dir` | Base project directory |
| `--scripts-dir` | Directory containing scripts and helper utilities |
| `--annotations-dir` | Directory containing TSS and feature annotations |
| `--feature-anno-dir` | Override genomic feature annotation directory |
| `--genome` | Genome build |
| `--distance1` | Proximal annotation window |
| `--distance2` | Distal annotation window |
| `--python-bin` | Python interpreter override |
| `--conda-yaml` | Conda YAML file. Defaults to `environment/epi_anno_env.yml` |
| `--conda-env` | Conda environment name |
| `--conda-prefix` | Explicit conda environment prefix |
| `--create-conda-env` | Create environment if missing |
| `--use-current-python` | Use current Python environment |
| `--skip-existing-anno` | Skip existing annotation files |
| `--skip-organize` | Skip final report generation |
| `--dry-run` | Validate commands only |
| `--run` | Execute workflow |

---

# Outputs

```text
<out-dir>/
├── finalReports/
├── allOtherFiles/
├── bedFileAnnotations/
└── GenomicFeaturesAnnotation/
```

Including:

- annotated region tables
- Excel annotation workbooks
- BED exports
- GSEA GMT files
- GSEA RNK files
- MA plots
- Volcano plots
- PCA plots
- Heatmaps
- Genomic feature summaries
- Combined annotation reports

---

# Best Practices

- Use absolute paths for reproducibility.
- Keep all helper utilities under `scripts/`.
- Keep all annotation resources under `annotations/`.
- Keep the default environment file at `environment/epi_anno_env.yml`.
- Use `--create-conda-env` to create the bundled environment.
- Use `--conda-prefix` for pre-existing shared environments.
- Use `--dry-run` before large analyses.
- Keep genome versions consistent.
- Version-control the wrapper and annotation resources.

---

# Troubleshooting

## Missing Helper Script

Example:

```text
wcn.sh: command not found
```

Verify:

```bash
which wcn.sh
```

and ensure the script exists under:

```text
scripts/
```

## Missing Genomic Feature BEDs

Example:

```text
KeyError: '2kb.promoter.up.bed'
```

Verify:

```text
annotations/hg38/2kb.promoter.up.bed
```

exists and that `--feature-anno-dir` points to the parent annotation directory.

## Default Environment YAML Not Found

Verify:

```bash
ls environment/epi_anno_env.yml
```

or provide:

```bash
--conda-yaml /path/to/env.yml
```

## Conda Environment Not Found

Use:

```bash
--conda-prefix /path/to/env
```

or:

```bash
--create-conda-env --conda-yaml env.yml
```

---

# References

- ENCODE: https://www.encodeproject.org/
- BEDTools: https://bedtools.readthedocs.io/
- Bioconductor: https://bioconductor.org/
- GSEA: https://www.gsea-msigdb.org/
