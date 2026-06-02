<p align="center">
  <img src="assets/CAB-aiSkills_epigenetic_annotation.svg" alt="epigenetic annotation skill badge" width="520" />
</p>

# Epigenetic NGS Annotation Pipeline — Agent Skill

Portable skill package for annotation and interpretation of epigenetic NGS datasets including ATAC-seq, ChIP-seq, CUT&Tag, CUT&RUN, and differential region analyses. The workflow performs nearby-gene annotation, genomic feature assignment, reporting, visualization, and GSEA-ready export generation. Agent instructions live in [SKILL.md](SKILL.md).

---

## Environment

### Python

- Python 3.10 or newer

### External tools

- bedtools
- bash
- Standard Unix command-line utilities

### Conda environment

A default environment specification is bundled:

```text
environment/epi_anno_env.yml
```

Create the environment:

```bash
conda env create -f environment/epi_anno_env.yml
conda activate epi_anno_env
```

Or allow the wrapper to create it automatically:

```bash
python run_epigenetic_annotation.py \
  --create-conda-env \
  --run
```

---

## Install in Cursor / Agent Clients

- Copy or symlink this skill directory into your agent skill path.
- Ensure `scripts/`, `annotations/`, and `environment/` are preserved.
- Invoke by name or ask the agent to run epigenetic region annotation as described in [SKILL.md](SKILL.md).

---

## Quick Start — BED Files

```bash
python run_epigenetic_annotation.py \
  --input-dir peaks \
  --genome hg38 \
  --run
```

The wrapper automatically detects:

```text
*.bed → bed6h1
```

and performs:

```text
BED
 ↓
voom2anno.sh
 ↓
annotateGenomicFeatures.py
 ↓
OrganizeAnnotationResults.py
```

---

## Quick Start — Differential Peak Files

```bash
python run_epigenetic_annotation.py \
  --input-dir differential_results \
  --genome hg19 \
  --run
```

The wrapper automatically detects:

```text
*.vout → pktesth1
```

---

## Dry Run

Validate all inputs, scripts, annotations, helper utilities, and conda configuration without executing:

```bash
python run_epigenetic_annotation.py \
  --input-dir peaks \
  --dry-run
```

---

## Run Layout and Metadata

Results are written into:

```text
<out-dir>/
├── finalReports/
├── allOtherFiles/
├── bedFileAnnotations/
└── GenomicFeaturesAnnotation/
```

Generated outputs include:

- Annotated region tables
- Excel workbooks
- BED exports
- GMT files
- RNK files
- MA plots
- Volcano plots
- PCA plots
- Heatmaps
- Genomic feature summaries

---

## Directory Layout

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

## Supported Genomes

| Genome | Annotation File |
|----------|----------|
| hg38 | gencode.v31.hg38.gtf.bed.sorted.tss |
| hg19 | gencode.v19.hg19.bed.tss |
| mm10 | gencode.vM22.mm10.gtf.bed.tss |
| mm9 | gencode.vM17.mm9.gtf.bed.tss |
| sacCer3 | sacCer3.shiftedBy125.flank375.bed.tss |

---

## Helper Script Validation

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

## Common Examples

### Custom output directory

```bash
python run_epigenetic_annotation.py \
  --input-dir peaks \
  --out-dir results \
  --run
```

### Use existing conda environment

```bash
python run_epigenetic_annotation.py \
  --conda-prefix /path/to/env \
  --run
```

### Use custom annotation directory

```bash
python run_epigenetic_annotation.py \
  --feature-anno-dir /path/to/annotations \
  --run
```

---

## Testing

```bash
python run_epigenetic_annotation.py --dry-run
```

Verify helper scripts:

```bash
which wcn.sh
which tabit.sh
```

---

## Citation

| Layer | Credit |
|-------|--------|
| Skill package | CAB AI Skills Epigenetic Annotation Pipeline |
| Gene annotation | voom2anno.sh |
| Feature annotation | annotateGenomicFeatures.py |
| Reporting | OrganizeAnnotationResults.py |
| Workflow orchestration | run_epigenetic_annotation.py |

---

## License

Follow the licenses and notices included with the bundled scripts and annotation resources.
