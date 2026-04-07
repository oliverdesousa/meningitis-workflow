
### README

---

# Meningitis Bacterial Genomics Workflow (v0.1)

## Purpose:

End-to-end workflow for analysing bacterial meningitis (MG) genomic data for:

- pathogen identification
- outbreak investigation
- transmission tracking
- cluster detection
- AMR and serotype surveillance


## Workflow Overview:

```
Patient → Sample + Metadata → Sequencing → Bioinformatics → Public Health Insight
```

![My Image](wf2.png)

---

### 🧍1. Sample & Metadata Collection
Samples:

- Cerebrospinal fluid (CSF)
- Cultured bacterial isolate (preferred)

Minimum Metadata:

- Sample ID
- Collection date
- Location
- Age group
- Exposure (if known)

---

### 🧪 2. Sequencing

Preferred (Bacterial WGS):

```
Culture → DNA extraction → Illumina sequencing → FASTQ reads
```

- Output: FASTQ files (with Phred quality scores)
- Standard platform: Illumina (short reads)

Quality Targets:
- Coverage: 30x–50x
- Quality: ≥80% bases Q30

Alternative (if pathogen unknown):

```
CSF → mNGS → pathogen detection
```
- Used when culture fails
- Limitation: high human DNA contamination

---

### 💻 3. Bioinformatics Pipeline

---

✅ **Step 1: Quality Control (QC)**

Tools:
- FastQC
- MultiQC

Checks:
- Base quality → ≥80% Q30
- Coverage → ≥30x
- Contamination → low

Output:
- PASS / WARN / FAIL

---
🔍 **Step 2: Pathogen Identification**

Goal: Confirm species (e.g. N. meningitidis, S. pneumoniae)

Methods:
- k-mer / classification: Kraken2
- MinHash: Mash
- Alignment: BLAST
```
FASTQ → species identification → confirm pathogen
```

---

🧱 **Step 3: Genome Reconstruction**

3A. Assembly (Default):

```
FASTQ → SPAdes → FASTA
```

Used for:
- typing
- AMR detection
- general analysis

3B. Mapping (Optional – Outbreak):

```
FASTQ → bwa-mem → SNP calling (GATK)
```

Used for:
- transmission tracking
- phylogenetics

---

🏷️ **Step 4: Typing**

Types:
- MLST / cgMLST
- Serotyping / serogrouping

Species-Specific Tools:

| Pathogen                 | Tool                          |
| ------------------------ | ----------------------------- |
| *Neisseria meningitidis* | meningotype                   |
| *S. pneumoniae*          | seroba / SeroCall / PneumoCaT |
| *H. influenzae*          | hicap                         |
| *S. agalactiae*          | GBS tools                     |

Key Output:
- Species
- Serotype
- MLST / cgMLST

---

🧬 **Step 5: AMR + Virulence Detection**

```
FASTA → AMR detection → Virulence detection
```
Tools:
- AMRFinderPlus
- Abricate (ResFinder, CARD, VFDB)

Output:
- AMR genes
- Virulence genes
- % identity / coverage

---

🔗 **Step 6: Comparative Analysis (Outbreak Core)**
- SNP comparison or cgMLST
- Determine genetic similarity

```
Similar genomes → likely outbreak  
Different genomes → unrelated cases
```

---

🌳 **Step 7: Phylogenetics**
- Build tree (e.g. FastTree, IQ-TREE)
- Identify clusters

---

🔗 **Step 8: Integrate Metadata**

Combine:
- genomic similarity
- time
- location
- exposure

👉 This is where outbreak detection actually happens

---

📊 **Step 9: Public Health Interpretation**

Answer:
- Is this an outbreak?
- How many clusters?
- Where is it spreading?
- Is intervention needed?

---

📦 **Inputs & Outputs:**

Inputs:
- FASTQ (paired-end)
- Metadata (CSV/TSV)
- Reference databases

Outputs:
- QC report (MultiQC)
- Assembly (FASTA)
- Typing results
- AMR/virulence results
- Phylogenetic tree (optional)
- Final summary table

*Example Final Output Table:*

| Sample | Species         | Serotype | MLST  | AMR | Cluster   |
| ------ | --------------- | -------- | ----- | --- | --------- |
| S1     | N. meningitidis | B        | ST-41 | Yes | Cluster 1 |
| S2     | ...             | ...      | ...   | ... | ...       |

---

🔑 **Key Principles:**
- QC is mandatory before analysis
- Typing is species-specific
- Assembly = default; Mapping = outbreak mode
- Metadata + genomics = outbreak detection
- Pipeline should be modular and reproducible

---