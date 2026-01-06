# ![nfcore/test-datasets](docs/images/test-datasets_logo.png)

Test data to be used for automated testing with the nf-core pipelines

## Test data for nf-core/pacsomatic

This branch contains test data for the [nf-core/pacsomatic](https://github.com/nf-core/pacsomatic) pipeline.

## Introduction

The nf-core/pacsomatic pipeline is designed for somatic variant calling using PacBio long-read sequencing data. This test dataset provides a minimal set of files to validate the pipeline's functionality during automated testing and development.

## Test Dataset Composition

### Sample Information

The test dataset includes paired tumor-normal samples from the Genome in a Bottle (GIAB) reference sample **HG008**:

| Patient       | Sample ID | Status | Data Type                          |
| ------------- | --------- | ------ | ---------------------------------- |
| Patient_HG008 | DS_MT_T   | Tumor  | Downsampled mitochondrial BAM      |
| Patient_HG008 | DS_MT_N   | Normal | Downsampled mitochondrial BAM      |

### Files Included

#### Sequencing Data (`testdata/`)

- `HG008_Downsample_MT_tumor.bam`: Downsampled tumor BAM file containing mitochondrial reads
- `HG008_Downsample_MT_normal.bam`: Downsampled normal BAM file containing mitochondrial reads

#### Reference Files (`reference/`)

- `MT.fa`: Human mitochondrial genome reference sequence (Homo sapiens mitochondrion, complete genome, accession: J01415.2, length: 16,569 bp)
- `MT_target.bed`: BED file defining target regions on the mitochondrial genome for variant calling

The target BED file contains two regions covering the mitochondrial genome:
- Region 1: MT:0-3106
- Region 2: MT:3107-16569

### Samplesheet Format

The `samplesheet.csv` follows the nf-core/pacsomatic input schema:

```csv
patient,sample,status,bam,pbi
Patient_HG008,DS_MT_T,1,https://raw.githubusercontent.com/nf-core/test-datasets/pacsomatic/testdata/HG008_Downsample_MT_tumor.bam,
Patient_HG008,DS_MT_N,0,https://raw.githubusercontent.com/nf-core/test-datasets/pacsomatic/testdata/HG008_Downsample_MT_normal.bam,
```

**Column descriptions:**
- `patient`: Patient identifier (must be unique per patient)
- `sample`: Sample identifier (must be unique)
- `status`: Sample status (0 = normal, 1 = tumor)
- `bam`: Path or URL to the BAM file
- `pbi`: Optional path to PacBio index file (.pbi)

## Documentation

nf-core/test-datasets comes with documentation in the `docs/` directory:

1. [Add a new test dataset](https://github.com/nf-core/test-datasets/blob/master/docs/ADD_NEW_DATA.md)
2. [Use an existing test dataset](https://github.com/nf-core/test-datasets/blob/master/docs/USE_EXISTING_DATA.md)

## Downloading test data

Due the large number of large files in this repository for each pipeline, we highly recommend cloning only the branches you would use.

```bash
git clone <url> --single-branch --branch pacsomatic
```

To subsequently clone other branches[^1]

```bash
git remote set-branches --add origin [remote-branch]
git fetch
```

## Usage with nf-core/pacsomatic

To run the pipeline with this test dataset:

```bash
nextflow run nf-core/pacsomatic \
  -profile test,docker \
  --outdir results
```

Or with a local clone:

```bash
nextflow run nf-core/pacsomatic \
  -profile docker \
  --input samplesheet.csv \
  --fasta reference/MT.fa \
  --target_bed reference/MT_target.bed \
  --outdir results
```

## Rationale for Test Data Selection

The mitochondrial genome was chosen for this test dataset because:

1. **Compact size**: At 16.5 kb, the mitochondrial genome is small, enabling fast CI/CD testing
2. **High coverage**: Mitochondrial DNA is present in high copy numbers, providing robust variant calling tests
3. **Somatic variants**: Mitochondrial heteroplasmy mimics somatic variation patterns
4. **Reference availability**: Well-characterized reference sequence (J01415.2)
5. **Downsampling**: The BAM files are downsampled to minimize repository size while maintaining sufficient data for pipeline validation

## Support

For further information or help, don't hesitate to get in touch on our [Slack organisation](https://nf-co.re/join/slack) (a tool for instant messaging).

[^1]: From [stackoverflow](https://stackoverflow.com/a/60846265/11502856)
