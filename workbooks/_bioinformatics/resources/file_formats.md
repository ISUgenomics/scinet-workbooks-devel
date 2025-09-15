---
title: File formats in Bioinformatics
svg: /genomics.svg
author: Aleksandra Badaczewska

header:
  overlay_image: 07-wrangling/assets/img/07_data_acquisition_banner.png
index: 
order: 3
wg: Bioinformatics
description: ""
type: reference material


objectives: 
  - Understand the role of standard file formats in bioinformatics workflows.
  - Learn the structure and key components of common formats (e.g., FASTQ, SAM/BAM, VCF, GTF).  
  - Identify which file types are used at different stages of analysis.  
  - Develop skills to inspect, interpret, and validate file content. 

applications:
  - Checking file integrity and consistency.  
  - Performing quality control on sequencing data.  
  - Preparing inputs for bioinformatics tasks.
  - Recognizing intermediate and final results.

terms: [File extension, File Compression]

takeaways: 
  - Use the correct file format for each stage of analysis (e.g., FASTQ for raw reads, VCF for variants).  
  - Validate files before use to catch truncation, formatting errors, or reference mismatches.  
  - Always inspect headers and a few sample lines to confirm file identity, structure, and metadata.  
  - Apply compression (bgzip, pigz) and indexing (.bai, .tbi, .fai) to handle large datasets efficiently.  
  - Use knowledge of format structure to troubleshoot pipeline errors and ensure smooth integration of tools.  
  - Convert between formats with reliable tools (samtools, bcftools, gffread) rather than manual editing.  
  - Maintain consistency between file formats and reference versions (e.g., genome build, chromosome naming).  


overview: [objectives, applications, terminology]

---


<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1">
Familiarity with basic biological concepts (e.g., DNA, RNA, genes, genomes, sequencing technologies, reads) is assumed. 
No prior experience with bioinformatics tools is required.

Familiarity with the [command line](/computing-skills/) is expected.
</div></div>


## Overview

Bioinformatics relies on **structured data formats** to represent sequencing outputs, alignments, annotations, and analysis results. 
These formats act as the "language" through which tools communicate, and understanding them is essential for interpreting results or building reliable workflows.

In this reference material, you will explore the major file formats used across genomics and transcriptomics, from raw reads to interpretable outputs. 
You'll learn what each format contains, how it's structured, why it's used, and how to identify or troubleshoot common issues.

{% include overviews %}


## File formats in Bioinformatics

Bioinformatics workflows depend on standard file formats to manage data generated at every stage of bioinformatics analysis: 
from raw sequencing reads to processed outputs like alignments, variants, or expression levels. 
Many of these formats require paired index files (`.bai`, `.tbi`, `.fai`) to allow fast access during analysis.

