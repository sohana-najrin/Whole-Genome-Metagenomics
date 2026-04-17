# Whole-Genome-Metagenomics
# Metagenomic Analysis Pipeline (Perl-based)

## 📌 Overview

This pipeline performs end-to-end processing of paired-end metagenomic sequencing data. It includes:

* Adapter trimming
* Host (human) contamination removal
* De novo assembly
* Assembly quality assessment
* Gene prediction
* Functional annotation
* Taxonomic profiling

The workflow is implemented in **Perl** and integrates widely used bioinformatics tools.

---

## ⚙️ Pipeline Workflow

### 1. Adapter Trimming

Removes sequencing adapters and low-quality bases using `fastq-mcf`.

**Output:**

* `*.AT.R1.fastq.gz`
* `*.AT.R2.fastq.gz`

---

### 2. Host Contamination Removal

Aligns reads against the human reference genome using `BWA` and removes mapped reads.

**Steps:**

* Alignment with BWA MEM
* Conversion, sorting, and indexing using SAMtools
* Extraction of unmapped reads

**Output:**

* `*.UC.R1.fastq.gz`
* `*.UC.R2.fastq.gz`

---

### 3. De novo Assembly

Assembles unmapped reads into contigs using `MEGAHIT`.

**Output:**

* `final.contigs.fa`

---

### 4. Back-Referencing

Maps reads back to assembled contigs to evaluate assembly quality.

**Output:**

* `*.backref.sorted.bam`
* `backref.log`

---

### 5. Assembly Quality Assessment

Evaluates assembly metrics using `MetaQUAST`.

**Output:**

* Assembly report directory (`*.report/`)

---

### 6. Gene Prediction

Predicts genes from assembled contigs using `Prodigal`.

**Output:**

* Protein sequences (`*.trans_file`)
* Nucleotide sequences (`*.nuc_file.fasta`)

---

### 7. ORF Filtering & Statistics

Filters predicted ORFs and computes length-based statistics.

**Output:**

* `ORF.filtered.ffn`
* `ORF.filtered.txt`

---

### 8. Functional Annotation

Annotates ORFs using `DIAMOND BLASTx` against the NR database.

**Output:**

* `*_Blastx_out.txt`

---

### 9. Taxonomic Analysis

Processes BLAST results using MEGAN tools to generate taxonomic profiles.

**Output:**

* `*_Blastx.rma`
* `*_Taxonomy.txt`

---

## 🧰 Requirements

Make sure the following tools are installed and available in your `$PATH`:

* fastq-mcf
* BWA
* SAMtools
* BEDTools
* MEGAHIT
* MetaQUAST
* Prodigal
* DIAMOND
* MEGAN (blast2rma)

---

## 📂 Input

* Paired-end FASTQ files:

  * `R1.fastq.gz`
  * `R2.fastq.gz`

---

## ▶️ Usage

```bash
perl pipeline.pl <R1.fastq.gz> <R2.fastq.gz> <sample_name>
```

**Example:**

```bash
perl pipeline.pl sample_R1.fastq.gz sample_R2.fastq.gz sample1
```

---

## 📊 Output Summary

| Step            | Output                             |
| --------------- | ---------------------------------- |
| Trimming        | `*.AT.R1/R2.fastq.gz`              |
| Host removal    | `*.UC.R1/R2.fastq.gz`              |
| Assembly        | `final.contigs.fa`                 |
| Gene prediction | `*.nuc_file.fasta`, `*.trans_file` |
| Annotation      | `*_Blastx_out.txt`                 |
| Taxonomy        | `*_Taxonomy.txt`                   |

