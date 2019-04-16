# TGStools
TGStools is a bioinformatics suit to facilitate transcriptome analysis of long reads from third generation sequencing platform.


----------------------------
# Table of Contents
----------------------------

   * [Overview](#overview)
   * [Installation](#installation)
   * [Command and subcommand structure](#command-and-subcommand-structure)
      * [staAS](#staAS)
      * [calScoreD](#calScoreD)
      * [GOenrich](#GOenrich)

   * [License](#license)




----------------------------
# Overview
----------------------------




----------------------------
# Installation
----------------------------





----------------------------
## main.py: an integration classification tool of CNCI and PLEK for identify coding or non-coding transcripts (fasta file and gtf file)
----------------------------

### Input files

#### hg19.2bit and hg38.2bit
hg19.2bit and hg38.2bit are 2bit formate of human genomes which can be downloaded on UCSC.

#### gtf file
An annotation file in GTF format is like:

```
chr14 Ensembl exon  73741918  73744001  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1"; 
chr14 Ensembl exon  73749067  73749213  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1";  
chr14 Ensembl exon  73750789  73751082  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1"; 
chr14 Ensembl exon  73753818  73754022  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1"; 
```
Note: chromosome id must have prefix chr.

#### fasta file
An annotation file in FASTA format is like:
```
> SEQ1
AGCACCAGCCGCCCCATCGCCACCGCCGCCGCCGCCGCCCGGATCCTGGCGCGCTGAATGCAGACTAACA
> SEQ2
CCATAAAGACTTCAAGGAACTAAGGTACAATAAGTGTCTTATGAACTTCAGCTGCAATGGAAAGAATGGAAGCT
```
Note: fasta file format must be the twolineFasta

### Usage
```
python3 main.py -f <file> -p <parallel> -d <directory> -g
or 
python3 main.py -f <file> -p <parallel>
```

- **-f**  | **--file**: input file of fasta file or gtf file, if the input is fasta file,the file format must be the twolineFasta

- **-p**  | **--parallel**: assign the running CUP numbers

- **-g**  | **--gtf**: if your input file is gtf format please use this parameter

- **-d**  | **--directory**: if you use the -g  this parameter must be assigned, within this parameter please assign the path of your reference genome. Some reference files which has been prepared could be download at [hg38](hgdownload.cse.ucsc.edu/goldenPath/hg38/bigZips/hg38.2bit), [hg19](hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/hg19.2bit).

### Example
```
python3 main.py -f candidate.gtf -p 6 -g -d hg38.2bit
or 
python3 main.py -f candidate.fasta -p 6
```

### Output files

mainly contains 4 files

#### input_no_suffix directory
the name is the intput file name without suffix. it contain the file of CNCI.index, which is the result of the software CNCI output.

#### input_no_suffix_PLEK
which is the result of the software PLEK output.

#### input_no_suffix_union_plek_cnci.txt
the output of union of the software CNCI and PLEK, in which the first column is the tanscript ID

#### input_no_suffix_intersect_plek_cnci.txt
the output of intersect of the software CNCI and PLEK, in which the first column is the tanscript ID


----------------------------
## extract_lncRNA_gtf.py: A tool that extract lncRNA information of GTF format based on the tanscript ID of the candidate lncRNA
----------------------------

### Input files

#### index index
input file of the candidate lncRNA, in which the first column is the tanscript ID of the candidate lncRNA.

#### gtf file

### Usage
```
python3 extract_lncRNA_gtf.py -f <file> -g <gtf> -o <out>
```

- **-f**  | **--file**: input file of the candidate lncRNA, in which the first column is the tanscript ID of the candidate lncRNA. This file also can be the output file of CNCI.py

- **-g**  | **--gtf**: GTF file corresponding to fasta in the main.py, where the last column contain the tanscript ID

- **-o**  | **--out**: output file extract lncRNA information of GTF format

### Example
```
python3 extract_lncRNA_gtf.py -f test.index -g unannotation.gtf -o out
```

### Output files
output file extract lncRNA information of GTF format

----------------------------
## tiss_specific.py : A tool that extract cancer-specific lncRNA information of GTF format
----------------------------

### Input files

#### FILE 
files of the candidate lncRNA gtf format.

#### TISSUE FILE


### Usage
```
python3 tiss_specific.py -f <file> <file> -t <tss> -o <out>
or 
python3 tiss_specific.py -f <file> -t <tss> -o <out>
```

- **-f**  | **--file**: input files of the candidate lncRNA gtf format, if the input files have two, the first set control sample, the other set cancer sample. If the input files have only one, there are two situations. The one is based on a background control tissue. The other have not a background control tissue. Related knowledge can refer to the https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3185964/

- **-t**  | **--tss**: set tissue name. The background control tissue : 'adipose', 'adrenal', 'brain', 'breast', 'colon', 'heart', 'kidney', 'liver', 'lung', 'lymphNode', 'ovary', 'prostate', 'skeltalMuscle', 'whiteBloodCell', 'testes', 'thyroid', 'placenta', 'foreskin', 'hLF'. 

- **-o**  | **--out**: output file which extract lncRNA-specific information of GTF format

### Example
```
python3 tiss_specific.py -f control.gtf sample.gtf -t breast -o out.gtf
or 
python3 tiss_specific.py -f sample.gtf -t breast -o out.gtf

```

### Output files
GTF file which extract lncRNA-specific information

----------------------------
## staAS: calculate the proportion of each alternative splicing event in different samples and create graphs.
----------------------------

### Input files

#### gtf file
An annotation file in GTF format is required:

```
chr14 Ensembl exon  73741918  73744001  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1"; 
chr14 Ensembl exon  73749067  73749213  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1";  
chr14 Ensembl exon  73750789  73751082  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1"; 
chr14 Ensembl exon  73753818  73754022  0.0 - . gene_id "ENSG00000000001"; transcript_id "ENST00000000001.1"; 
```

### Usage
```
python3 TGStools.py staAS -n <names> -p <prefix>
```
List of options available:

- **-n**  | **--names**: names of GTF format file/files containing at least "exon" lines

- **-p**  | **--prefix**: prefix of output files

The command line to generate local AS events will be of the form:

### Example
```
python3 TGStools.py staAS -n K140,K510,SEC,SHEE,TE5 -p TEST
```

### Output files

#### statistics of alternative splicing events
TEST_AS.txt
```
	A3	A5	AF	AL	MX	RI	SE
sample1	2131	2224	4850	1050	330	2652	4608
sample2	2638	2737	5562	1170	462	3130	5397
sample3	1689	1628	2911	431	261	2104	3267
sample4	2849	2666	6876	954	389	3851	4870
```

#### bar image
<img src="https://github.com/BioinformaticsSTU/SMRCanaToolkits/blob/master/CDZ/bar.png" width = "500" height = "400"  />
TEST_bar.png

#### barh_align image
<img src="https://github.com/BioinformaticsSTU/SMRCanaToolkits/blob/master/CDZ/barh_align.png" width = "400" height = "400"  />
TEST_barh_align.png

----------------------------
## calScoreD: calculate score_D of each gene.
----------------------------

### Input files

#### ioi file
ioi file contains the transcript "events" in each gene.

### Usage
```
python3 TGStools.py calScoreD -c <control> -t <treated>
```
List of options available:

- **-c**  | **--control**: name of control sample

- **-t**  | **--treated**: names of treated samples

- **-p**  | **--prefix**: prefix of output files

The command line to generate local AS events will be of the form:

### Example
```
python3 TGStools.py calScoreD -c control -t treated1,treated2 -p TEST
```

### Output files

TEST_score_D.txt
```
	control_transcripts	treated1_transcripts	treated1_score	treated2_transcripts	treated2_score	
ENSG00000151466	ENST00000281142,ENST00000506368	ENST00000281142,ENST00000511426	0.667	ENST00000506368,ENST00000511426	0.667	1.334
ENSG00000128059	ENST00000264220	ENST00000264220	0	ENST00000264220	0	0
ENSG00000033178	ENST00000322244,ENST00000429659	ENST00000322244,ENST00000429659	0	ENST00000322244,ENST00000429659	0	0
ENSG00000145414	ENST00000274054	ENST00000274054	0	ENST00000274054	0	0
ENSG00000138767	ENST00000504123,ENST00000512485	ENST00000504123,ENST00000512485	0	ENST00000504804	1	1
```

To quantify the differential isoform usage between cells, we defined the score D of each gene as follows:

<img src="https://github.com/BioinformaticsSTU/SMRCanaToolkits/blob/master/CDZ/formula.png"  />

where gene j has isoform set a , and set b respectively in cell line X and Y ; c is the number of isoform intersection for set a and set b; d is the number of isoform union for set a and set b. Thus D sums up scores when comparing the control sample and treated samples.

----------------------------
## GOenrich: select top genes and make GO enrichment analysis
----------------------------

### Input files

#### score D file

### Usage
```
python3 TGStools.py GOenrich -i <input> -t <threshold> -n <number> -f <type> -p <prefix>
```
List of options available:

- **-i**  | **--input**: score D result

- **-t**  | **--threshold**: threshold for adjusted p-value

- **-n**  | **--number**: number of top genes for analysis

- **-f**  | **--type**: type of image, 'bar' and 'scatter' can be chosen

- **-p**  | **--prefix**: prefix of output files

### Example
```
python3 TGStools.py GOenrich -i TEST_score_D.txt -t 0.05 -n 500 -f all -p TEST
```

### Output files

#### TEST_GO_reports.txt
```
Gene_set	Term	Overlap	P-value	Adjusted P-value	Old P-value	Old Adjusted P-value	Z-score	Combined Score	Genes
GO_Biological_Process_2018	cellular response to DNA damage stimulus (GO:0006974)	26/330	9.13787228190808e-09	1.6576100319381273e-05	1.6286367911914452e-08	2.9543471392212826e-05	-1.3493239020889152	24.977116526080287	RPAIN;MUS81;BOD1L1;NUDT1;RECQL4;HERC2;RECQL5;CHEK2;MACROD1;NEK1;POLK;FNIP2;ZNF385A;VAV3;SHPRH;SLF1;DDX11;FANCA;RAD52;RNF168;RAD51B;PSME4;ATM;MMS19;CEP63;TP73
GO_Biological_Process_2018	regulation of striated muscle cell differentiation (GO:0051153)	3/9	0.0007078338095233947	0.18343007578220555	0.0015745180149574901	0.3173528532369876	-2.903223500208859	21.057954568005567	HDAC5;NRG1;HDAC9
GO_Biological_Process_2018	intraciliary retrograde transport (GO:0035721)	3/11	0.0013474224910073086	0.22220221806247806	0.0025281461092817154	0.36497538704896487	-2.67387109608216	17.67311619442282	DYNC2H1;ICK;DYNC2LI1
GO_Biological_Process_2018	double-strand break repair (GO:0006302)	11/142	0.00021322248264889278	0.09669639588127289	0.00028269216039523195	0.12820089473923768	-1.9459585449063224	16.4495269901104	RAD52;RECQL4;RNF168;RAD51B;HERC2;RECQL5;KDM4D;EME2;CHEK2;ATM;POLK
```

#### barh image
<img src="https://github.com/BioinformaticsSTU/SMRCanaToolkits/blob/master/CDZ/GO enrichment barh.png"  width="550" height="450" />
TEST_GO_enrichment_barh.png

#### scatter image
<img src="https://github.com/BioinformaticsSTU/SMRCanaToolkits/blob/master/CDZ/GO enrichment scatter.png" width="550" height="400" />
TEST_GO_enrichment_scatter.png





----------------------------
# License
----------------------------

TGStools is released under the MIT license.