| **Format**              | **Content**                                | **Notes**                                                    |
|-------------------------|--------------------------------------------|--------------------------------------------------------------|
| [FASTA](#fasta)         | reference sequences or assembled contigs   | contains plain sequence with headers; used for sequenece assembly (genome or transcriptome) |
| [FASTQ](#fastq)         | raw sequencing reads                       | contains reads + quality scores; often gzipped; 2 files for paired-end (PE) sequencing mode |
| [SAM](#sam--bam--cram)  | read alignments (text-based)               | human-readable; includes mapping, flags, optional tags       |
| [BAM](#sam--bam--cram)  | read alignments (binary)                   | compressed version of SAM; requires `.bai` index             |
| [CRAM](#sam--bam--cram) | compressed alignments                      | smaller than BAM; needs reference to decompress              |
| [GTF](#gtf)             | gene annotations                           | tab-delimited; used by many RNA-seq tools                    |
| [GFF3](#gff)            | general feature format (genome annotations)| more flexible than GTF; used in genome browsers              |
| [BED](#bed)             | genomic intervals                          | used for peak regions, annotation features, etc.             |
| [VCF](#vcf--bcf)        | variant calls (SNPs, indels)               | text-based; includes genotypes, quality, annotations         |
| [BCF](#vcf--bcf)        | binary VCF                                 | compressed version of VCF; faster to parse                   |
| **INDEX FILES**         | `.bai`, `.tbi`, `.fai`, `.crai`, `.dict`   | required for random access in BAM/VCF/FASTA/CRAM             |


In addition to the core formats, bioinformatics workflows also make use of **complementary formats** for visualization, metadata, and specialized data types:

| **Format**     | **Used For**                               | **Notes**                                                    |
|----------------|--------------------------------------------|--------------------------------------------------------------|
| **BigWig**     | scaled coverage data (visualization)       | compressed, indexed binary format for fast display           |
| **BEDGraph**   | raw coverage signal (text-based)           | similar to BigWig; used as input or intermediate format      |
| **AGP**        | scaffold layout information                | describes how contigs form scaffolds, including gaps         |
| **MTX**        | sparse gene expression matrix              | from single-cell RNA-seq; often paired with barcodes & genes |
| **HDF5**       | hierarchical data (e.g., 10x single-cell)  | efficient storage for large, structured data (e.g. `.h5ad`)  |
| **TSV / CSV**  | expression matrices, metadata              | output of quant tools like Salmon, DESeq2, etc.              |
| **JSON / YAML**| pipeline configs, metadata                 | used by workflow managers (e.g., Snakemake, Nextflow)        |

*Note: Different tools may use different formats for the same analysis step, e.g., Cell Ranger’s `.h5` vs STARsolo’s `.tsv` output.*


### Principles behind standard formats

Bioinformatics file formats provide consistency, interoperability, and structure, ensuring that tools can exchange data reliably across workflows. 
They enforce **standardization** (e.g., `FASTQ` as the universal format for sequencing reads) to ensure compatibility across pipelines and tools, 
balance **readability** (e.g., human-readable formats like `SAM` versus compressed binaries like `BAM` or `CRAM`), 
and make use of **indexing and compression** (e.g., `bgzip`, `.bai`, `.tbi`) to handle (access and store) large datasets efficiently. 
Many formats also embed critical **metadata**, such as sample identifiers, barcodes, or read groups that support accurate downstream analysis. 
Knowing what each file format contains, and why it is used, is essential for troubleshooting, validating results, and building custom pipelines.


## FASTA

**Text-based format for storing nucleotide or amino acid sequences.**

| **Data Type**       | **Components**           | **Molecule**      | **Application**                       |
|---------------------|--------------------------|-------------------|---------------------------------------|
| reference           | genome, transcriptome    | DNA, RNA          | input for mapping/alignment tasks (e.g., RNA-seq pipeline)     |
| assembly            | contigs, scaffolds       | DNA, RNA          | genome or transcriptome assembly task |
| query               | specific sequences       | DNA, RNA          | BLAST queries, motif searching, primer design                  |
| database resource   | collections of sequences | DNA, RNA, Protein | protein DBs (e.g., UniProt), nucleotide DBs (e.g., NR, RefSeq) |


### Example FASTA file(s)

<details markdown="1"><summary><b>DNA (nucleotide) sequence</b></summary>
```markdown
>seq1 Example DNA sequence
ATGCGTAACGTAGCTAGCTAGCTAAGCTAGCTAAGCTAGCTAAGCTAGCTAAGCTAGCTA
GCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGC
>seq2 Another example sequence
TTGCGTAACGTAGCTAGCTAGCTAAGCTAGCTAAGCTAGCTAAGCTAGCTAAGCTAGC
TAGCTAGCTAGCT
```
This file represents a DNA sequence in FASTA format. The header (`>`) identifies the sequence, typically by **chromosome** or **scaffold** name. 
The sequence is shown in standard IUPAC nucleotide codes. Line breaks are for readability and have no biological significance. 
Typically stores the reference genome assembly used in the alignment step.
</details>

<details markdown="1"><summary><b>RNA (nucleotide) sequence</b></summary>
```markdown
>transcript1 Example mRNA sequence
AUGCGUAACGUAGCUAGCUAGCUAAGCUAGCUAAGCUAGCUAAGCUAGCUAAGCUAGCU
GCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGCUAGC
```
This file represents an RNA sequence in FASTA format. The sequence uses `U` (uracil) instead of `T` for RNA. The header (`>`) identifies the **transcript**. 
This format is used in transcriptomics and RNA-seq analyses.
</details>


<details markdown="1"><summary><b>protein (amino acid) sequence</b></summary>
```markdown
>protein1 Example protein sequence
MKTAYIAKQRQISFVKSHFSRQDILDLWIYHTQGYFPDWQNYTPGPGIRYPLKF
GNSHVAQVKETQAAEGLKQGVAIALKALF
```
This file shows an amino acid sequence in FASTA format. Each letter corresponds to an amino acid using standard 20 one-letter codes. 
The header (`>`) typically contains a **protein identifier** and optional description. Protein FASTA files are used in functional annotation and similarity searches.
</details>


### Format structure

Each entry begins with a **header line** starting with `>` (sequence identifier and description), followed by one or more lines of **sequence data**. 
```markdown
> sequence_id description
SEQUENCE-IN-ONE-LETTER-NOTATION
```
- Recommended sequenece line length: 60–80 characters.  
- Empty lines are typically ignored by parsers.  
- Multiple sequences can be included in one file.


<div class="highlighted highlighted--error ">
<div class="highlighted__body"  markdown="1"><h4>Common issues</h4></div>
<div style="margin-left: 1.3em; margin-right: 1.3em;" markdown="1">
**Line wrapping mismatch**   
Some tools require sequences on a *single line*, others require *wrapped lines (80 characters)*.

**Improper headers**   
Missing `>` or malformed identifiers can break parsing. 

**Hidden spaces**   
Trailing spaces or carriage returns may cause unexpected errors. 
</div>
&nbsp;
</div>


### File validation

Always check file integrity after download/transfer and validate FASTA format before use.

<div class="usa-accordion " >

{% include accordion title="Compression" class=" " controls="fasta-1" icon=false %}
<div id="fasta-1" class="accordion_content" markdown='1' hidden>
Decompress FASTA files:  
```bash
# Decompress with gunzip
gunzip sequence.fasta.gz          # restores: sequence.fasta
# Decompress bgzip
bgzip -d sequence.fasta.gz        # restores: sequence.fasta
```

Compress FASTA files with `gzip` (or `bgzip` for block compression) to save disk space: 
```bash
gzip sequence.fasta               # output: sequence.fasta.gz 
bgzip sequence.fasta              # output: sequence.fasta.gz (bgzip block-compressed)
```
**Note:** `bgzip` is preferred when the file will be indexed (e.g., for random access).
</div>

{% include accordion title="File integrity" class=" " controls="fasta-2" icon=false %}
<div id="fasta-2" class="accordion_content" markdown='1' hidden> 
Verify file integrity with checksums:
```bash
md5sum sequence.fasta > sequence.fasta.md5
md5sum -c sequence.fasta.md5      # confirm integrity after transfer
```
</div>

{% include accordion title="FASTA validation" class=" " controls="fasta-3" icon=false %}
<div id="fasta-3" class="accordion_content" markdown='1' hidden> 
Use `seqkit` to validate format and detect errors:
```bash
seqkit stats sequence.fasta      # reports #sequences, total length, N content
seqkit validate sequence.fasta   # checks file structure for format errors
```
Common checks:
- All sequences have unique IDs.
- Sequences only contain valid IUPAC characters.
- No truncated or empty entries.
- Consistent line endings (avoid mixing LF/CRLF).
</div>

{% include accordion title="Sequence line wrapping" class=" " controls="fasta-4" icon=false %}
<div id="fasta-4" class="accordion_content" markdown='1' hidden>
Reformat sequence lines for tool compatibility with `seqtk`:
```bash
# Convert to single-line FASTA
seqtk seq -l 0 sequence.fasta > sequence_oneline.fasta
# Re-wrap lines at 60 characters
seqtk seq -l 60 sequence.fasta > sequence_wrapped.fasta
```
Some programs require single-line sequences, others expect wrapped lines (80 chars).
</div>

{% include accordion title="Indexing" class=" " controls="fasta-5" icon=false %}
<div id="fasta-5" class="accordion_content" markdown='1' hidden>
Create a FASTA index for fast random access with `samtools`:
```bash
samtools faidx sequence.fasta     # output: sequence.fasta.fai
```
The `.fai` index stores sequence name, length, and line offsets. Required by many tools (e.g., aligners, variant callers).
</div>


{% include accordion title="Genome completeness" class=" " controls="fasta-6" icon=false %}
<div id="fasta-6" class="accordion_content" markdown='1' hidden> 
Check completeness of a genome assembly with `BUSCO` (Benchmarking Universal Single-Copy Orthologs):
```bash
busco -i assembly.fasta -l eukaryota_odb10 -o busco_out -m genome
```
</div>

</div>
        

## FASTQ

**Text-based format for storing nucleotide sequences together with quality scores.**

| **Data Type**       | **Components**           | **Molecule** | **Application / Analysis Step**                     |
|---------------------|--------------------------|--------------|-----------------------------------------------------|
| raw sequencing      | reads + quality scores   | DNA, RNA     | primary output of NGS machines <br>(Illumina, Nanopore) |
| pre-processed data  | trimmed/filtered reads   | DNA, RNA     | input for alignment or assembly pipelines           |
| subsets             | selected read sets       | DNA, RNA     | downstream testing, debugging, tool benchmarking    |


### Example FASTQ file(s)

<details markdown="1"><summary><b>DNA/cDNA sequencing reads</b></summary>
```markdown
@SRR4420293.1 HWI-ST1136:361:HS250:1:1101:1130:2234/1
GCAACTTCTACAAGCAACTGGGTCGCTCTCTTCCATGACGACTCATCAGAACTGTCATACACGAAGACAGCTATGTCACATGCAGCTAAGGATTCCTTGG
+
CCCFFFFFHHHHHJJJJJJJHIIJJJJJIJJJJIJJJJJJJJJJJJIJJJJJJJIIJJIJJJHHHFFFFEDEEEEEEEDDDDDDDDDDDDDDDDDEDDDD
@SRR4420293.2 HWI-ST1136:361:HS250:1:1101:1409:2108/1
GGCTTTTCCTGCGCAGCTTAGGTGGAAGGCGAAGAAGGCCCCCTTCCGGGGGGCCCGAGCCATCAGTGAGATACTACTCTGGAAGAGCTAGAATTCTAAC
+
CCCFFFFFHHHHHJJJJJJJJJFHJJJJJJJJJJJJJJJJJJJJJJJHHFDDDDDDDDDDDDDDDDEDDDDDDDDDDDDDDDDDDDDDDDDDDDDDEEED
```
The excerpt contains 2 records (reads). Each read entry has four lines. The header line starting with @ (identifier) identifies the read. 
The second line contains the raw DNA sequence (`A`/`C`/`G`/`T` and `N` for unknown bases). A separator line starting with `+` and may repeat the identifier. 
The final line encodes per base Phred quality scores (ASCII-encoded)
</details>


<div class="highlighted highlighted--info ">
<div class="highlighted__body"  markdown="1">
<h4 class="highlighted__heading">SE vs. PE reads</h4>
Sequencing can be performed in **single-end (SE)** mode, producing one FASTQ file per sample, 
or in **paired-end (PE)** mode, producing two files per sample (typically `*_R1.fastq.gz` and `*_R2.fastq.gz`) corresponding to forward and reverse reads.  
</div>
</div>


### Format structure

Each read is represented by four lines:
```bash
@SEQ_ID                                  #1
SEQUENCE-IN-ONE-LETTER-NOTATION          #2
+                                        #3
PER-BASE-QUALITY-SCORE                   #4
--------------------------------------
@SEQ_ID/1                                #1
GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTTA     #2
+                                        #3
!''*((((***+))%%%++)(%%%%).1***-+*''))   #4
```
- Line 1: starts with @, contains sequence identifier.
- Line 2: raw nucleotide sequence.
- Line 3: + separator (optionally repeats identifier).
- Line 4: ASCII-encoded quality scores, same length as sequence.

<div class="highlighted highlighted--error ">
<div class="highlighted__body"  markdown="1"><h4>Common issues</h4></div>
<div style="margin-left: 1.3em; margin-right: 1.3em;" markdown="1">
**Mismatched lengths**  
Sequence and quality strings must have identical length.  

**Encoding confusion**  
Old FASTQ files may use **Phred+64** instead of the standard **Phred+33**.  

**Corrupted compression**  
Interrupted transfers can leave `.fastq.gz` truncated or corrupted.  

**Identifier inconsistencies**  
Paired-end reads may lose sync if filenames or read IDs are inconsistent.  
</div>
&nbsp;
</div>


### File validation

Always check FASTQ files for integrity, encoding, and read quality before analysis.  

<div class="usa-accordion " >

{% include accordion title="Compression" class=" " controls="fastq-1" icon=false %}
<div id="fastq-1" class="accordion_content" markdown='1' hidden>
Compress and decompress FASTQ files:  

```bash
# Compress
gzip reads.fastq
bgzip reads.fastq

# Decompress
gunzip reads.fastq.gz
bgzip -d reads.fastq.gz
```
For large datasets, use `pigz` (parallel `gzip`) for faster compression.
</div>

{% include accordion title="File integrity" class=" " controls="fastq-2" icon=false %}
<div id="fastq-2" class="accordion_content" markdown='1' hidden> 
Verify integrity with checksums: 
```bash 
md5sum reads.fastq.gz > reads.fastq.gz.md5 
md5sum -c reads.fastq.gz.md5      # confirm integrity after transfer 
``` 
</div>

{% include accordion title="FASTQ validation" class=" " controls="fastq-3" icon=false %}
<div id="fastq-3" class="accordion_content" markdown='1' hidden> 
Validate read structure and quality encoding with `seqkit`. Check sequence count, length, and quality.
```bash
seqkit stats reads.fastq.gz
seqkit fqchk reads.fastq.gz
```

</div>

{% include accordion title="Quality control" class=" " controls="fastq-4" icon=false %}
<div id="fastq-4" class="accordion_content" markdown='1' hidden> 
Generate QC reports with `FastQC`:
```bash
fastqc reads.fastq.gz -o qc_reports/
```
Summarize across multiple samples with MultiQC:
```bash
multiqc qc_reports/
```
</div>

{% include accordion title="Conversion" class=" " controls="fastq-5" icon=false %}
<div id="fastq-5" class="accordion_content" markdown='1' hidden> 
Convert FASTQ to FASTA (discarding qualities) with `seqkit` or `seqtk`:
```bash
seqkit fq2fa reads.fastq.gz > reads.fasta
seqtk seq -A reads.fastq.gz > reads.fasta
```
</div>

</div>


## SAM / BAM / CRAM

**Formats for storing aligned sequencing reads to a reference genome.**  
- **SAM**: plain-text, human-readable.  
- **BAM**: binary, compressed SAM (faster and smaller).  
- **CRAM**: reference-based compression (even smaller, requires reference for decompression).  

| **Data Type** | **Components**                 | **Molecule** | **Application / Analysis Step**                       |
|---------------|--------------------------------|--------------|-------------------------------------------------------|
| alignment     | mapped reads, flags, tags      | DNA, RNA     | output of aligners (e.g., BWA, STAR, HISAT2)          |
| post-alignment| sorted & indexed alignments    | DNA, RNA     | input for variant calling, expression quantification  |
| reduced size  | compressed alignments (`CRAM`) | DNA, RNA     | long-term storage, efficient disk use                 |


### Example SAM file(s)

<details markdown="1"><summary><b>aligned reads / alignment in SAM format (text-based)</b></summary>
```markdown
@SQ SN:chr1 LN:248956422
@PG ID:bwa PN:bwa VN:0.7.17-r1188 CL:bwa mem ref.fa reads.fq

r001  99  chr1  7  60  8M2I4M1D3M  =  37  39  TTAGATAAAGAGGATACTG  *  NM:i:1  MD:Z:8A4^G3
```
**Header line(s)** start with @ (e.g., reference info, program info).   
**Alignment lines** contain fields: *QNAME*, *FLAG*, *RNAME*, *POS*, *MAPQ*, *CIGAR*, *RNEXT*, *PNEXT*, *TLEN*, *SEQ*, *QUAL*. 
Optional tags (e.g., NM:i, MD:Z) carry alignment metadata.
</details>

<div class="highlighted highlighted--info ">
<div class="highlighted__body"  markdown="1">
<h4 class="highlighted__heading">BAM / CRAM are binary files</h4>
Unlike SAM (plain text), **BAM** and **CRAM** are binary formats that cannot be viewed directly in a text editor. 
Use `samtools view` for on-the-fly conversion and content preview (see [File validation](#file-validation-2) section for example commands).  
</div>
</div>


### Format structure

Each alignment line has **11 mandatory fields**, followed by optional tags:  

```bash
   #1   #2    #3  #4   #5    #6    #7    #8   #9 #10  #11 optional #12+ 
QNAME FLAG RNAME POS MAPQ CIGAR RNEXT PNEXT TLEN SEQ QUAL [TAG:TYPE:VALUE]
```
- **QNAME**: query (read) name
- **FLAG**: bitwise flag encoding read properties (e.g., paired, mapped, reverse strand)
- **RNAME/POS**: reference sequence name & alignment start position
- **MAPQ**: mapping quality score
- **CIGAR**: alignment string (matches, insertions, deletions)
- **SEQ/QUAL**: read sequence & qualities

<div class="highlighted highlighted--error ">
<div class="highlighted__body"  markdown="1"><h4>Common issues</h4></div>
<div style="margin-left: 1.3em; margin-right: 1.3em;" markdown="1">
**Unsorted files**  
Variant callers and many tools require coordinate-sorted BAM files.  

**Missing index**  
`.bam` files must be indexed (`.bai`) for random access.  

**Reference mismatch**  
CRAM decompression requires the same reference genome used during compression.  

**Corrupted compression**  
Interrupted downloads/transfers may truncate BAM/CRAM files.  

**Incorrect flags**  
Misinterpretation of FLAG values can lead to wrong assumptions about read pairing or orientation.  
</div>
&nbsp;
</div>


### File validation

Always validate SAM/BAM/CRAM for format correctness, index availability, and alignment integrity before downstream analysis.  

<div class="usa-accordion " >

{% include accordion title="Compression (format conversion)" class=" " controls="sam-1" icon=false %}
<div id="sam-1" class="accordion_content" markdown='1' hidden>
Convert between formats with `samtools`:  

```bash
# SAM to BAM
samtools view -bS input.sam > output.bam

# BAM to SAM
samtools view -h input.bam > output.sam

# BAM to CRAM
samtools view -C -T reference.fa input.bam > output.cram

# CRAM to BAM
samtools view -b -T reference.fa input.cram > output.bam
```
</div>

{% include accordion title="Sorting & Indexing" class=" " controls="sam-2" icon=false %}
<div id="sam-2" class="accordion_content" markdown='1' hidden> 
```bash 
# Sort by coordinate 
samtools sort input.bam -o input.sorted.bam
# Create index
samtools index input.sorted.bam
```
Generates `.bai` (for BAM) or `.crai` (for CRAM). Required for random access.
</div>

{% include accordion title="File integrity" class=" " controls="sam-3" icon=false %}
<div id="sam-3" class="accordion_content" markdown='1' hidden>
Check integrity with `samtools quickcheck`. Verify file consistency and absence of truncation.  
```bash
samtools quickcheck -v input.bam
```
</div>

{% include accordion title="Alignment statistics" class=" " controls="sam-4" icon=false %}
<div id="sam-4" class="accordion_content" markdown='1' hidden> 
Generate alignment summary with `samtools flagstat`:
```bash
samtools flagstat input.bam
```
More detailed stats with:
```bash
samtools stats input.bam > stats.txt
plot-bamstats -p plots/ stats.txt
```
</div>

{% include accordion title="Viewing & Filtering" class=" " controls="sam-5" icon=false %}
<div id="sam-5" class="accordion_content" markdown='1' hidden> 
View alignments or filter by criteria:
```bash
samtools view input.bam | head
samtools view -f 4 input.bam        # unmapped reads
samtools view -q 30 input.bam       # reads with MAPQ ≥30
```
</div>
</div>

## GTF

**Gene Transfer Format is a tab-delimited text format for describing gene annotations on a reference genome.** 
It is a more rigid, widely used variant of [GFF](#gff), often required by RNA-seq tools.  

| **Data Type** | **Components**                          | **Molecule** | **Application / Analysis Step**                     |
|---------------|-----------------------------------------|--------------|-----------------------------------------------------|
| annotation    | exons, transcripts, genes               | DNA, RNA     | RNA-seq quantification (e.g., featureCounts, HTSeq) |
| genome model  | gene models from reference databases    | DNA, RNA     | input for alignment and expression analysis         |
| processed data| standardized annotations (Ensembl, GENCODE) | DNA, RNA | consistent references across pipelines              |


### Example GTF file

<details markdown="1"><summary><b>annotation records in GTF format</b></summary>
```markdown
chr1    HAVANA  gene        11869   14409   .   +   .   gene_id "ENSG00000223972"; gene_name "DDX11L1";
chr1    HAVANA  transcript  11869   14409   .   +   .   gene_id "ENSG00000223972"; transcript_id "ENST00000456328";
chr1    HAVANA  exon        11869   12227   .   +   .   gene_id "ENSG00000223972"; transcript_id "ENST00000456328"; exon_number "1";
```
These GTF lines describe a **gene annotation on chromosome 1**: the gene *DDX11L1* (`ENSG00000223972`) located on the forward strand 
from positions **11869–14409**, one of its **transcripts** (`ENST00000456328`) spanning the same coordinates, and the first **exon** of that transcript covering **11869–12227**.  

</details>


### Format structure

GTF files are **tab-delimited with 9 required fields**:  

```bash
     #1     #2       #3        #4      #5    #6     #7    #8          #9
seqname source  feature     start     end score strand frame  attributes
chr1    HAVANA  gene        11869   14409   .   +   .         gene_id "ENSG00000223972"; gene_name "DDX11L1";
```
- **col 1 (seqname)**: chromosome
- **col 2 (source)**: annotation source (e.g., HAVANA)
- **col 3 (feature)**: feature type (gene, transcript, exon, CDS, UTR); features are hierarchical: `gene → transcript → exon/CDS`.
- **cols 4–5**: start and end positions (genomic coordinates)
- **col 6**: score (often “.” if not used)
- **col 7**: strand (+ or -)
- **col 8**: frame/phase (for CDS features)
- **col 9 (attributes)**: Attributes in 9th column encodes metadata as `key "value`. Common attributes include: `gene_id`, `transcript_id`, `gene_name`, `exon_number`.

<div class="highlighted highlighted--error ">
<div class="highlighted__body"  markdown="1"><h4>Common issues</h4></div>
<div style="margin-left: 1.3em; margin-right: 1.3em;" markdown="1">
**Incorrect delimiters**  
Spaces instead of tabs can break parsers.  

**Missing attributes**  
Some tools strictly require `gene_id` and `transcript_id`.  

**Version mismatches**  
GTF annotations must match the reference genome version used in alignment.  

**Mixed GFF/GTF use**  
Confusion between GTF (rigid) and GFF3 (flexible) formats can cause pipeline errors.  
</div>
&nbsp;
</div>


### File validation

Validate GTF annotations for format correctness, genome build consistency, and completeness before RNA-seq analysis.  

<div class="usa-accordion " >

{% include accordion title="Compression" class=" " controls="gtf-1" icon=false %}
<div id="gtf-1" class="accordion_content" markdown='1' hidden>
```bash
# Compress
gzip annotations.gtf
bgzip annotations.gtf

# Decompress
gunzip annotations.gtf.gz
bgzip -d annotations.gtf.gz
```
</div>

{% include accordion title="File integrity" class=" " controls="gtf-2" icon=false %}
<div id="gtf-2" class="accordion_content" markdown='1' hidden> 
```bash 
md5sum annotations.gtf > annotations.gtf.md5 
md5sum -c annotations.gtf.md5     # confirm integrity after transfer
``` 
</div>

{% include accordion title="Format validation" class=" " controls="gtf-3" icon=false %}
<div id="gtf-3" class="accordion_content" markdown='1' hidden> 
Use `gffread` to validate the format:
```bash
gffread annotations.gtf -E -T -o validated.gtf
```
Writes a normalized GTF to `validated.gtf` (not a format change). This is strict validation: reports problems to STDERR or exits non-zero on fatal errors.
- `-E` enables stricter checks and emits warnings/errors. 
- `-T` forces GTF output (without `-T`, output is GFF3).
</div>

{% include accordion title="Format conversion" class=" " controls="gtf-4" icon=false %}
<div id="gtf-4" class="accordion_content" markdown='1' hidden> 
Convert between GTF and GFF formats:
```bash
# Convert **GTF → GFF3**
gffread annotations.gtf -o annotations.gff3
# Convert **GFF3 → GTF**
gffread annotations.gff3 -T -o annotations.gtf
```
</div>

{% include accordion title="Genome build consistency" class=" " controls="gtf-5" icon=false %}
<div id="gtf-5" class="accordion_content" markdown='1' hidden> 
Check annotation matches reference FASTA. Ensure chromosome naming conventions match (e.g., `chr1` vs `1`).
```bash
grep -v "^#" annotations.gtf | cut -f1 | sort | uniq | head
samtools faidx reference.fa
```
Compare/assess annotation consistency with `gffcompare`:
```bash
gffcompare -r reference.gtf -o qc annotations.gtf       # outputs: qc.stats (summary), qc.annotated.gtf (classified features)
```
Compares your GTF against a trusted reference to detect mismatches/novel features.
</div>
</div>

## GFF

## BED

## VCF / BCF






## Practical guidance



{% if page.takeaways %}
## Best Practices

<ul>
  {% for takeaway in page.takeaways %}
    <li>{{ takeaway }}</li>
  {% endfor %}
</ul>
{% endif %}