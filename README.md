[![GitHub Downloads](https://img.shields.io/github/downloads/seppinho/haplogrep-cmd/total.svg?style=flat)](https://github.com/seppinho/haplogrep-cmd/releases)
[![Java CI with Maven](https://github.com/seppinho/haplogrep-cmd/actions/workflows/maven.yml/badge.svg)](https://github.com/seppinho/haplogrep-cmd/actions/workflows/maven.yml)

Haplogrep is a command-line tool for mtDNA haplogroup classification. We also provide haplogrep as a fast and free [haplogroup classification web service](https://haplogrep.i-med.ac.at/). You can upload your mtDNA profiles aligned to **rCRS** or **RSRS** (beta) and receive mitochondrial haplogroups in return. **FASTA**, **VCF** and **hsd** input files are supported. As of today (August 18, 2021), Haplogrep and the updated Haplogrep2 have been cited over 920 times (Google Scholar - August 18, 2021). 


## Cite us
If you use Haplogrep, please cite our latest [Haplogrep2 paper](http://nar.oxfordjournals.org/content/early/2016/04/15/nar.gkw233) or the initial [Haplogrep paper](https://onlinelibrary.wiley.com/doi/abs/10.1002/humu.21382). 

## Requirements

Java 8 or higher

## Download and Install

Download and install the latest commandline version using the following commands:

```
curl -sL haplogrep.now.sh | bash
./haplogrep 
```
If you want to use our web service, please click [here](https://haplogrep.i-med.ac.at/app/index.html).

## Available Tools
Currently two tools are available. 

* [Classify](#haplogrep-classify) allows to classify input profiles into haplogroups. 
* [Distance](#haplogrep-distance) calculates the distance between two haplogroups. 

## Haplogrep Classify 
### Run Haplogrep Classification with test data
      wget https://github.com/seppinho/haplogrep-cmd/raw/master/test-data/vcf/HG00097.vcf.gz
      ./haplogrep classify --in HG00097.vcf.gz --format vcf --out haplogroups.txt
      
      
### Input File Formats

#### VCF or Fasta
The recommended input format is a **single-sample/multi-sample VCF** (\*.vcf.gz or \*.vcf).

#### FASTA
For alignment, [bwa version 0.7.17](https://github.com/lh3/bwa/releases/tag/v0.7.17) is used. For each input sequence, haplogrep excludes positions from the tested range that are (1) not covered by the input fragment or (2) has marked with a N in the sequence.

#### hsd Format 
You can also specify your profiles in the original Haplogrep **hsd** format, which is a simple tab-delimited file format consisting of 4 columns (ID, Range, Haplogroup and Polymorphisms). 

`Sample1 1-16569 H100 263G 315.1C 750G	1041G	1438G	4769G	8860G	9410G	12358G	13656C	15326G	16189C	16192T	16519C`  
`Sample2 1-16569 ? 73G	263G	315.1C 750G	1438G	3010A	3107C	4769G	5111T	8860G	10257T	12358G	15326G	16145A	16222T	16519C`

For readability, the polymorphisms are also tab-delimited (so columns >= 4). A hsd example can be found [here](https://raw.githubusercontent.com/seppinho/haplogrep-cmd/master/test-data/h100.hsd.txt). 

### Required Parameters   
|Parameter| Description|
|---|---|
|```--in``` | Please provide the input file name|
|```--format``` | Please provide the input format of your data - valid options are: ```hsd, vcf, or fasta``` files|
|```--out``` | Please provide an output name|

### Additional Parameters   
|Parameter| Description|
|---|---|
|```--rsrs```| By default Haplogrep expects that your data is aligned against **rCRS** (which is included in the human references hg19 and hg38). If your data is aligned against **RSRS**, add the `--rsrs` parameter (Default: off). Please read [this blog post](http://haplogrep.uibk.ac.at/blog/rcrs-vs-rsrs-vs-hg19/) carefully before adding this option.|
|```--metric```| To **change the classification metric** to Hamming Distance (```hamming```) or Jaccard (```jaccard```) add the `--metric` parameter (Default: Kulczynski Measure).|
|```--extend-report```| For additional information on mtSNPs (e.g. found or remaining polymorphisms) please add the `--extend-report` flag (Default: off).|
|```--phylotree```|  The used **Phylotree version** can be changed using the `--phylotree` parameter (Default: ```17```, allowed numbers from ```10,11,12,..,17``` ([latest version](http://phylotree.org/rCRS-oriented_version.htm))).|
|```--chip```| If you are using **genotyping arrays**, please add the `--chip` parameter to limit the range to array SNPs only (Default: off, VCF only). To get the same behaviour for hsd files, please add **only** the variants to the range, which are included on the array or in the range you have sequenced (e.g. control region). Range can be sepearted by a semicolon `;`, both ranges and single positions are allowed (e.g. 16024-16569;1-576;8860). |
|```--fixNomenclature```|  To fix the mtDNA nomenclature after alignment of fasta files, set the `--fixNomenclature` parameter. See below for further information.|
|```--hits``` |  To export the **best n hits** for each sample add the `--hits` parameter. By default only the tophit is exported.|
|```--lineage```|  Create a **graph** of all input samples by using the `--lineage` parameter. (Default: 0). 0=no tree, 1=tree with genotypes, 2=only structure, no genotypes. As an output we provide a [Graphviz](http://www.graphviz.org/documentation/) DOT file. You can then use graphviz (`sudo apt-get install graphviz`) to convert the dot file to a e.g. pdf (`dot <dot-file> -Tpdf > graph.pdf`).|

## Haplogrep Distance
This tool allows to calculate the distance between two haplogroups. 

### Required Parameters   
|Parameter| Description|
|---|---|
|```--in``` | Input file must include 2 columns named "hg1" and "hg2" seperated by ";" |
|```--out``` | Output location of distance file |

## mtDNA reference sequences
Several mtDNA references exist, Haplogrep supports rCRS and RSRS. Please checkout [our blog post](http://haplogrep.uibk.ac.at/blog/rcrs-vs-rsrs-vs-hg19/) to learn more about this topic.

## Genotyping arrays
If you are using Haplogrep for genotyping array data, please have a look at the `--chip` parameter above. 

## mtDNA Nomenclature
When using fasta as an input format, Haplogrep uses bwa mem to align data. Since the mitochondrial phylogeny is using a 3′ alignment, indels are often not correctly placed for haplogroup classification, when using standard-aligner designed for nuclear DNA. In some cases, where haplogroup defining indels are expected (e.g. missing 8281d-8289d) this can yield to a lower haplogroup quality. To adjust for that, we provide a set of currently 66 rules that can be applied prior to classification. The rules have been estimated based on 7,848 fasta files in 4 steps:
   1. Downloading Phylotree defining sequences from GenBank
   2. Aligning data with bwa mem 
   3. Classifying the profiles using Haplogrep
   4. Comparing final fasta profiles with the Phylotree input profiles (remaining vs. not found) in a txt format (derived from parsing Phylotree). 
For example, the subsequent rule changes input polymorphisms `309.1CCT 310C` **to** `309.1C 309.2C 315.1C`. 

## Heteroplasmies (VCF only)
Heteroplasmies are often stored as heterozygous genotypes (0/1). If a **AF tag** (= Allele Frequency) is specified in the VCF file, we add variants with a AF > 0.90 to the input profile. [Mutation Server](https://github.com/seppinho/mutation-server) is able to create a valid VCF including heteroplasmies starting from **BAM or CRAM**. 

Please have a look at [mtDNA-Server](http://mtdna-server.uibk.ac.at) to check for heteroplasmies and contamination in your NGS data.

## Blog
Check out our [blog](http://haplogrep.uibk.ac.at/blog/) regarding mtDNA topics.

## Contact
[Sebastian Schoenherr](mailto:sebastian.schoenherr@i-med.ac.at) ([@seppinho](https://twitter.com/seppinho))  
[Hansi Weissensteiner](mailto:hansi.weissensteiner@i-med.ac.at) ([@haansi](https://twitter.com/whansi))  
Institute of Genetic Epidemiology, Medical University of Innsbruck
