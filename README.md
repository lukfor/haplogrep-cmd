# HaploGrep 2

We provide a fast and free [haplogroup classification service](https://haplogrep.uibk.ac.at/). You can upload your mtDNA profiles (VCF or HSD) and receive the mitochondrial haplogroup in return. So far, HaploGrep and the updated HaploGrep 2 have been cited over 400 times (Google Scholar - June 2018). 

## Commandline Version for local usage
HaploGrep only requires Java 8 and therefore works on Linux, Windows and Mac systems. 

### Download & Run
The latest release can be downloaded from [here](https://github.com/seppinho/haplogrep-cmd/releases/download/v2.1.6/haplogrep-2.1.6.jar) (Currently v2.1.6). VCF and HSD sample files can be found [here](https://github.com/seppinho/haplogrep-cmd/tree/master/haplogrep/test-data).
 
    java -jar haplogrep-2.1.6.jar --in <input> --format vcf/hsd --out haplogroups.txt
   
## Additional Parameters      
* For adding additional output columns (e.g. found or remaining polymorphisms) please add the `--extend-report` flag. (Default: off).
* To change the metric to Hamming or Jaccard add the `--metric` parameter. (Default: kulczynski).
* The used Phylotree version can be changed using the `--phylotree` parameter. (Default: 17).
* If your variants are from genotyping arrays, please addd the `--chip` parameter. The range will then be limited to available array SNPs. (Default: off).

## Heteroplasmies
Heteroplasmies are often stored as heterozygous genotypes (0/1). If a HF field (= Heteroplasmy Frequency of variant allele; introduced by MToolBox) is specified in the VCF header, we add variants with a HF > 0.96 to the input profile.

Please have a look at [mtDNA-Server](http://mtdna-server.uibk.ac.at) to check for heteroplasmies and contamination in your NGS data.   

## Google User Group
We would love to hear your input. If you have any questions regarding HaploGrep, please join our [Google User Group](https://groups.google.com/forum/#!forum/haplogrep).

## Blog
Check out our [blog](http://haplogrep.uibk.ac.at/blog/) regarding mtDNA topics.

   
## Cite use
If you use HaploGrep, please cite 
http://nar.oxfordjournals.org/content/early/2016/04/15/nar.gkw233

as well as Mannis van Ovens work - Phylotree 17: 
https://www.sciencedirect.com/science/article/pii/S1875176815302432
or Phylotree in general
https://www.ncbi.nlm.nih.gov/pubmed/18853457

## Contact
Division of Genetic Epidemiology

Medical University of Innsbruck 

[Sebastian Schoenherr](mailto:sebastian.schoenherr@i-med.ac.at) and [Hansi Weissensteiner](mailto:hansi.weissensteiner@i-med.ac.at) 
