# Viral read detection (DNAnexus Platform App)

Counts reads from certain viruses in UCSF 500 panel

This is the source code for an app that runs on the DNAnexus Platform.
For more information about how to run or modify it, see
https://documentation.dnanexus.com/.
<!-- /dx-header -->

<!--
Viral detection applet
What does this app do?
This app maps paired reads from a BAM file to viral reference genomes and gives statistics on output.

What data are required for this app to run?
This app requires a BAM file previously mapped to the human reference genome (usually hg19),
a reference genome sequence fasta of viral genomes, and a bed file with the position information of specific viral genes found in the viral reference sequences in the fasta file.                                                                                                                                                    

The reference genome currently used contains *complete* viral genomes from Refseq or Genbank as well as HPV genomes from the HPV PAVe consortium.
Genomes downloaded from Refseq or Genbank have their accession IDs included in the sequence name.
There is only one sequence per HPV type, ie there is only one HPV30 sequence present.
The reference genome only contains viruses that are detected in the UCSF500 panel and their close relatives.
The reference genome doesn't contain any bacterial genomes.

The bed file with gene annotations is specific for each viral sequence in the fasta file. If you decide to use a different viral sequence in the fasta, you will have to change the gene bed file.
If the reference genome is changed, when selecting sequences please note that many GenBank sequences are of poor quality or are contaminated.

What does this app output?
Outputs the following files:
1. BAM file mapped to reference viral genome.
2. Bed file with coverage information.
3. Tsv file with summarized coverage statistics
4. Tsv file with summarized read information
5. Tsv file with coverage details by gene and virus
6. Tsv summary file with combined outputs of the above files (combined_annot_res_tsv)

This app performs the following steps:

Extracts unmapped reads with samtools.
Mapping with bwa mem to the viral reference genome.
Sorting with samtools sort.
Annotating viral reads using bedtools genomecov to get viral sequence coverage and depth statistics.
Annotating viral reads using bedtools coverage and custom scripts to get gene coverage information.




OPTIONS
•	count_cutoff: viruses that have a read count lower than this number will not be included in the report files. Default setting is 10. Change to 1 include all viruses detected regardless of read count.
•	mapq_filter: Excludes reads with mapq score below this cutoff value. Default is 10. A value of 0 means no filtering is applied.
•	bwa_advanced_options: Advanced command line options that will be supplied directly to the 'bwa mem' execution.


Reference fasta file:

The reference file currently contains 88 HPV sequences, HTLV1, HTLV2, HTLV3, 13 Hepatitis C sequences, 4 Hepatitis B sequences, one HHV4 sequence (EBV) and one HHV8 sequence.
The 88 HPV sequences (and gene annotations) were downloaded from https://pave.niaid.nih.gov/explore/reference_genomes/human_genomes

Table of sequences, subtypes, accession numbers, and sources:
https://docs.google.com/spreadsheets/d/1ilaGChfKWYNSgCmRu-fkmer-Omko7pnTtQN12p6iiPY/edit?usp=sharing
This table is also in my DNANexus project in Reference_data_viral_detection> fasta_reference_sequences> reference_viral_sequences_source.tsv





Gene reference bed file:

The gene positions were annotated based on the data available in Refseq or Genbank (or PaVE consortium in the case of HPV types). The gene annotations are specific per each sequence in the fasta file, so if you edit the fasta file, make sure you edit the bed file as well!


Description of probe set
There are 1069 viral probes in the current set. Each probe is 60 bp long.
The current probe set in the UCSF500 panel was designed by the lab of Charles Chiu. “I designed the probes using the MSSPE algorithm was developed in my laboratory (https://www.nature.com/articles/s41564-019-0637-9). They were not designed to be specific for individual viral species but were designed as highly conserved probes to broadly capture as many divergent viral sequences as possible. The annotation on the probes means that the probe sequences was derived from the HPV16 or HPV18 genome, for instance, but does not mean that the probe sequences are specific for that viral genome. The specificity should be provided by the sequence that is recovered.”

Using blast, I have made a table showing what viral species, if any, the probes map to. These
were run against any viral sequence in GenBank.
https://docs.google.com/spreadsheets/d/1bHP7e3b9mpkxZI8CqQdLP5WyupDgDnwyNUp5exz5rLk/edit?usp=sharing

Because there are multiple sequences in GenBank for each viral species, the percent identity of a species to a probe varies with the sequence. The min_ident column contains the % identity for the sequence with the lowest % identity of all the sequences for that virus species. Please note that the quality of sequences in GenBank varies widely and most sequences are not validated.



-->
